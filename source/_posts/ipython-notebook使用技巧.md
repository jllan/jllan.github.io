---
title: ipython notebook使用技巧
date: 2017-03-22 22:25:38
categories: IT技术
tags: python
---
1.  可以选择MarkDown，用MarkDown来写说明
2. 如果在函数、模块、类后面输入‘?’，按住Ctrl-Entry的话就回跳出帮助文档。如果是两个‘??’的话连,源代码都会给出来的。
3. 在网页中输入%matplotlib inline将matplotlib库导入，要显示的图片就可以嵌入到网页中了
4. 使用ipython nbconvert [.ipynb文件]命令来生成默认格式(html格式)还可以使用--to选项来转换为指定的格式，如：
```
ipython nbconvert --to latex mynotebook.ipynb
ipython nbconvert mynotebook.ipynb --to pdf
ipython nbconvert --to html --template basic mynotebook.ipynb
ipython nbconvert mynotebook.ipynb --to markdown
```
5. %load可以从文件或者网址载入代码到一个新的单元中，例如下面载入某个matplotlib的示例程序，并执行
```
%load http://matplotlib.org/mpl_examples/pylab_examples/histogram_demo.py
```
6. IPython中Magic命令有两种执行方式，以%开始的命令被称为行命令，它只对单行有效，以%%开头的为单元命令，它放在单元的第一行，对整个单元有效。
7. %prun用于代码的执行性能分析，可以作为行命令和单元命令使用

参考
- http://ipython.org/ipython-doc/1/interactive/nbconvert.html
- http://www.jianshu.com/p/0b7a834b2c1e