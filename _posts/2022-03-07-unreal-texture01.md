---
layout: post
title: UE4：通过实例更改材质颜色
categories:
    - GameDevelopment
tags:
    - UnrealEngine
---

![](https://s2.loli.net/2022/03/09/VyGQWkB84YmaRbM.png)

材质球中，基础颜色设置一个向量参数，利用乘法连接（如图），将向量默认值设置为(1,1,1,1)。

<br>

创建材质实例并打开，在“细节”中，设置向量参数的值。

![](https://s2.loli.net/2022/03/09/BYDzlrQxCagS8E6.png)

------

## 分别控制RBG三个通道的输出

![](https://s2.loli.net/2022/03/09/QogU2pViF7CISev.png)

这种方式可以混合出更多颜色。

![](https://s2.loli.net/2022/03/09/wIJWQB7udLp8fvt.png)