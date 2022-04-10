---
title: "Unity iTween 动画插件"
date: 2022-03-29
draft: false
type: "post"
tags: ["Unity"]
---

> iTween 插件 API 全解析：https://www.bilibili.com/video/BV1T4411t7RS

## 1. iTween 的下载和安装

- 在 Unity 的 AssetStore 中可以下载，可以直接免费入手
- 官网链接：[iTween for Unity by Bob Berkebile (pixelplacement)](http://www.pixelplacement.com/itween/index.php)

## 2. iTween 的使用方式

### 静态方法

```c#
void Start()
{
    iTween.MoveAdd(gameObject, new Vector3(0,1,0), 3f);
}
```

- 直接调用 iTween 的静态方法，传入需要的参数即可

### Hashtable 方法

- 可以参考文档：[iTween for Unity by Bob Berkebile (pixelplacement)](http://www.pixelplacement.com/itween/documentation.php)

```c#
void Start()
{
    Hashtable hash = new Hashtable();
    hash.Add("amount", new Vector3(0,1,0));
    hash.Add("time", 2f);
    iTween.MoveAdd(gameObject, hash);
}
```

### 使用 iTween 提供的 Hash

```c#
void Start()
{
    iTween.MoveAdd(gameObject, iTween.Hash("amount", new Vector3(0,1,0), "time", 2f));
}
```

## 3. iTween 中的枚举

### iTween.EaseType

- 指的是动画过程中的运动曲线的效果
- 参考网站：[Easing Functions Cheat Sheet (easings.net)](https://easings.net/)

### iTween.LoopType

- `none`：不循环
- `loop`：完全重复轨迹
- `pingPong`：往返重复

### iTween.NamedValueColor

在 Shader 中改变颜色的动画

- `_Color`：材质的主颜色
- `_SpecColor`：材质的镜面颜色（用于镜面/光滑/顶点着色）
- `_Emission`：一种材料的发射颜色（用于顶点着色器）
- `_ReflectColor`：材质的反射颜色（用于反射着色器）

## 4. iTween Hashtable 传入的通用参数

- 可以参考文档中的参数 [iTween for Unity by Bob Berkebile (pixelplacement)](http://www.pixelplacement.com/itween/documentation.php)

| 属性名           | 类型            | 说明                                                       |
| ---------------- | --------------- | ---------------------------------------------------------- |
| name             | string          | 标志性名字，用于停止动画时的指定名字                       |
| time             | float/double    | 动画的时长，单位为秒                                       |
| speed            | float/double    | 可代替 time 属性，使动画基于速度参数                       |
| delay            | float/double    | 动画播放延迟的时间，单位为秒                               |
| easetype         | EaseType/string | 动画播放时间变换曲线样式                                   |
| looptype         | LoopType/string | 动画的循环方式                                             |
| onstart          | string          | 动画开始时的回调函数名                                     |
| onstarttarget    | GameObject      | 指定 ‘onstart’ 的回调方法所在的脚本挂载在哪个游戏物体上    |
| onstartparams    | Object          | 传递给 ‘onstart’ 方法的参数                                |
| onupdate         | string          | 动画执行过程中每步执行时的回调函数名                       |
| onupdatetarget   | GameObject      | 指定 ‘onupdate’ 的回调方法所在的脚本挂载在哪个游戏物体上   |
| onupdateparams   | Object          | 传递给 ‘onupdate’ 方法的参数                               |
| oncomplete       | string          | 动画结束时的回调函数名                                     |
| oncompletetarget | GameObject      | 指定 ‘oncomplete’ 的回调方法所在的脚本挂载在哪个游戏物体上 |
| oncompleteparams | Object          | 传递给 ‘oncomplete’ 方法的参数                             |
| ignoretimescale  | boolean         | 忽略时间缩放，设置为 true，则动画独立于时间缩放参数进行    |

## 5. Audio 相关动画

| 属性名      | 类型         | 说明                         |
| ----------- | ------------ | ---------------------------- |
| audiosource | AudioSource  | 动画指定使用 AudioSource     |
| volume      | float/double | 指定的 volume 音量改变到的值 |
| pitch       | float/double | 指定的 pitch 音高改变到的值  |

### AudioFrom

渐变的修改 AudioSource 的音量和音高，在指定时间内从指定值逐渐恢复到初始值

如果 AudioSource 未指定，则默认使用 GameObject 上关联的 AudioSource

### AudioTo

渐变的修改 AudioSource 的音量和音高到达指定目标值

如果 AudioSource 未指定，则默认使用 GameObject 上关联的 AudioSource

### AudioUpdate

类似于 AudioTo，但是是在 Update 方法或者循环反复中调用

使用可以减小开销，一般使用一组变化的值，所以不应用 EaseType

### Stab

根据所提供的音量和音高播放一次音频，并应用任何延迟

AudioSource 是可选的，因为 iTween 将提供一个

| 属性名    | 类型      | 说明                              |
| --------- | --------- | --------------------------------- |
| audioclip | AudioClip | 动画指定播放的 AudioClip 音频片段 |

## 6. Material 相关动画

## 7. Look 相关动画

| 属性名     | 类型              | 说明                         |
| ---------- | ----------------- | ---------------------------- |
| looktarget | Transform/Vector3 | 动画的游戏物体的朝向目标     |
| axis       | string            | 约束动画只再指定的该轴上旋转 |

### LookFrom

旋转游戏物体使其朝向指定的位置，然后再指定的时间内逐渐恢复到初始旋转

### LookTo

旋转游戏物体使之在一定时间内逐渐朝向指定目标

### LookUpdate

类似于 LookTo，但是是在 Update 方法或者循环反复中调用

## 8. Path 相关动画