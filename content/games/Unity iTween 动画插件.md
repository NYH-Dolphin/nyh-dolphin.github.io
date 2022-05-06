---
title: "Unity iTween 动画插件"
date: 2022-05-06
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

## 6. Color 相关动画

| 属性名          | 类型                          | 说明                                        |
| --------------- | ----------------------------- | ------------------------------------------- |
| color           | Color                         | 动画指定给物体的颜色                        |
| r               | float/double                  | 指定颜色的颜色 red 值，范围 0-1             |
| g               | float/double                  | 指定颜色的颜色 green 值，范围 0-1           |
| b               | float/double                  | 指定颜色的颜色 blue 值，范围 0-1            |
| a               | float/double                  | 指定颜色的颜色 alpha 值，范围 0-1           |
| namedColorValue | iTween.NamedColorValue string | shader 中使用 color 属性名，默认为 `_Color` |
| includechildren | boolean                       | 是否包含游戏的子物体，默认为 true           |

- Renderer 材质球上的 shader 如果有 `_Color` 属性也可以执行动画，Shader 属性名只限 `_Color`、`_SpeColor`、`_Emission`、`_ReflectColor`
- 可以应用于 Shader 或者 Light

### ColorFrom

改变一个 GameObject 的 color 颜色值，然后渐变地经过给定时间恢复到初始的颜色

### ColorTo

改变一个 GameObject 的 color 颜色值，使其经过一段时间到达指定的值

### ColorUpdate

效果类似 ColorTo，放在 Update 方法中使用一组动态改变的值进行设置，不应用 EaseType

## 7. Fade 相关动画

| 属性名 | 类型         | 说明                     |
| ------ | ------------ | ------------------------ |
| alpha  | float/double | 指定的透明度值，范围 0-1 |
| amount | float/double | 指定的透明度值，范围 0-1 |

### FadeFrom

改变 GameObject 的 alpha 透明度到指定值，然后逐渐恢复到初始值，使用效果类似 ColorFrom 中属性 `a` 的设置

### FadeTo

改变 GameObject 的透明度到某个指定的目标值

### FadeUpdate

效果类似 FadeTo，放在 Update 方法中使用减小开销，参数用一组动态改变的值，不应用 `EaseType`

## 8. Look 相关动画

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

## 9. Move 相关动画

### MoveAdd

在指定时间内逐渐移动游戏物体的位置，使用 Translate 实现，即再当前物体位置的基础上加上移动的值

| 属性名       | 类型              | 说明                                                         |
| ------------ | ----------------- | ------------------------------------------------------------ |
| amount       | Vector3           | 物体动画移动到目标位置                                       |
| x            | float/double      | 单独设置 x 轴的移动                                          |
| y            | float/double      | 单独设置 y 轴的移动                                          |
| z            | float/double      | 单独设置 z 轴的移动                                          |
| orienttopath | boolean           | 游戏物体是否会朝向它的运动方向，默认为 flalse                |
| looktaget    | Transform/Vector3 | 指定动画过程中游戏物体朝向的目标                             |
| looktime     | float/double      | 使用 "orienttopath" 或者 "looktaget" 属性时，物体朝向动画的时长，单位为秒 |
| axis         | string            | 约束动画只在指定的该轴上旋转                                 |
| space        | Space             | 指定物体移动时的坐标空间（世界坐标或者自身坐标）默认自身坐标 |

### MoveFrom

将游戏物体的位置改变到指定的位置，然后在指定时间内逐渐恢复到初始位置

| 属性名       | 类型                     | 说明                                                         |
| ------------ | ------------------------ | ------------------------------------------------------------ |
| position     | Transform/Vector3        | 动画指定的目标位置                                           |
| path         | Transform[] 或 Vector3[] | 为动画指定一组坐标位置绘制曲线路径                           |
| movetopath   | boolean                  | 是否自动生成一条曲线从 GameObject 的当前位置到路径起点，默认为 true |
| orienttopath | boolean                  | 游戏物体是否会朝向它的运动方向，默认为 flalse                |
| lookahead    | float/double             | 对 ”orienttopath“ 朝向目标看的百分比，范围 0-1               |
| islocal      | boolean                  | 是否是世界坐标或者本地坐标，默认为 false，即世界坐标         |

### MoveTo

将游戏物体的位置移动到指定的目标位置，然后在指定时间内逐渐恢复到初始位置，参数与 MoveFrom 类似

### MoveUpdate

效果类似 MoveTo，在 Update 方法中使用，或者在循环调用的方法中使用动态数据，不应用 EaseType

## 10. Rotate 相关动画

### RotateAdd

在指定的时间内渐变地将提供的欧拉角角度添加到物体的旋转，即在当前旋转角度的基础上再进行旋转

| 属性名 | 类型    | 说明                         |
| ------ | ------- | ---------------------------- |
| amount | Vector3 | 物体的旋转添加的指定欧拉角度 |

### RotateBy

将提供的值乘以 360，在给定时间内渐变地将计算的结果对物体进行旋转操作

| 属性名 | 类型    | 说明                                   |
| ------ | ------- | -------------------------------------- |
| amount | Vector3 | 将指定的值乘以 360° 对游戏物体进行旋转 |

（即会转多很多圈）

### RotateFrom

改变游戏物体的欧拉角度到指定值，然后在指定时间内渐变地逐渐恢复到初始角度

