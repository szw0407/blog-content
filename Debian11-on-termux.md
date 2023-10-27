# 在 Termux 上安装 Debian 11 并启用桌面环境

> #### 为什么要安装 Debian 11？
> 最新版本的 Arch Linux 和 Debian 12 Bookworm 的 GUI 安装依赖的一个库，比较新的版本有个 bug 直接导致桌面环境无法启动。Debian 11 Bullseye 是我见到的，唯一一个有先例能正常启动桌面环境的 Proot-distro。
> 本人的设备是 Samsung Galaxy Tab S7 FE，搭载了骁龙 778G 处理器，6GB RAM 和 128GB ROM，系统是 One UI 5.1.1 （基于 Android 13）；Termux 是 F-Droid 下载的最新版本，其他依赖都用的是理论上的最新版本。

> #### 为什么不用 VNC？
> VNC 有两个问题，一个是性能问题，另一个是安全问题。性能问题是因为 VNC 是远程桌面，所以会有延迟，而且在 Termux 上，VNC 会占用大量的 CPU 资源；安全问题是因为 VNC 会在本地开放一个端口，会有安全隐患。
> 相比之下。Termux-x11 可能更加轻量级，而且启动安装由于为了 Termux 优化过，体验应该更好一点，因此我们选择 Termux-x11。

## 预备工作

1. 从 F-Droid 安装 Termux；从 GitHub Action Build 下载最新编译好的 Termux-x11。
2. 打开 Termux，输入 `termux-setup-storage`，并授予存储权限。
3. 输入 `pkg update && pkg upgrade`，更新 Termux 的包。理论上 Termux 会选择一个最好的源，但是我们也可以手动指定，使用 `termux-change-repo`。
4. 输入 `pkg install proot-distro`，安装 proot 依赖。
5. 输入 `pkg install termux-x11-nightly`，安装 Termux-x11 的最新版本。
6. 输入 `pkg install x11-repo`，安装 X11 依赖。
7. 输入 `pkg install virglrenderer-android` 安装 Virglrenderer 依赖以支持 GPU 3D 加速；输入 `pkg install pulseaudio` 安装 Pulseaudio 依赖以支持音频。
8. 在 Android 12 以后的版本，要避免 Termux 被杀后台，需要在 ADB shell 里运行（具体可以使用 USB 调试、shizuku + Termux ADB 网络调试等办法）：

    ```
    settings put global settings_enable_monitor_phantom_procs false
    ```

    然后重启 Termux。

## 安装 Debian 11

由于 proot-distro 提供的已经是 Debian 12 了，我们需要手动指定版本号。

