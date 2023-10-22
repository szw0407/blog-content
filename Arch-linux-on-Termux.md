# 在 Termux 上安装 Proot-Distro Arch Linux 的一些记录

这个文档主要参考了[Termux：從0到1安裝Arch Linux桌面系統＋一鍵啟動指令稿 | Ivon的部落格](https://ivonblog.com/posts/termux-proot-distro-archlinux/)

## 安装 Termux

Termux 是一个 Android 上的终端模拟器，可以在 Android 上运行 Linux 环境。

同时，为了一些事情干的方便一些，安装 Termux:X11 以及 Termux:Widget 。

## 安装 Proot-Distro

Proot-Distro 是一个在 Termux 上安装 Linux 发行版的工具。

```bash
pkg install proot-distro
```

## 安装 Arch Linux

```bash
proot-distro install archlinux
```

## 配置 Arch Linux

```bash
proot-distro login archlinux
```

### 修改 pacman 源

```bash
vim /etc/pacman.conf
```

```bash
[archlinuxcn]
SigLevel = Optional TrustedOnly
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
```
