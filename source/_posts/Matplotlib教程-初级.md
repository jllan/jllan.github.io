---
title: Matplotlib教程-初级
date: 2017-03-23 11:42:00
categories: IT技术
tags: 
- python
- matplotlib
---
[TOC]

## 1. 画图——基础

了解图的基本组成部分
```python
%matplotlib inline # 在当前页面输出图像而不是打开一个新的页面

from matplotlib import pyplot as plt

x = [1,2,3,1]
y = [1,3,0,1]

plt.plot(x, y)

plt.title('Triangle')
plt.ylabel('Y axis')
plt.xlabel('X axis')

# plt.xticks([1,6]) # x轴的刻度是1和6
# plt.yticks([1,6])
# plt.xticks([1,6],['a','b']) # x轴的刻度是a和b

# plt.ylim([-1,4]) # x轴的范围是-1～4
# plt.xlim([-1,4])

# plt.grid(True,color='r') # 显示网格，网格根据ticks来画
plt.show()
```
![output_2_0.png](http://upload-images.jianshu.io/upload_images/1713353-e2dbe2bc90f3fe17.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 2. 画图——样式

数据线的样式设置

    颜色缩写    全称
    b          blue
    c          cyan
    g          gree
    k          black
    m          magenta
    r          red
    w          white
    y          yellow
   
   
    线型缩写    含义
    --         --虚线
    -.         -.虚线
    :          .虚线
    -          实线
    
    marker类型参考 
    http://www.labri.fr/perso/nrougier/teaching/matplotlib/#line-properties

```python
from matplotlib import pyplot as plt
import numpy as np

x = [1,2,3,1]
y = [1,3,0,1]

# np.array将列表转换成numpy的数组后可以支持广播broadcast运算
plt.plot(x, y, color='r', linewidth=2, linestyle='--', marker='D', label='one')
plt.plot(np.array(x)+1, np.array(y)+1, color='g', linewidth=3, linestyle=':', marker='o', label='two')
plt.plot(np.array(x)+2, np.array(y)+3, color='k', linewidth=1, linestyle='-.', marker='>', label='three')
plt.plot(np.array(x)+3, np.array(y)+4, 'bo-.', label='forth') # 可以把color,maker,linestyle写到一起

plt.grid(True)
plt.legend()
# plt.legend(loc='upper left') # 图例位置

plt.show()
```

![output_5_0.png](http://upload-images.jianshu.io/upload_images/1713353-a8e2b61df898b8c7.png)


```python
from matplotlib import pyplot as plt
import numpy as np

x = [1, 2, 3, 1]
y = [1, 3, 0, 1]

x2 = np.array(x) + 1
y2 = np.array(y) + 1

plt.plot(x, y, 'g', label='第一个', linewidth=5)
plt.plot(x2, y2, 'c', label='第二个', linewidth=5)

plt.title('两个三角形')
plt.ylabel('Y轴')
plt.xlabel('X轴')

plt.legend()

plt.show()
```

![output_6_0.png](http://upload-images.jianshu.io/upload_images/1713353-fc8e3b0659ac5380.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## 3. 画图——公式TeX, 文本和标注支持


```python
import matplotlib.pyplot as plt
# fig = plt.figure() 
fig = plt.figure(figsize=(10, 6)) # 图像大小
ax= fig.add_subplot(111)
ax.set_xlim([1, 6]);
ax.set_ylim([1, 10]);
ax.text(2, 8,  r"$ \mu \alpha \tau \pi \lambda \omega \tau \
    lambda \iota \beta $",color='r', fontsize=12);
ax.text(2, 6, r"$ \lim_{x \rightarrow 0} \frac{1}{x} $", fontsize=20);
ax.text(2, 4, r"$ a \ \leq \ b \ \leq \ c \ \Rightarrow \ a \
    \leq \ c$", fontsize=20);
ax.text(2, 2, r"$ \sum_{i=1}^{\infty}\ x_i^2$", fontsize=20);
ax.text(4, 8, r"$ \sin(0) = \cos(\frac{\pi}{2})$", fontsize=20);
ax.text(4, 6, r"$ \sqrt[3]{x} = \sqrt{y}$", fontsize=20);
ax.text(4, 4, r"$ \neg (a \wedge b) \Leftrightarrow \neg a \
    \vee \neg b$");
ax.text(4, 2, r"$ \int_a^b f(x)dx$", fontsize=20);
ax.text(3, 9, '公式样例', fontsize=20);
plt.show()
```
![output_8_0.png](http://upload-images.jianshu.io/upload_images/1713353-addc97e76a4bc7d2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Annotate各种格式可参考文档

http://matplotlib.org/users/annotations_guide.html#annotating-with-arrow


```python
import matplotlib.pyplot as plt

plt.figure(1, figsize=(3,3))
ax = plt.subplot(111)

ann = ax.annotate("Test",
                  xy=(0.2, 0.2), # 箭头的位置
                  xytext=(0.8, 0.8), # 'test'文本的位置
                  size=20, va="center", ha="center",
                  bbox=dict(boxstyle="round4", fc="w"),
                  arrowprops=dict(arrowstyle="-|>",
                                  connectionstyle="arc3,
                                  rad=0.2", # 箭头弧度
                                  fc="r"), 
                  )
ax.grid(True)
plt.show()
```
![output_10_0.png](http://upload-images.jianshu.io/upload_images/1713353-a13450639eb2a140.png)



## 4. 画图——多子图结构

<table class="table table-bordered">
<tr><td>1</td><td>2</td></tr>

<tr><td>3</td><td>4</td></tr>
</table>


```python
from matplotlib import pyplot as plt
import numpy as np

# arange类似python里的range
t = np.arange(0, 5, 0.2)

fig = plt.figure()#figsize=(10,6)

#行, 列, 序号
ax1 = fig.add_subplot(331)  # 把fig分成3*3，第一个位置
ax2 = fig.add_subplot(332)
ax4 = fig.add_subplot(333)
ax5 = fig.add_subplot(323)  # 把fig分成3*2，第3个位置
ax6 = fig.add_subplot(324)
ax3 = fig.add_subplot(313)  # 把fig分成3*1，第3个位置

ax1.plot(t, t, 'r--', t, t**2, 'bs', t, t**3, 'g^')
ax1.grid(True)
ax1.set_title('ax1') # 子图里设置标题用set_title

ax2.semilogy(t, t, 'r--', t, t**2, 'bs', t, t**3, 'g^') # semilogy: y轴使用对数刻度
ax2.grid(True)
ax2.set_title('ax2')

ax3.loglog(t, t, 'r--', t, t**2, 'bs', t, t**3, 'g^')  # loglog: x,y轴都使用对数刻度
ax3.grid(True)
ax3.set_title('ax3')

ax4.loglog(t, t, 'r--', t, t**2, 'bs', t, t**3, 'g^')
ax4.grid(True)
ax4.set_title('ax4')

ax5.loglog(t, t, 'r--', t, t**2, 'bs', t, t**3, 'g^')
ax5.grid(True)
ax5.set_title('ax5')

ax6.loglog(t, t, 'r--', t, t**2, 'bs', t, t**3, 'g^')
ax6.grid(True)
ax6.set_title('ax6')

# fig.suptitle('normal vs ylog vs loglog')
fig.subplots_adjust(hspace=0.8) # 子图的垂直间隔

plt.show()
```
![output_13_0.png](http://upload-images.jianshu.io/upload_images/1713353-125745909f8f179f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)