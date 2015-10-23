---
title: Ubuntu 软件精选
date: 2014-06-24 20:10:33
updated: 2015-08-31 22:58:00
categories:
- tools

---

整理了下 Ubuntu 常用软件，备忘。

## Productivity

* [Chrome](https://www.google.com/chrome/browser/desktop/) 

PC， 手机默认浏览器都是 Chrome。如果网络条件能好一点，甚至系统都想用 Chrome OS。

* [DropBox](https://www.dropbox.com/install)

由于一些原因，Dropbox 使用比较困难，需要添加 hosts 才能正常使用。

* [Android Studio & SDK](https://developer.android.com/sdk/index.html)

安装过程
1. Unpack the `.tar` file where it lays

2. Using a Terminal window, navigate to that folder location (probably cd ~/Downloads), invoke `sudo -i` (You will need administrator/su permissions for the next few steps)

2. `mv android-studio /opt` moves the files you unpacked (requires permissions)

3. `gedit android-studio.desktop` this opens a text editor so that we may create a shortcut icon to open your new program. Insert the following code, then save the .desktop file.

	[Desktop Entry]
	 Name=Android-Studio
	 Comment=your comments
	 Exec=/opt/android-studio/bin/studio.sh
	 Icon=/opt/android-studio/bin/idea.png
	 Terminal=false
	 Type=Application
	 Categories=Utility;IDE;Development;

4. To finish up, `desktop-file-install android-studio.desktop`.

* [Genymotion](https://www.genymotion.com/#!/download)

可能是最快的虚拟机，Android 开发必备。

* Sublime Text 3 & 中文输入补丁

```bash
sudo add-apt-repository ppa:webupd8team/sublime-text-3
sudo apt-get update
sudo apt-get install sublime-text
```

[解决Ubuntu(Linux)下Sublime Text 3无法使用中文输入法的问题](http://www.lanmeng.org/2015/05/linux-sublimetext-chinese.html)

* JDK8

```bash
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get install oracle-java8-installer
sudo apt-get install oracle-java8-set-default
```

* [Smartgit](http://www.syntevo.com/smartgit/)

图形化的 Git 客户端，个人 License 免费。

* [CopyQ](https://github.com/hluk/CopyQ/releases)

一个剪贴板管理工具。

* [有道词典](http://cidian.youdao.com/index-linux.html)，[搜狗输入法](http://pinyin.sogou.com/linux/?r=pinyin)

## Watch and play

* Spotify

体验非常优秀的音乐社交平台，有了它再也不用发愁音乐的同步问题啦。而且居然广告都能被设计的令人想看了再看！强烈推荐。

```bash
sudo sh -c 'echo "deb http://repository.spotify.com stable non-free" >> /etc/apt/sources.list.d/spotify.list'
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 94558F59
sudo apt-get update
sudo apt-get install spotify-client
```

* ~~Zeal~~

~~Zeal is an offline documentation browser inspired by Dash, available for Linux and Windows.~~

```bash
sudo add-apt-repository ppa:zeal-developers/ppa
sudo apt-get update
sudo apt-get install zeal
```

* Calibre, VLC, GIMP, OpenShot, Kazam, Stellarium

*Software Center*

## System tools

* [Cow](https://github.com/cyfdecyf/cow)

COW 是一个简化的 HTTP 代理服务器。它能自动检测被墙网站，仅对这些网站使用二级代理。

```bash
sudo apt-get install curl
curl -L git.io/cow | bash
```

* ~~indicator-netspeed-unity~~

~~用于在状态栏显示实时流量的插件。~~

```bash
sudo add-apt-repository ppa:fixnix/netspeed
sudo apt-get update
sudo apt-get install indicator-netspeed-unity
```

* ~~Caffeine~~

```bash
sudo add-apt-repository ppa:caffeine-developers/ppa
sudo apt-get update
sudo apt-get install caffeine
```

* ~~F.lux~~

~~一个 ‘Keeps Your Eyes Sharp’ 的小工具，通过调节屏幕色温适应各个时间段的光照亮度来保护眼睛。有 Windows 、 MAC 及 Linux 版本，开发者诚意十足。~~

```bash
sudo add-apt-repository ppa:kilian/f.lux
sudo apt-get update
sudo apt-get install fluxgui
```

* VirtualBox

*Software Center*  
良好的多平台支持，加上较小的体积，推荐。另需要 USB 等支持的话必须添加用户组，`gedit /etc/group`，然后添加自己用户名 `vboxusers:x:126:steve`.

* [Theme - Ambiance & Radiance Flat Colors](http://gnome-look.org/content/show.php/Ambiance+%26+Radiance+Flat+Colors?content=168155)

Ambiance & Radiance Flat is a Modern, Beautiful and Vivid Re-imagination of the Ambiance & Radiance Color GTK 2/3 Themes. It features a Modern, Clean And "Flat" look in your choice of 13 vibrant colors. A theme For Ubuntu, Mint Or any Distro and nearly any GTK Desktop.

* Denyhosts

一款 Python 语言的程序，用于分析 sshd 的日志文件。防止 SSH 被暴力扫描破解。

* System Monitor Indicator, FileZilla, Wireshark, Unity Tweak Tool, Supervisor

*Software Center*

Closed.
