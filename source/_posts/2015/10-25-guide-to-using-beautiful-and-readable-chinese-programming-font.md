---
title: 改善 Windows 上 Source Code Pro 等编程字体的中文显示效果
date: 2015-10-25 09:43:00
categories:
- tool
tags:
- ide
- font
coverImage: sleepsort.png
coverMeta: out
---

>   写代码时用上一款易于辨识且美观的字体是非常节约生命，和保护眼睛的做法。但由于大多数很棒的编程字体都不包含中文字符，导致显示中文常常调用「宋体（SimSun）」，这绝妙的反差实在是不忍直视。其实在 Windows 上我们可以使用「字体链接（FontLink）」技术解决这个问题。最终可以实现 Source Code Pro + 微软雅黑 搭配这样的效果。

Sublime Text 在 Windows 系统上默认英文字体是 Consolas，默认中文字体是「宋体（SimSun）」。作为经典的无衬线字体，Consolas 的显示效果称得上优秀。不过一旦出现中文字符，时间就像倒回了 10 年前。。。

不过我们可以简单的在【Preferences.sublime-settings】中添加 `"font_face":"微软雅黑"` 来设置中文为雅黑字体。这个方法比较简单，保证中文的显示效果的同时，英文字体仍为默认的 Consolas。

但如果我们想同时修改默认的中英文字体，这个方法就不那么管用了。
当然，你可以选择去下载一款中英文字型打包的字体，比如 "YaHei Consolas Hybrid.ttf"。但是不是每个人都有相关的知识去整合任意两款中英文字体。而且由于个人整合，字体文件缺乏测试与后期维护，可能包含各种未知的问题。
网上比较常见的这款 "YaHei Consolas Hybrid.ttf" 字体就是 2012 年左右整合的，到今天这段时间，微软其实早已更新了雅黑字体的版本，来适应 Win10 等新系统的需要。

那么问题来了。
有没有办法在使用原生字体的基础上，实现任意混合中英文的显示效果呢？

### 字体链接（FontLink）

简单来说，「字体链接（FontLink）」就是在使用英文字体时，如果遇到这种字体中没有的字符，系统就会到注册表相应的位置去找它链接的字体。

个人对编程字体的偏好有如下几个点：

*   单词中的每个字母易于分辨，例如 "I" 与 "l"，"O" 与 "0" 等
*   符号（如 # % $ * \）与字母大量混杂时仍能保持整齐美观
*   与中文字型观感上无冲突
*   无衬线（无衬线字体更加现代，也能与移动设备上的培养起来的字体观感习惯保持一致）

这里我们可以非常流行 Source Code Pro 作为英文字体。中文有「微软雅黑（Microsoft YaHei）」和「思源黑体（Source Han Sans / Noto Sans CJK）」两个不错的选择。以雅黑为例。

注册表进入

    HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\FontLink\SystemLink

你会发现有很多名称为字体名的键，这就是为什么我们使用英文字体时仍然可以正常显示中文。

![](1.png)

新建多重字符串键 "Source Code Pro"（必须与字体名称完全一致，键类型为 Multi-String），然后照下图输入：

![](2.png)

这里的 "MSYH.TTF" 就是微软雅黑字体的文件名。其后跟的数字是为了调整中文字体大小，使其能和英文字体相匹配（默认是 128,96，这里 "中文字体：英文字体" 为 1.6:1，所以设置为 `128*1.2,96*1.2` 即 `154,115`）。当一个汉字的宽度等于两个小写字母，即两者匹配。

![](3.png)

第二行重复的 "MSYH.TTF" 是为了使 GDI+ 能够识别该字体，第一行则是 GDI 调用。完成后重启系统生效。

在 Sublime Text 个人配置文件中添加 `"font_face":"微软雅黑"` 即可。

最后附上效果图，已及部分引用。

![](4.png)

### Resource

*   [Source Code Pro](https://github.com/adobe-fonts/source-code-pro)
*   [Source Han Sans](https://github.com/adobe-fonts/source-han-sans)
*   [Predawn - Sublime Text Theme](https://packagecontrol.io/packages/Predawn)
*   [修改 cmd 控制台字体、巧用 FontLink 使中英文独立设置](http://www.cnblogs.com/RhinoC/p/4470338.html)