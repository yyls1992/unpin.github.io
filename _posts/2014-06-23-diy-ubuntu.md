---
layout: post
title: "如何优雅的使用 Ubuntu 14.04 LTS"
date: 2014-06-23 20:10:33 +0800
author: "Steve"
catalog: true
tags:
- ubuntu

---

大概在 2010 年，我首次接触到 Ubuntu 和 Fedora，尽管因为图形界面、交互等种种原因很快不了了之，但是我对的 Fedora 抱了非常大的期待。记忆小键盘锁定状态等一些在 Windows 上习以为常的细节，只有 Fedora 作了比较好的支持。

最近实在不堪忍受 Windows 上配置 Ruby 等开发环境的繁琐，打算迁移到 Linux 上来，反正 Dota2 for Linux 已经发布，哈。

经历大约半个月的摇摆期，最终确定 Ubuntu 为目标系统。期间折腾过 Fedora, OpenSUSE, Linux Mint 等几个发行版本，感觉 Fedora 改进缓慢，OpenSUSE 软件源不够丰富，Linux Mint 太像 Windows 审美疲劳。

期间出现操作失误导致硬盘全格一次，幸好有备份重要数据到网盘的习惯。不过突然发现 Dropbox 连不上是闹哪样啊，同步自己数据还得翻墙？支持国货也不是这样搞的吧，幸好翻墙技能早已 get。

把这段时间使用下来，能提升自己体验的一些 Ubuntu 14.04 LTS 优化方案整理分享，希望对大家有所帮助。

## 优化列表目录

