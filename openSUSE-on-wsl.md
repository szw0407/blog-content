# 折腾WSL2 openSUSE的一些记录

这是本人第一次，从头开始折腾一个Linux系统。本人希望通过这次折腾，学习Linux的基本使用。

由于WSL上的一些操作和实体的Linux安装不完全一样，本人也仅仅是尽力尝试吧。

至于为什么使用openSUSE……一方面它确实体积比Ubuntu小很多，另一方面它有官方的支持（虽然实际上作用有限），而且可玩性较高。

## 安装系统和初始化

安装过程非常简单。开启WSL之后，用微软应用商店安装就好了。我选择的是“风滚草”版，追时髦嘛。

第一次启动的时候，会配置用户名和密码。操作也很简单，不多赘述了。

## 简单使用

安装完了，就可以开始折腾了。

根据安装指导，先运行：

```bash
sudo zypper ref -b && sudo zypper dup
```

更新到最新滚动发行版。

大致浏览系统情况，在~文件夹下有一个/bin文件夹，里面是空的。

启动`yast2`的时候报了个错误：

>Qt GUI wanted but not found, falling back to ncurses.

这个问题我们稍后解决~~（不过应该会很久以后了）~~

先测试一些常见的命令：

vim安装了

Ruby安装了

Python3安装的是Python3.11，如果要绑定`python`可以运行

```bash
sudo ln -sf /usr/bin/python3 /usr/bin/python
```

openSUSE的包管理使用的是rpm，以及专用的zypper

还有一个系统管理的工具YaST，甚至提供了GUI（但是好像不能开箱即用，需要折腾）。

## 配置GUI

在WSL2有不完整的GUI支持WSLg，而且GUI根本不是开箱即用的。

先初始化

```bash
sudo zypper in -t pattern wsl_gui
```

以安装YaST2的GUI举例。理论上安装了YsST2的GUI后，可以输入`yast2`打开GUI，同时输入`yast`打开TUI程序。

YaST理论上内容不止两行，以前我瞎搞过一次，装上了很完整的YaST，但是已经忘记那会安装了什么了。

> 码住，后面再折腾。
> 上次配置这个地方的时候失败了，而且关键是我还不知道问题出在哪里，就搞的系统无法启动了。
> 现在暂时打算只使用命令行，除非后面再有什么需求

## 启用systemd

```bash
sudo zypper install -t pattern wsl_systemd
```

然后

```bash
sudo vi /etc/wsl.conf
```

将`[boot]`下添上一行`systemd=True`，然后退出`wsl`终端，输入`wsl --shutdown`重启，然后再次开机就正常使用了。

