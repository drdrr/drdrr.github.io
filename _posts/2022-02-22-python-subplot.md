---
layout: post
title: Python可视化：子图
categories:
    - ComputerScience
tags:
    - Python
    - matplotlib
    - 笔记
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

![](https://s2.loli.net/2022/03/09/vlNg9wX8siH2hoL.png)

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

![](https://s2.loli.net/2022/03/09/1P2XTyL6FRsJv5S.png)