---
title: 在github上搭建个人博客（进阶）
date: 2016-08-20 16:12:32
tags: hexo
categories: Hexo
---


在github上搭建博客，博客分为两部分东西：本地的hexo和github上仓库。工作模式是这样的：
1. 在本地通过hexo new来生成md文件（所有文章的md文件都在source/_post/下）
2. 然后编辑md文件
3. 然后hexo g渲染出html文件
4. 最后hexo d来部署文章到github仓库
这样就会有个问题，如果哪天换电脑了，本地的hexo文件就没有了，所有文章的md文件也没有了。解决这个问题最笨的方法就是时刻备份本地的hexo的一堆文件，换电脑了，就把这堆文件拷到新电脑上，然后就可以继续写文章，发布文章了。但是这个方法极不方便，另外一个很高明的方法就是把本地这一堆hexo文件也放在github上。

### 具体搭建步骤如下
1. 在github上建仓库yourname.github.io，为youname.github.io建立一个分支hexo用来存放hexo的文件，master分支用来存放发布的文章，并设hexo为默认分支，好像空仓库没法建立分支，可以先随便写入一个文件，然后就可以创建分支，设立默认分支了。
2. git clone yourname.github.io仓库到本地，当前在hexo分支。
3. 执行npm install hexo-cli,hexo init,npm install ,npm install hexo-deployer-git等命令安装hexo（**注意:**
*hexo init会清空当前目录，所以本地的.git目录就被删除了，所以要提前备份.git目录，hexo init后再把.git目录恢复到yourname.github.io*）
4. 配置hexo，安装配置主题等
5. git add,git commit, git push origin hexo提交该分支的文件到github上yourname.github.io仓库的hexo分支。.gitignore文件一般会由hexo自动生成，如果没有这个文件，可以手动添加，内容如下
```
.DS_Store
Thumbs.db
db.json
*.log
node_modules/
public/
```

这样就可以了，以后换电脑了，在新电脑上执行下2,3,4,5步就行了（**注意：**千万不要再执行hexo init了）
***

### 写文章，发布文章的流程
这种方式下，每次写完文章后，要把hexo的内容同步到github
1. `hexo new 'topic' # 生成topic.md`
2. 编写文章
3. git add -u, git commit, git push origin hexo把新写的文章同步到仓库，平时对hexo进行的配置也需要这样同步一下
4. hexo g,hexo clean,hexo d来发布文章

### 参考
http://crazymilk.github.io/2015/12/28/GitHub-Pages-Hexo%E6%90%AD%E5%BB%BA%E5%8D%9A%E5%AE%A2/