*	[解决 Ubuntu 屏幕亮度参数不能保存](#解决 Ubuntu 屏幕亮度参数不能保存)

*	[消除单系统下引导 Ubuntu 时黑色屏幕出现的紫色边框](#消除单系统下引导 Ubuntu 时黑色屏幕出现的紫色边框)

*	[解决启动中出现 INFO @wl_cfg80211_attach : Registered CFG80211 phy 影响美观的问题](#解决启动中出现 INFO @wl_cfg80211_attach : Registered CFG80211 phy 影响美观的问题)

*	[Ubuntu 14.04 LTS 英文环境下系统及 Chrome 中字体美化](#Ubuntu 14.04 LTS 英文环境下系统及 Chrome 中字体美化)

---

## 解决 Ubuntu 屏幕亮度参数不能保存

这里不得不吐槽下，我在 10 年初次接触 Ubuntu 就遇到过这个问题，过了 4 年居然还没有得到修复，搜索一下这个问题有接近 6w 的帖子。而红帽系的 Fedora 上这个问题早就解决了...

好了赶快进入正题。

每次登陆 Ubuntu 14.04 LTS 系统时屏幕亮度都为最高，网上能搜索到的几种解决方法要么太复杂要么无效。自己经过尝试提供一种比较好的解决方案。

AMD 显卡的 Ubuntu 14.04 LTS 的亮度设置保存在 `/sys/class/backlight/acpi_video0/brightness`。每次登陆系统时这个文件都会被重置为最大值，所以直接更改这个文件的值是没用的。

可以用以下两个步骤来让系统启动后自动设置屏幕的亮度值：

第一步，更改 `rc.local` 文件使系统在启动时自动更改 `/sys/class/backlight/acpi_video0/brightness` 文件权限。

在终端输入

	sudo gedit /etc/rc.local

如果询问密码的话，输入你安装系统时设置的密码

打开后你会看到如下内容：

```
	#!/bin/sh -e
	#
	# rc.local
	#
	# This script is executed at the end of each multiuser runlevel.
	# Make sure that the script will “exit 0″ on success or any other
	# value on error.
	#
	# In order to enable or disable this script just change the execution
	# bits.
	#
	# By default this script does nothing.
	
	#exit 0
```

插入这句话到代码中：

	chmod a+w /sys/class/backlight/acpi_video0/brightness

保证修改后代码如下（注意如果 `exit` 前有 `#` 请删除）：

```
	#!/bin/sh -e
	#
	# rc.local
	#
	# This script is executed at the end of each multiuser runlevel.
	# Make sure that the script will “exit 0″ on success or any other
	# value on error.
	#
	# In order to enable or disable this script just change the execution
	# bits.
	#
	# By default this script does nothing.
	chmod a+w /sys/class/backlight/acpi_video0/brightness
	exit 0
```

保存退出。

第二步，更改 `.profile` 文件使系统在用户登录时自动更改 birghtness 的值

进入主文件夹，点击 “查看”，点击 “显示隐藏文件”。找到 `.profile` 文件，双击打开。

在 `.profile` 最后一行添加

	echo 7 > /sys/class/backlight/acpi_video0/brightness

**其中数字7为你设定的屏幕亮度值，最大值不同硬件为不同值，可自行尝试。**

保存退出。

这样每次进入系统后屏幕的亮度都会自动设置为 `.profile` 文件中设定的值。此方法的好处是配置保存在个人配置文件里，修改起来非常方便。而且无论睡眠唤醒，还是注销重登陆，亮度设置都能生效。

---

## 除去 Ubuntu 系统引导时黑色屏幕出现的紫色边框

作为一名出色的强迫症患者，有问题要解决，没有问题要创造问题来解决。

所以这个启动是紫框和下面一个出现黑字，纯属没事找事。只是自己看到 Ubuntu 启动时不能像 Windows 一样纯粹，就感觉天崩地裂无法接受。扯远了，这个问题主要有两个方案：

1.	更换 grub 主题或背景色
2.	命令修改 grub 参数

**方案1：**

ubuntu开机紫色背景有两个阶段：Grub、Plymouth，我们这里需要修改 Grub 阶段，简单点就是把紫色改为黑色即可。改动这个文件 `ubuntu-logo.grub` 。

	sudo gedit /lib/plymouth/themes/ubuntu-logo/ubuntu-logo.grub

修改配色参数。

	sudo gedit /lib/plymouth/themes/ubuntu-logo/ubuntu-logo.grub

修改背景色参数。

```
	#原来的紫色
	if background_color 44,0,30; then
	  clear
	fi

	#改成黑色
	if background_color 0,0,0; then
  	  clear
	fi
```
保存后终端执行

	sudo update-grub

至于更换 grub 主题，比较适合比我这种强迫症还 zuo 的同学采用，具体方法自行 Google。

**方案2：**

如果觉得系统默认的紫色还是挺好看，不想更换，就要用到第二种方法。

Ubuntu 官方的 Bug 专区里给出了临时解决方案。但这个方案可能导致引导时过快跳过 grub 菜单，不过因为我是单系统，所以它对我来说不是一个问题。具体讨论可以参看 [Grub theme: black screen with purple border](https://bugs.launchpad.net/ubuntu/+source/plymouth/+bug/1289809)

临时解决方案如下：

在终端输入

	sudo gedit /lib/plymouth/themes/ubuntu-logo/ubuntu-logo.grub

找到这句话 

	if background_color 44,0,30; then clear fi 

在 `if` 后面添加一个 `!` ，改动后为

	if ！ background_color 44,0,30; then clear fi`

保存后终端执行

	sudo update-grub

---

## 解决启动中出现 INFO @wl_cfg80211_attach : Registered CFG80211 phy 影响美观的问题

此问题的解决方案同样出自 Ubuntu 官方 Bug 专区，步骤如下：

在终端中执行

	sudo gedit /etc/initramfs-tools/conf.d/splash

添加这句话

	FRAMEBUFFER=y

保存后终端执行

	sudo update-initramfs -u

*注意，此解决方案提供者说可能会轻微影响启动速度。*

自己测试下来会增加 1-2 秒的开机时间，但是开机过程完美了有没有，强迫症得到了解脱！

具体讨论参看[[ 21.125934] INFO @wl_cfg80211_attach : Registered CFG80211 phy](https://bugs.launchpad.net/ubuntu/+source/linux/+bug/1160322)

---

## Ubuntu 14.04 LTS 英文环境下系统及 Chrome 中字体美化

我习惯使用英文界面，但是又希望看见漂亮的中文字体。但是在這一版更新完语言配置之后却会得到一个预设字体是 `标楷体` 以及 `细明体` 的中文显示...

感觉上来说，Ubuntu 的英文字体渲染甚至比 Win8 上还要漂亮。但是中文字体我真的不能忍，字体之难看，而且 Chrome 网页字体还严重发虚!以至于我把眼镜摘戴数次才确认，不是自己眼睛出了毛病。

赶快开始动手解决。因为个人比较喜欢看文泉驿微米黑，所以决定用其替换系统默认的中文字体。 Ubuntu 14.04 LTS 默认不包含文泉驿微米黑字体，我们首先来安装。终端输入

	sudo apt-get install ttf-wqy-microhei

然后把终端的路径指向 `/etc/fonts/conf.d`，这个路径下保存的就是字体配置文件的软连接。

	cd /etc/fonts/conf.d

**修改設定檔 `65-nonlatin.conf` **

先修改非拉丁字体的配置文件，加上文泉驿微米黑的设定。

	sudo gedit 65-nonlatin.conf

将 `<family>WenQuanYi Micro Hei</family>` 加在 `sans-serif` 区域里。

```
	<family>sans-serif</family>
	<prefer>
		<family>WenQuanYi Micro Hei</family>
		<family>Nachlieli</family> <!-- hebrew -->
		<family>Lucida Sans Unicode</family>
		......
```

将 `<family>WenQuanYi Micro Hei Mono</family>` 则是加在 `monospace` 区域里。

```
	<family>monospace</family>
	<prefer>
		<family>WenQuanYi Micro Hei Mono</family>
		<family>Miriam Mono</family> <!-- hebrew -->
		<family>VL Gothic</family>
		......
```

**修改配置文件 `69-language-selector-zh-cn.conf` **

注意，这里因为我的位置设为大陆，所以修改 `69-language-selector-zh-cn.conf`。如果处于台湾，需要修改的文件为 `69-language-selector-zh-tw.conf` 。以此类推。

	sudo gedit 69-language-selector-zh-cn.conf

这里 `<string>WenQuanYi Micro Hei</string>` 一样是加在 `sans-serif` 里。

```
	<match target="pattern">
		<test qual="any" name="family">
			<string>sans-serif</string>
		</test>
        <test name="lang">
            <string>zh-cn</string>
        </test>
		<edit name="family" mode="prepend" binding="strong">
			<string>WenQuanYi Micro Hei</string>
			<string>Droid Sans Fallback</string>
			<string>WenQuanYi Zen Hei</string>
			......
```

而 `<string>WenQuanYi Micro Hei Mono</string>` 也是加在 `monospace` 里。

```
	<match target="pattern">
		<test qual="any" name="family">
			<string>monospace</string>
		</test>
        <test name="lang">
            <string>zh-cn</string>
        </test>
		<edit name="family" mode="prepend" binding="strong">
			<string>WenQuanYi Micro Hei Mono</string> <!-- add by Steve -->
			<string>Droid Sans Fallback</string>
			<string>WenQuanYi Zen Hei Mono</string>
			......
```

**修改 `49-sansserif.conf` 防止 `Chrome` 的标签栏及书签栏中文变成乱码**

	sudo gedit 49-sansserif.conf

修改最后的 `<edit name="family" mode="append_last">` 中 `sans-serif` 为 `WenQuanYi Micro Hei`。*注意整个文件里会看到两个 `sans-serif` ，修改后后面一个，别改错位置了。*

```
	<edit name="family" mode="append_last">
		<!-- <string>sans-serif</string> -->
		<string>WenQuanYi Micro Hei</string>
	</edit>
```

保存后终端执行 `sudo fc-cache -fv` 或是 `sudo fc-cache -v` ，新字体配置都会立即生效。

Done.
