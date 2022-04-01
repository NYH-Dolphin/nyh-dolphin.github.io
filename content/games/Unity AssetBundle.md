---
title: "Unity AssetBundle"
date: 2022-04-01
draft: false
type: "post"
tags: ["Unity"]
---

- 参考链接：[AssetBundle详解 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/38122862#:~:text=AssetBun,频，场景等资源。)

## 1. AssetBundle 介绍

- AssetBundle 是将资源使用 Unity 提供的一种用于存储资源的压缩格式打包后的集合
- 它可以存储任何一种 Unity 可以识别的资源，如模型，纹理图，音频，场景等资源，也可以加载开发者自定义的二进制文件
- 文件类型是 `.assetbundle/.unity3d` 
- 它们先前被设计好，很容易就下载到我们的游戏或者场景当中

### AssetBundle 开发流程

1. 创建 Asset bundle，开发者在 unity 编辑器中通过脚本将所需要的资源打包成 AssetBundle 文件
2. 上传服务器，开发者将打包好的 AssetBundle 文件上传至服务器中，使得游戏客户端能够获取当前的资源，进行游戏的更新
3. 下载 AssetBundle，首先将其下载到本地设备中，然后再通过 AssetBudle 的加载模块将资源加到游戏之中
4. 加载，通过 Unity 提供的 API 可以加载资源里面包含的模型、纹理图、音频、动画、场景等来更新游戏客户端
5. 卸载 AssetBundle，卸载之后可以节省内存资源，并且要保证资源的正常更新

## 2. AssetBundle 多平台打包

### 创建 AssetBundle

- 只有在 Asset 窗口中的资源才可以打包，先以 Prefab 的形式保存
- 单击要打包的 Prefab，在 Inspector 窗口右下角属性窗口底部会有一个 AssetBundle 的创建工具，直接创建
  ![img](https://pic1.zhimg.com/80/v2-77e20d87ea1185871beb4b55bda6fbc4_720w.jpg)
  - 名称固定为小写，如果使用了大写字母之后，系统会自动转换为小写格式

### 打包 AssetBundle

AssetBundle 创建之后需要导出，这一个过程就需要编写相应的代码实现

从 Unity5.x 之后，提供了一套全新简单的 API 来实现打包功能

- 在 Asset 目录下创建 Editor 目录
  - 表示该脚本是对于编辑器的一个扩展
- 在 Asset 目录下创建 StreamingAssets 目录
  - 该目录下存放着打包好的文件
- 创建下面所示的脚本，放在 Editor 目录下
  - 脚本写完之后，也不需要进行挂载，会自动在 Unity 的菜单栏中生成
  - 单击子菜单，既可以进行打包 AssetBundle

```c#
using UnityEditor;
public class ExportAssets : MonoBehaviour {

	[@MenuItem("Test/Build Asset Bundles")]
	static void BuildAssetBundles(){
        BuildPipeline.BuildAssetBundles(Application.streamingAssetsPath,BuildAssetBundleOptions.UncompressedAssetBundle,BuildTarget.StandaloneOSXUniversal);
	}
}
```

- 所有的 AssetBundle 已经被导出，此时每一个 AssetBundle 资源会有一个和文件相关的 Mainfest 的文本类型的文件，该文件提供了所打包资源的 CRC 和资源依赖的信息
  ![img](https://pic1.zhimg.com/80/v2-bf24cf1fcbca6bd185e0b4c330021eb4_720w.jpg)
  <img src="https://pic3.zhimg.com/80/v2-1bd3b4c3790ab8f2b8aef3493ed4ca2e_720w.jpg" alt="img" style="zoom:101%;" />

### AssetBundle 的压缩类型

Unity 为我们提供了三种压缩策略来处理 AssetBundle 的压缩

#### LZMA 格式

- 在默认情况下，打包生成的 AssetBundle 都会被压缩
- 在 Unity 中，AssetBundle 的标准压缩格式便是 LZMA（LZMA是一种序列化流文件）
- 因此在默认情况下，打出的 AssetBundle 包处于 LZMA 格式的压缩状态
- 在使用 AssetBundle 前需要先解压缩
- 使用 LZMA 格式压缩的 AssetBundle 的包体积最小（高压缩比），但是相应的会增加解压缩时的时间

#### LZ4 格式

- Unity 5.3 之后的版本增加了 LZ4 格式压缩
- 由于 LZ4 的压缩比一般，因此经过压缩后的 AssetBundle 包体的体积较大
- 使用 LZ4 格式的好处在于解压缩的时间相对要短

要使用 LZ4，在打包的时候开启 `BuildAssetBundleOptions.ChunkBasedCompression`

```c#
using UnityEditor;
public class ExportAssets : MonoBehaviour {

	[@MenuItem("Test/Build Asset Bundles")]
	static void BuildAssetBundles(){

BuildPipeline.BuildAssetBundles(Application.streamingAssetsPath,BuildAssetBundleOptions.ChunkBasedCompression,BuildTarget.StandaloneOSXUniversal);
	}
}
```

#### 不压缩

- 当然，也可以不对 AssetBundle 进行压缩
- 没有经过压缩的包体积最大，但是访问速度最快

要使用 LZ4，在打包的时候开启 `BuildAssetBundleOptions.UncompressedAssetBundle`

```c#
using UnityEditor;
public class ExportAssets : MonoBehaviour {

	[@MenuItem("Test/Build Asset Bundles")]
	static void BuildAssetBundles(){

BuildPipeline.BuildAssetBundles(Application.streamingAssetsPath,BuildAssetBundleOptions.UncompressedAssetBundle,BuildTarget.StandaloneOSXUniversal);
	}
}
```

## 3. AssetBundle 资源加载与卸载

### AssetBundle 加载

#### 获取 AssetBundle 对象

使用 AssetBundle 动态加载资源首先要获取 AssetBundle 对象

要在运行时加载 AssetBundle 对象主要可以分为两大类途径

- **先获取 WWW 对象，再通过 WWW.assetBundle 获取 AssetBundle 对象**

  - `public WWW(string url)`
    直接调用 WWW 类的构造函数，目标 AssetBundle 所在的路径作为其参数
    构造 WWW 对象的过程中会加载 Bundle 文件并返回一个 WWW 对象，完成后会在内存中创建较大的WebStream（解压后的内容，通常为原 Bundle 文件的 4~5 倍大小，纹理资源比例可能更大）
    因此后续的 AssetBundle.LoadAsset **可以直接在内存中进行**

    ```c#
    // [优点]
    // 后续的 Load 操作在内存中进行，相比 IO 操作开销更小
    // 不形成缓存文件，不需要额外的磁盘空间存放缓存
    // 能通过 WWW.texture，WWW.bytes，WWW.audioClip 等接口直接加载外部资源
    
    // [缺点]
    // 每次加载都涉及到解压操作
    // 在内存中会有较大的 WebStream
    IEnumerator GetData(){
        WWW data = new WWW ("file://"+Application.streamingAssetsPath + "/cube.assetbundle");
        yield return data;
        Material mat = (Material)data.assetBundle.LoadAsset ("Red");
        GetComponent<MeshRenderer> ().material = mat;	
    }
    ```

  - `public satic WWW LoadFromCacheOrDownload(string url, int version, unit crc=0)`
    WWW 类的一个静态方法，调用该方法同样会加载 Bundle 文件同时返回一个 WWW 对象
    该方法会将解压形式的 Bundle 内容**存入磁盘**中作为缓存（如果该Bundle已在缓存中，则省去这一步）
    完成后只会在内存中创建较小的 SerializedFile，而后续的 AssetBundle.LoadAsset **需要通过 IO 从磁盘中的缓存获取**

    ```c#
    // [优点]
    // 第二次加载时就省去了解压的开销
    // 一般情况下，内存中只有通常较小的 SerializedFile
    
    // [缺点]
    // IO 操作开销大
    // 需要额外的磁盘空间存放缓存
    // 只能加载 AssetBundle
    IEnumerator GetData(){
        Hash128 hash = new Hash128 ();
        WWW data=WWW.LoadFromCacheOrDownload("file://"+Application.streamingAssetsPath+ "/cube.assetbundle",new Hash128(1,1,1,1));
        yield return data;
    	Material mat = (Material)data.assetBundle.LoadAsset ("Red");
    	GetComponent<MeshRenderer> ().material = mat;
    }	
    ```

> 若使用 www 加载本地 AssetBundle 则需要在路径前加 `file://` 即可
>
> 例如 `file://"+Application.streamingAssetsPath + "/cube.assetbundle`

- **直接获取 AssetBundle**

  - `public static AssetBundle CreateFromFile(string path)`
    通过未压缩的 Bundle 文件，同步创建 AssetBundle 对象
    创建完成后只会在内存中创建较小的 SerializedFile，而后续的 AssetBundle.Load 需要通过 IO 从磁盘中获取

  - `public static AssetBundleCreateRequest CreateFromMemory(byte[] binary)`

    通过 Bundle 的二进制数据，异步创建 AssetBundle 对象
    完成后会在内存中创建较大的 WebStream
    调用时，Bundle的解压是异步进行的

```c#
IEnumerator GetData(string url){
    WWW data = new WWW (url);
	yield return data;
    AssetBundle bundle = AssetBundle.CreateFromMemoryImmediate(data.bytes);
	sphere.GetComponent<Renderer> ().material.mainTexture = (Texture)bundle.LoadAsset ("01");
    bundle.Unload (false);
}
```

#### AssetBundle 内部资源加载

- **LoadAsset：同步方法，根据名称进行加载**

  ```c#
  Texture mat = (Texture)data.assetBundle.LoadAsset ("1");
  GetComponent<MeshRenderer> ().material.mainTexture = mat;
  data.assetBundle.Unload (false);
  ```

- **LoadAllAsset：同步方法，加载所有的 GameObject 类型**

  ```c#
  GameObject[] obj = data.assetBundle.LoadAllAssets<GameObject> ();
  ```

- **LoadAssetAsync：异步方法，根据名称进行加载**

  ```c#
  AssetBundleRequest req = data.assetBundle.LoadAssetAsync("1");
  yield return req;
  if (req.isDone) {
  	Texture tex = (Texture)req.asset;
  }
  ```

- **LoadAllAssetsAsync ：异步方法，加载所有的 GameObject 类型**

  ```c#
  AssetBundleRequest req = data.assetBundle.LoadAllAssetsAsync<GameObject>();
  ```

### AssetBundle 卸载

待补充
