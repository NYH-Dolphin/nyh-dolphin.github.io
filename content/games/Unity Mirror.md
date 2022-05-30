---
title: "Unity Mirror"
date: 2022-05-13
draft: false
type: "post"
showTableOfContents: true
tags: ["Unity"]
---

## 1. Mirror 介绍

- Mirror 是完全免费的，它有很好的文档和说明
- Github Link: https://github.com/vis2k/Mirror
- 文档：https://mirror-networking.gitbook.io/docs/
- 教程
  - https://www.bilibili.com/video/BV1q3411e7QV?spm_id_from=333.337.search-card.all.click
  - https://www.youtube.com/watch?v=77vYKsXC4IE&list=PLXEG2omgKgCapAmGe20XBgd87rmxFdKhK&index=3

- SDK：https://mirror-networking.com/docs/api/Mirror.html
- 先修
  - C#
  - Unity  基础操作
  - 网络相关知识

### 安装

- 通过 Assets Store 可以直接下载 Mirror
  - 当安装好后，你会有一个 `Mirror` 文件夹和一个 `ScriptTemplates` 文件夹
- 在 Mirror/Examples 下面有一个 `Pong` 的文件夹，我们将以它为示例

## 2. Pong 游戏介绍

### 介绍

<img src="/images/Unity Mirror.assets/image-20220513092714779.png" alt="image-20220513092714779"/>	

- 首先，打包一个文件出来，可以用来做 Server，Unity 内的用于 Client
- 运行 exe 文件，可以看到游戏内有一些按钮
  
  <img src="/images/Unity Mirror.assets/image-20220513093150529.png" alt="image-20220513093150529"/>
- 点击 `Server Only` 将打包出来的应用作为服务器
  
  <img src="/images/Unity Mirror.assets/image-20220513093323182.png" alt="image-20220513093323182"/>
- 运行 Unity 内的游戏，点击 `Client` ，作为客户端
  
  <img src="/images/Unity Mirror.assets/image-20220513093437885.png" alt="image-20220513093437885"/>
- 此时可以看到 Client 里面的画面同步到了 Server 上
- 你可以按动上下键控制板子移动，可以观察到 Server 中的画面同步
- 如果在 Server 端点击 `Host(Server + Client)` 事实上，它会既是一个 Server，又是一个 Client，此时运行 Unity 内的 Client 点击 `Client` 加入，就可以开始玩了

### 代码分析

Scene 下有如下的 GameObject

<img src="/images/Unity Mirror.assets/image-20220513094140780.png" alt="image-20220513094140780" style="zoom:67%;" />

#### NetworkManager

这是控制整个网络的管理器，它的组件如下

<img src="/images/Unity Mirror.assets/image-20220513094311369.png" alt="image-20220513094311369" style="zoom:67%;" />	

- `Dont Destory On Load`：当进入一个新场景时，这个组件依旧存在
- `Run In Background`：运行在后台
- `Server Tick Rate`：高同步的游戏可以设置成 60，一般的游戏 30 就可以
- `Offline Scene`：当断开连接的时候，进入的 Scene
- `Online Scene`：当连接好的时候，进入的连接
- `Transport`：使用哪个传输层协议
- `Network Address`：具体连接到哪个 ip 地址
- `Max Connection`：最多有多少人连接服务器
- `Authenticator`：身份验证，可以添加用户名和密码
- `Player Prefab`：当你加入的时候，生成的东西
- `Plaer Spawn Method`
  - `Round Robin`：轮流在指定的两个位置生成
  - `Random`：随机在指定的两个位置生成
- `Left Racket Spawn`：左边生成的位置
- `Right Racket Spawn`：右边生成的位置
- `Registered Spawnable Prefabs`：两边需要同步的内容，在运行时生成

代码内部逻辑如下

```c#
public class NetworkManagerPong : NetworkManager
{
    public Transform leftRacketSpawn;
    public Transform rightRacketSpawn;
    GameObject ball;

    public override void OnServerAddPlayer(NetworkConnectionToClient conn)
    {
        // add player at correct spawn position
        Transform start = numPlayers == 0 ? leftRacketSpawn : rightRacketSpawn;
        GameObject player = Instantiate(playerPrefab, start.position, start.rotation);
        
        // 将 Player 分配给特定的 Connection
        NetworkServer.AddPlayerForConnection(conn, player);

        // spawn ball if two players
        if (numPlayers == 2)
        {
            // 找到共享的 Prefab，将它共享给连接到服务器内所有的玩家
            ball = Instantiate(spawnPrefabs.Find(prefab => prefab.name == "Ball"));
            NetworkServer.Spawn(ball);
        }
    }

    public override void OnServerDisconnect(NetworkConnectionToClient conn)
    {
        // destroy ball
        // 连接到服务器内的所有玩家出现的 ball 都会被摧毁
        if (ball != null)
            NetworkServer.Destroy(ball);

        // call base functionality (actually destroys the player)
        base.OnServerDisconnect(conn);
    }
}
```

<img src="/images/Unity Mirror.assets/image-20220513095404372.png" alt="image-20220513095404372" style="zoom:67%;" />	

