---
title: numpy基础入门
date: 2017-04-21 09:50:46
categories: IT技术
tags: numpy
---
```python
import numpy as np
```

### 1. 矩阵的创建

```python
a=np.arange(1,5)
a=np.array([1,2,3,4,5])
print(a, a.dtype, a.shape, a.size, a.ndim)
```

    [1 2 3 4 5] int64 (5,) 5 1


- np.arange类似range函数
- np.array用来生成矩阵
- dtype是数据类型，有int64, complex, uint16等
- shape是个元组属性，表示每一维的宽度
- size是所有元素个数
- ndim是维数


```python
b=np.array([1,2,3],dtype='int') # int64, complex, uint16......
h = b.astype(np.float)  # 用另一种类型表示
print(b, b.dtype, h.dtype)
```

    [1 2 3] int64 float64



```python
m=np.array([np.arange(6),np.arange(6)])
print(m, m.shape, m.size)
```

    [[0 1 2 3 4 5]
     [0 1 2 3 4 5]] (2, 6) 12



```python
# 每一个[]代表一维，比如
# [[[1,2,3],[4,5,6]],[[7,8,9],[10,11,12]]], 代表矩阵的维度是(2,2,3)
# 其中第一个2，代表最外层的两个[]，第二个2代表第二层[]，第三个3代表最里层的维度。
n=np.array([[1,2,3,4],[5,6,7,8]])
print(n, n[0,2], n[1,1])

m=np.array([[[1,2,3],[4,5,6]],[[7,8,9],[10,11,12]]])
print(m.shape)
```

    [[1 2 3 4]
     [5 6 7 8]] 3 6
    (2, 2, 3)



```python
x=m.ravel()
y=n.flatten()
print(x)
print(y)
```

    [ 1  2  3  4  5  6  7  8  9 10 11 12]
    [1 2 3 4 5 6 7 8]


ravel()和flatten()看起来效果一样，都是把矩阵展平了。它们的区别在于
- ravel()返回的是原有数据的一个映射(view)，没有分配新的存储
- flatten()返回的是新的数据
因此如果我们改变它们的值，就可以看出区别

numpy还有一些函数有这样的区别，关键在于判断函数返回是原数据的映射还是返回新的数据。


```python
x[3]=10;y[3]=10
print(m)
print(n)
# 看看m，n哪个的值改变了
```

    [[[ 1  2  3]
      [10  5  6]]
    
     [[ 7  8  9]
      [10 11 12]]]
    [[1 2 3 4]
     [5 6 7 8]]



```python
# reshape返回一个view
x=m.reshape(3,4)
# resize直接在当前数据上更改，返回空
y=n.resize(3,4)
print(x,y,'\n',n)
```

    [[ 1  2  3 10]
     [ 5  6  7  8]
     [ 9 10 11 12]] None 
     [[1 2 3 4]
     [5 6 7 8]
     [0 0 0 0]]



```python
x[2]=10
print(x, m)
# 看看m和n哪个改变了，有什么区别
```

    [[ 1  2  3 10]
     [ 5  6  7  8]
     [10 10 10 10]] [[[ 1  2  3]
      [10  5  6]]
    
     [[ 7  8 10]
      [10 10 10]]]



```python
m=np.array([np.arange(6),np.arange(6)])
# copy()可以强制返回一个新的数据
x=m.reshape(3,4).copy()
x[2]=10;print(x);print(m)
#看看这次m的值随x改变而改变吗
```


```python
# linspace返回0,1之间的10个数据，等差数列
x=np.linspace(0,1,10)
print(x)
```

    [ 0.          0.11111111  0.22222222  0.33333333  0.44444444  0.55555556
      0.66666667  0.77777778  0.88888889  1.        ]



```python
a = np.arange(10)
np.random.shuffle(a)  # 随机排第一维
print(a)

a = np.arange(9).reshape((3, 3)) # 随机排第一维，想一想结果是什么
np.random.shuffle(a)
print(a)
```

    [1 5 0 6 4 7 3 2 9 8]
    [[3 4 5]
     [6 7 8]
     [0 1 2]]



```python
# 全0矩阵
a=np.zeros((3,3))
# 全1矩阵
b=np.ones((5,4))
# 单位矩阵
c=np.eye(3)
# 取对角元素
print(b)
np.diag(b) # np.diag(b) 返回b的对角线元素
```

    [[ 1.  1.  1.  1.]
     [ 1.  1.  1.  1.]
     [ 1.  1.  1.  1.]
     [ 1.  1.  1.  1.]
     [ 1.  1.  1.  1.]]
    [3 3 3 3]



```python
print(m.transpose()) # 转秩
print(m.T) # 转秩
```

    [[[ 1  7]
      [10 10]]
    
     [[ 2  8]
      [ 5 10]]
    
     [[ 3 10]
      [ 6 10]]]
    [[[ 1  7]
      [10 10]]
    
     [[ 2  8]
      [ 5 10]]
    
     [[ 3 10]
      [ 6 10]]]


### 2.索引


