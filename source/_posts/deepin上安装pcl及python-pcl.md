---
title: deepin上安装pcl及python-pcl
date: 2016-11-07 13:36:57
tags: 
- linux
- python
- pcl
categories: Linux
---

[TOC]

# 一 从预编译版本或从仓库安装pcl
## 1. 安装pcl
- ### 使用官方提供的预编译版本
地址
[http://www.pointclouds.org/downloads/linux.html](http://www.pointclouds.org/downloads/linux.html)

```
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-key 19274DEF
sudo echo "deb http://ppa.launchpad.net/v-launchpad-jochen-sprickerhof-de/pcl/ubuntu maverick main" >> /etc/apt/sources.list
sudo apt-get update
sudo apt-get install libpcl-all
```
**问题**
`sudo apt-get install libpcl-all`时有依赖问题无法解决

- ### 从deepin软件仓库直接安装
```
sudo apt-get install libpcl1.7   # 也有其他版本
```
此方法安装成功

## 2. 安装python-pcl
地址
[https://github.com/strawlab/python-pcl](https://github.com/strawlab/python-pcl)
```
git clone https://github.com/strawlab/python-pcl
cd python-pcl
make 
sudo python setup.py install  
```
**问题**
make时失败，提示
> python setup.py build_ext --inplace
setup.py: error: cannot find PCL, tried
    pkg-config pcl_common-1.7
    pkg-config pcl_common-1.6
    pkg-config pcl_common
Makefile: 5:  recipe for target 'pcl/_pcl.so' failed
make: *** [pcl/_pcl.so] Error 1


---

# 二. 以源码编译方式安装pcl
## 1. 安装pcl
因为安装预编译过的或者从从仓库中安装pcl后，都存在问题，所以从源码编译安装试试
**参考**
[http://blog.csdn.net/u014222645/article/details/45232917](http://blog.csdn.net/u014222645/article/details/45232917)

[http://blog.csdn.net/zhuquan945/article/details/52809064](http://blog.csdn.net/zhuquan945/article/details/52809064)

### 1. 下载PCL源代码
```
$ git clone https://github.com/PointCloudLibrary/pcl.git 
```
### 2. 安装相关的库
```
$ sudo apt-get install  libboost-all-dev libeigen3-dev libflann-dev python libusb-1.0-0-dev libudev-dev freeglut3-dev doxygen graphviz libpng12-dev libgtest-dev libxmu-dev libxi-dev libpcap-dev libqhull-dev libvtk5-qt4-dev python-vtk libvtk-java
```
**注意**：libflann版本要大于1.7，libvtk5-qt4-dev和 libpng12-dev可能会有冲突

### 3. 编译pcl源码
```
cd pcl
mkdir build & cd build
cmake  -D CMAKE_BUILD_TYPE=None  -D BUILD_GPU=ON  -D BUILD_apps=ON  -D BUILD_examples=ON ..   # 如果这一步提示缺什么就再装什么
make
sudo make install
```

## 2. 安装python-pcl
```
git clone https://github.com/strawlab/python-pcl
cd python-pcl
make 
sudo python setup.py install  
```
**注意**：
make时如果出现如下错误
> Package pcl_2d-1.8 was not found in the pkg-config search path.
Perhaps you should add the directory containing pcl_2d-1.8.pc' to the PKG_CONFIG_PATH ......

需要修改` /usr/local/lib/pkgconfig/pcl_features-1.8.pc`文件，删除第10行的`pcl_2d-1.8 `

参考[https://github.com/strawlab/python-pcl/issues/97](https://github.com/strawlab/python-pcl/issues/97)