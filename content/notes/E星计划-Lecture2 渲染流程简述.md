---
title: "E星计划-Lecture2 渲染流程简述"
date: 2022-04-03
draft: false
type: "post"
tags: ["E星计划"]
---

## 1. E 星计划大作业

### 基本要求

- 渲染引擎：BGFX
- 编程语言：C++/GLSL
- 平台：Windows/Visual Studio 2022
- 工程组织：CMake

### Level

作业分为 5 个 level，每完成一个 level 的所有需求，则可挑战下一个 level

详见作业包里面的 README.md

- **Level1（15%）**
  - 加载模型，绘制在屏幕上
  - 添加环绕相机（Orbit Camera），并可以使用鼠标操控（类似 geometryv 的操作方式）
    - 鼠标左键拖拽旋转镜头
    - 鼠标滚轮缩放镜头
- **Level2（15%）**
  - 为模型添加基础纹理
  - 为模型添加基础光照（Blinn-Phong）
  - 在保留环绕相机功能的情况下，为相机添加键盘控制
    - WASD 控制镜头上下左右平移
- **Level3（25%）**
  - 把模型的直接光漫反射光照改为 PBR 模型实现
    - 模型的金属度能正确影响漫反射光照
    - 模型的 albedo（反照率）使用纹理控制
  - 把模型的直接光改为 PBR 模型实现
    - 模型的金属度、粗糙度通过纹理控制
- **Level4（25%）**
  - 使用 IBL（Image-Based Lighting），为模型添加环境光照的漫反射部分
  - 使用 IBL，为模型添加环境光照的高光反射部分
  - 添加一个包住场景的天空盒
  - 天空盒使用 cubemap 纹理，对应 IBL 图中的 mipmap level 0
- **Level5（20%）**
  - 使用 ShadowMap 的方式，为模型添加阴影

### 提交要求

- 统一压缩为一个文件，格式为 zip 或 rar，大小在 200mb 以内
  - 包含编译好的，可正常运行的二进制文件
  - 包含录制的演示视频
  - 包含源码文件，可以使用 CMake 生成 VS 工程，可编译运行
  - 包含程序架构说明文档

## 2. GPU

- 全称图形处理单元 Graphics Processing Unit
- GPU 是显卡最核心的部件
- 显卡除了 GPU 外还有散热器，通讯元件，与显示器连接的各类插槽等
- 显卡不能直接工作，必须插在主板上

### GPU 的发展史

#### 厂商

<img src="/images/E星计划-Lecture2 渲染流程简述.assets/image-20220402001118296.png">

#### 历史

- 1995：NV1
- 1997：Riva 128（NV3）
- 1998：Riva TNT（NV4）
- 1999：GeForce256（NV10）
- 2001：GeForce 3
- 2003：GeForce FX 系列（NV3x）
- 2004：GeForce 6 系列（NV4x）
  - 孤岛惊魂系列 第一代
- 2006：GeForce 6 系列（G8x）
  - 孤岛危机 第一代
- 2010：GeForce 405（GF119）
  - 支持 Direct3D
- 2014：GeForce 710（GK208）
- 2016：GeForce GTX 1060 6GB
  - 支持光追技术
- 2018：TITAN RTS（TU102）
  - 支持光追 + AI 技术

#### 性能

<img src="/images/E星计划-Lecture2 渲染流程简述.assets/image-20220402001758650.png">

- GPU 每半年性能可以翻一倍

### GPU 的功能

- **图形绘制**
  - 为大多数 PC 桌面，移动设备等提供图形处理和绘制功能
- **物理模拟**
  - GPU 硬件集成的物理引擎（PhysX、Havok），为游戏、电影、教育、科学模拟等领域提供高性能的物理模拟，使得以前需要长时间计算的物理模拟得以实时呈现
- **海量运算**
  - 计算着色器及流输出，实现并行计算的海量需求，例如 CUDA
- **AI 运算**
  - AI 的崛起推动了 GPU 集成 AI Core 运算单元，反哺 AI 运算能力，给各行各业带来了计算能力的提升
