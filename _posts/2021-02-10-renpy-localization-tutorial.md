---
layout: post
title: Ren'Py游戏汉化教程
categories:
    - 教程
tags:
    - Ren'Py
    - 汉化
---

Ren'Py是一个galgame玩家熟悉的引擎，基于Python打造。使用这个引擎可以很方便地制作出基于文本和对话的游戏，同时，也可以自行定制更加复杂的游戏界面。

由于Ren'Py的特性，基于该引擎的游戏的汉化不需要多少计算机基础，~~只需要会打字就行~~。基本的汉化方式有两种：

+ 直接替换字符串。非常不推荐，因为后续游戏更新的话，需要重新替换，而且改动了代码，容易出现各种错误。
+ 利用Ren'Py引擎的多语言支持。推荐使用此种方式。Renpy引擎有着良好的多语言支持特性，不会改动源代码，翻译文件全部外置。

这里以<a href='https://drakes64.itch.io/limits'>Limits</a>为例，介绍一下基于Ren'Py引擎的游戏汉化过程。

-------

## Renpy游戏的结构

将Renpy游戏解压缩后，可以看到其基本结构如下：

![ywqFK0.png](https://i.postimg.cc/8k6kxsPg/image.png)

+ `/game`：游戏核心脚本和资源文件
+ `/lib`：Ren'Py运行所需的库
+ `/renpy`：引擎核心
+ `***.exe`：游戏启动程序
+ 其他文件、日志文件

打开game文件夹，可以看到里面存放的是一些游戏资源（音乐、字体、图片等）和脚本。我们的翻译文件也会存在这里。其中比较重要的是拓展名为rpa的文件，里面打包了大部分的游戏资源和脚本。一般的游戏只有一个`archive.rpa`，但Limits这个有点特殊，它的脚本单独打包了一个`scripts.rpa`。

![ywqJaD.png](https://i.postimg.cc/52NV9t10/image.png)

## 前置工作

翻译Renpy游戏需要的一些工具有：

+ Python 3
+ <a href='https://github.com/Lattyware/unrpa'>unrpa</a>
+ <a href='https://www.renpy.org/latest.html'>Ren'Py sdk</a>
+ 任意一种代码编辑器（我这里使用的是Visual Studio Code）

## 解包

要进行翻译，第一步是解包游戏，即把拓展名为rpa的文件解包。这里需要用到unrpa。

### 安装unrpa

安装好Python后，在cmd或Powershell中输入：

```powershell
py -3 -m pip install "unrpa"
```
或者

```powershell
py -m pip install "unrpa"
```

如果安装速度慢的，可以更换国内源（清华、豆瓣等都可以）：

```powershell
py -m pip install "unrpa" -i https://pypi.tuna.tsinghua.edu.cn/simple
```
安装完成后，输入unrpa，如果安装成功则会出现以下提示：

```powershell
> unrpa
usage: unrpa [-h] [-v] [-s] [-l | -t] [-p PATH] [-m] [--version]
             [--continue-on-error] [-f VERSION] [-o OFFSET] [-k KEY]
             FILENAME [FILENAME ...]
unrpa: error: the following arguments are required: FILENAME
```

### 解包rpa

在命令行输入以下代码：

```powershell
unrpa -mp "输出目录（推荐游戏目录下的game文件夹）" "rpa文件的绝对位置"
```

以Limits为例，输入：

```powershell
unrpa -mp "G:\TEMP\Limits-1.06-pc\game" "G:\TEMP\Limits-1.06-pc\game\archive.rpa"
```
点击回车，等待提取完成。同样地，`scripts.rpa`也用同样的方式解包。

或者更简单地，在游戏`/game`文件夹内，按住`shift`键，同时右键鼠标，选择在此处打开PowerShell，然后输入：

```powershell
unrpa "archive.rpa"
```

即可将rpa文件解包至当前文件夹。

解包完成后，会发现game文件夹里多了很多rpy格式的文件，这些就是游戏的源代码，所有的字符串都在里面。图片资源一般存在`/image`文件夹中。解包完成后，原rpa文件可以删除。

## 加入Ren'Py的多语言支持

Ren'Py需要每一个作品都使用一种主语言编写。这种主语言无论具体是哪国的，都被称作 None 语言。(如果使用英语编写，英语就是 None 语言。)

当选择使用None语言后，Ren'Py的大多数多语言支持功能都会关闭。

Ren'Py的多语言开启后，游戏运行时，在显示字符串前会先在翻译文件中搜索，如果匹配到了对应的翻译，则显示翻译；若没匹配到，则显示原文。

这里推荐在游戏中加入语言选择功能，方便用户自行切换语言。

### 在设置菜单中加入语言选项

打开game文件夹中的`screens.rpy`，找到`Preferences screen`，这里就是游戏中的一些选项所在的位置。

```python
screen preferences():
    tag menu
    use game_menu(_("Preferences"), scroll="viewport"):

        vbox:
            hbox:
                box_wrap True

                if renpy.variant("pc"):
                    vbox:
                        style_prefix "radio"
                        label _("Display")
                        textbutton _("Window") action Preference("display", "window")
                        textbutton _("Fullscreen") action Preference("display", "fullscreen")

            ...
```

在合适的地方加入以下代码：

```python
vbox:
    style_prefix "radio"
    label _("Language")
    textbutton _("English") action Language(None)
    textbutton _("Chinese") action Language("chinese")
```

比如在Limits中，加在Preferences中的最前面：

```python
screen preferences():
    tag menu
    use game_menu(_("Preferences"), scroll="viewport"):

        vbox:
            hbox:
                box_wrap True

                vbox:
                    style_prefix "radio"
                    label _("Language")
                    textbutton _("English") action Language(None)
                    textbutton _("Chinese") action Language("chinese")

                if renpy.variant("pc"):
                    vbox:
                        style_prefix "radio"
                        label _("Display")
                        textbutton _("Window") action Preference("display", "window")
                        textbutton _("Fullscreen") action Preference("display", "fullscreen")

            ...
```

保存后，进入游戏，就可以发现设置中多了语言选项，只不过此时Chinese点不了，因为还没有生成翻译文件。

![ywvWDI.png](https://i.postimg.cc/yNg2WgGV/image.png)

### 生成翻译文件

打开Ren'Py sdk，更改工程目录为游戏文件夹所在的文件夹，之后便可在工程目录中找到游戏。

![ywxTdx.png](https://i.postimg.cc/d3455wfH/image.png)

![ywxqJO.png](https://i.postimg.cc/DwtBjCCT/image.png)

点选目标游戏后，点击右下角的生成翻译文件。语言输入chinese，然后点击生成。注意，最好不要选择生成空字符串，不然未翻译的字符串全会显示成空白。

![ywzPFf.png](https://i.postimg.cc/VsD4Qs74/image.png)

等待完成后，所有的翻译文件就存放在了`game/tl/chinese`文件夹中。其中，rpy格式的可用任何一种编辑器打开，rpyc是对应的二进制文件，不用管。

此时再进游戏，Chinese选项就可以点击了，但是由于还没有翻译，显示出来的还是和原来一样的英文（如果你之前选择了生成空字符串，切换语言后就会发现全是空白！！）

![ywz3pF.png](https://i.postimg.cc/CKGHvwBG/image.png)

## 进行翻译

### Ren'Py翻译文件的结构

打开任意一个翻译文件，可以发现其基本结构如下：

```python
# game/script.rpy:828
translate chinese start_ebe9ea82:

    # voice1 "I fucked up... This is a mess!"
    voice1 "I fucked up... This is a mess!"

...

translate chinese strings:

    # game/script.rpy:891
    old "Coffee."
    new "Coffee."
```

有两种格式：

+ 原文用井号注释，翻译字符串紧跟其后。这是一般的对话。这里需要改动下面的那个字符串。
+ 原文是`old "..."`，翻译是`new "..."`。只需改动new里面的。（**注意：如果改了old，那这一句就无法翻译了！！！**）

比如：
```python
# game/script.rpy:828
translate chinese start_ebe9ea82:

    # voice1 "I fucked up... This is a mess!"
    voice1 "我搞砸了……真是一团糟！"

...

translate chinese strings:

    # game/script.rpy:891
    old "Coffee."
    new "咖啡。"
```

打开游戏看看，嗯？怎么这一句变成了方块呢？

![y0SOrd.png](https://i.postimg.cc/xdnb1M8m/image.png)

原因很简单，就是游戏默认的字体不支持中文字符，所以显示不出来。为此，我们还需要更改游戏字体。

### 更改字体以正确显示中文

打开game目录下的`gui.rpy`，找到`define gui.text_font`这几句，更改为自己喜欢的字体,比如这里使用OPPO Sans，将字体文件放入game文件夹，然后更改代码：

``` python
define gui.text_font = 'OPPOSans-M-2.ttf' #对话文本用的字体
define gui.name_text_font = 'OPPOSans-M-2.ttf' #人名用的字体
define gui.interface_text_font = 'OPPOSans-M-2.ttf' #菜单等用的字体
```

然后再打开游戏，就发现中文可以正常显示了。

![y09QtP.png](https://i.postimg.cc/jqw4WVt4/image.png)

### 进行翻译

接下来就是漫长的翻译过程啦！把`game/tl/chinese`目录下的全部rpy文件翻译完。Bon courage!

-----

## 非标准字符串和变量的翻译

由于Ren'Py游戏可以自行定制界面，作者可能会使用一些非标准的字符串显示，导致Ren'Py不能自动生成翻译。这时就需要我们对源代码进行一些简单的修改，然后再更新翻译文件。

### 变量的翻译

某些游戏可能在对话中引入变量，而变量使用的是Python的赋值，Ren'Py不会翻译，比如：

```python
$ gender_noun = "女孩"  #用美元符号的是Python语句，定义了一个变量

nar "You're a good [gender_noun]."  #在Ren'Py对话中引用时，用方括号括起来
```

生成翻译文件后，会发现`gender_noun`这个变量并不会自动翻译，找不到这一项。

为了让Ren'Py知道这个字符串需要翻译，我们要对源代码进行小小的修改：

```python
$ gender_noun = _("女孩")  #用美元符号的是Python语句，定义了一个变量
```

这里做的一处修改，其意义是：

+ `_("字符串")`：告诉游戏引擎这一句是需要翻译的，这样再次生成翻译文件就会生成这一句的翻译。单下划线和括号只是改变了传入到显示界面的字符串，并不会改变游戏内部运行时传递的内容，因此不会影响游戏运行。同时，有了这个字符串的翻译后，再删除`_()`标记也无妨。

然后，在生成的翻译文件中，为了让该变量正常翻译，我们需要在翻译中稍作修改：

```python
translate ch xxxxxx_xxxxx:

    # nar "You're a good [gender_noun]."
    nar "你是个好[gender_noun!t]。"
```

+ `[变量名!t]`：告诉游戏引擎这个变量需要翻译。这样，游戏运行时就会将变量也翻译。

### 界面字符串的翻译

有的作者可能给游戏增加了很多界面，这些界面里的字符串默认都不会被Ren'Py多语言识别。为了让这些非标准字符串支持翻译，我们也需要对其源代码进行修改。

如某游戏有背包界面，界面上的某些字符串是无法正常翻译的：

![y0iHuF.png](https://i.postimg.cc/CxmqPJwK/image.png)

这时，我们需要在源代码中找到这一段，让其支持多语言。同样是在字符串两边加上`_()`：

```python
    vbox:
        spacing 10
        text _("{b}Consumables:{/b}")  #标记该字符串，生成翻译时即可自动生成该句
        $ i = 0
        for kazdy_element in ekwipunek.items:
            if kazdy_element.category == "consumable":
                ...
                    text _("([amountsstring]) [kazdy_element.name]")  #这里有变量引用，同样地，我们需要在生成的翻译中加入!t，让其支持多语言。
            $ i+=1
```

生成翻译后：

```python
old "{b}Consumables:{/b}"
new "{b}消耗品：{/b}"

old "([amountsstring]) [kazdy_element.name]"
new "([amountsstring]) [kazdy_element.name!t]" #根据需要，加入!t标记
```

### renpy.notify()的翻译

renpy.notify()是一个在Python中调用renpy通知的方法。让其支持多语言很简单，也是加入_()，标记该字符串，生成翻译：

```python
renpy.notify(_("字符串"))
```

## 游戏资源的多语言支持

以上说的都是文本，那么图片要怎么办呢？

其实很简单，把汉化后的图片放到`game/tl/chinese`对应的文件夹中即可。Ren'Py在运行时会优先在这里找对应的资源文件。

比如，有一个原版文件`game/images/a.png`，用PhotoShop改好后，将其放入`game/tl/chinese/images/`文件夹，这样选择中文语言时，对应的图片也会更换。音乐资源同理。

## 加入字体切换功能

如果有人不喜欢给定的字体，那我们可以给游戏加入一个更高级的功能：字体切换。

和刚才改字体一样，在`gui.rpy`中找到定义字体的部分，将其改为以下：

``` python
define gui.text_font = gui.preference("font_1", "OPPOSans-M-2.ttf")
define gui.name_text_font = gui.preference("font_2", "OPPOSans-M-2.ttf")
define gui.interface_text_font = gui.preference("font_3", "OPPOSans-M-2.ttf")
```

然后，在`screens.rpy`的preferences中加入切换字体选项：

```python
vbox:
    style_prefix "pref"
    label _("Font")
    textbutton _("OPPO Sans") action [gui.SetPreference("font_1", "OPPOSans-M-2.ttf"), gui.SetPreference("font_2", "OPPOSans-M-2.ttf"), gui.SetPreference("font_3", "OPPOSans-M-2.ttf")]
    textbutton _("方正准圆") action [ gui.SetPreference("font_1", "fzzy.ttf"), gui.SetPreference("font_2", "fzzy.ttf"), gui.SetPreference("font_3", "fzzy.ttf")]
```

其他字体的字体文件同样放入game文件夹。这样就可以自行切换字体了。

## 更新翻译

当游戏更新后，我们的翻译也需要更新。这里就显示出Ren'Py多语言支持的好处了。只需要把翻译文件放入新版本的对应文件夹，按上面的方法让游戏支持多语言，再到Ren'Py sdk中生成翻译文件即可。

新生成的并不会覆盖原文件，而是在文件末尾加入新的、没有翻译的字符串，如：

```python
# TODO: Translation updated at 2021-02-02 11:26

translate ch strings:

    # game/job.rpy:46
    old "{b}Information:{/b}"
    new "{b}Information:{/b}"
...
```

先前的版本进行的修改，新版也要记得一并修改。

## 补丁包生成

如何生成汉化补丁？

WIP

-----

> 参考资料
>
> 多语言支持 — Ren'Py 中文文档. (2021). Retrieved 10 February 2021, from <a href='https://mabbs.github.io/RenPy_Docs_CHS/RenPy/translation.html'> here </a>