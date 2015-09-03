---
title: 探索工作流：Travis CI 自动化构建 Hexo (二)
date: 2015-09-03 13:14:00
categories:
- flow
tags:
- github
- travis
---

## 功能实现

思路理顺之后，实现起来就很简单了。每个步骤都能搜索到大量的资料。  
不打算一一复制，那样篇幅就太长了。  
依次列出详细步骤，并附上挑选出来的参考教程，小伙伴们只需要照着链接的教程 step by step，指哪儿点哪儿就可以了。

1. 准备 Dev repo 与 Pages repo.

  `username.github.io` 下添加分支 `blog`, 同时登陆 Travis CI 开启 `username.github.io` 源的同步开关，设置中也要启用 `Build only if .travis.yml is present` 项。  
  清空该分支内所有文件，添加 hexo 框架所需的基础文件，包括主题、日志等，并提交。  
  关于如何建立 hexo 框架的博客，可参考官网教程：[DOCS](https://hexo.io/docs/).

2. 生成SSH Key.

  参看 Github 官网教程操作即可：[Generating SSH keys](https://help.github.com/articles/generating-ssh-keys/).  
  需要注意的是，这个 SSH key 不应成为你账号的全局 SSH key（因为这样 Travis CI 就获得了你所有代码库的提交权限，这是不严谨的），而应该添加至 `https://github.com/username/username.github.io/settings/keys` ，这样能更好的限制 Travis CI 的提交权限。  
  这步最后我们得到了 `id_rsa.pub` 和 `id_rsa` 两个秘钥，`id_rsa` 是我们的之后要用到的私钥，保管好它。

3. 配置加密私钥的环境。

  操作需要事先配置好 Ruby 和 gem 环境，参看官网教程：[Installing Ruby](https://www.ruby-lang.org/en/documentation/installation/), [Download RubyGems](https://rubygems.org/pages/download).  
  不想手动配置的话，推荐使用在线IDE应用，例如 Cloud9. 注册账号后创建一个 gem 环境的虚拟主机即可在线操作。  

  在该 gem 环境下 git 到你的 Dev repo.
  ```bash
  git clone https://github.com/username/username.github.io.git
  git checkout blog
  ```

  将上一步得到的 `id_rsa` 复制到 Dev repo 根目录下。  
  创建 `.travis.yml` 的文件，内容为空即可。

4. 加密私钥并上传至 Travis CI.
  
  命令行工具中执行以下命令：  
  ```bash
  # 安装Travis CI的命令行工具
  gem install travis
  # 使用 Github 账号登陆 Travis CI
  travis login --auto
  # 加密私钥并上传至Travis
  travis encrypt-file id_rsa --add
  ```

  这一步 Travis 的命令行工具会生成加密过得新秘钥 `id_rsa.enc`, 并自动将 Dev repo 的 git 信息及解密秘钥的相关信息添加到 `.travis.yml` 中。  
  然后手动删除私钥文件 `id_rsa`， 以保证代码仓库的安全。

5. 配置 push 时的 SSH.
  修改 `.travis.yml` 中自动插入的解密指令(不要照抄，注意修改密钥)，来指定解密后的 `id_rsa` 生成位置。其中 `xxxxxxxxxx` 部分就是你的解密参数，不要去改动它。
  
  ```bash
  - openssl aes-256-cbc -K $encrypted_xxxxxxxxxx_key -iv $encrypted_xxxxxxxxxx_iv
    -in travis.enc -out ~/.ssh/id_rsa -d
  ```

  当前目录下新建文件 ssh_config，内容如下。

  ```bash
  Host github.com
    User git
    StrictHostKeyChecking no
    IdentityFile ~/.ssh/id_rsa
    IdentitiesOnly yes
  ```

6. 完善 Travis CI 的脚本文件 `.travis.yml`.

  下面对 `.travis.yml` 文件各块添加了注释。有几个地方必须要修改：  
  两处 `xxxxxxxxxx` 修改为你之前获得的解密参数；`你的姓名` 和 `你的邮箱` 最好与你 Github 上的信息保持一致。  
  同时附上本博客的 `.travis.yml` 源文件，以供下载：[.travis.yml](https://raw.githubusercontent.com/v2cc/v2cc.github.io/blog/.travis.yml).

  ```bash
  # 指定环境
  language: node_js
  
  node_js:
  - '0.12' //指定使用 node.js 最新的稳定版0.12
  
  # 指定分支
  branches:
    only:
    - blog //这个分支应当使用自己的 Dev repo
  
  before_install: 
  - openssl aes-256-cbc -K $encrypted_xxxxxxxxxx_key -iv $encrypted_xxxxxxxxxx_iv //注意将xxxx内容修改为你之前获得的解密参数
    -in id_rsa.enc -out ~/.ssh/id_rsa -d
  - chmod 600 ~/.ssh/id_rsa //修改目录权限
  - eval $(ssh-agent)//将密钥加入系统
  - ssh-add ~/.ssh/id_rsa
  - cp ssh_config ~/.ssh/config //修改 git 信息
  - git config --global user.name "你的姓名"
  - git config --global user.email 你的邮箱
  
  # 配置 Hexo
  install:
  - npm install hexo-cli -g
  - npm install
  - npm install hexo-generator-feed --save
  - npm install hexo-deployer-git --save
  
  # 执行 Hexo 的编译操作
  script:
  - hexo clean
  - hexo g
  - hexo d
  ```

7. Push 到 Dev repo

  将本地源的所有改动 push 到 Github 上的 Dev repo.  
  此时就能在 Travis CI 页面上看到，push 触发了自动构建。  
  如果构建成功，就能在自己的 pages 上查看刚生成的博客了；如构建失败，Travis CI 会显示出哪步脚本导致了构建失败，本地源里修改它，然后再次 push 即可。

整个工作流已经完整了。之后对 Dev repo 所有的改动，Travis CI 都会自动帮我们构建并发布它。

## 小结

总结下来本 case 要点有二：

* 配置 `.travis.yml` 脚本，使 Travis CI 自动构建和测试 Github 上的代码。
* 添加加密 SSH Key, 使 Travis CI 自动部署构建好的项目。

PS：
注意到 Travis CI 的标语其实特别有意思—— Test and Deploy Your Code with Confidence.