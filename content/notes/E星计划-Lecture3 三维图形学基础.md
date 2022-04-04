---
title: "E星计划-Lecture3 三维图形学基础"
date: 2022-04-04
draft: false
type: "post"
tags: ["E星计划"]
---

## 1. 变换

### 变换简介

#### 什么是变换

- 将一个坐标的点 (x, y) 映射到另一个坐标系中的点 (x', y')

#### 为什么是顶点变换

- 行业规范
  <img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404131918611.png" alt="image-20220404131918611" style="zoom:50%;" />
  - 三角形网格模型作为美术制作的基础
  - 三角形作为 GPU 绘制基础图元
  - 顶点变换可以简化表示三角形的网格变换，属性插值表示三角形

#### 顶点变换在游戏中的应用

- 成为游戏行业的基准
  - 场景摆放
  - 模型动作
  - 渲染管线
  - 美术建模

#### 二维变换

<img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404132717155.png" alt="image-20220404132717155" style="zoom:50%;" />	

- 可以任意组合
- 可以变换回去（可逆）

#### 三维变换

三维变换可以用于

- 几何建模
- 在场景中摆放物体
  <img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404132848677.png" alt="image-20220404132848677" style="zoom:50%;" />
- 骨骼动画
  <img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404132920572.png" alt="image-20220404132920572" style="zoom:50%;" />
- 基于投影渲染管线

#### 其它的模型

其实，除了顶点变换，还有其它的模型

- 贝塞尔曲面
- 体素
- 点云
- 隐式表面
- SDF

### 变换的分类

#### 刚体变换 Rigid Body Transform

<img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404133614744.png" alt="image-20220404133614744" style="zoom:50%;" />	

<img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404133240997.png" style="zoom:50%;" >	

- 保距变换：线段经过变换后，距离不变
- 保角变换：夹角经过变换后，角度不变

#### 相似变换 Similarity Transform

<img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404133548841.png" alt="image-20220404133548841" style="zoom:50%;" />	

<img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404133757153.png" alt="image-20220404133757153" style="zoom: 50%;" />	

- 保形变换
- 保角变换

#### 线性变换 Linear Transform

<img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404133658184.png" alt="image-20220404133658184" style="zoom: 50%;" />	

<img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404133720618.png" alt="image-20220404133720618" style="zoom: 50%;" />	

- L(p + q) = L(p) + L(q)
- L(ap) = a L(p)

#### 仿射变换 Affine Transform

<img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404133922300.png" alt="image-20220404133922300" style="zoom:50%;" />	

- 保平行
- 保线段的比例关系

#### 投影变换 Projection Transform

<img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404134005704.png" alt="image-20220404134005704" style="zoom:50%;" />	

- 保共线
- 可以反应不同平面直接的变换

#### 自由变换 Free Transform

<img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404134149996.png" alt="image-20220404134149996" style="zoom:50%;" />	

- 不保共线
- 更加复杂
- eg：鱼眼相机

### 变换的数学表示

#### 仿射变换

|          | 二维                                                         | 三维                                                         |
| -------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **公式** | <img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404135735314.png" alt="image-20220404135735314"  /> | <img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404141305015.png" alt="image-20220404141305015"  /> |
| **矩阵** | <img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404135750235.png" alt="image-20220404135750235"  /> | <img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404141701239.png" alt="image-20220404141701239"  /> |

#### 齐次变换

使用齐次变换矩阵，拓展一个位图，可以更加简洁的表示

|              | 二维                                                         | 三维                                                         |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **齐次变换** | <img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404135758386.png" alt="image-20220404135758386"  /> | <img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404141902162.png" alt="image-20220404141902162" style="zoom:100%;" /> |

#### 齐次坐标与欧式坐标的转换

![image-20220404135805645](/images/E星计划-Lecture3 三维图形学基础/image-20220404135805645.png)	

- x,y,w：齐次坐标下的三维向量
- x',y'：欧式坐标下的二维向量

#### 齐次坐标与平行线

- 欧式坐标：平行线没有解（c ≠ d）
  ![image-20220404140654843](/images/E星计划-Lecture3 三维图形学基础/image-20220404140654843.png)
- 齐次坐标：平行线（c ≠ d）存在解 w = 0
  ![image-20220404140806645](/images/E星计划-Lecture3 三维图形学基础/image-20220404140806645.png)
- (x, y, 0) 在齐次坐标下面表示无限远（只有方向的向量）

#### 齐次坐标变换

- 仿射变换（不涉及到投影变换） w 不会发送变换
  <img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404142106036.png" alt="image-20220404142106036" style="zoom: 33%;" />

- 平移变换可以表示在 4 x 4 的矩阵里面

  <img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404142137400.png" alt="image-20220404142137400" style="zoom:33%;" />	

- 等比缩放
  <img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404142235031.png" alt="image-20220404142235031" style="zoom:33%;" />

- 非等比缩放
  <img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404142305231.png" alt="image-20220404142305231" style="zoom:33%;" />

- 旋转

  - 绕 z 轴旋转
    <img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404142402823.png" alt="image-20220404142402823" style="zoom:33%;" />
  - 绕 x 轴旋转
    <img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404142518262.png" alt="image-20220404142518262" style="zoom:33%;" />
  - 绕 y 周旋转
    <img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404142447160.png" alt="image-20220404142447160" style="zoom:33%;" />

#### 齐次坐标的存储

- 压缩存储空间，齐次坐标按欧式坐标存储
- 在处理向量、点的时候需要特别小心
  - 向量 w = 0
  - 点 w = 1

#### 3D 旋转表示方法

