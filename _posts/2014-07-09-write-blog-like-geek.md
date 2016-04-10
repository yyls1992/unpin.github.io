---
layout: post
title: "像 Geek 一样写作：用 Jekyll + Github + Markdown 搭建 Blog 的介绍"
date: 2014-07-09 20:10:33 +0800
author: "Steve"
catalog: true
tags:
- jekyll
- github

---


在互联网愈来开放自由的今天，通过 Blog 表达自己的声音成为越来越大众化的事情。
于是涌现出了琳琅满目的个人 Blog 方案，对于选择恐惧症来讲，这真是最坏的年代。
让我赶快摇个骰（tóu，这里突然发现原来是读 "tóu" ，文盲到鼻酸）子吧。

最后选择了 Jekyll + Github 这种托管方式，搭建一个静态、版本可控、便于分享的 Blog (其实真实原因是作为一个码农，我觉得这样能略微提升自己在云云众码农中的逼格)。Blog 的写作，则采用 Markdwon 格式，专注快捷。
因为 Github 美好的开源特性，对本站感兴趣的同学可以很方便的查看全部源码，并 clone 自用。

搭建过程对熟悉 git 和 front-end 的来说非常轻松，所以本篇文章主要站在完全初次接触二者的角度来做讲解。

##  何为 Github

对个人用户来讲，Github 是一个云平台，能方便的对项目或文件进行版本管理，让我们不必再担心备份与恢复，一切提交，均有记录。

*补充介绍*  
Github 的本意，是一个针对开源代码的云仓库，用于代码版本的协同管理。这不妨碍我们把它活用为 Blog 服务的托管服务器。事实上，Github 主动为个人或项目提供了 Pages 这项服务，才使本方案成为可能。不得不感叹 Github 的诚意满满。

*   [Github.com](https://github.com/)

##  何为 Markdown

简单来说 Markdown 就是向纯文本中有规则的添加某些符号。更简单的说法就是我们常见的 ".txt" 文件，把它重命名成 ".md" 就叫新名字 Markdown 文件啦。

当然 Markdown 的重点就是添加符号的规则，该怎么加符号呢。
基础就是 " # ", " * ", " - ", " ` ", " > " 五个符号。

当我们要写一篇文章的大标题，就这样  
	#  我是大标题

然后在 Blog 中的显示效果就是  
# 我是大标题

够不够大 :)  
类似，当我们需要小二标题、小三标题等等，就这样  
	##  我是小二标题  
	###  我是小三标题

在 Blog 中的显示效果就是  
##  我是小二标题
###  我是小三标题

其他几个符号也各有用处。有了这五个基础符号，就能完成本 Blog 大部分的格式整齐的文章啦。

*补充介绍*  
Markdown 的完整教程由于篇幅就不展开了，给出链接，大家可以前去学习。

*   [Markdown 语法中译](http://markdown.tw/)
*   [Markdown 在线写作神器 Stackedit](https://stackedit.io/)

###  又何为 Jekyll

Jekyll 作用是把我们放在 Github 服务器上的零散文件组合、转换为精美的静态网页。
上文我们写的  
	#  我是大标题

就是 Jekyll "噗" 的一下把它变成  
# 我是大标题

*补充介绍*  
熟悉 front-end 开发，又想制作独一无二的精美 Blog 的同学，可以参看 Jekyll 官方文档。图省事的就像我一样，直接用别人制作的模板吧。

*   [Jekyll Docs](http://jekyllrb.com/docs/home/)

### 搭建

直接参看 Jekyll 官网的教程即可：[Quick-start guide](http://jekyllrb.com/docs/quickstart/).