1. 登入 [termux/proot-distro: An utility for managing installations of the Linux distributions in Termux.](https://github.com/termux/proot-distro)
2. 阅读 README 中 Adding distribution 段。理论上应该在 Release 里面找对应于 Debian 11 的旧版本，但是由于他给了个样例而且版本肯定不晚于 Debian 11，所以我们直接用样例，到时候直接升级大版本就好了。

    ```bash
    DISTRO_NAME="Debian"
    TARBALL_URL['aarch64']="https://github.com/termux/proot-distro/releases/download/v1.10.1/debian-aarch64-pd-v1.10.1.tar.xz"
    TARBALL_SHA256['aarch64']="f34802fbb300b4d088a638c638683fd2bfc1c03f4b40fa4cb7d2113231401a21"
    ```

    把这个脚本保存在 `$PREFIX/etc/proot-distro` 并命名为 `debian-legacy.sh`，然后执行 `proot-distro install debian-legacy`，就可以安装这个旧版本 Debian 了。

3. 安装完成后，输入 `proot-distro login debian-legacy`，就可以进入 Debian 的终端了。我暂时还没测试出来这是什么版本的 Debian ，但是我猜测早于 Debian 10 Buster ，因为居然它**不支持使用 HTTPS 的源 URL**。
4. **重要**：更换源到 Debian bullseye 的源（本人参考的是使用[TUNA](https://mirror.tuna.tsinghua.edu.cn/help/debian/)），然后使用 `apt update && apt full-upgrade` 更新系统。注意此处不需要`sudo` 而且也没有安装 `sudo`，当前用户就是 root 用户；此外源选择 Debian 11 (bullseye)，其他都跟推荐的走就好了。注意在此之前千万**不要**安装任何包，否则依赖会直接出问题！如果不熟悉 Vim 的话，请先熟悉一下 Vim 的基本操作，因为唯一一个预装好的编辑器是 Vim！

    ```bash
    vim /etc/apt/sources.list
    # 修改 sources.list 为 清华源 的 bullseye 源，使用 HTTP ，然后用 :wq 保存并退出
    apt update && apt full-upgrade
    ```
5. 更新完了系统，安装好 HTTPS 支持所需的包之后，把源换成 HTTPS，然后安装 `sudo`。
    
    ```
    apt install apt-transport-https ca-certificates
    vim /etc/apt/sources.list
    # 修改 sources.list 为 清华源 的 bullseye 源，使用 HTTPS ，然后用 :wq 保存并退出 
    apt update && apt upgrade
    apt install sudo 
    ```
    
6. 添加普通用户组并创建这个用户，并允许普通用户使用 `sudo`。
    
    ```bash
    groupadd storage
    groupadd wheel
    useradd -m -g users -G wheel,audio,video,storage -s /bin/bash user  # 用户名和用户组名我们暂时用这个
    passwd # 设置 root 密码
    passwd user  # 设置密码
    visudo # 打开 sudoers 文件
    # 找到 root ALL=(ALL:ALL) ALL 这一行，然后在下面添加 user ALL=(ALL:ALL) ALL，然后保存退出
    su user # 切换到 user 用户
    cd # 切换到用户主目录
    sudo apt update && sudo apt install vim # 测试一下 sudo 是否正常工作，并且把 Vim 的依赖补充完整
    ```

## 安装桌面环境

建议选择 Xfce4，因为它比较轻量级，而且在 Termux 上已经有人测试过了。安装好之后，输入 `startxfce4` 就可以启动桌面环境了。

```bash
sudo apt install xfce4
```

安装过程中，键盘可以选择 US 也可以选择简中，反正简中的键盘布局和 US 没有什么区别。

安装完成后需要设置好 locales 等。默认的时区是 UTC ，默认的 locale 是 C.UTF-8 。这些都需要我们手动设置。

```bash
sudo apt install tzdata locales
sudo dpkg-reconfigure tzdata
sudo dpkg-reconfigure locales
```

安装中文输入法（我选择的是fcitx5 pinyin）。

```bash
sudo apt install fcitx5 fcitx5-pinyin
```

然后我们配置一下环境变量：

```bash
sudo vim ~/.profile
```

在文件里添加以下内容：

```properties
LANG=zh_CN.UTF-8
LC_CTYPE=zh_CN.UTF-8
LC_NUMERIC=zh_CN.UTF-8
LC_TIME=zh_CN.UTF-8
LC_COLLATE=zh_CN.UTF-8
LC_MONETARY=zh_CN.UTF-8
LC_MESSAGES=zh_CN.UTF-8
LC_PAPER=zh_CN.UTF-8
LC_NAME=zh_CN.UTF-8
LC_ADDRESS=zh_CN.UTF-8
LC_TELEPHONE=zh_CN.UTF-8
LC_MEASUREMENT=zh_CN.UTF-8
LC_IDENTIFICATION=zh_CN.UTF-8
LC_ALL=

GTK_IM_MODULE=fcitx
QT_IM_MODULE=fcitx
XMODIFIERS=@im=fcitx
SDL_IM_MODULE=fcitx
GLFW_IM_MODULE=ibus

fcitx5 &
```

然后我们输入 `exit` 就可以退出我们当前账户回到 root 账户了；再输入 `exit` 就可以退出 Debian 了。

## 启动 Termux-x11

首先编辑 `~/.termux/ter,ux.properties`，添加以下内容：

```properties
allow-external-apps=true
```

然后输入以下命令：

```bash
pulseaudio --start --load="module-native-protocol-tcp auth-ip-acl=127.0.0.1 auth-anonymous=1" --exit-idle-time=-1
pacmd load-module module-native-protocol-tcp auth-ip-acl=127.0.0.1 auth-anonymous=1

export DISPLAY=:0
termux-x11 :0 &

virgl_test_server_android &
```

然后我们继续输入 `proot-distro login debian-legacy --user user --shared-tmp`，进入 Debian 11：

```bash
export DISPLAY=:0
PULSE_SERVER=tcp:127.0.0.1
fcitx5 &
dbus-launch --exit-with-session startxfce4 &
```

这样我们就可以打开 Termux-X11 APP 看到桌面环境了。

你可以在设置里面配置一下 UI、输入法，也可以安装自己喜欢的软件，比如终端模拟器安装一个更好看的qterminal，浏览器安装一个firefox。

有几个需要注意的地方：

1. 安装 Chromium 的时候，由于安卓的限制，非 Root 用户无法真正地 chmod ，导致 sandbox 的权限在用户，导致 chromium 无法正常启动，并且所有的 electron 软件，包括但不限于 VSCode、QQ ，都无法正常启动。目前本人的办法是，给他们添加上 `--no-sandbox`。应用程序列表里面的快捷方式目录是 `~/.local/share/applications`，可以在里面找到对应的快捷方式，然后修改 `Exec` 一项，添加 `--no-sandbox`。比如：

    ```properties
    [Desktop Entry]
    Name=Chromium
    Exec=chromium --no-sandbox
    Icon=chromium
    Type=Application
    Categories=Network;WebBrowser;
    ```

    由于`--no-sandbox`会导致安全问题，建议还是使用 Firefox吧。

    当然如果你使用QQ、VS Code，也记得加上 `--no-sandbox`。

2. 安装WPS Office 的时候不仅需要根据他们的介绍安装依赖，更重要的是，安装`qt5ct`，然后在`~/.profile`里面添加以下内容：

    ```properties
    export QT_QPA_PLATFORMTHEME=qt5ct
    ```

    这样才能正常启动 WPS Office。

3. 可以使用 Xfce4 的设置调节 DPI 但是对于 QT 软件（比如 WPS Office）无效，需要在 `~/.profile` 里面添加以下内容：

    ```properties
    export QT_FONT_DPI=144 # 这里的 DPI 根据自己的设备调节
    ```

    这样可以让那些软件字体大小合理点。

如果想偷懒，可以把这些命令写到一个 shell 文件里面，保存在 Termux 的 `~/.shortcuts/` 目录下，然后在 Termux:Widget 的快捷方式里面添加这个 shell 文件，这样就可以直接启动了。当然注意一下chmod +x。

值得一提的是 Termux:X11 这个 APP 对于三星的适配还不错，甚至能做到键盘独占。