- **欧拉角**
  <img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404143216398.png" alt="image-20220404143216398" style="zoom: 67%;" />
  <img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404143339242.png" alt="image-20220404143339242" style="zoom: 50%;" />
  - Roll - Pitch - Yaw = (x, y, z)
  - 三个数，按照顺序进行主轴旋转，不能连续绕同一个主轴旋转，一共有 12 种顺序
  - 和矩阵一样，顺序不能第哦啊换，游戏引擎中旋转轴都是世界空间的主轴
  - 常用于给美术编辑建模
- **四元数**
  <img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404143407187.png" alt="image-20220404143407187" style="zoom:50%;" />
  - 动力学计算
  - 球面线性插值
- **矩阵**
  - 顶点变换表示

#### 连续变换

- 变换**不满足交换律**
  - 放缩后移动：p' = T(S(p))
    <img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404143703917.png" alt="image-20220404143703917" style="zoom:50%;" />
    <img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404143739896.png" alt="image-20220404143739896" style="zoom:50%;" />
  - 移动后放缩：p' = S(T(p))
    <img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404143717608.png" alt="image-20220404143717608" style="zoom:50%;" />
    <img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404143803594.png" alt="image-20220404143803594" style="zoom:50%;" />

### 游戏中的变换

#### 场景表示

<img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404144032893.png" alt="image-20220404144032893" style="zoom:50%;" />

#### 场景描述

<img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404144121773.png" alt="image-20220404144121773" style="zoom:50%;" />	

#### 节点层级表示

- 场景表示
  <img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404144344168.png" alt="image-20220404144344168" style="zoom: 33%;" />
- 骨骼表示
  <img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404144507652.png" alt="image-20220404144507652" style="zoom:50%;" />

## 2. 顶点变换流水线

### 介绍

#### 基于投影变换的 3D 顶点变换流水线

<img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404150429421.png" alt="image-20220404150429421" style="zoom:50%;" />

### 相机模型

#### 真实的相机绘制

- 焦点、景深
  <img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404150850930.png" alt="image-20220404150850930" style="zoom:33%;" />

- 移动模糊
  <img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404150901198.png" alt="image-20220404150901198" style="zoom:50%;" />

### 投影变换

#### 模型空间

<img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404151132750.png" alt="image-20220404151132750" style="zoom:50%;" />	

- 由美术定义，没有标准答案
- 一般是由物体的中心、边界为原点

#### 世界空间

<img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404151150379.png" alt="image-20220404151150379" style="zoom:50%;" />	

- 场景中所有的物体放在一个公共的参考系
- 选定没有通用标准，使用即可

#### 世界空间 -> 相机空间/视空间（视图变换 view transformation）

相机矩阵
<img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404151450662.png" alt="image-20220404151450662" style="zoom:50%;" />

- `mtxLookAt(view, eye, d, up)`

  - eye：投影中心
  - d：观察位置
  - up：上方向

- z 轴
  <img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404151755020.png" alt="image-20220404151755020" style="zoom:50%;" />

- x 轴

  <img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404151817250.png" alt="image-20220404151817250" style="zoom: 25%;" />	

- y 轴

  <img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404151843290.png" alt="image-20220404151843290" style="zoom: 25%;" />	

#### 相机空间/视空间

<img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404151325182.png" alt="image-20220404151325182" style="zoom:50%;" />	

- 场景的观察者
- 投影整个世界场景到相机所在的空间
  - 相机坐标系
  - XY 平面与图像平面平行
  - Z 轴垂直于图像平面

#### 相机空间/视空间 -> 投影/裁剪空间（投影变换 projection transformation）

给定视空间上的一个点，求在相机平面上的位置坐标

<img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404152209128.png" alt="image-20220404152209128" style="zoom:50%;" />	

- 垂直投影 orthonormal projection
  <img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404152327660.png" alt="image-20220404152327660" style="zoom: 33%;" />

  - 忽略 z 值
  - 使用相机坐标作为图形屏幕坐标
  - 按照平行线投影到 xy 平面

- 透视投影 perspective projection
  <img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404152539545.png" alt="image-20220404152539545" style="zoom:50%;" />

  - 游戏中最常用的投影方式（近大远小）

  - 人眼模型的简化，小孔相机模型

  - 透视投影矩阵

    <img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404152709816.png" alt="image-20220404152709816" style="zoom:67%;" />

    <img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404152737425.png" alt="image-20220404152737425" style="zoom:50%;" />	

#### 投影/裁剪空间

<img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404153106407.png" alt="image-20220404153106407" style="zoom:50%;" />	

#### 投影/裁剪空间 -> 归一化空间

- 除以齐次坐标系中的 w，完成归一化
- 剔除视锥外的三角形

#### 归一化空间

<img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404153429718.png" alt="image-20220404153429718" style="zoom:50%;" />	

<img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404153257482.png" alt="image-20220404153257482" style="zoom: 33%;" />	

- 将视锥体之外的东西删除
- 一般可以和投影/裁剪空间合为一个，做一个隐式的归一化

#### 归一化空间 -> 设备空间

- 需要映射到图像（像素）空间
- 使用平移或缩放
- `bgfx::setViewRect(vireid,x0,y0,x1,y1)`
- <img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404153603015.png" alt="image-20220404153603015"  />



#### 设备空间

- 具体的设备屏幕上的显示

#### 总结

<img src="/images/E星计划-Lecture3 三维图形学基础/image-20220404154251522.png" alt="image-20220404154251522" style="zoom:50%;" />·

- 从模型空间到设备空间的转换与 3D 场景独立
- 可编程管线表达
- 现在，PVW 可以被合并成一个矩阵
