# Debian小指南


## 前言

{{< admonition type=info title="注意" open=true >}}
本文档默认以普通用户身份运行系统
{{< /admonition >}}

## 换源

- [清华大学开源软件镜像站使用帮助-Debian](https://mirrors.tuna.tsinghua.edu.cn/help/debian/) 

- [中国科学技术大学开源软件镜像站使用帮助-Debian](https://mirrors.ustc.edu.cn/help/debian.html) 

随便选一个，记得把 `source.list` 里的CD源注释掉，一般安装完系统就用不上了，要是不注释掉，每次 `apt update` 都会被提醒这个源不能正常使用。

## 驱动安装

首先可以先检测已安装的驱动：`lspci -knn`

### RTL8821CE无线网卡

1. 到tomaspinho的 [GitHub仓库](https://github.com/tomaspinho/rtl8821ce) 下载驱动

2. ```shell
   sudo apt install bc module-assistant build-essential dkms
   sudo m-a prepare
   ```

3. 在包含源代码的目录中打开一个终端，然后执行以下命令：

   ```shell
   sudo ./dkms-install.sh
   ```

4. 第3步一般都会被告知权限不够，不能运行 `dkms-install.sh` 。就算 `su root` 后，权限还是不够，因为这里的权限指的是文件的操作权限。可以先更改该文件的操作权限：`chmod 777 dkms-install.sh` ，然后再使用 `sudo ./dkms-install.sh` 运行。这算是运行脚本时常见问题。

{{< admonition type=success title="其他方式" open=true >}}
对于这个网卡驱动，这一步也可以使用以下命令编译： `sudo make` 、`sudo make install`
{{< /admonition >}}

5. 重启Debian

### 显卡

各类显卡驱动[WiKi](https://wiki.debian.org/GraphicsCard)

#### NVIDIA显卡

{{< admonition type=info title="注意" open=true >}}
WiKi最好用英文看，因为官方的中文版本比较落后。比如下面第一个，简体中文版只有Debian 6和Debian 7，而英文版有Debian 9和Debian 10。
{{< /admonition >}}

系统自带的是开源驱动`nouveau`。但我用NVIDIA官方的驱动。

1. 首先安装NVIDIA官方驱动，可以在 [WiKi](https://wiki.debian.org/NvidiaGraphicsDrivers) 的列举里下载（注意查看是否为受支持的设备），并按照 [WiKi](https://wiki.debian.org/NvidiaGraphicsDrivers) 安装。
2. 也可以到NVIDIA [官网](https://www.nvidia.cn/Download/index.aspx?lang=cn) 下载 `run` 包，手动编译安装（[手动编译安装指南](https://us.download.nvidia.cn/XFree86/Linux-x86_64/450.57/README/index.html)）。
3. 如果是双显卡，可以从这里选择一个方案来切换显卡：[NVIDIA Optimus](https://wiki.debian.org/NVIDIA%20Optimus)
4. 使用 `glxinfo` 程序. 这个程序是在 `mesa-utils` 软件包中，如果


```shell
glxinfo | grep rendering
```

返回值为 `direct rendering: Yes` 表示已成功安装。

#### Intel显卡

{{< admonition type=quote title="官方方案" open=true >}}
- 如果显卡是2007年及以后生产的，请尝试卸载[xserver-xorg-video-intel](https://packages.debian.org/xserver-xorg-video-intel) 软件包，并使用内置的模式设置驱动程序（[xserver-xorg-core](https://packages.debian.org/xserver-xorg-core)）。
- 对于较旧的设备：[xserver-xorg-video-intel](https://packages.debian.org/xserver-xorg-video-intel)
- 如果已安装驱动程序，但Debian/Ubuntu找不到，请尝试通过运行安装 `mesa-utils`。
{{< /admonition >}}

#### 切换显卡

##### Prime方案

首先安装 `nvidia-prime` ，源中是找不到它的，需要手动下载安装，[点此下载](http://archive.ubuntu.com/ubuntu/pool/main/n/nvidia-prime/nvidia-prime_0.8.14_all.deb)

选择Intel显卡：

```shell
sudo prime-select intel
```

选择NVIDIA显卡：

```shell
sudo prime-select nvidia
```

查看正在运行的显卡：

```shell
prime-select query
```

## 可能遇到的问题

### 用户不在sudoers文件中

**场景：**

`sudo` 失败，提示“用户不在sudoers文件中此事将被报告”。

{{< admonition type=success title="解决方法👇" open=false >}}
{{< /admonition >}}

1. `su root` ，以root身份运行 `vi /etc/sudoers`，或者 `nano /etc/sudoers`，总之打开 `sudoers` 这个文件并编辑它。

2. 在

   ```shell
   # User privilege specification
   root ALL=(ALL:ALL) ALL
   ```

   下添加一行

   ```shell
   username ALL=(ALL:ALL) ALL
   ```

3. 保存退出即可。

### 无法调节音量

**场景：**

在有声音输出的前提下，无法调节音量。

{{< admonition type=success title="解决方法👇" open=false >}}
{{< /admonition >}}

1. 使用 `alsamixer` 命令直接调节。

2. 安装Kmix音量调节器。

   ```shell
   sudo apt-get install kdemultimedia kmix
   ```

{{< admonition type=note title="额外参考" open=false >}}
https://wiki.debian.org/Sound

[检测音频设备命令](https://www.jianshu.com/p/1b79537da86d)
{{< /admonition >}}

### 锁屏后黑屏

{{< admonition type=success title="解决方法" open=true >}}
安装 `laptop-mode-tools` ，参靠 [这里](http://www.linuxdiyf.com/linux/18722.html)
{{< /admonition >}}

### 提示缺失firmware

**场景1：**

```shell
Possible missing firmware /lib/firmware/rtl_nic/rtl8168e-2.fw for module r8169
```

**解决方法：**

```shell
sudo apt-get install firmware-realtek
```

**场景2：**

```shell
Possible missing firmware /lib/firmware/i915/glk_dmc_ver1_04.bin for module i915
```

{{< admonition type=success title="解决方法" open=true >}}
到 [这里](https://git.kernel.org/pub/scm/linux/kernel/git/firmware/linux-firmware.git/tree/i915) 找到缺失的固件，点击 `plain` 下载，把它们复制到 `/lib/firmware/i915` 下。
{{< /admonition >}}

### 配置环境变量

**场景**：

```shell
dpkg: warning: 'ldconfig' not found in PATH or not executable.
dpkg: warning: 'start-stop-daemon' not found in PATH or not executable.
dpkg: error: 2 expected programs not found in PATH or not executable.
Note: root's PATH should usually contain /usr/local/sbin, /usr/sbin and /sbin.
```
翻译成中文：
```shell
dpkg：警告：在路径中找不到'ldconfig'或它不是可执行文件。
dpkg：警告：在PATH中找不到'start-stop-daemon'或它不是可执行文件。
dpkg：错误：在PATH中找不到2个预期程序或它们不可执行。
注意：root的PATH通常应该包含/usr/local/sbin, /usr/sbin and /sbin.
```

{{< admonition type=success title="解决方法" open=true >}}
参考 [这里](https://segmentfault.com/a/1190000017270408?utm_source=tag-newest) 以及 [更多配置环境变量的方法](https://cloud.tencent.com/developer/article/1640616)
{{< /admonition >}}

### 软件无法启动

**场景**：一般是root身份登录系统，打不开某些软件（google chrome），而普通用户可以打开；然而有时，某些软件普通用户也打不开（PicGo的AppImage）。以下解决方法适用于上述两种情况

{{< admonition type=success title="解决方法👇" open=false >}}
{{< /admonition >}}

以google chrome为例：

  1. 找到`usr/share/applications`中的google chrome启动文件或图标，右击用文本编辑器打开
  2. 在`Exec=typora %U`后面添加` --no-sandbox`（注意`--`的前面有个空格）

### 双系统时间不一致

{{< admonition type=success title="解决方法" open=true >}}
参考 [这里](https://sspai.com/post/55983)
{{< /admonition >}}

### 双系统磁盘读写问题

#### Windows磁盘挂载到Linux

{{< admonition type=question title="原因" open=true >}}
挂载后，有时处于只读状态，这是由于Windows的“快速启动”(Fast Startup)功能开启后，关闭计算机时，Windows将一部分RAM里的内容保存到磁盘，下次启动时，将该部分内容重新加载到内存中。
{{< /admonition >}}

{{< admonition type=success title="解决方法" open=true >}}
1. 从grub界面选择Windows启动，然后重启而不是关机。

   虽然不方便，但这也是解决问题的最快方法，不需要像其他解决方法那样长期更改任何内容。Windows重启时，不会在下次启动时使用“快速启动”功能。这意味着它不会进入休眠状态、获取系统运行状态的快照或将任何内存数据保存到磁盘。分区上没有休眠数据，这意味着可以安全地写入到分区上，Linux会识别出这一点。

2. 禁用快速启动。

   如果常常需要从Linux写入到Windows分区上，可以选择这个方法。 **缺点是Windows需要更长的开机引导时间。** 要禁用快速启动，可以在设置或者控制面板里面找“电源选项”。
{{< /admonition >}}

以上两种方法是安全的做法，当然也可以在网上搜索使用linux命令的方法来解决问题。

#### Linux磁盘挂载到Windows

{{< admonition type=success title="解决方法" open=true >}}
在Windows上使用 [Paragon ExtFS for Windows](https://china.paragon-software.com/home-windows/extfs-for-windows/download.html) 这个软件。
{{< /admonition >}}

### ROOT身份运行应用

{{< admonition type=success title="解决方法" open=true >}}
使用 `pkexec` 命令
{{< /admonition >}}

如以ROOT身份运行应用Dlophin：

```shell
sudo pkexec env DISPLAY=$DISPLAY XAUTHORITY=$XAUTHORITY dolphin
```

## 常用软件安装参考

[**Chrome**](https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb)

[**Qv2ray**](https://qv2ray.github.io/getting-started/)

[**Telegram**](https://desktop.telegram.org/)

[**Teamviewer**](https://www.teamviewer.cn/cn/download/linux/)

**Typora**

{{< admonition type=tip title="下载技巧" open=false >}}
不建议官网中添加第三方源的方式安装，那种方式慢的不可思议。而是参考 [issues 2625](https://github.com/typora/typora-issues/issues/2625) ，在 [Packages](http://typora.io/linux/Packages) 中找到要下载的版本文件名，然后手动将主页+文件名组合成下载链接，比如：`http://typora.io/linux/typora_0.9.89_amd64.deb`
{{< /admonition >}}

[**VS Code**](https://code.visualstudio.com/)

[**WPS for Linux**](https://www.wps.cn/product/wpslinux#)

[**OBS Studio**](https://obsproject.com/zh-cn/download)

[**命令行下载软件**](https://linux.cn/article-7369-1.html)

## 优化与美化

### fcitx 输入法

{{< admonition type=warning title="注意" open=true >}}
以下方案用的输入法是 `fcitx-pinyin` ，而不是 `fcitx-googlepinyin` 或者其他输入法。
{{< /admonition >}}

#### 搜狗词库转换

参考 [搜狗细胞词库转换](https://github.com/Chopong/fcitx-dict)

1. 安装 `fcitx-tools`

2. 下载 [createPYMB](https://github.com/Chopong/fcitx-dict/raw/master/createPYMB) 、[gbkpy.org](https://raw.githubusercontent.com/Chopong/fcitx-dict/master/gbkpy.org) 、[pyPhrase.org](https://github.com/Chopong/fcitx-dict/raw/master/pyPhrase.org) 、[搜狗scel词库](https://pinyin.sogou.com/dict/) ，假设都保存在 `/home/username/下载/`

4. ```shell
   #新建一个文件夹，即/home/username/下载/orgfile
   mkdir -p orgfile
   #将scel转换为org
   for dict in *.scel; do scel2org $dict -o orgfile/$dict.org    ; done
   #复制pyPhrase.org到orgfile中
   cp pyPhrase.org orgfile
   #去重、排序、汇总
   cat orgfile/* | sort | uniq > dicts.org
   #将org转换为mb
   createPYMB gbkpy.org dicts.org 
   ```

5. 正常应生成 `dicts.org` 、`pybase.mb` 、`pyERROR` 、`pyphrase.mb` 、`pyPhrase.ok` 其中 `pybase.mb` 为拼音单字库，`pyphrase.mb` 为拼音词库

6. 将 `dicts.org` 、`pybase.mb` 、`pyphrase.mb` 移动到全局配置拼音词库 `/usr/share/fcitx/pinyin/` 中（用户各自的配置拼音词库在 `/home/username/.config/fcitx/pinyin/` ），如果这文件夹不存在，手动新建即可，放好词库后，重启 Fcitx

{{< admonition type=note title="其他工具" open=true >}}
也有人用 [深蓝词库转换](https://github.com/studyzy/imewlconverter) 进行词库转换的，虽然它支持的词库类型更多，但是使用深蓝词库转换，还得配置 `dotnet`
{{< /admonition >}}

#### 搜狗皮肤转换

网上都推荐用这个工具：[ssf2fcitx](https://github.com/VOID001/ssf2fcitx) ，然而我总是被提示缺失几个头文件，总也配置不好，干脆就换成
[ssfconv](https://github.com/fkxxyz/ssfconv) ，这个工具比较新，而且对于ssf2fcitx还有改进。

下载[搜狗皮肤](https://pinyin.sogou.com/skins/)，转换好皮肤后，将皮肤文件夹放到全局设置 `/usr/share/fcitx/skin/ ` ，或者用户设置 `/home/username/.config/fcitx/skin/` ，如果这两个文件夹不存在，手动新建即可

### Grub

{{< admonition type=warning title="注意" open=true >}}
用PS编辑过的图片可能嵌入了grub不支持的一些东西，具体是什么我也没排查出来，如果强行应用就会得到grub的报错。
{{< /admonition >}}

1. [grub主题配置](https://blog.csdn.net/qq_27525611/article/details/104075692)  、[grub参数介绍](https://blog.csdn.net/xinlan3618/article/details/78963513)
2. 选择系统后，加载linux内存盘时，屏幕中央会有一个一闪而过的图片，它位于 `/etc/alternatives/desktop-theme/grub/`
3. 登录界面的背景图位于 `/etc/alternatives/desktop-theme/login/`

系统有一些主题位于 `/usr/share/desktop-base/` ，这里面的其他主题怎么启用我不知道。另外可以看到 

`/usr/share/desktop-base/active-theme/` 其实默认是指向 `/etc/alternatives/desktop-theme` 的

### 添加软件到系统应用列表

**场景：**

比如下载了一个PicGo的AppImage，但在应用程序面板的应用列表（相当于Windows开始菜单里的应用列表）中找不到。

{{< admonition type=success title="解决方法" open=true >}}
在任意位置右键新建一个 `链接到应用程序` ，然后在 `应用程序` 标签下的 `命令` 里填入软件所在路径（如果配置好后因为权限问题打不开软件，可在路径后加上 ` --no-sandbox` ），当然也可以直接浏览，然后确定。之后将这个配置好的 `.desktop` 文件（相当于Windows中的快捷方式）移动到 `/usr/share/applications/` 即可。
{{< /admonition >}}

### 为软件更改图标

{{< admonition type=success title="解决方法" open=true >}}
在 `/usr/share/applications/` 中，编辑要更改的软件的 `.desktop` 文件，在 `Icon=` 后填入自定义的软件图标路径即可。
{{< /admonition >}}

为防止以后误删该图标，也可将图标放入 `/usr/share/pixmaps/` 文件夹下（第三方图标都默认存放在这里），然后在 `Icon=` 后接图片名称即可，不用加文件后缀，比如现在有文件 `/usr/share/pixmaps/PicGo.png` ，写 `Icon=PicGo` 即可生效。

###  字体

参考 [如何在linux系统上安装字体](https://www.linuxdashen.com/%E5%A6%82%E4%BD%95%E5%9C%A8linux%E7%B3%BB%E7%BB%9F%E4%B8%8A%E5%AE%89%E8%A3%85%E5%AD%97%E4%BD%93part-1)