| 属性名   | 类型              | 说明                                                   |
| -------- | ----------------- | ------------------------------------------------------ |
| rotation | Transform/Vector3 | 游戏物体旋转到目标角度                                 |
| islocal  | boolean           | 旋转的坐标空间是世界还是本地，默认是 false，即世界空间 |

### RotateTo

在指定时间内将游戏物体旋转到指定的目标欧拉角度，使用参数参考 RotateFrom

### RotateUpdate

效果类似于 RotateTo，在 Update 方法中使用一组动态变化的值，不应用 EaseType

## 11. Scale 相关动画

### ScaleAdd

再指定时间内将游戏物体的 scale 缩放添加给定的值

| 属性名 | 类型    | 说明                                   |
| ------ | ------- | -------------------------------------- |
| amount | Vector3 | 为游戏物体当前缩放加上指定的值作为目标 |

### ScaleBy

在指定时间内将游戏物体的 scale 缩放乘以给定的值

| 属性名 | 类型    | 说明                                   |
| ------ | ------- | -------------------------------------- |
| amount | Vector3 | 为游戏物体当前缩放乘以指定的值作为目标 |

### ScaleFrom

改变游戏物体的缩放到指定的值，然后在指定时间内逐渐动画恢复到初始缩放值

| 属性名 | 类型              | 说明         |
| ------ | ----------------- | ------------ |
| scale  | Transform/Vector3 | 指定的缩放值 |

### ScaleTo

在指定时间内将游戏物体缩放逐渐动画到给定的值，使用参数参考 ScaleFrom

### ScaleUpdate

效果类似 ScaleTo，在 Update 方法中或循环调用的方法中减小开销，使用一组动态变化的数据，不应用 EaseType

## 12. Punch 相关动画

### PunchPosition

对游戏物体位置施加一个冲击力，并将其摇晃回初始位置

| 属性名 | 类型    | 说明         |
| ------ | ------- | ------------ |
| amount | Vector3 | 指定的目标点 |

### PunchRotation

对游戏物体旋转施加一个冲击力，并将其摇晃回初始的旋转，参考 PunchPosition

### PunchScale

对游戏物体施加一个冲击力，使其摇晃回初始位置的缩放，参考 PunchPosition

## 13. Shake 相关动画

### ShakePosition

在指定时间内根据指定的幅度随机摇晃物体位置并逐渐衰减

| 属性名 | 类型    | 说明           |
| ------ | ------- | -------------- |
| amount | Vector3 | 震动的幅度大小 |

### ShakeRotation

在指定时间内根据指定的幅度随即摇晃游戏物体的旋转并逐渐衰减，参考 ShakePosition

### ShakeScale

在指定时间内根据指定的幅度随即摇晃游戏物体的缩放并逐渐衰减，参考 ShakePosition

## 14. 动画控制方法

### Init

设置一个 GameObject，以避免在添加初始动画时出现问题

当天你打算运行 iTween 的每个对象的 Start 或 Awake 时都使用这个方法

### Pause

用于暂停动画，如果动画标志了 ignoreTimeScale 则不起作用

### Resume

用于继续动画

### Stop

用于停止动画，且摧毁 GameObject 上面的 iTween 动画组件

由于 Unity 线程的一些操作，你可能需要在调用 Stop 后插入一个任意的暂停（可以很短），调用 `WaitForSeconds()` ，然后添加一个新的动画

如果你正在使用键盘/鼠标交互，建议**在键盘或鼠标按下的时候调用 Stop，然后在按键或鼠标抬起的时候创建新的动画**

```c#
iTween.Stop(obj);
WaitForSeconds(0.001f);
iTween.MoveTo(....);
```

### StopByName

通过动画名字停止动画

### Count

获取 iTween 动画的个数

## 15. Update 数值系列方法

### FloatUpdate

返回一个 float 值，在给定值的当前值和目标值之间，基于给定的速度渐变

```c#
void Update(){
    // f: 当前值
    // 5: 目标值
    // 1: 速度
    f = iTween.FloatUpdate(f, 5, 1);
    Debug.Log(f);
}
```

### RectUpdate

返回一个 Rect 值，该值通过提供的速度在当前值和目标值之间取值

### Vector2Update

返回一个 Vector2，该值通过提供的速度在当前值和目标值之间取值

### Vector3Update

返回一个 Vector3，该值通过提供的速度在当前值和目标值之间取值

## 16. Path 相关方法

### DrawLine

在 `OnDrawGizmos()` 方法中通过提供的 `Vector3[]` 或者 `Transform[]` 画线

```c#
Vector3[] path = new Vector3[]{...};

private void OnDrawGizmos(){
    iTween.DrawLine(path, Color.RED);
}
```

### DrawPath

在 `OnDrawGizmos()` 方法中通过提供的 `Vector3[]` 或者 `Transform[]` 画一条曲线，区别于 DrawLine

```c#
Vector3[] path = new Vector3[]{...};

private void OnDrawGizmos(){
    iTween.DrawPath(path, Color.RED);
}
```

### PathLength

返回一条通过提供的 `Vector3[]` 或 `Transform[]` 所化的**曲线**路径的长度，float 的长度值

```c#
Vector3[] path = new Vector3[]{...};
float distance = iTween.PathLength(path);
```

### PutOnPath

将游戏物体放到提供路径的指定百分比的位置点

### PointOnPath

根据提供的路径的指定百分比，返回一个 Vector3 的位置