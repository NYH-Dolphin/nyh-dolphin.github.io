---
title: "Unity 行走与鼠标视角移动"
date: 2022-04-01
draft: false
type: "post"
tags: ["Unity"]
---

## 1. 行走部分

### WASD 控制行走

```c#
public float fMoveSpeed = 10f; // WASD移动速度

void Update(){
    KeyBoardControl();
}

private void KeyBoardControl()
{
    if (Input.GetKey(KeyCode.W))
    {
        transform.Translate(Vector3.forward * fMoveSpeed * Time.deltaTime);
    }

    if (Input.GetKey(KeyCode.S))
    {
        transform.Translate(Vector3.back * fMoveSpeed * Time.deltaTime);
    }

    if (Input.GetKey(KeyCode.A))
    {
        transform.Translate(Vector3.up * fMoveSpeed * Time.deltaTime);
    }

    if (Input.GetKey(KeyCode.D))
    {
        transform.Translate(-1 * Vector3.up * fMoveSpeed * Time.deltaTime);
    }
}
```

### 鼠标点击移动到指定位置

待补充

## 2. 鼠标移动视角部分

### 左右移动转动物体，上下移动转动摄像机

注意的问题是，上下移动不能转动物体，不然视角会转飞

```c#
public float fRotateSpeed = 200; // 视角移动速度
private float fLastPosX; // 上一次鼠标的位置 x
private float fLastPosY; // 上一次鼠标的位置 y
private float fRange = 0.01f; // 判定移动范围

void Update(){
    MouseControl();
}

private void MouseControl()
{
    float RotaSpeed = fRotateSpeed * Time.deltaTime;
    // 左右转动物体
    if (Input.mousePosition.x > fLastPosX + fRange)
    {
        transform.Rotate(Vector3.right, RotaSpeed);
        fLastPosX = Input.mousePosition.x;
    }
    else if (Input.mousePosition.x < fLastPosX - fRange)
    {
        transform.Rotate(Vector3.right, -RotaSpeed);
        fLastPosX = Input.mousePosition.x;
    }
            
    // 上下转动相机
    if (Input.mousePosition.y > fLastPosY + fRange)
    {
    // Vector3.right 根据不同的游戏视角会有调整
        transform.Find("Main Camera").Rotate(Vector3.right, -RotaSpeed);
        fLastPosY = Input.mousePosition.y;
    }
    else if (Input.mousePosition.y < fLastPosY - fRange)
    {
        transform.Find("Main Camera").Rotate(Vector3.left, -RotaSpeed);          
        fLastPosY = Input.mousePosition.y;
    }
}
```

