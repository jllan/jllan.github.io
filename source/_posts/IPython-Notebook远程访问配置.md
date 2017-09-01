---
title: IPython Notebook远程访问配置
date: 2017-09-01 23:44:23
categories: IT技术
tags: python
---
## 1.安装IPython 与 IPython Notebook
```
pip3 install ipython
pip3 install ipython notebook
```
此时可以本地访问notebook。

## 2.创建登录密码
在服务器上启动IPython，生成自定义密码的sha1：
```
In [1]: from IPython.lib import passwd
In [2]: passwd()
Enter password:
Verify password:
Out[2]: 'sha1:................................' # 根据你的密码生成sha1值
```
记住自己输入的密码，并且记录下生成的加密字符串。

## 3.创建IPython notebook服务器
```
ipython3 profile create myserver
```
这里的myserver是自定义的服务器名字。
执行之后，命令行会有输出，告诉我们生成的文件在哪里。一般在/home/yourname/.ipython/profile_myserver/这个文件夹下。
我们可以进入到该文件夹下，并查看一下生成的文件：
一般没有问题的话，会生成**ipython_config.py**，**ipython_kernel_config.py**和**ipython_notebook_config.py**三个文件。
需要修改该**ipython_notebook_config.py**文件来配置服务器。不过，我测试的时候这个文件不能生成，直接手动创建即可。

## 4.修改ipython_notebook_config.py配置文件
在该文件中输入如下配置并保存:
```
c = get_config()
c.IPKernelApp.pylab = 'inline'
c.NotebookApp.ip='*'
c.NotebookApp.open_browser = False
c.NotebookApp.password = u'...........'  # 第2步生成的sha1值
c.NotebookApp.port = 6789 # 端口号，设置一个没被占用的
```

## 5.启动IPython notebook服务器
```
ipython notebook --config=/home/qiang/.ipython/profile_myserver/ipython_notebook_config.py
```
或者使用jupyter notebook命令，即：
```
jupyter notebook --config=/home/qiang/.ipython/profile_myserver/ipython_notebook_config.py
```
可以看到，该条命令启动了IPython Notebook服务器，并指向了我们刚刚编辑保存过的配置文件。
如果正常的话，我们会看到这样的输出:
`The Jupyter Notebook is running at: http://[all ip addresses on your system]:6789/`

访问`ip:6789`，然后输入第二步的密码就可以了。
![](http://upload-images.jianshu.io/upload_images/1713353-e4665b9697ba6669.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
