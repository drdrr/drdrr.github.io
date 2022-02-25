---
layout: post
title: Python可视化：子图
categories:
    - 笔记
tags:
    - Python
    - matplotlib
---

## 利用subplot创建子图

```python
import matplotlib.pyplot as plt

# 创建figure实例
fig = plt.figure() 

# 使用add_subplot()方法创建子图。
# 三个参数的意义分别是：将图分为x行、将图分为y列的第z个子图。
ax_0_0 = fig.add_subplot(2,2,1)
ax_0_1 = fig.add_subplot(2,2,2)
ax_1 = fig.add_subplot(2,1,2)

# 确保各子图间距合适
fig.tight_layout()
```

运行结果：

![](https://i.postimg.cc/d1mxsPck/2022-02-17-16-28-28-image.png)

左上、右上和下方图的z位置分别是：1,2,2。

------

## 利用subplots快速创建规则子图

```python
import matplotlib.pyplot as plt

# 创建3行、2列的子图。所有子图将以array的形式传递给axs。
fig, axs = plt.subplots(3,2)

# 通过axs[x,y]即可访问子图。
fig.tight_layout()
```

运行结果：

![](https://i.postimg.cc/vTYSDqGP/2022-02-17-16-21-54-image.png)