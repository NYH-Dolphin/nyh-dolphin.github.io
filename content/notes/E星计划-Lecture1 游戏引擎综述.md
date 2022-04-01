---
title: "E星计划-Lecture1 游戏引擎综述"
date: 2022-04-01
draft: false
type: "post"
tags: ["E星计划"]
---

## 1. E 星计划介绍

### 排期计划

![image-20220401164138702](/images/E星计划-Lecture1/image-20220401164138702.png)

- Lecture1 开营仪式暨游戏引擎综述
- Lecture2 渲染流程概述
- Lecture3 三维图形学 & 基础数学
- Lecture4 纹理综述
- Lecture5 光照综述
- Lecture6 性能优化综述
- Lecture7 全局光照技术综述

### 完课要求

<img src="/images/E星计划-Lecture1/image-20220401164749644.png" alt="image-20220401164749644" style="zoom:67%;" />	

<img src="/images/E星计划-Lecture1/image-20220401164917951.png" alt="image-20220401164917951" style="zoom: 33%;" />	

## 2. 游戏引擎及简史

### 什么是游戏引擎

<img src="/images/E星计划-Lecture1/image-20220401183651292.png" alt="image-20220401183651292" style="zoom: 25%;" />	

- 一种快速开发游戏的工具，将游戏开发中繁琐的组件进行系统化、标准化的处理
- 可以延伸到影视作品、工业仿真等
- 游戏引擎 = 引擎核心组件 + 工具
- 游戏 = 引擎核心组件 + 内容（资源、玩法）

### 主流的游戏引擎

- UE
- Unity
- 自研引擎

### 游戏的历史

- 1993 最早出现游戏引擎的时间，那时候游戏和引擎是一起开发的
  - 第一个商业授权的引擎
- 1998 UE 1.0
  - 到目前已经到 5.0
- 2005 Unity 1.0

## 3. 游戏引擎架构

### 架构概述

<img src="/images/E星计划-Lecture1/image-20220401184022495.png" alt="image-20220401184022495" style="zoom:25%;" />	

- 引擎核心组件
  - Core：支撑模块：内存管理、文件管理、对象系统、反射系统、多线程、主循环等
  - 动画、渲染、物理、声音、脚本、AI、网络
- 工具

### 渲染

#### 什么是渲染

https://www.youtube.com/watch?v=NEzJH-JrAdw

使用  CPU 和 GPU，将 3D 的环境呈现在电子设备屏幕上

- 图像平面：包含很多个像素
- 3D 模型：多边形的美术素材，由很多三角形组成
- 相机：透视变换，将 3D 模型呈现在图像平面上

##### 什么是光栅化

https://www.youtube.com/watch?v=t7Ztio8cwqM

- 通过 3D 模型，计算屏幕上各个像素的颜色
- 3D 模型的三角形顶点通过透视变换投影到屏幕上，然后通过插值补充内部像素点的颜色

##### 什么是光照和着色

https://www.youtube.com/watch?v=bsSveswnQik

- 通过光照，计算阴影

##### 单帧示例

https://www.youtube.com/watch?v=ygndZ5eIFO4

<img src="/images/E星计划-Lecture1/image-20220401185342458.png" alt="image-20220401185342458" style="zoom:30%;" />	

- 数据从 CPU 发送到 GPU -> 逐个物体完成计算，渲染每个物体并提交
- CPU 决定渲染哪些物体
- GPU 负责渲染计算

#### 渲染特性

- 不同的游戏引擎实现的，用来表达图形在计算机中的数据结构和算法

##### 网格（植被/头发/水等）

https://www.youtube.com/watch?v=wavnKZNSYqU

https://www.youtube.com/watch?v=ool2E8SQPGU

https://www.youtube.com/watch?v=Gk_mTHvBx1w

http://advances.realtimerendering.com/s2019/hair_presentation_final.pdf

<img src="/images/E星计划-Lecture1/image-20220401185546171.png" alt="image-20220401185546171" style="zoom: 25%;" />	

- 三角形的集合，表示不同的物件
  - 房子 500 个面
  - 角色 10000 个面

##### 材质（PBR/卡通/皮肤/眼睛）

https://www.youtube.com/watch?v=j-A0mwsJRmk&t=663s

https://www.gdcvault.com/play/1018270/Next-Generation-Character

https://www.youtube.com/watch?v=-JFyAdI_rO8