- 这是游戏左上角的 GUI

<img src="/images/Unity Mirror.assets/image-20220513095444889.png" alt="image-20220513095444889" style="zoom:67%;" />	

- `Port`：运行的端口位置
- `Dual Mode`：勾选上同时支持 iPv6 和 iPv4 的广播，如果取消勾选仅支持 iPv4
- `No Delay`：降低时延
- `Interval`：KCP 内部更新间隔，KCP的默认值是100ms，但是建议使用较低的间隔来最小化延迟并扩展到更多的网络实体
- `Timeout`：KCP 超时(毫秒)，注意，KCP会自动发送一个ping
- 其它的部分是一些网络相关的细节参数，在这里就不过多介绍，可以自行查看

#### Ball

在 `Prefabs` 中，存在着需要同步的 Ball Prefab

<img src="/images/Unity Mirror.assets/image-20220513100813263.png" alt="image-20220513100813263" style="zoom:67%;" />	

```c#
public class Ball : NetworkBehaviour
{
    public float speed = 30;
    public Rigidbody2D rigidbody2d;

    // 当 Server 激活它的时候调用该方法 -> 这部分代码只在 Server 上运行
    public override void OnStartServer()
    {
        base.OnStartServer();

        // only simulate ball physics on server
        rigidbody2d.simulated = true;

        // Serve the ball from left player
        rigidbody2d.velocity = Vector2.right * speed;
    }

    float HitFactor(Vector2 ballPos, Vector2 racketPos, float racketHeight)
    {
        // ascii art:
        // ||  1 <- at the top of the racket
        // ||
        // ||  0 <- at the middle of the racket
        // ||
        // || -1 <- at the bottom of the racket
        return (ballPos.y - racketPos.y) / racketHeight;
    }

    // only call this on server
    [ServerCallback]
    void OnCollisionEnter2D(Collision2D col)
    {
        // Note: 'col' holds the collision information. If the
        // Ball collided with a racket, then:
        //   col.gameObject is the racket
        //   col.transform.position is the racket's position
        //   col.collider is the racket's collider

        // did we hit a racket? then we need to calculate the hit factor
        if (col.transform.GetComponent<Player>())
        {
            // Calculate y direction via hit Factor
            float y = HitFactor(transform.position,
                                col.transform.position,
                                col.collider.bounds.size.y);

            // Calculate x direction via opposite collision
            float x = col.relativeVelocity.x > 0 ? 1 : -1;

            // Calculate direction, make length=1 via .normalized
            Vector2 dir = new Vector2(x, y).normalized;

            // Set Velocity with dir * speed
            rigidbody2d.velocity = dir * speed;
        }
    }
}
```

<img src="/images/Unity Mirror.assets/image-20220513100756751.png" alt="image-20220513100756751" style="zoom:67%;" />	

- 需要有一个 Network Identity 来拥有一个 Network Transform

<img src="/images/Unity Mirror.assets/image-20220513100853611.png" alt="image-20220513100853611" style="zoom:67%;" />	

- `Client Authority`：如果移动是由客户端做的，那么设置成 True

## 3. Unity 多人游戏功能介绍

<img src="/images/Unity Mirror1.assets/image-20220515185820672.png" alt="image-20220513100853611" style="zoom:67%;" />	

### 组件

#### 传输

- 负责在网络间发送数据，同步数据信息

#### 运行

- 网络顶层，你的服务器/客户端
- 如果你使用 `Mirror` 的话，它们都是使用 Unity Build 出来的

#### 后端

- 数据库服务，安全验证，第三方 api 调用

#### 舰队

- 服务器舰队
- 分布式，管理更新和补丁，维护服务器 24 小时运行

### 网络拓扑

#### 本地多人游戏

- 其实是单机游戏，处理 2-4 个人同时玩
- 不需要传输/后端
- 延迟为零

#### LAN

- 在一定的局域网下运行
- 不使用互联网，但是需要传输
- 延迟很小，几乎为零

#### P2P

- **对等 P2P**：每个玩家都与其它所有玩家连接
  - 很便宜，但是逻辑很难处理
  - 很难扩展
- **C-S 架构**：其中一个玩家同时承担玩家和服务器的功能，其它玩家连接上它
  - 不是所有人的权限是相同的，核心玩家享有零延迟和一些特殊权限
  - 容易作弊

#### 委托服务器

- 专用服务器负责管理（本地/云）
- 容易扩展和制作功能
- 开销大

### 架构

<img src="/images/Unity Mirror1.assets/image-20220518180519142.png" alt="image-20220518180519142" style="zoom: 67%;" />	

#### 游戏要求

- 每场比赛支持 60 名玩家
- 实时游戏动作和物理
- 身份验证
- 游戏匹配
- 好友系统
- 排行榜
- 存储玩家数据
- 分析数据：每日活跃用户/每月活跃用户等

#### 服务器

采用委托舰队服务器，可以使用云服务器

- Azure Server
- Google Server
- Amazon Server

#### 身份验证

- 用户名/密码
- Playfab Authentication

#### 匹配

