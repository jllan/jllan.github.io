---
title: 在github上搭建个人博客
date: 2016-08-20 15:14:04
tags: hexo
categories: Hexo
---


之前在vps上用wordpress搭建的博客被我拆了，虽然平时也很少写东西，即使写了质量也不高，但是一下子没有了自己的博客还是挺不爽的，于是就研究了两天，终于在github上重新搭了一个。
***

### 搭建步骤

1. 随便建一个文件夹用来存放hexo相关的文件，如myblog
2. 安装nodejs和npm
```
sudo apt-get install nodejs-legacy
sudo apt-get install npm
```
3. 进入myblog，安装hexo
``` 
sudo npm install -g cnpm --registry=https://registry.npm.taobao.org #设置淘宝镜像
sudo npm install hexo-cli
```
4. 初始化
```
hexo init
```
5. 配置_config.yml，安装主题，我装的是[cover](https://github.com/daisygao/hexo-themes-cover)，然后配置主题。
6. 
```
hexo generate # 生成内容
```
7. 发布，首先在github上建立一个仓库，名字格式为:youname.github.io，然后配置_config.yml
```
npm install hexo-deployer-git
hexo server # 在本地启动服务
hexo d #部署到github上
```
***

### 写文章，发布文章的流程
1. 
```
hexo new 'topic' # 生成topic,md
```
2. 编写文章
3. 
```
hexo g
hexo clean
hexo d
```