<img src="/images/E星计划-Lecture1/image-20220401185649065.png" alt="image-20220401185649065" style="zoom: 50%;" />	

- 光照计算里面的一个概念，表示物体表面如何与光起作用

##### 环境效果（雨、雪、云、大气）

<img src="/images/E星计划-Lecture1/image-20220401185925121.png" alt="image-20220401185925121" style="zoom: 33%;" />	

##### 光追和混合渲染

https://www.youtube.com/watch?v=CsBHQGN0Q-0

https://www.youtube.com/watch?v=Qx_AmlZxzVk

https://www.youtube.com/watch?v=Bk5HOtRwgd8

https://www.youtube.com/watch?v=VtHCpO88l_w

- 从屏幕像素出发投射到世界空间中，计算光线的效果
- 也会根据材质表面进行二次光线追踪
- 现在主题走光栅化，辅助光线追踪

##### 虚拟几何体（UE5 nantie）

https://advances.realtimerendering.com/s2015/aaltonenhaar_siggraph2015_combined_final_footer_220dpi.pdf

https://github.com/nvpro-samples/gl_vk_meshlet_cadscene

https://www.notion.so/Brief-Analysis-of-Nanite-94be60f292434ba3ae62fa4bcf7d9379

https://www.youtube.com/watch?v=YCUgI-c9jJ0

https://www.youtube.com/watch?v=qCs1CU8_TTU

- 现在的游戏需要渲染的物体特别多，对传统的管线开销很大，CPU 的计算开销随着物体的增加会增大，然而 GPU 性能很好，不太经常存在满载的情况
- 想把一部分工作从 CPU 搬到 GPU
- 在 15 年刺客信条提出 Mesh Cluster Rendering 的概念，只渲染物体的某些小块
- 由 UE 在 Mesh Cluster Rendering 拓展，根据距离进行分块渲染，最大化表现模型细节

### 动画

#### 动画类型

##### 骨骼动画

<img src="/images/E星计划-Lecture1/image-20220401190934466.png" alt="image-20220401190934466" style="zoom:80%;" />	

- 驱动角色中间的骨架，带动网格，形成角色动画
- 游戏动画师编写动画关键帧，插值补充中间帧

##### 变形动画

https://www.youtube.com/watch?v=Hskhx4Kxmrk&list=PL2e4mYbwSTbbHAJT7OdK5mv-idhlLOTl0&index=6

![image-20220401191012155](/images/E星计划-Lecture1/image-20220401191012155.png)	

##### 顶点动画

<img src="/images/E星计划-Lecture1/image-20220401191142932.png" alt="image-20220401191142932" style="zoom:50%;" />	

- 几何体的序列帧
- 一般用于很难用上面两种技术完成的动画
  - 如翻书、云彩等

##### 粒子系统

https://www.youtube.com/watch?v=2WbJxLur5Uk

<img src="/images/E星计划-Lecture1/image-20220401191216229.png" alt="image-20220401191216229" style="zoom:50%;" />	

- 通过定义粒子喷射器，模拟粒子的运动，渲染图形或者序列帧

#### 程序化角色动画

https://slideplayer.com/slide/12479068/

https://slideplayer.com/slide/15399099/

##### 融合

<img src="/images/E星计划-Lecture1/image-20220401191420850.png" alt="image-20220401191420850" style="zoom:67%;" />	

- 不同的融合姿态，通过混合姿态，达到一个近似抓握不同地点的动态

##### 反向动力学 IK

<img src="/images/E星计划-Lecture1/image-20220401191449181.png" alt="image-20220401191449181" style="zoom:50%;" />	

- 固定末端骨骼的位置，反向计算其它骨骼的位置

##### 布娃娃 Ragdoll

<img src="/images/E星计划-Lecture1/image-20220401191509684.png" alt="image-20220401191509684" style="zoom:50%;" />	

- 模拟死亡状态

### 物理

https://www.sciencedirect.com/science/article/abs/pii/S0029801819307917

https://www.reddit.com/r/MachineLearning/comments/phvgzb/r_how_machine_learning_will_revolutionise_physics/

https://www.youtube.com/watch?v=zTDi7I7oIfY

- 模拟真实世界的各种物理行为

#### 刚体碰撞检测与响应

<img src="/images/E星计划-Lecture1/image-20220401191915815.png" alt="image-20220401191915815" style="zoom:67%;" />	

主要过程

