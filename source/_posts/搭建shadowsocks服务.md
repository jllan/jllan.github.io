---
title: 搭建shadowsocks服务
date: 2016-08-25 13:44:43
tags: 
- linux
- shadowsocks
categories: IT技术
---

------

由于vpn是全局代理，如果开了vpn，所有网络都要走代理通道，这样的话要是访问国内的网站就会很慢，用起来挺不方便的。而shadowsocks可以自己决定让哪些网站走代理通道，所以还是shadowsocks用起来更爽，下边记录下在ubuntu上搭shadowsocks服务的步骤。
***
### 1. 安装shadowsocks
```
sudo apt-get install python-gevent python-pip
sudo pip install shadowsocks
```
如果这一步提示缺什么包，就安装上
***
### 2. 配置shadowsocks
编辑/etc/shadowsocks.json，如果没有该文件就手动创建
内容如下
```
{
    "server":"server_ip",
    "server_port":8989,
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"password",
    "timeout":300,
    "method":"aes-256-cfb",
}
```
server,server_port,password需要根据你的情况修改
***
### 3. 启动服务
```
sudo ssserver -c /etc/shadowsocks.json -d start
```
如果没有意外，服务就启动成功了
***
### 4. 加入开机自动启动
把命令`ssserver -c /etc/shadowsocks.json -d start`加入/etc/rc.local文件中，写在exit 0之前。
***
### 5. 客户端使用
- windows电脑
下载[shadowsocks windows客户端](https://github.com/shadowsocks/shadowsocks-windows/releases)，然后chrome浏览器装SwitchyOmega插件，SwitchyOmega配置方法如下图所示
![shadowsocks配置1.png](http://oceas72q5.bkt.clouddn.com/shadowsocks%E9%85%8D%E7%BD%AE1.png)
![shadowsocks配置2.png](http://oceas72q5.bkt.clouddn.com/shadowsocks%E9%85%8D%E7%BD%AE2.png)

- linux电脑
安装shadowsocks-qt5
`sudo apt-get install shadowsocks-qt5`
shadowscoks-qt5配置如下图所示
![shadowsocks-qt5配置.png](http://oceas72q5.bkt.clouddn.com/shadowsocks-qt5%E9%85%8D%E7%BD%AE.png)
然后，chrome浏览器安装SwitchyOmega插件，SwitchyOmega配置方法如上图所示

- 安卓
下载[影梭](https://github.com/shadowsocks/shadowsocks-android/releases)，配置方法如下
![406670196.jpg](http://oceas72q5.bkt.clouddn.com/%E5%BD%B1%E6%A2%AD%E9%85%8D%E7%BD%AE.jpg)

***
### 6. 可能出现问题
我在使用过程中出现了google play,google map可以顺利连接，但是手机和电脑上的浏览器都无法通过ss上网的问题，首先这个不是配置问题（认证方式是正确的），因为之前我的手机和电脑浏览器都是可以顺利连接的。查看日志`/var/log/shadowsocks.log`，有提示`unsupported addrtype 108, maybe wrong password or encryption method`，搜索了一番，别人也出现过这问题，但没找到解决方法。后来我把windows上的客户端从[shadowsocks-qt5](https://github.com/shadowsocks/shadowsocks-qt5)换成[shadowsocks-windows](https://github.com/shadowsocks/shadowsocks-windows)，然后竟然可以了。安卓手机上浏览器换成chrome竟然也可以了，具体原因不明。
