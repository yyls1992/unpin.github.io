---
title: Sublime Text 3 采用 Markdown 格式编辑 Evernote（印象笔记）
date: 2014-11-10 20:10:33
categories:
- tools

---

由于跨平台的 Evernote（印象笔记）体验实在愉悦，近期把知识管理、笔记记录的主要工具从 Onenote 切换到了 Evernote。  
美中不足的是，原生的编辑功能对常接触代码及表格的我来说比较单薄，排版又略复杂。不过幸运的事有3件：

Evernote（印象笔记）提供了 API，供第三方应用管理笔记。  
这世界上有 markdown 这样舒服的标记语言。  
美妙的 IDE —— Sublime Text

得益于上述三件幸运的事，使得我们可以十分方便的在 Sublime Text 中，利用 markdown 格式编辑修改 Evernote（印象笔记）中的笔记。

***

##	安装 Sublime Text 3

Sublime Text 3 是一款共享性质的编辑器，比记事本强大、美观，又比常规 IDE 轻巧、灵活。同时针对个人非商业用途的用户免费，请自行前往[官网下载](http://www.sublimetext.com/3) Sublime Text 3。

注意，Evernote 插件只支持 Sublime Text 3，所以请不要下载成 Sublime Text 2.

安装好 Sublime Text 3 后需要为其添加插件管理功能 Package Control。

**自动安装：**

1.	通过快捷键 `ctrl+`` 或者 `View > Show Console` 菜单打开控制台  
2.	粘贴以下代码后回车

```
import urllib.request,os,hashlib; h = '7183a2d3e96f11eeadd761d777e62404' + 'e330c659d4bb41d3bdf022e94cab3cd0'; pf = 'Package Control.sublime-package'; ipp = sublime.installed_packages_path(); urllib.request.install_opener( urllib.request.build_opener( urllib.request.ProxyHandler()) ); by = urllib.request.urlopen( 'http://packagecontrol.io/' + pf.replace(' ', '%20')).read(); dh = hashlib.sha256(by).hexdigest(); print('Error validating download (got %s instead of %s), please try manual install' % (dh, h)) if dh != h else open(os.path.join( ipp, pf), 'wb' ).write(by)
```

**手动安装：**

1.	点击 `Preferences > Browse Packages…` 菜单  
2.	进入 `Installed Packages/` 目录  
3.	下载 [Package Control.sublime-package](https://sublime.wbond.net/Package%20Control.sublime-package)，并复制文件到 `Installed Packages/` 目录  
4.	重启 Sublime Text

##	安装 Evernote 插件

打开 Sublime Text 后，`cmd + shift + P` 打开 Package Control，输入 `install`，跳出插件库，然后输入 `Evernote`  安装 Evernote 插件。该插件的介绍和详细的使用说明请参考：[插件介绍](https://sublime.wbond.net/packages/Evernote)。

插件安装完成之后，第一次使用时需要进行授权认证。步骤如下：

1.	打开 Package Control，输入 `send to evernote`，在窗口底部会弹出一个小窗口，窗口中的内容为一个带 token 的链接。该 token 后面用到。  
2.	Evernote 国际版用户，将链接复制到浏览器并访问，登录授权后，会有一个 NoteStore URL。  
印象笔记用户，直接用浏览器访问链接：[https://app.yinxiang.com/api/DeveloperToken.action](https://app.yinxiang.com/api/DeveloperToken.action)，进行登陆授权，并记录 token 和 NoteStore URL。  
3.	打开 Sublime Text，打开 `Preferences > Package Settings > Evernote > Settings User`，将上步骤中的 token 和 NoteStore URL 复制到对应位置。参照格式如下：

```
{
	"noteStoreUrl": "这里填入你的 NoteStore URL",
	"token": "这里填入你的 token",
}
```

好了，Sublime Text 已经可以访问你的 Evernote 笔记了。

##	编辑方法

`cmd + shift + P` 打开 Package Control，输入 `evernote`，就能看到所有功能选项了，这里介绍几个比较常用的选项功能。

**Evernote: New empty note**

新建一个笔记。点选后弹出选单可以选择在哪个笔记本下建立笔记。

**Evernote: Open Evernote note**

打开一个已存在的笔记。点选后弹出菜单可以选择打开任何已存在笔记。

**Evernote: Update Evernote note**

更新当前编辑的笔记到 Evernote。

更多功能请大家自行发掘。

##	Markdown 扩展

支持 Evernote 的待办事项，格式如下：

```
- [ ] Unchecked
- [x] Checked
```

支持表格，但需在 `Settings – User` 中加入配置 `"wiki_tables": "true"`。格式如下：

```
|| *Year* || *Temperature (low)* || *Temperature (high)* ||
|| 1900 || -10 || 25 ||
|| 1910 || -15 || 30 ||
```

更多信息参考官方文档 [Supported-Markdown](https://github.com/bordaigorl/sublime-evernote/wiki/Supported-Markdown)。