- 检测（检测两个物体是否相交/碰撞）
  - 先进行粗判 -> 包围盒是否碰撞
  - 再进行精细的判断
- 相交碰撞：根据冲量/力的算法进行计算，并基于一些速度

#### 柔体模拟

##### 弹簧质点模型

![image-20220401191955105](/images/E星计划-Lecture1/image-20220401191955105.png)	

- 将物体的质量离散到质点，质点再用弹簧连接
- 经常用于模拟布料、头发等

##### 基于位置的动力学

![image-20220401192035002](/images/E星计划-Lecture1/image-20220401192035002.png)	

- 统一支持刚体柔体
- 都是粒子 + 约束
- 不同的约束表现不同的物理效果
- 不是真实的物理模拟，仅是一种视觉模拟

##### 有限元算法

<img src="/images/E星计划-Lecture1/image-20220401192114314.png" alt="image-20220401192114314" style="zoom:50%;" />	

- 在游戏中不太常用

##### 材料质点算法

<img src="/images/E星计划-Lecture1/image-20220401192144346.png" alt="image-20220401192144346" style="zoom:50%;" />	

- 高精度无网格方法
- 但是目前没有办法做到实时

#### 物理引擎

<img src="/images/E星计划-Lecture1/image-20220401192321190.png" alt="image-20220401192321190" style="zoom: 25%;" />	

### AI

https://unity.com/cn/products/machine-learning-agents

https://www.gdcvault.com/play/1026172/Unity-AI-and-Machine-Learning

https://www.youtube.com/watch?v=ZdN8dDa0ff4

https://www.youtube.com/watch?v=o-QLSjSSyVk

https://www.gdcvault.com/play/1014514/AI-Navigation-It-s-Not

<img src="/images/E星计划-Lecture1/image-20220401192444322.png" alt="image-20220401192444322" style="zoom:50%;" />	

- 寻路算法
  - A*
  - 群体训练
  - 机器学习
    <img src="/images/E星计划-Lecture1/image-20220401192623457.png" alt="image-20220401192623457" style="zoom:33%;" />
- 战斗
- 动画

### 声音

https://www.youtube.com/watch?v=L4GuM15QOFE

https://www.youtube.com/watch?v=BrUQdd96qzk

http://kunzhou.net/zjugaps/bst/bst.pdf

https://www.audiokinetic.com/library/edge/?source=SDK&id=raytracing_geometry_guide.html

#### 音乐

- 声音混响的效果，反应出空间的大小等

#### 音效

- 战斗、走路等效果

<img src="/images/E星计划-Lecture1/image-20220401192723608.png" alt="image-20220401192723608" style="zoom: 67%;" />	

- 空间音效，基于 HTRF 声音定位的算法

### 网络

https://www.youtube.com/watch?v=zrIY0eIyqmI

https://www.youtube.com/watch?v=7jb0FOcImdg&list=PLSWK4JALZGZNVcTcoXcTjWn8DrUP7TOeR&index=1

- 解决如何快速可靠通信
- 传输层：TCP/UDP/RUDP/KCP
- 应用层：Json/ProtoBuf/MsgPack/其它自定义
- 不同的游戏引擎有自己的解决方案
  - UE ：dedicated servers
- 常见同步方案
  - 帧同步
  - 预表现 + 回滚

### 脚本

https://www.youtube.com/watch?v=mxs_x5ne9JY

https://www.youtube.com/watch?v=5jP0z7Atww4

- 不同的引擎可能对不同的编译器进行扩展
- 主流的脚本
  - UE：蓝图，C++
  - Unity：C#

### 工具与编译器

<img src="/images/E星计划-Lecture1/image-20220401193547964.png" alt="image-20220401193547964" style="zoom: 33%;" />	

- 场地编译器
- 模型编译器
- 动画/物理编译器

## 4. 网易自研引擎

<img src="/images/E星计划-Lecture1/image-20220401193729824.png" alt="image-20220401193729824" style="zoom: 50%;" />	

### NeoX

![image-20220401193858231](/images/E星计划-Lecture1/image-20220401193858231.png)

### Messiah

![image-20220401194125418](/images/E星计划-Lecture1/image-20220401194125418.png)

![image-20220401194107129](/images/E星计划-Lecture1/image-20220401194107129.png)

- 原生多线程
- 适配全平台
- 高效率运行
- 快速迭代开发
- 完整的工具链
