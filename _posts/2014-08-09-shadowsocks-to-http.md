---
layout: post
title: "Ubuntu 下使用 Shadowsocks + Supervisor + Privoxy 配置稳定的客户端代理"
date: 2014-08-09 20:10:33 +0800
author: "Steve"
catalog: true
tags:
- shadowsocks
- supervisor

---

##  写在前面：

因为一些众所周知的原因，国外众多优秀服务在中国访问起来总需要借助梯子的帮助。 
最近一直在使用 Shadowsocks 作为梯子，这里简单介绍下其有点。

1. Shadowsocks 在 Win、Mac、Linux、Android、IOS 各平台都能使用。 
2. 相比与 VPN，影梭（Shadowsocks）在移动设备上更节约电力。 VPN 会使设备保持数据长连接，移动设备上网络连接不能按需中断，因此耗电高。 
3. 具有多种加密方式。

这里整理了下，在 Ubuntu 上如何借助 Shadowsocks + Supervisor + Privoxy 打造稳定梯子环境。

```
Supervisor 能方便的管理服务的自启动。
由于 Linux 上没有类似其它平台的 Shadowsocks Gui 客户端，所以每次使用要手动启动代理服务，非常麻烦。
Supervisor 的作用就是自动启动  Shadowsocks 代理服务。

Privoxy 是一个智能代理切换软件。Shadowsocks 提供的是 Sock5 代理，而例如 Dropbox 等软件不支持 Socks5 代理，只支持 HTTP 代理。
我们就需要用到 Privoxy，转发 Sock5 为更普遍的 HTTP代理。

```

##  准备工作：

1. 一台部署有 Shadowsocks 的境外代理服务器。可以自己花钱买一个最低配的，一个月30块钱就能畅游全球互联网。
如果直接购买了别人配置好的代理使用，可以直接看下一步， *安装步骤* 。 
2. 配置好境外代理服务器上的代理服务，只需按照下面的 *安装步骤* 中 Shadowsocks 配置、Supervisor 配置以及启动代理的步骤即可。
其中 Supervisor 配置中 sslocal 替换为 ssserver 即可。 
3. 本地系统为 Ubuntu 14.04，验证通过。

##  安装步骤：

安装代理工具 Shadowsocks-python 版和后台进程管理器 Supervisor 来设置代理和管理服务的自启动。
然后通过 Privoxy 来将 Shadowsocks 的 Socket 转换成 HTTP 代理。

	sudo apt-get update
	sudo apt-get install python-pip python-m2crypto supervisor privoxy
	sudo pip install shadowsocks

##  相关配置：

#### Shadowsocks配置：

	sudo mkdir -p /etc/shadowsocks
	sudo gedit /etc/shadowsocks/config.json

config.json配置内容为：

```
{
	"server":"bot02.rom.mk", //（你的代理服务器IP或域名地址）
	"server_port":8388, //（代理服务器上监听的端口号）
	"local_port":1080, //（本地的监听端口号）
	"password":"xiaobaibiechao", //（代理服务器的密码）
	"timeout":400,
	"method":"aes-256-cfb" //（加密方式）
}
```

#### Supervisor配置：

	sudo mkdir -p /etc/supervisor/conf.d
	sudo gedit /etc/supervisor/conf.d/shadowsocks.conf

shadowsocks.conf配置内容为：

```
[program:shadowsocks]
command=sslocal -c /etc/shadowsocks/config.json
autorestart=true
user=nobody
```

####  Privoxy配置：

	sudo vi /etc/privoxy/config

内容最末尾另起一行输入 

    forward-socks5 / 127.0.0.1:1080 .

`forward-socks5` 代表转发到 Socks5 代理，`/` 代表所有的 URL 都转发，
`127.0.0.1:1080` 是 Socks 代理的位置，最后的一点 `.` 代表没有 HTTP 代理，具体请上官网查阅。  
保存即生效。

现在的 `127.0.0.1:8118` 就是一个HTTP 代理了。`8118` 是 Privoxy 默认的端口。

####  启动代理和相关服务：

	sudo service privoxy restart
	sudo service supervisor restart
	sudo supervisorctl reload

最后在软件中设置代理使用即可。

##  写在最后：

本文为参考了 Martincz 在 MoKee OpenSource Project 原创首发的 [使用Shadowsocks+Supervisor+Privoxy打造Ubuntu下稳定的Android源码同步环境](http://www.mokeedev.com/2014/07/23/syncandroidsource/)。

再提供一篇对各梯子介绍比较详细的文章，供大家参考： [目前热门科学上网方式介绍及优缺点简评](http://cokebar.info/archives/236)。