```python
a = np.arange(24).reshape((2, 3, 4))

# 用:表示当前维度上所有下标
c = a[:, 2, :]
d = a[:, :, 1]
e = a[:, 1:, 1:-1]

# 用...表示没有明确指出的维度
f = a[..., 1] # 等价于[:, :, 1]

print(a)
print(c)
print(d)
print(e)
print(f)
```

    [[[ 0  1  2  3]
      [ 4  5  6  7]
      [ 8  9 10 11]]
    
     [[12 13 14 15]
      [16 17 18 19]
      [20 21 22 23]]]
    [[ 8  9 10 11]
     [20 21 22 23]]
    [[ 1  5  9]
     [13 17 21]]
    [[[ 5  6]
      [ 9 10]]
    
     [[17 18]
      [21 22]]]
    [[ 1  5  9]
     [13 17 21]]


### 3. 矩阵的加法


```python
a = np.arange(9).reshape((3, 3))

## 每个元素的broadcast
print(a)
print(a+3)

## 行broadcast
print(a+np.arange(3))

## 列broadcast
print(a+np.arange(3).reshape(3,1))
```

    [[0 1 2]
     [3 4 5]
     [6 7 8]]
    [[ 3  4  5]
     [ 6  7  8]
     [ 9 10 11]]
    [[ 0  2  4]
     [ 3  5  7]
     [ 6  8 10]]
    [[ 0  1  2]
     [ 4  5  6]
     [ 8  9 10]]


broadcast广播操作，运算不仅仅作用在某个元素，而是作用在整个矩阵，或者整行，或者整列。
比如
 - n*m + 1*1 就是将1*1的元素作用在n*m的整个矩阵
 - n*m + n*1 就是将n*1的元素作用在n*m的每一列
 - n*m + 1*m 就是将1*m的元素作用在n*m的每一行
 - 乘法类似


```python
a = np.array([[1, 2], [3, 4]])
print(np.sum(a,axis=0))
print(np.sum(a,axis=1))
print(np.mean(a,axis=0))
print(np.mean(a,axis=1))
print(np.std(a,axis=0)) # 标准差

#axis=0，就是按第一个维度进行计算，行向量[1,2], [3,4]
#axis=1，就是按第二个维度进行计算，列向量[1,3], [2,4]
```

    [4 6]
    [3 7]
    [ 2.  3.]
    [ 1.5  3.5]
    [ 1.  1.]



```python
#再拓展到3维
a = np.arange(12).reshape((2,2,3))
print(a)
print(np.sum(a,axis=0)) #按第一维度加，结果为2*3矩阵, 可以理解为1*2*3
print(np.sum(a,axis=1))#按第二维度加，结果为2*3矩阵, 可以理解为2*1*3
print(np.sum(a,axis=2)) #按第三维度加，结果为2*2矩阵, 可以理解为2*2*1
```

    [[[ 0  1  2]
      [ 3  4  5]]
    
     [[ 6  7  8]
      [ 9 10 11]]]
    [[ 6  8 10]
     [12 14 16]]
    [[ 3  5  7]
     [15 17 19]]
    [[ 3 12]
     [21 30]]


### 4. 矩阵的乘法


```python
a = np.arange(12).reshape((6, 2))
b = np.arange(10).reshape((2,5))

# 两种矩阵乘法形式
print(a.dot(b))
print(np.dot(a,b))
```

    [[  5   6   7   8   9]
     [ 15  20  25  30  35]
     [ 25  34  43  52  61]
     [ 35  48  61  74  87]
     [ 45  62  79  96 113]
     [ 55  76  97 118 139]]
    [[  5   6   7   8   9]
     [ 15  20  25  30  35]
     [ 25  34  43  52  61]
     [ 35  48  61  74  87]
     [ 45  62  79  96 113]
     [ 55  76  97 118 139]]



```python
## 每个元素broadcast
print(a*2)
```

    [[ 0  2]
     [ 4  6]
     [ 8 10]
     [12 14]
     [16 18]
     [20 22]]



```python
# 行broadcast
print(a*[1,2])
```

    [[ 0  2]
     [ 2  6]
     [ 4 10]
     [ 6 14]
     [ 8 18]
     [10 22]]



```python
# 列broadcast
print(a*np.arange(6).reshape(6,1))
```

### 5.矩阵的拼接和拆分


```python
a = np.arange(9).reshape(3,3)
b = 2 * a
c = np.hstack((a, b)) # 以面向列的方式进行堆叠（axis=1）
print(c)
```

    [[ 0  1  2  0  2  4]
     [ 3  4  5  6  8 10]
     [ 6  7  8 12 14 16]]



```python
print(np.concatenate((a, b), axis=1))
```

    [[ 0  1  2  0  2  4]
     [ 3  4  5  6  8 10]
     [ 6  7  8 12 14 16]]



```python
c=np.vstack((a, b)) # 以面向行的方式进行堆叠（axis=0）
c=np.concatenate((a, b), axis=0)
# hsplit, vsplit, dsplit 分别沿轴0,1,2进行拆分
print(np.hsplit(a, 3)) 
print(np.vsplit(a,3))
```

    [array([[0],
           [3],
           [6]]), array([[1],
           [4],
           [7]]), array([[2],
           [5],
           [8]])]
    [array([[0, 1, 2]]), array([[3, 4, 5]]), array([[6, 7, 8]])]



