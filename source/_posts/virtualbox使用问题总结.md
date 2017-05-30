---
title: virtualbox使用问题总结
date: 2017-05-30 16:58:45
categories: IT技术
tags: 
- virtualbox
- linux
---
## 设置共享目录
### 1 安装功能增强包

选择virtualbox菜单"设备"->"安装增强功能"。

### 2 配置共享目录

![virtualbox设置共享目录.png](http://upload-images.jianshu.io/upload_images/1713353-6c7fd772f4c0c065.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 3 挂载
```
sudo mount -t vboxsf E_DRIVE /mnt/E    #手动挂载
sudo echo 'E_DRIVE /mnt/E vboxsf defaults 0 0' >> /etc/fstab    #设置开机自动挂载
```

### 4 问题
```
sudo mount -t vboxsf E /mnt/E/    #挂载点名和共享目录名一样
#这样会出现如下问题：
/sbin/mount.vboxsf: mounting failed with the error: Protocol error
```

原因是共享目录名和挂载点名相同，修改共享目录名就可以了
```
sudo mount -t vboxsf E_DRIVE /mnt/E/
```
这样就可以了。

***

## 使用usb设备
### 1. 安装[Oracle_VM_VirtualBox_Extension_Pack](http://download.virtualbox.org/virtualbox/5.1.22/Oracle_VM_VirtualBox_Extension_Pack-5.1.22-115126.vbox-extpack)

### 2. 设置->usb设备->添加筛选器

![深度截图20170530165225.png](http://upload-images.jianshu.io/upload_images/1713353-218513f6a54d6e77.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
如果没有可供选择的筛选器，需要在host进行如下操作：
```
sudo groupadd vboxusers
sudo groupadd usbfs
sudo adduser jlan vboxusers
sudo adduser jlan usbfs
```
然后重启电脑应该就可以啦。