- **其它计算**
  - 音频编解码，加解密，科学计算，离线渲染等都离不开现代 GPU 的并行计算能力和海量吞吐能力

### GPU 的架构

#### GPU vs CPU

<img src="/images/E星计划-Lecture2 渲染流程简述.assets/image-20220402002302959.png" alt="image-20220402002302959" style="zoom:67%;" />

- GPU 将更多的晶体管用于数值计算，取代了缓存 Cache 和控制单元 Control
- GPU 的运算单元 Core 数量远多于 CPU
- 图像指令在同一时刻可以由多个单元并行处理，使得 GPU 可以同时执行上千个线程
- CPU 更擅长逻辑的计算，GPU 更擅长并行的程序

#### NVIDIA Fermi 架构

<img src="/images/E星计划-Lecture2 渲染流程简述.assets/image-20220402002733677.png" alt="image-20220402002733677" style="zoom:50%;" />	

- 拥有 16 个 SM（Stream Multiprocessor）

- 每个 SM

  - 2 个 Warp Scheduler
  - 2 个 Dispatch Unit（分发单元）
  - 2 个 Warp（线程束）
  - 16 组 LD/ST（加载存储单元）
  - 4 个 SFU（特殊函数单元）
  - 128KB Register File（寄存器）
  - 64KB Shared Memory/ L1 Cache
  - Uniform Cache（全局缓存）

  - 32 个 Core（计算单元）

- 每个 Core：

  - 1 个 FPU（浮点数单元）
  - 1 个 ALU（算数逻辑单元）

#### 存储架构

- 存储，在 CPU 被称为内存，在 GPU 被称为显存
- 分离式
  - 通过 PCI-E 总线通讯，一般用在 PC 上
    <img src="/images/E星计划-Lecture2 渲染流程简述.assets/image-20220402003443964.png" alt="image-20220402003443964" style="zoom:50%;" />
- 耦合式
  - 由 MMU 进行存储管理，比如 APU、游戏主机、移动设备
    <img src="/images/E星计划-Lecture2 渲染流程简述.assets/image-20220402003459875.png" alt="image-20220402003459875" style="zoom:50%;" />

存储架构如下

- 寄存器（SM 专用）
- 共享内存（SM 专用）
- L1 缓存（SM 专用）
- L2 缓存（SM 共享）
- GPU 显存（SM 共享）
- 系统内存（SM 共享）

它们的存取速度从寄存器到显存依次变慢

<img src="/images/E星计划-Lecture2 渲染流程简述.assets/image-20220402003728296.png">

<img src="/images/E星计划-Lecture2 渲染流程简述.assets/image-20220402003827619.png">

## 3. 渲染管线

### 渲染管线流程

<img src="/images/E星计划-Lecture2 渲染流程简述.assets/image-20220402003916212.png">

#### 应用程序阶段

1. **逻辑数据更新**
   - 通过动画/物理仿真/碰撞检测/几何变换来更新需要渲染的数据
2. **渲染数据准备**
   - 视锥体裁剪/LOD选择（透明/半透明/不透明物体）/状态排序（着色器/纹理决定排序规则等）
   - 生存渲染物体列表
3. **发起绘制**
   - 渲染状态（顶点/纹理/参数/着色器）的设置
   - 绘制调用（draw call）

#### 几何处理阶段

1. **顶点着色**
   - 计算顶点位置信息：模型空间 -> 世界空间 -> 相机空间 -> 正交/透视投影
   - 计算其它信息：法线、贴图 UV、光照信息
2. **顶点处理（可选）**
   - 曲面细分/几何着色器/流送输出
3. **裁剪**
   - 舍弃屏幕外的几何图元，裁剪与屏幕相交的图元
     <img src="/images/E星计划-Lecture2 渲染流程简述.assets/image-20220403221116773.png" alt="image-20220403221116773" style="zoom:50%;" />
4. **屏幕映射**
   - 裁剪空间坐标 -> 屏幕空间（计算屏幕坐标 (x, y)）
     <img src="/images/E星计划-Lecture2 渲染流程简述.assets/image-20220403221157904.png" alt="image-20220403221157904" style="zoom: 50%;" />

