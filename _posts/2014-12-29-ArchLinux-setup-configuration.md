---
layout: post
title: ArchLinux安装与配置
categories: [Linux]
---

电脑在Ubuntu14.04下发热厉害，网页开多了也经常死机，也差不多该更新换代了，但目前资金不足，便决定再次安装轻量级的[ArchLinux](https://www.archlinux.org/)，鉴于之前失败过一次，这次更加小心了。
先上图：
![](http://ww4.sinaimg.cn/large/6120fe13jw1enu2a88703j20qa09zn0y.jpg)

## 安装ArchLinux

这块直接参考[ArchWiki](https://wiki.archlinux.org/index.php/Beginners%27_guidehttps://wiki.archlinux.org/)，不喜欢英文的可以选择中文显示，ArchLinux的Wiki写的非常不错，一步一步照着敲肯定不会出错，这里我就写下几个我认为比较重要的地方：

**制作U盘镜像**

1. Linux用户可以直接用`dd`命令，用U盘替换 /dev/sdx，如 /dev/sdb（不要加上数字，也就是说，不要键入 /dev/sdb1 之类的东西)：

    ```sh
    $ dd bs=4M if=/path/to/archlinux.iso of=/dev/sdx && sync
    ```

2. Windows用户用Wiki上推荐的USBwriter制作的镜像可能引导不了，推荐大家使用[Win32 Disk Imager](http://sourceforge.net/projects/win32diskimager/)。
3. 不过用这两种方式装完之后，都需要用Linux上的dd命令往U盘的前512字节写零来恢复它完整的容量：

    ```sh
    $ dd count=1 bs=512 if=/dev/zero of=/dev/sdx && sync
    ```

**字符乱码问题** locale.gen不要全部配成zh开头的，还是加上en_US，这样安装完之后不会出现乱码。

**登陆管理器** 如果进入不了图形界面，一定要检查一下自己是否安装了登陆管理器，我现在使用的是slim，轻量级，并配置为随机主题，如果登陆管理器进不了图形界面，一般是配置文件有问题，可以按Ctrl+Alt+F2再开一个终端用root进行修改。

## 钟爱的窗口管理器

先比较下我用过的这些wm：gnome，i3wm，awesome，xmonad，herbstluftwm

1. gnome还是挺好看的，我喜欢它的gdm，就是太大太占资源，本本吃不起；
2. i3wm用了将近一个月，配置简单，反应也不慢，但是我的chromium在里面运行时经常无故关闭，重装chromium，检查快捷键都没用，而且上面的i3bar看不习惯，就换了xmonad；
3. 看了一点haskell之后配置了一下xmonad，发现确实不好配置，haskell这门语言也需要花时间去学习，就试试awesome；
4. 相比xmonad，awesome配置起来还是很方面的，用了将近一个星期，感觉awesome过于复杂，而且那个bar也不好看，换成herbstluftwm；
5. 目前用的就是herbstluftwm，配置简单，操作方便，运行速度也非常快，而且为了让窗口达到最大化，我直接去掉了状态栏。

下面列一下我使用的配置文件中重要的配置(~/.config/herbstluftwm/autostart):

```sh
Mod=Mod4   # Use the super key as the main modifier
feh --bg-scale /home/quentin/Entertainment/Pictures/niu.jpg # background
xcompmgr -c # transparent
# keybindings
hc keybind Control-bracketleft spawn emacs
hc keybind Control-F1 spawn deepin-screenshot
hc keybind Control-F2 spawn chromium
hc keybind Control-F3 spawn firefox
hc keybind Control-F4 spawn /opt/ideaIU/bin/idea.sh
hc keybind Control-F5 spawn virtualbox
hc keybind Control-F6 spawn python /home/quentin/git/my_profile/OpenOrClosePSMouse.py
hc keybind Control-F7 spawn evince # PDF
hc keybind Control-F9 spawn deadbeef # music
```

## 常用软件

|software|description|
|:-------:|:-------------|
|Emacs|神的编辑器|
|Urxvt|最常用的终端|
|zsh|自动补全很给力|
|ibus|中文输入法|
|git|版本控制|
|chromium|书签同步太强大|
|firefox|离不开firebug等各种插件|
|IDEA|Java IDE|
|evince|看PDF|
|deadbeef|听音乐|

需要说的是deepin-screenshot这款截图软件，相比QQ上的截图工具也毫不逊色，但使用命令`yaourt -S deepin-screenshot`安装会出现:
`在构建deepin-pygtk时提示：无法下载pygtk_2.24.0-3deepin3.debian.tar.gz`
故需要在`/etc/pacman.conf`中添加源:

```sh
[home_metakcahura_arch-deepin_Arch_Extra]
SigLevel = Never
Server = http://download.opensuse.org/repositories/home:/metakcahura:/arch-deepin/Arch_Extra/$arch
# 若升级时若总是提示文件校验失败，使用备用地址即可
# Server = http://anorien.csc.warwick.ac.uk/mirrors/download.opensuse.org/repositories/home:/metakcahura:/arch-deepin/Arch_Extra/$arch
```

再直接用pacman安装即可，详细请看我之前再Arch Forums上发的帖子[用yaourt安装deepin-screenshot失败，archlinux12.01-ideapadY460](https://bbs.archlinuxcn.org/viewtopic.php?id=3040)

## 配置文件下载
[github链接](https://github.com/jlovedragon/profile/tree/master/archlinux)
