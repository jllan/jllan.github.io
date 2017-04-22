---
title: Linux源码编译软件安装和卸载方法
date: 2016-12-30 18:13:38
categories: IT技术
tags: linux
---

有时候没有现成的deb包或者rpm包的话，我们需要从源码编译来安装软件，下边介绍了源码编译安装的方法和卸载方法。
### 不指定安装路径

#### 安装
```
./configure && make && sudo make install
```

#### 卸载
不指定安装路径，软件就会按照Linux的标准目录配置[FHS](http://baike.baidu.com/link?url=zTBXXkkWwZUg8_3ZBsnRToh3w6O5tF9WwV8_F7Z59vmIcdULndFAz10riZZfNw4WFPjAcE2SvJYmr1WXZYzNIa)把文件分布到各个目录下，自己编译的软件一般在/usr/local/的各个目录里。因为文件不在同一个目录里，所以下载会比较麻烦。
- 方法一
如果大致记得软件的安装日期，比如两天内，可以先用find命令查找，然后删除
````
sudo find /usr/local/ -mtime -2 | grep packname | xargs sudo rm -rf
```
- 方法二
1. 首先指定安装目录把该软件重新安装一次
```
./configure --prefix=/tmp/packname/ && make && sudo make install
```
2. 然后遍历/tmp/packname/的文件，删除/usr/local下每个目录里对应的文件

这两种方法都不会卸载的太干净，不过一般也可以了，如果删除后需要修改什么可以灵活应对，比如多个版本的python，这样删除编译安装版本的话，一些指向这个版本的python可执行文件如pip可能救没法运行了，需要手动修改下pip

### 指定安装路径
#### 安装
```
./configure --prefix=/opt/packname/ && make && sudo make install
```
#### 卸载
直接删除`/opt/packname`

### 使用checkinstall来安装
checkinstall可以把源码包编译成deb包或rpm包

执行如下命令
```
./configure && make && sudo checkinstall
```
就会在当前目录生成一个deb包，然后安装或卸载很方便了。