#### 光栅化阶段

<img src="/images/E星计划-Lecture2 渲染流程简述.assets/image-20220403221412242.png" alt="image-20220403221412242" style="zoom: 33%;" />	

真实的屏幕上是由 RGB 三种颜色信号组成，光栅化也就是把计算出来的效果从一个连续的空间，映射到离散的空间（屏幕上）的过程

1. **三角形装配**
   - 准备“三角形遍历”阶段需要用到的数据
2. **三角形遍历**
   - 判断某个像素是否被三角形覆盖
   - 若被覆盖则生成一个片元（fragment）
   - 计算每个片元插值后的数据
3. **输出**
   - 片元

#### 像素处理阶段

1. **像素着色**

   - 计算当前片元的颜色
     <img src="/images/E星计划-Lecture2 渲染流程简述.assets/image-20220403221837544.png" alt="image-20220403221837544" style="zoom:50%;" />

2. **合并**

   - 深度测试
     <img src="/images/E星计划-Lecture2 渲染流程简述.assets/image-20220403221931220.png" alt="image-20220403221931220" style="zoom:50%;" />

   - 模板测试

     计算是否有被遮挡的部分
     <img src="/images/E星计划-Lecture2 渲染流程简述.assets/image-20220403222116274.png" alt="image-20220403222116274" style="zoom: 50%;" />

   - 混合
     <img src="/images/E星计划-Lecture2 渲染流程简述.assets/image-20220403222131096.png" alt="image-20220403222131096" style="zoom: 33%;" />

3. **输出**

   - 像素的最终颜色，深度，模板信息

### 着色器原理

<img src="/images/E星计划-Lecture2 渲染流程简述.assets/image-20220403222247581.png" alt="image-20220403222247581" style="zoom:50%;" />	

- GPU 在绘制图形的过程中，有部分是**可编程**的，这部分就被称为**着色器 Shader**

#### 图形 API

<img src="/images/E星计划-Lecture2 渲染流程简述.assets/image-20220403222419244.png" alt="image-20220403222419244" style="zoom: 33%;" />	

- OpenGL
- DirectX
- Vulkan
- Metal
- GAM/GNM/GNMX

#### 着色器语言

- GLSL/ELSL
- HLSL
- CG
- SPIR-V
- MSL

#### 交叉编译

<img src="/images/E星计划-Lecture2 渲染流程简述.assets/image-20220403222621182.png" alt="image-20220403222621182" style="zoom: 33%;" />	

- 由于有很多种 shader 语言，现在存在着写一份 shader 语言，自动转换成其它 shader 语言
- 这种转换过程就叫做交叉编译

### 绘制一个三角形

<img src="/images/E星计划-Lecture2 渲染流程简述.assets/image-20220403225903134.png" alt="image-20220403225903134" style="zoom:50%;" />	

- 准备数据（Vertex Data[]），这个阶段是在 CPU 中进行的

- 程序会通过 Graphics API 发出 Drawcall 指令，然后指令会被推送到驱动程序，驱动程序会检测指令的合法性，然后把指令放到 GPU 可以读取的 Pushbuffer 中
  <img src="/images/E星计划-Lecture2 渲染流程简述.assets/image-20220403224516179.png" alt="image-20220403224516179" style="zoom: 50%;" />

- 经过一段时间或者主动调用 flush 指令（清缓冲区）后，驱动程序把 Pushbuffer 中的内容发送给 GPU，GPU 通过主机接口（Host Interface）接受这些命令，并通过前端（Front End）处理这些命令
  <img src="/images/E星计划-Lecture2 渲染流程简述.assets/image-20220403224657105.png" alt="image-20220403224657105" style="zoom:50%;" />

- 然后图元分配器（Primitive Distributor）开始通过 Index Buffer 中的顶点生成三角形，然后将三角形分批次（batch）发给各个 SM（Stream Multiprocessor）

  <img src="/images/E星计划-Lecture2 渲染流程简述.assets/image-20220403224906190.png" alt="image-20220403224906190" style="zoom:50%;" />	

