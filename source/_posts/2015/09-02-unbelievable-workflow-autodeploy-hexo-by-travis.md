---
title: 探索工作流：Travis CI 自动化构建 Hexo (一)
date: 2015-09-02 12:43:00
updated: 2015-09-03 12:58:00
categories:
- flow
tags:
- github
- travis
---

## 前言
---

社会效率在不断提升的今天，人却开始越来越懒。能网购的不出门，能拖延的不决断，能自动的不手动。所以持续集成这个概念愈发重要也是有很充分理由的。  

> 持续集成是一种软件开发实践。在持续集成中，团队成员频繁集成他们的工作成果，每人每天可能集成一次，甚至多次。每次集成会经过自动构建（包括自动测试）的检验，以尽快发现集成错误。许多团队发现这种方法可以显著减少集成引起的问题，并可以加快团队合作软件开发的速度。

自动构建工具是持续集成的一种出色实践。  	
简单来说它的思路就是，写完代码，起来喝茶，keep calm and drink tea. 听见你说，朝阳起又落。。。  
当然它不是指这个，原谅我的不羁爱跑题。

实际上，代码提交后，由软件自动完成代码的测试、构建，并将过程中状态与构建物产出才是持续集成的意义。  
本文针对的 Travis CI 就是一个在线的、分布式的持续集成服务，用来构建及测试在 GitHub 托管的代码。这个软件的代码同时也是开源的，提供了多种编程语言的支持，包括 Ruby, JavaScript, Java, Scala, PHP 等。

顺便介绍一下它的原理：  
Travis CI 会在每一次提交之后生成一个虚拟机来执行事先安排好的 build 任务，你可以调整这个虚拟机的软件环境，甚至能执行 `sudo apt-get install` 自定义虚拟环境。

有没有感觉到持续集成的工作流非常适用 Hexo？每次改动功能或添加新文章后 Hexo 都要重新构建、发布，这样的机械化工作交给 Travis CI 再合适不过了。

## 工作流
---

Travis CI 自动构建 Hexo 的工作流的构思是：  
本地向 Github 上 push 代码后，如果该代码属于目标源（我们暂时称它为 dev repo），Travis CI 就自动构建 Hexo 环境编译它，并将产出的静态博客 push 回我们的 Github pages 源（我们就称它为 pages repo）。然后即可在 pages 上查看新发布的博客。  
上草图。  
![travis hexo flowing](travis-hexo-flowing.png)

画的很简单，像构建失败等一些情况都没绘出。大家领悟思路即可。

下面分解工作流，看看具体每一步都有哪些工作。

1. User - push -> Dev repo  
  事先在 Github 上建立好 repository 即可。有两种方案可供参考：

  * 建立 `username.github.io` 源，并开设 `blog` 分支，将此分支作为 Dev repo. 而 `master` 分支就是 Pages repo. 这样的好处是只用维护一个 repository.
  * 另一种方案，建立两个 repositories, 一个作为 Dev repo, 另一个`username.github.io` 源就作 Pages repo. 此方案的好处是可以开设多个项目 pages.

  本文采用前一种方案，后续的步骤及代码都针对此方案进行。  
  其实两种方案实现代码基本相同，用第二种方案的小伙伴注意最后配置正确 Pages repo 的 push 路径和权限即可。

2. Dev repo - sync -> Travis CI  
  在 Travis CI 中开启 Dev repo 的同步开关，然后在 Dev repo 中添加 `.travis.yml` 文件。这样 Travis CI 就能自动同步之后 push 的代码了。
  另外记得在 Travis CI 的同步设置中启用 `Build only if .travis.yml is present` 项，这样能在 repository 中有多个 branch 时，让 Travis CI 只构建放置了`.travis.yml` 文件的 branch.

3. Travis CI - build and push -> Pages repo  
  这里再分解为 build 和 push 两步：

  * build  
  Travis CI 的自动化构建完全依靠唯一的 `.travis.yml` 脚本文件。需要在此文件中添加构建环境、构建 Hexo、生成博客及后续 push 到 Pages repo 的全部脚本。熟悉 Docker 的同学有没有联想到 `makefile`？

  * push  
  这一步是最麻烦的。要做到 Travis CI 向 Pages repo 自动推送就必须用到 Github SSH Key. 但是如果直接放置 SSH 私钥在 Dev repo 中，等于向所有人开放了代码仓库的提交权限！  
  没有一点点防备，也没有一丝顾虑，你就这样出现在我的世界里，带给我惊喜——大概就会出现这种状况。  
  这不符合程序员的严谨美学（即使这个项目除了自己外根本无人 care)。  
  我们要把私钥加密并上传到 Travis CI. 然后会得到一个加密过得公钥和一段解密脚本。这个公钥只能被 Travis CI 解密，所以可以放心地把公钥放置于 Dev repo 中。 
  在 `.travis.yml` 中添加解密公钥、SSH 加密 push 等步骤的脚本。
  
4. View the pages  
  最有一步没什么好讲的，就是打开新生成的博客查看效果。

整个工作流的构想大致就是这样。

## 目标
---

看到有调查，互联网用户在单个页面的平均停留时间不到 20 秒钟。  
我觉得 20 秒都已经是乐观估计。“太长不看”才是主流。  
前文 Blahblah 写了这么多，不知道还有没有人能看到这里。

最近工作和生活遇到的一些事情，让我萌生这样一个想法——做正确的事情，大多比努力做事更有效。  
那什么才是做正确的事，或者说如何在合适的时间做合适的事？

这是个问题。  
问题很难直接回答，但系统的、科学的思维模式将帮助我们离答案近一点。

本文着重记录了持续集成的模式，与工作流的规划。  
希望这些系统性的模式能对坚持看到这里的小伙伴有所帮助，那再好不过了。

顺便推荐下前一篇文章里提到的， Coursera 的招牌课程之一—— Michigan 大学的 [Model Thinking 模型思维](http://v2cc.github.io/2015/08/20/course-notes-of-model-thinking/)。深入浅出的讲解了众多优秀的模型及其应用。涉及社会心理学、统计学等多个学科，但又对各科专业知识要求不高。总之就是非常涨姿势。

扯太远了，实现代码请参看下一篇：[Travis CI 自动化构建 Hexo (二)](http://v2cc.github.io/2015/09/03/unbelievable-workflow-autodeploy-hexo-by-travis-2/)。