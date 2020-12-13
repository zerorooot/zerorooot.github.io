---
title: idea设置新字体
tags:
  - idea 
  - fonts 
  - linux
date: 2020-12-13 22:18:06
description: 最近给电脑加了个新固态并装上了manjaro，打算把和编程有关的东西全转移到manjaro上，但在win10上很好看的字体微软雅黑在manjaro上并没有，上网搜了搜，于是有了这篇博文
---

步骤来自[官方](https://www.jetbrains.com/lp/mono/)的mono字体安装教程，不过我们可以用它来安装我们想安的字体

# 下载字体并解压

# 安装字体

## mac

选择所有字体，双击“安装字体”按钮

## win

选择所有字体，左键单击任意一个，然后点击“安装”

## linux

复制字体到

```
~/.local/share/fonts
```

然后运行命令

```
fc-cache -f -v
```

# 重启idea

# 设置字体

进idea里点击“ Preferences/Settings → Editor → Font”，取消勾选“show only monospaced fonts”，选择你安装的字体即可

