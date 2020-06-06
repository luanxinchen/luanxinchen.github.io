---
title: 可能是目前最好用的L2TP VPN解决方案：SoftetherVPN
top: false
cover: false
toc: true
mathjax: true
date: 2019-10-24 16:35:19
photos: [
    [http://ww1.sinaimg.cn/large/005DqEfjgy1g8afapquqmj30sg0aggmq.jpg]
]
tags: [softether,VPN,网络,运维]
categories: [VPN,运维]
---

> 提到VPN，可能大部分人联想到的是翻墙、爬梯子等关键词，这里需要先纠正一下，VPN并不等于翻墙，VPN的全称是Virtual Private Network（[虚拟专用网络](https://zh.wikipedia.org/wiki/%E8%99%9B%E6%93%AC%E7%A7%81%E4%BA%BA%E7%B6%B2%E8%B7%AF)），它是一种基于隧道协议的网络服务，常用于连接中、大型企业或团体与团体间的私人网络。国内由于GFW的存在，早期许多公司采用VPN服务连接国外网络的翻墙方法，后商业化的一键VPN逐渐成熟，久而久之，许多人便把VPN与翻墙画上了等号，实际这是一种错误的理解。

公司早先为了方便异地办公向员工提供了VPN服务，使用的方案是路由设备自带的PPTP+L2TP服务，随着的使用VPN的员工逐渐增加，这种方案的缺点也逐渐暴露出来：

<!-- more -->

* 设备支持的用户数量十分有限
* 路由设备的性能负载受VPN连接数影响
* 用户管理功能薄弱，没有详细的日志记录功能
* 自带的L2TPoveripsec协议对MAC、IOS用户不友好

几经筛选，终于找到了最符合公司需求的一款开源VPN解决方案，也就是我们今天的主角：[SoftetherVPN](https://www.softether.org/)。

*SoftEtherVPN是一个由日本筑波大学研究生Daiyuu Nobori（登大游）因硕士论文而开发的开源、跨平台、多重协议的虚拟专用网方案，此方案让一些虚拟专用网协议像是SSL、VPN 、L2TP、IPsec、OpenVPN以及微软SSTP都由同一个单一VPN服务器提供。在2014年1月4日以GNU通用公共授权条款的方式转变为开源软件。*

![1.2.jpg](http://ww1.sinaimg.cn/large/005DqEfjgy1g8ab3gmkjbj317m148aut.jpg)

从当前的使用情况来看，它可能是目前最好用的L2TP VPN解决方案：
* 支持SSL、L2TP、IPsec、OpenVPN、MS-SSTP等多种协议
* 支持radius、AD域等多种认证方式，方便用户统一管理
* 完善的日志记录功能，可记录数据包头和载荷信息
* 支持Windows、MAC环境下的图形化管理软件
* 支持集群部署高可用方案
* 开源项目，拥有较为活跃的社群
等等

有点啰嗦了，进入正题。

## Server端部署

### 下载

#### 二进制安装
进入[Softether下载发布页](https://www.softether-download.com/cn.aspx)，组件选择VPN Server，系统选择linux（这里博主使用的服务器是Centos 7.2环境），在下方的列表里选择一个稳定版本复制链接，下载到本地后使用FTP工具传输到服务器，或在服务器环境使用`curl` `wget`等命令直接下载。

#### 源码Build
查看[Github项目](https://github.com/SoftEtherVPN/SoftEtherVPN/)

### 部署
下载完成后解压tar包，进入vpnserver目录，执行`make`开始部署
```
[root@localhost vpnserver]# tar zxvf softether-vpnserver-v4.29-9680-rtm-2019.02.28-linux-x64-64bit.tar.gz
[root@localhost vpnserver]# cd vpnserver/
[root@localhost vpnserver]# make
```
*执行make时报错按照提示安装依赖即可*

### 启用
```
[root@localhost vpnserver]# ./vpnserver start #启动服务
[root@localhost vpnserver]# ./vpnserver stop #关闭服务
[root@localhost vpnserver]# ./vpncmd #进入命令行配置菜单
```
*启动后默认侦听443、992、1194、5555端口，请注意防火墙放行或关闭防火墙*

## ServerManager的使用

