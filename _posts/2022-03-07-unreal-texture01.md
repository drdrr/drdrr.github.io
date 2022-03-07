---
layout: post
title: UE4：通过实例更改材质颜色
categories:
    - UnrealEngine
tags:
    - GameDevelopment
---

![](https://i.postimg.cc/XqF5WfVv/2022-03-06-20-45-47-image.png)

材质球中，基础颜色设置一个向量参数，利用乘法连接（如图），将向量默认值设置为(1,1,1,1)。

<br>

创建材质实例并打开，在“细节”中，设置向量参数的值。

![](https://i.postimg.cc/02RmfBfK/2022-03-06-20-50-09-image.png)

------

## 分别控制RBG三个通道的输出

![](https://i.postimg.cc/d1hdHMpr/2022-03-06-21-07-52-image.png)

这种方式可以混合出更多颜色。

![](https://i.postimg.cc/fTcdgf22/2022-03-06-21-11-23-image.png)