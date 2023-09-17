# 折腾WSL openSUSE的一些记录

这是本人第一次，从头开始折腾一个Linux系统。本人希望通过这次折腾，学习Linux的基本使用。

由于WSL上的一些操作和实体的Linux安装不完全一样，本人也仅仅是尽力尝试吧

至于为什么使用openSUSE。。。。这个问题我们后面再说

## 安装系统和初始化

安装过程非常简单。开启WSL之后，用微软应用商店安装就好了。我选择的是“风滚草”版，追时髦嘛。

第一次启动的时候，会配置用户名和密码。操作也很简单，不多赘述了。

## 简单使用

安装完了，就可以开始折腾了。

在~文件夹下有一个/bin文件夹，里面是空的。

启动yast2的时候报了个错误：Qt GUI wanted but not found, falling back to ncurses.

这个问题我们稍后解决，即安装QT的runtime

先测试一些常见的命令：

vim安装了

Ruby安装了

Python3安装的是Python3.11

openSUSE的包管理使用的是rpm，以及专用的zypper

还有一个系统管理的工具YAST，甚至提供了GUI（但是好像不能开箱即用）

## 配置GUI

在WSL2有不完整的GUI支持，而且GUI根本不是开箱即用的