- 玩家 A 发送一个匹配请求，将它提交到匹配服务器上
- 服务器执行相关算法，匹配到某个房间
- 当房间里所有玩家接受后，匹配服务器会将房间放到服务器舰队中的某个服务器中
- 匹配服务器向游戏服务器申请开始，完成会话建立
- 游戏服务器准备好后，通知匹配服务器
- 匹配服务器向玩家传递信息，开始游戏
- 一些比较好的游戏匹配服务
  - flexmatch
  - openmatch

#### 好友系统

- 连接 Steam，Xbox 的好友
- 连接第三方的 api
- 使用 playfab 服务

#### 排行榜

- 可以使用第三方 api
  - Steamworks api
  - Playfab api

#### 数据库

- 可以使用 SQL 数据库
  - Amazon
  - Azure
  - Google

#### 分析数据

- gameanalytics.com

## 4. 数据同步

<img src="/images/Unity Mirror1.assets/image-20220518193424314.png" alt="image-20220518193424314" style="zoom:80%;" />	

### Network Identity

Mirror 是如何跟踪游戏中的 GameObject 的

<img src="/images/Unity Mirror1.assets/image-20220518181425282.png" alt="image-20220518181425282" style="zoom:67%;" />	

- 挂载 `NetworkIdentity` 的 GameObject，可以传输信息
- 你在运行中产生的所有与网络行为有关的 Prefab，都需要挂载 NetworkIdentity
- 如果你勾选 `Server Only` 的话，这个 Prefab 只会在服务器上生成，而不会在客户端

### Network Authority

- 知道哪个 Object 是属于谁的

#### 权限给客户端

添加权限

- `NetworkSetver.AddPlayerForConnection(conn, player)`
  - 添加一个 player prefab
  - 是 conn 的那个  client 拥有处理该 prefab 的能力
- `NetworkServer.Spawn(obj, connectionToClient)`
- `NetworkIdentity.AssignClientAuthroity(conn)`

取消权限

- `NetworkIdentity.RemoveClientAuthority()`
- `NetworkServer.ReplacePlayerForConnection(conn, player)`
  - 取消 player object 的权限

### 回调

Mirror 提供很多回调函数，用于各种情况下的事件处理

#### NetworkManager

| 回调函数          | Server                                                       | Client                                                       |
| ----------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Start             | `OnStartServer`<br />`OnServerSceneChanged`                  | `OnStartClient`<br />`OnClientConnect`<br />`OnClientChangeScene`<br />`OnClientSceneChanged` |
| Client Connect    | `OnServerConnect`<br />`OnServerReady`<br />`OnServerAddPlayer` |                                                              |
| Client Disconnect | `OnServerDisconnect`                                         |                                                              |
| Stop              | `OnStopServer`                                               | `OnStopClient`<br />`OnClientDisconnect`                     |

#### NetworkBehavior

|           | Server          | Client                                    |
| --------- | --------------- | ----------------------------------------- |
| Start     | `OnStartServer` | `OnStartClient`<br />`OnStartLocalServer` |
| Authority |                 | `OnStartAuthority`<br />`OnStopAuthority` |
| Stop      | `OnStopServer`  | `OnStopClient`                            |

### 属性

#### [Client]

- 标识只在客户端执行该函数

#### [Server]

- 标识只在服务器执行该函数

#### [Command]

- 客户端调用，但是会在服务器执行
- 只有自己的 Player Object / 有客户端权限的 GameObject / [Command(ignoreAuthority = true)] 可以执行该函数

```c#
[Commabd]
void MyCommand()
{

}
```

我们只能传输下面的数据类型

- Primitive(byte, int, float, string, etc)
- Built-in Unity math(Vector3, Quaternion, etc)
- Arrays of Primitive type
- Structs containing these allowable type
- NetworkIdentity
- GameObject with NetworkIdentity

#### [ClientRpc]

- 服务器调用，所有客户端执行
- 可以发送给所有已经生成的 NetworkIdentity
- 服务器有权限，所以所有的 server 都可以调用 client rpc

```c#
[ClientRpc]
void MyClientRPC()
{

}
```

- 同理，我们只能传输指定的数据类型

#### [TargetRpc]

- 服务器调用，指定的客户端执行
- 服务器的 GameObject 调用后，在指定的客户端的相同的 GameObject 执行

```c#
[TargetRpc]
void MyTargetRPC(NetworkConnection conn)
{
    // 以 NetworkConnection 作为参数开头的
    // 指定的 conn 执行该函数
}

[TargetRpc]
void MyTargetRPC()
{
    // 以其它参数或者空参数开头的
	// 所有脚本上拥有这段代码的执行该函数
}
```

#### [SyncVars]

- 属性，在客户端和服务器之间同步
- 只有继承 NetworkBehavior 的类才可以同步
- 只能由服务器向客户端同步，客户端不能给服务器同步

```c#
[SyncVar]
int x;
```

- 当属性改变了触发某个函数

```c#
[SyncVar(hook = OnChange)]
int x;

void OnChnage(int oldX, int newX)
{
	// 处理改变的回调函数
}
```
