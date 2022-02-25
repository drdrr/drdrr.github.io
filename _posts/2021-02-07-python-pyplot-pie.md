---
layout: post
title: Python可视化：饼图
categories:
    - 笔记
tags:
    - Python
    - matplotlib
---

## 语法

```python
matplotlib.pyplot.pie(x, explode=None, labels=None, colors=None, autopct=None, pctdistance=0.6, shadow=False, labeldistance=1.1, startangle=None, radius=None, counterclock=True, wedgeprops=None, textprops=None, center=(0, 0), frame=False, rotatelabels=False, hold=None, data=None)
```

|参数|数据类型|说明|
|:------|:------|:------|
|x|array-like|各区块的尺寸|
|explode|array-like|各区块的偏移量|
|labels|array-like|各区块对应的标签|
|colors|array-like|各区块的颜色|
|autopct|string or function|控制饼图内百分比设置|
|pctdistance|float|指定autopct的位置|
|shadow|bool|饼图阴影|
|labeldistance|float|指定标签位置|
|startangle|float|起始绘制角度，默认图是从x轴正方向逆时针画起|
|radius|float|饼图半径|
|counterclock|bool|饼图绘制的方向（默认逆时针）|
|wedgeprops|dict|区块的属性|
|textprops|dict|字体属性|
|center|list of float|图表中心位置|
|frame|bool|绘制带有表的轴框架|
|rotatelabels|bool|是否根据每一区块旋转标签|


## 代码举例

```python
import matplotlib.pyplot as plt

plt.figure(figsize=(480/300, 480/300), dpi=300) #设置输出的图片尺寸和DPI

x = [37, 8, 35, 5, 15]
labels = ['女方因素', '男方因素', '男女双方因素', '原因不明', '研究期间妊娠']
explode = [0.1, 0, 0, 0, 0]
colors = ['#60acfc', '#32d3eb', '#5bc49f', '#feb64d', '#ff7c7c']
textprops = {
    'fontsize':5,
    'fontfamily':'Microsoft YaHei'  #更改字体以正确显示中文
}
autopct = '%1.f%%' #格式化字符串

plt.pie(x, labels=labels, explode=explode, colors=colors, textprops=textprops, autopct=autopct)
plt.title("不孕不育的原因分布", fontproperties="Microsoft YaHei")
plt.show()
```

## 绘制效果

![yN6x0O.png](https://s3.ax1x.com/2021/02/07/yN6x0O.png)