---
title: "Unity Oculus SDK"
date: 2022-07-01
draft: false
type: "post"
showTableOfContents: true
tags: ["Unity"]
---

## 0. Oculus 开发相关资源

- 开发者社区+SDK：https://developer.oculus.com/

## 1. 介绍与配置

> Unity Asset Store Oculus Integration：https://assetstore.unity.com/packages/tools/integration/oculus-integration-82022

Oculus 集成包 `OculusIntegration.unitypackage` 是核心 VR 功能、组件、脚本和插件的多合一源，可简化和增强 Unity 中的 Oculus 应用程序开发过程。它打包了几个重要的 SDK，为您的应用程序提供高级渲染、社交和社区建设、示例框架、音频和头像开发支持。它包括：[VR](https://developer.oculus.com/documentation/unity/unity-utilities-overview/)、[Audio Manager](https://developer.oculus.com/documentation/unity/unity-audio/)、[Avatar](https://developer.oculus.com/documentation/unity/as-avatars-sdk-intro/)、[Platform](https://developer.oculus.com/documentation/unity/ps-platform-intro/)、[Sample framework](https://developer.oculus.com/documentation/unity/unity-sample-framework/)、[Spatializer ](https://developer.oculus.com/documentation/unity/audio-osp-unity/)和 [LipSync](https://developer.oculus.com/documentation/unity/audio-ovrlipsync-unity/)。

- 搭建环境与配置 https://developer.oculus.com/documentation/unity/unity-conf-settings/

## 2. 摄像机 Camera

> https://developer.oculus.com/documentation/unity/unity-add-camera-rig/

Oculus Integration SDK 包括 `OVRCameraRig` Prefab，只需要把它添加到你需要的 Scene 里即可

1. 在 Scene 中删除预先有的 `MainCamera`
2. 在 Assets/Oculus/VR/Prefab 文件夹中，拖入 `OVRCameraRig` 到 Scene 中

该 Prefab 包括下面几个重要的组件

#### OVR Camera Rig

<img src="../../static/images/Unity Oculus SDK.assets/image-20220705232348166.png" alt="image-20220705232348166" style="zoom:67%;" />	

- Use Per Eye Camera：选择此选项可分别为左眼和右眼使用摄像头
- Use Fixed Update For Tracking：选择此选项可以在 `FixedUpdate()` 方法中更新所有被跟踪的锚点，而不是 `Update()` 方法，以提高物理保真度。
- Disable Eye Anchor Cameras：选择此选项以禁用眼睛锚点上的摄像头，在这种情况下，游戏的主摄像机被用来提供VR渲染，跟踪空间锚被更新来提供参考姿态

#### OVRmanager

<img src="../../static/images/Unity Oculus SDK.assets/image-20220705232653860.png" alt="image-20220705232653860" style="zoom:67%;" />	

- Target Device：你可以通过调用 `OVRManager.systemHeadsetType()` 来返回当前 app 所在是 Oculus 类型，方法返回 `Oculus_Quest` 或 `Oculus_Quest_2`
- Performance/Quality
  - Use Recommended MSAA Level：默认情况下设置为 True，选择这个选项可以让 OVRManager 根据 Oculus 设备自动选择合适的 MSAA 级别
  - Monoscopic：如果为真，两只眼睛从中心眼睛姿势看到相同的图像，在低端设备上节省性能，我们不建议使用这个设置，因为它不能在VR中提供正确的体验
  - Min Render Scale：设置自适应分辨率的最小范围（默认值为0.7）
  - Max Render Scale (Rift only)：设置自适应分辨率的最大界限（默认值为1.0）
  - Head Pose Relative Offset Rotation: 设置头部姿势的相对偏移旋转
  - Head Pose Relative Offset Translation：设置头部姿势的相对偏移平移
  - Profiler TCP Port：Oculus Profiler Service的TCP监听端口，在调试或开发版本中激活，当应用程序在编辑器或设备上运行时，去Oculus > Tools > Oculus Profiler Panel查看实时系统指标
- Tracking
  - Tracking Origin Type：设置初始跟踪类型
    - Eye Level：跟踪相对于设备位置的位置和方向
    - Floor Level：根据用户在 Oculus 配置实用程序中指定的站立高度，跟踪相对于地面的位置和方向
  - Use Positional Tracking：启用时，头部跟踪会影响虚拟摄像头的位置
  - Use IPD in Positional Tracking：启用时，用户眼睛之间的距离会影响 OVRCameraRig 的每个摄像头的位置
  - Allow Recenter：选择此选项可在用户从通用菜单中单击“Reset View”选项时重置姿势。对于在虚拟世界中处于固定位置的应用程序，您应该选择此选项，并允许重置视图选项将用户放置到预定义的位置(如座舱座位)，如果有移动系统，请不要选择此选项，因为重置视图会有效地将用户传送到可能无效的位置
  - Late Controller Update：选择此选项可以在渲染之前立即更新控制器的姿态，以降低现实世界和虚拟控制器运动之间的延迟，如果控制器姿态用于模拟/物理，位置可能会稍微落后于用于渲染的位置(~10ms)。任何计算在模拟时间可能不完全匹配控制器的渲染位置
- Display
  - 重新控制您的应用程序通过在运行时设置特定的颜色空间，以克服由于使用不同的颜色空间可能发生的颜色变化

<img src="../../static/images/Unity Oculus SDK.assets/image-20220705233631727.png" alt="image-20220705233631727" style="zoom: 67%;" />	

- Quest Features
  - Focus Aware：选择此选项允许用户访问系统 UI，而不需要从应用程序切换上下文
  - Hand Tracking Support：从列表中，选择应用程序的输入支持类型
    - https://developer.oculus.com/documentation/unity/unity-handtracking/
  - Hand Tracking Frequency：从列表中，选择手的跟踪频率，更高的频率允许更好的手势检测和更低的延迟，但从应用程序的预算中保留了一些性能空间
    - https://developer.oculus.com/documentation/unity/unity-handtracking/#enable-hand-tracking
  - Requires System Keyboard：选择此选项允许用户与系统键盘交互
    - https://developer.oculus.com/documentation/unity/unity-keyboard-overlay/
  - System Splash Screen：点击 Select 打开 2D 纹理列表，选择你想设置为启动画面的图像
  - Allow Optional 3DoF Head Tracking：选择这个选项来支持 3DoF 和 6DoF，并让应用程序在没有头部跟踪的情况下运行

## 3. 输入

### 控制器 Controller

`OVRInput` 为多种控制器类型公开统一的输入 API

- 用于查询控制器的虚拟或原始状态，如按钮、操纵杆、触发器、电容触摸数据等，它支持 Oculus Touch控制器
- 对于键盘和鼠标控制，我们推荐使用 UnityEngine 的原始 API

> 注意
>
> - 在需要的 Scene 的任何地方包含 `OVRManager` 的实例
> - 在任意一个组件的 `Update` 和 `FixedUpdate` 方法的开头分别调用一次 `OVRInput.Update()` 和 `OVRInput.FixedUpdate()`

### OVRInput 使用

`OVRInput` 的主要用途是通过 `Get()`、`GetDown()` 和 `GetUp()` 来访问控制器输入状态

| 函数        | 作用                         |
| ----------- | ---------------------------- |
| `Get()`     | 查询控制器的当前状态         |
| `GetDown()` | 查询该帧是否按下了控制器     |
| `GetUp()`   | 查询该帧是否释放了一个控制器 |

#### 枚举类

`Get()` 有多种变体，提供对不同控件集的访问，这些控件集通过 `OVRInput` 定义的枚举公开，如下所示

| 控制器种类(枚举类)   | 解释                                                |
| -------------------- | :-------------------------------------------------- |
| `OVRInput.Button`    | 游戏手柄、Oculus Touch 控制器和后退按钮上的传统按钮 |
| `OVRInput.Touch`     | Oculus Touch 控制器上的电容敏感控制表面             |
| `OVRInput.NearTouch` | 在 Oculus Touch 控制器上发现了近距离敏感的控制表面  |
| `OVRInput.Axis1D`    | 报告浮点状态的触发器等一维控件                      |
| `OVRInput.Axis2D`    | 包含 thumbstick 的二维控件，报告 Vector2 状态       |

> 注意
>
> Oculus Touch 控制器除了传统的手柄按钮外，还具有电容感应式控制表面，它可以检测用户的手指或拇指是否有身体接触(Touch)，以及是否有近距离接触(NearTouch)，这允许检测用户与特定控制面交互的几个不同状态
>
> - 如果用户的食指完全从控件表面移开，该控件的 NearTouch 将报告false
> - 当用户的手指靠近并接近控件时，NearTouch会在用户进行身体接触之前报告为 true
> - 当用户进行物理接触时，该控件的Touch将报告为true
> - 当用户按下索引触发器时，该控件的Button将报告为true
>
> 这些不同的状态可用于准确地检测用户与控制器的交互，并使各种控制方案成为可能

#### 使用示例

```c#
// returns true if the primary button (typically “A”) is currently pressed.
OVRInput.Get(OVRInput.Button.One);

// returns true if the primary button (typically “A”) was pressed this frame.
OVRInput.GetDown(OVRInput.Button.One);

// returns true if the “X” button was released this frame.
OVRInput.GetUp(OVRInput.RawButton.X);

// returns a Vector2 of the primary (typically the Left) thumbstick’s current state.
// (X/Y range of -1.0f to 1.0f)
OVRInput.Get(OVRInput.Axis2D.PrimaryThumbstick);

// returns true if the primary thumbstick is currently pressed (clicked as a button)
OVRInput.Get(OVRInput.Button.PrimaryThumbstick);

// returns true if the primary thumbstick has been moved upwards more than halfway.
// (Up/Down/Left/Right - Interpret the thumbstick as a D-pad).
OVRInput.Get(OVRInput.Button.PrimaryThumbstickUp);

// returns a float of the secondary (typically the Right) index finger trigger’s current state.
// (range of 0.0f to 1.0f)
OVRInput.Get(OVRInput.Axis1D.SecondaryIndexTrigger);

// returns a float of the left index finger trigger’s current state.
// (range of 0.0f to 1.0f)
OVRInput.Get(OVRInput.RawAxis1D.LIndexTrigger);

// returns true if the left index finger trigger has been pressed more than halfway.
// (Interpret the trigger as a button).
OVRInput.Get(OVRInput.RawButton.LIndexTrigger);

// returns true if the secondary gamepad button, typically “B”, is currently touched by the user.
OVRInput.Get(OVRInput.Touch.Two);
```

- 除了指定一个控件外，Get() 还接受一个可选的控制器参数，支持的控制器列表由 OVRInput 定义
- 如果特定的控制方案只适用于特定的控制器类型，则可以使用指定控制器，如果没有向 Get() 提供控制器参数，则默认使用 Active 控制器，该控制器对应于最近报告用户输入的控制器
  - 例如，用户可能使用一对 Oculus Touch 控制器，把它们放下，拿起 Xbox 控制器，在这种情况下，一旦用户输入，Active 控制器就会切换到 Xbox 控制器。当前的 Active 控制器可以通过`OVRInput.GetActiveController()` 查询，所有连接控制器的位掩码可以通过`OVRInput.GetConnectedControllers()`查询

```c#
// returns a float of the Hand Trigger’s current state on the Left Oculus Touch controller.
OVRInput.Get(OVRInput.Axis1D.PrimaryHandTrigger, OVRInput.Controller.LTouch);

// returns a float of the Hand Trigger’s current state on the Right Oculus Touch controller.
OVRInput.Get(OVRInput.Axis1D.PrimaryHandTrigger, OVRInput.Controller.RTouch);
```

### touch 追踪

若要在游戏画面内显示手部控制器的模型的话，可以搜索 `OVRControllerPrefab`，将其分别加入 `OVRCameraRig` 下面的指定位置，展开后启用需要使用的模型，并且在 Inspector 面板中设置 Controller 的类型即可

<img src="../../static/images/Unity Oculus SDK.assets/image-20220706160317586.png" alt="image-20220706160317586" style="zoom:67%;" />	

`OVRInput` 通过 `GetLocalControllerPosition()` 和 `GetLocalControllerRotation()` 提供位置和方向数据，它们分别返回 `Vector3` 和 `Quaternion`

跟踪系统返回控制器姿势，并与头显同步预测，这些姿势与头戴设备在同一坐标系中报告，相对于最初的中心眼姿势，可以用于渲染 3D 世界中的手或物体

它们也被 `OVRManager.display.RecenterPose()` 重置，类似于头部和眼睛的姿势

> 注意
>
> - Oculus 手部控制器在 `OVRInput` 中区分了 Primary 和 Secondary：
>   - Primary 总是指左边的控制器
>   - Secondary 总是指右边的控制器

###  touch 输入映射

下面的图表说明了 Oculus Touch 控制器的常见输入映射

#### 虚拟映射（两个控制器一起作为结合的控制器/游戏控制器）

- 调用 `OVRInput.Controller.Touch`

<img src="https://scontent-lax3-2.xx.fbcdn.net/v/t39.2365-6/64515613_622041711631269_1500694343822868480_n.png?_nc_cat=103&ccb=1-7&_nc_sid=ad8a9d&_nc_ohc=_OsfBjw9k6MAX8udQwO&_nc_ht=scontent-lax3-2.xx&oh=00_AT-NpiEvalDdFaPWO32VFIuY5px38cuMbDXpXz8Jyh-pjg&oe=62C86D41" alt="img" style="zoom:67%;" />

#### 虚拟映射（两个控制器功能完全一样）

- 调用 `OVRInput.Controller.LTouch` 或者是  `OVRInput.Controller.RTouch`

<img src="https://scontent-lax3-2.xx.fbcdn.net/v/t39.2365-6/64700495_622041714964602_2286289187051143168_n.png?_nc_cat=106&ccb=1-7&_nc_sid=ad8a9d&_nc_ohc=GYCv0F8rxOwAX8YjmXj&_nc_ht=scontent-lax3-2.xx&oh=00_AT_bak51T6g1fGAlu_B2cJL3u2oWAVQcwF2KVRz-INuDHg&oe=62C8F0E5" alt="img" style="zoom:50%;" />

#### 原始映射

原始映射直接暴露控制器，控制器的布局与典型的左右分开手柄的布局非常相似

<img src="https://scontent-lax3-1.xx.fbcdn.net/v/t39.2365-6/64523870_622041708297936_5486636097175814144_n.png?_nc_cat=102&ccb=1-7&_nc_sid=ad8a9d&_nc_ohc=-4tRDTQg45wAX_ueQqM&_nc_ht=scontent-lax3-1.xx&oh=00_AT9IdfrJ1ByxCvY2yIudDzqSIIIEOfYe0PWyCcAgiTvdgw&oe=62CA014A" alt="img" style="zoom:50%;" />

### 手部追踪

参考设置方式：[Set Up Hand Tracking | Oculus Developers](https://developer.oculus.com/documentation/unity/unity-handtracking/)

## 4. 交互

### 示例场景

#### HandGrabExamples

HandGrabExamples 场景展示了手部的抓取

<img src="https://scontent-hkg4-2.xx.fbcdn.net/v/t39.2365-6/273047370_977092769887902_5419075921215623969_n.png?_nc_cat=109&ccb=1-7&_nc_sid=ad8a9d&_nc_ohc=3dVezgdeyLUAX_OsDlh&_nc_ht=scontent-hkg4-2.xx&oh=00_AT9dViIg-4bIlK8F0sGLX6w_w2rAVcLg8yuPEfgUpjaoFw&oe=62C980D4" alt="Basic grab image" style="zoom:50%;" />	

- Virtual Object 演示了一个简单的捏选择，没有手抓姿势
- Key 演示以捏为基础的抓取与单手抓取姿势
- Torch 演示了基于卷曲的抓取与单手抓取姿势（旋转对称）
- Cup 演示了多重捏和手掌抓取与相关的手抓取姿势互动
- 所有的手抓姿势展示手指在某些关节的自由抓取
- 所有物品都允许双手之间转移
- 这个场景还与控制器一起工作，控制器用手来表示

#### TransformerExamples

<img src="https://scontent-hkg4-2.xx.fbcdn.net/v/t39.2365-6/273116013_977092746554571_5543637192074529151_n.png?_nc_cat=104&ccb=1-7&_nc_sid=ad8a9d&_nc_ohc=PM1HEb7VwRwAX9FdNAu&_nc_ht=scontent-hkg4-2.xx&oh=00_AT-ubAJ7SCUp0vf1Isz2RvNDwrI9KhjtxGJ7n9Ayfc8eOA&oe=62CB4ACC" alt="Complex grab image" style="zoom:50%;" />	

TransformerExamples 场景展示了 GrabInteractor 和 HandGrabInteractable（分别用于控制器和手），以及物体上的物理、变形和约束

- 地图展示了平面约束下只能抓取的可翻译对象
- 这些石头宝石展示了可以用两只手拿起、投掷、变形和缩放的物理物体
- 盒子展示了一个有约束的单手旋转变换
- 这个小雕像展示了双手抓取时的隐藏功能

#### RayExamples

<img src="https://scontent-hkg4-2.xx.fbcdn.net/v/t39.2365-6/273125457_977092733221239_8914345502171791522_n.png?_nc_cat=110&ccb=1-7&_nc_sid=ad8a9d&_nc_ohc=YqqQl4U3hpwAX8hu-xJ&_nc_ht=scontent-hkg4-2.xx&oh=00_AT-_NFpfa41-EllvEWiSHD0jkmf9IunII10w8iKeG3tcVA&oe=62CB28D8" alt="Basic ray image" style="zoom:50%;" />	

RayExamples 场景展示了 RayInteractor 使用 CanvasCylinder 组件与一个弯曲的 Unity Canvas 进行交互

- 与 Unity Canvas 的射线交互
- 手的交互使用系统指针姿态和缩放来进行选择
- 射线交互使用控制器姿态来确定射线方向，并触发选择
- 允许各种材质类型的曲面：Alpha 切割，Alpha 混合和衬底（仅在设备上可见）

#### PokeExamples

<img src="https://scontent-hkg4-2.xx.fbcdn.net/v/t39.2365-6/275952769_1008007086796470_15710467349600420_n.png?_nc_cat=111&ccb=1-7&_nc_sid=ad8a9d&_nc_ohc=zAphW55K2-gAX_WG-I7&_nc_ht=scontent-hkg4-2.xx&oh=00_AT_6QqAynDd1fxj_ibxlu59EcO8nGsjXsam0ori7y6HCPg&oe=62CA5559" alt="Direct touch image" style="zoom:50%;" />	

PokeExamples 场景展示了 PokeInteractor 在各种表面上的触控限制

- 用独立按钮或 Unity Canva 交互和按按钮
- poke 是由射线投射到基于盒子的接近场检测
- 演示了多种视觉支持，包括各种按钮深度，以及按钮悬停状态的Hover on Touch和Hover Above变体
- 触摸限制现在保持手在按钮表面以上
- 大按钮与多个戳互动（手侧，手掌）
- 弯曲和扁平的统一画布与触摸有限的滚动和按钮

#### PoseExamples

<img src="https://scontent-hkg4-1.xx.fbcdn.net/v/t39.2365-6/273117286_977092779887901_4924288224043444800_n.png?_nc_cat=102&ccb=1-7&_nc_sid=ad8a9d&_nc_ohc=rJu8iaiuBgUAX8Q2v0o&_nc_ht=scontent-hkg4-1.xx&oh=00_AT8B8ZSBg9g5oOldSNExeMNxekXiaDRAQn0-_tF-vhaOVg&oe=62C9FC60" alt="Pose detection image" style="zoom:50%;" />	

PoseExamples 场景展示了六种不同的手部姿势，带有姿势识别的视觉信号

- 演示姿势检测几种手姿势:大拇指向上，大拇指向下，石头，布，剪刀，和停止
- 姿势可以由左手或右手触发
- 当姿势开始时触发粒子效果（每只手）
- 当姿势结束时隐藏粒子效果

#### GestureExamples

GestureExamples 场景展示了如何使用 Sequence 组件和 ActiveState 逻辑来创建一个简单的滑动手势<img src="https://scontent-hkg4-2.xx.fbcdn.net/v/t39.2365-6/278619243_1030644504532728_2273043170056198593_n.png?_nc_cat=111&ccb=1-7&_nc_sid=ad8a9d&_nc_ohc=rK7lqu7hIYAAX-jZpWQ&_nc_ht=scontent-hkg4-2.xx&oh=00_AT-cGiWRdEzxeYNuCReE3jDfD97yk6eHVX6UtO3ljv3Pjw&oe=62CAB4D5" alt="Gestures image" style="zoom:50%;" />

- 演示了滑动手势作为与对象的近交互的用例
- 手势只有在覆盖物体时才会活跃
- 石头显示由任意一只手触发的随机颜色变化
- 相框在旋转木马视图中通过图片循环方向变化取决于哪只手在滑动 

#### DistanceGrabExamples

DistanceGrabExamples 场景展示了发送信号、吸引和抓取距离对象的多种方法

<img src="https://scontent-hkg4-2.xx.fbcdn.net/v/t39.2365-6/289931274_1067825480814630_1157129558590603112_n.png?_nc_cat=109&ccb=1-7&_nc_sid=ad8a9d&_nc_ohc=PWRvU2cQmCsAX_moJAN&_nc_ht=scontent-hkg4-2.xx&oh=00_AT-lNV3C79Zowde0RxuULr12sj83KXkVQJCQ69g0f7zDAA&oe=62CAEB2E" alt="Distance Grab image" style="zoom: 67%;" />	

- Anchor at hand：锚定物品在手上而不吸引它
- Hand to interactable：使用了一个自定义的 IMovementProvider 和一个手对齐，对齐抓取来移动物体，就像手在那里一样
- ReticleGhostDrawer：在物体位置显示手的副本
- Interactable to hand：显示了一个自定义的 IMovementProvider，其中项目以快速的运动向手移动，然后保持连接到它。十字形网格抽屉表示物体如何被抓取的手吸引

#### TouchGrabExamples

TouchGrabExamples 场景展示了一个过程式姿势抓取交互

<img src="https://scontent-hkg4-2.xx.fbcdn.net/v/t39.2365-6/289978584_1067825504147961_3447768018627681865_n.png?_nc_cat=109&ccb=1-7&_nc_sid=ad8a9d&_nc_ohc=kcS9NhicAfEAX_HO3jN&_nc_ht=scontent-hkg4-2.xx&oh=00_AT8K2IQ771M1Isn8YWmaEPpcWvwwlrEJnsTLfJeg5H6GEQ&oe=62CA213D" alt="TouchGrabExample image" style="zoom:67%;" />	

- Kinematic 演示了与非物理对象的交互作用
- Physical 演示了非运动物体与重力的相互作用，以及物理碰撞中抓取和非抓取之间的相互作用
- Controller 可以作为手部姿势，具有相同的交互作用和程序姿势

### 输入数据

`InputOVR` Prefab 为手、控制器或 Hmd 组件附加源提供了基础，这些源数据来自 InteractionOVRCameraRig

你也可以找到手、控制器，Hmd，ControllerAsHand Prefab，这些 Prefab 可以与 InputOVR 相连

预制实例应该放在你的场景的根位置，或者它不会被上级链转换的位置



### 可抓取物体

#### Hand Pose Rocoder

带有 HandPose 的 HandGrabPoints 可以在编辑时手动创建，也可以在运行时使用手动跟踪（推荐）

在运行时候使用它们需要用到 Hand Pose Rocoder，在目录 Oculus/Interaction/Hand Grab Pose Rocorder

<img src="../../static/images/Unity Oculus SDK.assets/image-20220706203445366.png" alt="image-20220706203445366" style="zoom:67%;" />	

- Hand：HandGrabInteractor 将被用于记录，分配左手或右手取决于哪只手将被用于记录
  - 在编辑模式下分配这个值，然后进入播放模式可能会失去这个引用
- RigidBody：你需要记录抓取姿势的 GameObject
  - 它应该有一个 `RigidBody`
- Ghost Provider：指定一个 Ghost Provider 来立即可视化生成的姿势
  -  按下右边的圆圈应该可以选择默认值
- Hand Grab Interactable Data Collection：分配一个资产来存储或加载姿势，这样它们就可以在播放/编辑模式切换中存活下来
  - 如果没有提供资产，在存储时会自动生成一个新的资产

