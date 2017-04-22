---
title: Ubuntu上搭建pptp服务
date: 2017-03-03 14:26:55
tags: linux
categories: IT技术
---

## 1. 安装PPTP
```
sudo apt-get install pptpd
```
## 2. PPTP相关配置
- 配置ip
`sudo vim /etc/pptpd.conf`
取消**localip**和**remoteip**的注释（192.168.0.1改为你的服务器固定IP）
```
localip 服务器ip
remoteip 192.168.0.234-238,192.168.0.245
```
- 配置DNS
`sudo vim /etc/ppp/pptpd-options`
取消**ms-dns**的注释，改成你喜欢的DNS，比如：8.8.8.8,8.8.4.4
```
ms-dns 8.8.8.8
ms-dns 8.8.4.4
```

## 3. 其他配置
- 开启内核IP转发
`sudo vim /etc/sysctl.conf`
取消**net.ipv4.ip_forward=1** 这一行的注释.
然后运行`sysctl -p` 让上面的修改立即生效

- 添加iptables转发规则
```
sudo apt-get install iptables # 安装iptables
iptables -t nat -A POSTROUTING -s 192.168.0.0/24 -j SNAT --to-source 服务器ip   # 设置iptables转发规则
```

## 4. 创建账号
`sudo vim /etc/ppp/chap-secrets`
在里面添加账户按如下格式
`username  pptpd  "password"  *`
 > username为你的用户名，password为你的密码，密码用引号引起,*号表示允许在任意IP连接到服务

## 5. 启动服务
```
chkconfig pptpd on  # 设置开机自动启动
chkconfig iptables on
sudo service pptpd restart  # 启动服务
```

至此全部OK。