- 每个 SM 中的 PolyMorph Engine 负责通过三角形索引（triangle indices）取出三角形数据（vertex data），即途中的 Vertex Fetch 模块

- 获取数据后，在 SM 中以 32 个线程为一组的线程束（Warp）来调度，开始处理顶点数据

- 线程束执行 vertex shader 和 geometry shader 指令（可选）
  <img src="/images/E星计划-Lecture2 渲染流程简述.assets/image-20220403225132923.png" alt="image-20220403225132923" style="zoom: 67%;" />

- 一旦 Warp 完成了所有 vertex shader 和 geometry shader 的指令，运算结果会被视图变换（View Transform）模块处理，三角形会被裁剪然后准备光栅化
  <img src="/images/E星计划-Lecture2 渲染流程简述.assets/image-20220403225337524.png" alt="image-20220403225337524" style="zoom: 33%;" />、

- 通过分析三角形所占用的屏幕面积、区域，由 work distribution crossbar 来分发到一个或者多个 raster engine 中开始进行光栅化处理
  <img src="/images/E星计划-Lecture2 渲染流程简述.assets/image-20220403225503486.png" alt="image-20220403225503486" style="zoom:50%;" />

- 计算插值，并保证是 pixel shader 需要的格式，生成片元
  <img src="/images/E星计划-Lecture2 渲染流程简述.assets/image-20220403225603471.png" alt="image-20220403225603471" style="zoom: 33%;" />

- 使用 2x2 个像素块进行像素着色，2x2 个片元进行处理
  <img src="/images/E星计划-Lecture2 渲染流程简述.assets/image-20220403225717059.png" alt="image-20220403225717059" style="zoom: 33%;" />

- 最后，像素着色器已经完成了渲染目标的颜色和深度信息的计算，将数据传递给 ROP（渲染输出单元），在这里进行深度测试、帧缓存、混合等处理
  <img src="/images/E星计划-Lecture2 渲染流程简述.assets/image-20220403225832096.png" alt="image-20220403225832096" style="zoom:50%;" />

## 4. 拓展

### TBDR —— Tail Based Deferred Rendering

#### IMR Immediate Mode Rendering

传统的渲染流程，一个三角形一个三角形渲染

<img src="/images/E星计划-Lecture2 渲染流程简述.assets/image-20220403230154759.png" alt="image-20220403230154759" style="zoom: 33%;" />	

#### TBDR

<img src="/images/E星计划-Lecture2 渲染流程简述.assets/image-20220403230544558.png" alt="image-20220403230544558" style="zoom: 33%;" />	

- 将图片分成一个接着一个小块，使得足够小
- 每次都处理一个小块，这样能够大大降低读写速率

### GPU Driven

- CPU 的并行能力比较弱
- 如果应用程序很复杂，CPU 在准备数据上也会花费很多的时间
- 是否能够把 CPU 的部分工作给 GPU 处理呢？

#### Compute Shader - UE5 Nantie

<img src="/images/E星计划-Lecture2 渲染流程简述.assets/image-20220403231018627.png" alt="image-20220403231018627" style="zoom:50%;" />	

- 将裁剪，映射等工作给 GPU
- 可以同时显示上亿个面数

#### NC Mesh Shader

<img src="/images/E星计划-Lecture2 渲染流程简述.assets/image-20220403231139481.png" alt="image-20220403231139481" style="zoom: 50%;" />	

- 从硬件层面上的优化
- 将传统的渲染管线阶段合并，节省开销
- 延迟实际的顶点处理阶段

### 光线追踪

<img src="/images/E星计划-Lecture2 渲染流程简述.assets/image-20220403231401272.png" alt="image-20220403231401272" style="zoom:50%;" />	

- NVIDIA 发布支持光追的芯片 NV Turing, RT Core
- 光追第一个大范围应用的游戏是战地 5
- UE4 引入了结合了光栅化的光线追踪的混合渲染管线
- 光追一般用于解决一些光线比较复杂的环境

