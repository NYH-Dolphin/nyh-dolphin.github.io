---
title: "Unity TMP 设置中文字体"
date: 2022-03-29
draft: false
type: "post"
tags: ["Unity-Utils"]
---

为了解决 TMP 不支持中文字体的情况，采取以下方式

## 1. 找到一个中文常见汉字的文档

这里给出一个链接：[中文7000常用文字](https://github.com/DavidSheh/CommonChineseCharacter/blob/master/7000常用字优化版.txt)

## 2. 找到一个Font素材

在网上可以找到一些丰富的字体素材

或者本地可以下载一些字体文件，一般在 `C:/Windows/Fonts` 存放了一些常见字体

将想要使用的素材拖入 Unity 即可

<img src="/images/Unity TMP 设置中文字体/image-20220329102611408.png" alt="image-20220329102611408" style="zoom: 67%;" />	

## 3. 使用 TMP 创建字体 asset

打开 `Window/TextMeshPro/Font Asset Creator`

<img src="/images/Unity TMP 设置中文字体/image-20220329102751177.png" alt="image-20220329102751177" style="zoom:67%;" />	

将字体文件拖入 `Source Font File`，将下载的中文常见汉字文档拖入 `Character File`

注意设置 `Atlas Resolution`，**复杂的汉字需要最高的分辨率，也就是 8192 x 8192**，如果选择了小的分辨率，最后可能无法显示，其它的设置可以参考截图下面的示例，也可以自行修改

配置完成后，点击 `Generate Font Atlas`，由于文字个数很多，**可能需要相当长的一段时间才能完成生成**

<img src="/images/Unity TMP 设置中文字体/image-20220329102932496.png" alt="image-20220329102932496" style="zoom:67%;" />	

完成好后，旁边有一个 `Save` 按钮，将导出的 Font Asset 存放在指定位置即可

## 4. 使用 Font Asset

使用时，在创建了 TMP GameObject 的地方，把生成的 Font Asset 拖入到指定位置即可使用

<img src="/images/Unity TMP 设置中文字体/image-20220329103427405.png" alt="image-20220329103427405" style="zoom:67%;" />