```python
# 平均分成3份
g = np.split(np.arange(9), 3)

# 按照下标位置进行划分
h = np.split(np.arange(9), [2, -3])
print(g)
print(h)
```

    [array([0, 1, 2]), array([3, 4, 5]), array([6, 7, 8])]
    [array([0, 1]), array([2, 3, 4, 5]), array([6, 7, 8])]


- hsplit, vsplit, dsplit 分别沿轴0,1,2进行拆分
- hstack, vstack, dstack 分别以面向列(1)，行(0)，深度(2)的方式进行堆叠
- row_stack, column_stack 分别以面向列(1)，行(0)的方式进行堆叠
- np.r_, np.c_ 分别以面向列(1)，行(0)的方式进行堆叠

### 6. 矩阵的查找


```python
a = np.arange(12).reshape((3, 4))
b = a%2==0
c = a>4
print(b)
print (c)
# 这里也用到了broadcast
```

    [[ True False  True False]
     [ True False  True False]
     [ True False  True False]]
    [[False False False False]
     [False  True  True  True]
     [ True  True  True  True]]



```python
a = np.arange(12).reshape((3, 4))
print(a)
print(np.argmax(a))
# 其实是列broadcast，返回每列最大值的idx
print(np.argmax(a, axis=0))

# 行broadcast，返回每行最大值的idx
print(np.argmax(a, axis=1))
```

    [[ 0  1  2  3]
     [ 4  5  6  7]
     [ 8  9 10 11]]
    11
    [2 2 2 2]
    [3 3 3]



```python
# np.where支持多个逻辑组合, 得到满足条件的index
idx=np.where((a>3))
print(a[idx])

idx=np.where((a>3)&(a<7))
print(a[idx])
```

    [ 4  5  6  7  8  9 10 11]
    [4 5 6]


### 7. 元素的重复操作：tile和repeat


```python
# repeat会将数组中元素重复一定次数
a = np.repeat(3, 4) # 创建一个一维数组，元素值是把3重复4次，array([3, 3, 3, 3])

b = np.arange(3)
c = b.repeat(4) # 如果传入一个整数，则把每个元素重复多次
d = b.repeat([2,3,4]) # 如果传入一组整数, 则把个元素重复不同次数

e = np.arange(6).reshape(2,3)
f = e.repeat(2, axis=1) # 对于多维数组还可以沿指定轴重复
g = e.repeat([2, 3], axis=0)
print(a)
print(c)
print(d)
print(f)
print(g)

# tile会沿指定轴向堆叠数组的副本
h1 = np.tile(e, 2) # 对于标量按照水平铺设
h2 = np.tile(e, (2, 3)) # 对于原则则按照纵向和横向分别铺设
print(h1)
print(h2)
```

    [3 3 3 3]
    [0 0 0 0 1 1 1 1 2 2 2 2]
    [0 0 1 1 1 2 2 2 2]
    [[0 0 1 1 2 2]
     [3 3 4 4 5 5]]
    [[0 1 2]
     [0 1 2]
     [3 4 5]
     [3 4 5]
     [3 4 5]]
    [[0 1 2 0 1 2]
     [3 4 5 3 4 5]]
    [[0 1 2 0 1 2 0 1 2]
     [3 4 5 3 4 5 3 4 5]
     [0 1 2 0 1 2 0 1 2]
     [3 4 5 3 4 5 3 4 5]]

### 8. 随机模块（random）
```
import numpy as np
import numpy.random as random

# 设置随机数种子
random.seed(42)

# 产生一个1x3，[0,1)之间的浮点型随机数
random.rand(1, 3)

# 产生一个[0,1)之间的浮点型随机数
random.random()

# 下边4个没有区别，都是按照指定大小产生[0,1)之间的浮点型随机数array，不Pythonic…
random.random((3, 3))
random.sample((3, 3))
random.random_sample((3, 3))
random.ranf((3, 3))

# 产生10个[1,6)之间的浮点型随机数
5*random.random(10) + 1
random.uniform(1, 6, 10)

# 产生指定形状的随机数
random.randint(1, 6, 10)
random.randint(1, 6, size=(3,3))

# 产生2x5的标准正态分布样本
random.normal(size=(5, 2))

# 产生5个，n=5，p=0.5的二项分布样本
random.binomial(n=5, p=0.5, size=5)

a = np.arange(10)

# 从a中有回放的随机采样7个
random.choice(a, 7)

# 从a中无回放的随机采样7个
random.choice(a, 7, replace=False)

# 对a进行乱序并返回一个新的array
b = random.permutation(a)

# 对a进行in-place乱序
random.shuffle(a)

# 生成一个长度为9的随机bytes序列并作为str返回
# '\x96\x9d\xd1?\xe6\x18\xbb\x9a\xec'
random.bytes(9)
```

### 9. numpy 100题

http://www.labri.fr/perso/nrougier/teaching/numpy.100/