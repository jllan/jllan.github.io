---
title: github用户主页不显示contributions记录的问题
date: 2017-03-14 23:41:24
categories: IT技术
tags:
- git
- github
---
## 缘起
一直纠结为啥自己的github个人主页不显示contributions记录中的commit记录，而只显示了create repo的记录。看着那寥寥无几的绿方格，想着自己好歹也经常commit呢，这连个鼓励都看不到好伤心，今天忍不住在网上查了下，终于找到问题原因。

## 原因
原来是因为commit时的邮箱不知道什么时候被修改了（难道是我自己git config时不小心设置错了），然后这个被篡改的邮箱没有被关联到我的github，因此github认为这不是我自己提交的，因此不显示提交记录。

## 解决方法
解决方法有两个
### 1. 添加这个邮箱到自己的github账户中
在[设置邮箱](https://github.com/settings/emails)添加邮箱就行了。

### 2.修改提交记录中的邮箱为已被关联的邮箱
1. clone你需要修改commit记录的repo
```
git clone --bare https://github.com/user/repo.git
cd repo.git
```
2. 复制以下代码，建立script.sh文件（文件名随便），并根据你的信息修改以下变量：**旧的Email地址**(就是commit记录中的那个)，**正确的用户名**，**正确的邮件地址**
```
#!/bin/sh
git filter-branch --env-filter '
OLD_EMAIL="旧的Email地址"
CORRECT_NAME="正确的用户名"
CORRECT_EMAIL="正确的邮件地址"
if [ "$GIT_COMMITTER_EMAIL" = "$OLD_EMAIL" ]
then
    export GIT_COMMITTER_NAME="$CORRECT_NAME"
    export GIT_COMMITTER_EMAIL="$CORRECT_EMAIL"
fi
if [ "$GIT_AUTHOR_EMAIL" = "$OLD_EMAIL" ]
then
    export GIT_AUTHOR_NAME="$CORRECT_NAME"
    export GIT_AUTHOR_EMAIL="$CORRECT_EMAIL"
fi
' --tag-name-filter cat -- --branches --tags
```
然后执行script.sh
3. 把正确历史 push 到 github
```
git push --force --tags origin 'refs/heads/*'
```
4. 删掉你clone的repo

至此就可以了，刷新github主页，没有意外就可以看到全部提交记录了。

## 总结
不显示commit记录的原因还有其他的，我这个只针对邮箱错误的原因，如不是这个原因请参考[https://help.github.com/articles/why-are-my-contributions-not-showing-up-on-my-profile/](https://help.github.com/articles/why-are-my-contributions-not-showing-up-on-my-profile/)

## 参考
- [https://segmentfault.com/a/1190000004318632](https://segmentfault.com/a/1190000004318632)
- [http://www.cnblogs.com/qianyuqianxun/p/5381584.html](http://www.cnblogs.com/qianyuqianxun/p/5381584.html)
