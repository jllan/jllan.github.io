---
title: 《Python CookBook》读书笔记-数据结构和算法（二）
date: 2017-05-11 21:08:43
categories: IT技术
tags: python
---
## 从序列中移除重复项且保持元素间顺序不变

### 方法
可以用集合和生成器来解决

先来了解下什么是可哈希(hashable)：
如果一个对象在自己的生命周期中有一哈希值（hash value）是不可改变的，那么它就是可哈希的（hashable）的。可哈希对象是对象有__hash__(self)内置函数的对象。对于可哈希的对象执行这个函数将会返回一个整数。可哈希对象判断相等的唯一条件就是两者的哈希值相等。Python中所有不可改变的的对象（imutable objects）都是可哈希的，比如字符串，元组，也就是说可改变的容器如字典，列表不可哈希（unhashable）。我们用户所定义的类的实例对象默认是可哈希的（hashable），它们都是唯一的，而hash值也就是它们的id()。


```python
t = (1, 2, 3)
s = '123'
l = [1,2,3]
d = [{'a': 1},{'b':2}]
print(t.__hash__())
print(s.__hash__())
print(l.__hash__) # list不是可哈希的
print(l[1].__hash__()) # list的对象可哈希
print(d.__hash__) # dict不是可哈希的
# set(d)            # 列表的元素是字典，字典是不可哈希的，所以返回TypeError: unhashable type: 'dict'
print(set([(1,2), (1,2), (2,)])) # 列表的元素是元组，元组是可哈希的
```

    2528502973977326415
    -8527273320470595707
    None
    2
    None
    {(1, 2), (2,)}



```python
# 当序列中的元素是可哈希时
def dedupe(items):
    seen = set()
    for item in items:
        if item not in seen:
            yield item
            seen.add(item)
            
a= [1, 5, 2, 1, 7, 5, 9 ,0]
b = set(a)
c = dedupe(a)
print(b)
print(list(c))
```

    {0, 1, 2, 5, 7, 9}
    [1, 5, 2, 7, 9, 0]



```python
# 当不确定序列中的元素是否可哈希时，需要对程序改造如下
def dedupe(items, key=None):
    seen = set()
    for item in items:
        val = item if key is None else key(item)
        if val not in seen:
            yield item
            seen.add(val)
            
a= [{'x':1, 'y':2}, {'x':1, 'y':3}, {'x':1, 'y':2}, {'x':2, 'y':4}]
# b = set(a) # a的元素是字典，字典不是可哈希的，所以返回TypeError: unhashable type: 'dict'
c = dedupe(a, key=lambda d: (d['x'], d['y']))
d = dedupe(a, key=lambda d: (d['x']))
print(list(c))
print(list(d))
```

    [{'y': 2, 'x': 1}, {'y': 3, 'x': 1}, {'y': 4, 'x': 2}]
    [{'y': 2, 'x': 1}, {'y': 4, 'x': 2}]



```python
# 读取一个文件，去除其中的重复行
with open('a.txt', 'r') as f:
#     print(list(f)) # ['abc\n', 'a\n', 'df\n', 'abc\n', 'df\n', '123\n']
    print(list(dedupe(f))) # ['abc\n', 'a\n', 'df\n', '123\n']
```

    ['abc\n', 'a\n', 'df\n', '123\n']


## 找出序列中出现次数最多的元素
### 方法
可以用collections模块中的Counter类来实现
Counter的底层是一个字典，在元素和它们出现的次数间做了一个映射。Counter对象提供任何可哈希的对象序列作为输入。


```python
from collections import Counter

words = ['a', 'a', 'b', 'a', 'e', 'f', 'a', 'e', 'e', 'd', 'd']
count = Counter(words)
print(count) 
print(count.most_common(3)) # 出现次数最多的前三个元素
```

    Counter({'a': 4, 'e': 3, 'd': 2, 'f': 1, 'b': 1})
    [('a', 4), ('e', 3), ('d', 2)]



```python
print(count['f'])
count['f'] += 1 # 还可以手动增加某个元素的出现次数
print(count['f'])
```

    1
    2



```python
print(count)
words_2 = ['b', 'a', 'c']
count.update(words_2) # update方法可以更新count的数据
print(count)
```

    Counter({'a': 8, 'e': 6, 'd': 4, 'f': 3, 'b': 2})
    Counter({'a': 9, 'e': 6, 'd': 4, 'f': 3, 'b': 3, 'c': 1})



```python
# Counter对象的各种运算
a = Counter(words)
b = Counter(words_2)
print(a)
print(b)
print(a + b)
print(a- b)
```

    Counter({'a': 4, 'e': 3, 'd': 2, 'f': 1, 'b': 1})
    Counter({'c': 1, 'b': 1, 'a': 1})
    Counter({'a': 5, 'e': 3, 'b': 2, 'd': 2, 'f': 1, 'c': 1})
    Counter({'a': 3, 'e': 3, 'd': 2, 'f': 1})


## 对字典列表的排序



```python
friends = [
    {'name': 'jlan', 'age': 27, 'gender': 'm'},
    {'name': 'lann', 'age': 25, 'gender': 'f'},
    {'name': 'bob', 'age': 23, 'gender': 'm'},
    {'name': 'herry', 'age': 28, 'gender': 'f'},
    {'name': 'dairy', 'age': 26, 'gender': 'm'}
]

print(friends)
from operator import itemgetter
print(sorted(friends, key=itemgetter('age')))
print(sorted(friends, key=itemgetter('age', 'name')))

# itemgetter()的参数可以是字典的键、用数字表示的列表元素等任何可以穿给对象的__getitem__()方法的值。
# 用lambda也可以实现这样的功能，但是用itemgetter通常效率更高
print(sorted(friends, key=lambda f: f['age']))
print(sorted(friends, key=lambda f: (f['age'], f['name'])))
```

    [{'gender': 'm', 'age': 27, 'name': 'jlan'}, {'gender': 'f', 'age': 25, 'name': 'lann'}, {'gender': 'm', 'age': 23, 'name': 'bob'}, {'gender': 'f', 'age': 28, 'name': 'herry'}, {'gender': 'm', 'age': 26, 'name': 'dairy'}]
    [{'gender': 'm', 'age': 23, 'name': 'bob'}, {'gender': 'f', 'age': 25, 'name': 'lann'}, {'gender': 'm', 'age': 26, 'name': 'dairy'}, {'gender': 'm', 'age': 27, 'name': 'jlan'}, {'gender': 'f', 'age': 28, 'name': 'herry'}]
    [{'gender': 'm', 'age': 23, 'name': 'bob'}, {'gender': 'f', 'age': 25, 'name': 'lann'}, {'gender': 'm', 'age': 26, 'name': 'dairy'}, {'gender': 'm', 'age': 27, 'name': 'jlan'}, {'gender': 'f', 'age': 28, 'name': 'herry'}]
    [{'gender': 'm', 'age': 23, 'name': 'bob'}, {'gender': 'f', 'age': 25, 'name': 'lann'}, {'gender': 'm', 'age': 26, 'name': 'dairy'}, {'gender': 'm', 'age': 27, 'name': 'jlan'}, {'gender': 'f', 'age': 28, 'name': 'herry'}]
    [{'gender': 'm', 'age': 23, 'name': 'bob'}, {'gender': 'f', 'age': 25, 'name': 'lann'}, {'gender': 'm', 'age': 26, 'name': 'dairy'}, {'gender': 'm', 'age': 27, 'name': 'jlan'}, {'gender': 'f', 'age': 28, 'name': 'herry'}]



```python
# key=itemgetter()或lambda方法同样也可以用于min，max之类的函数
```

## 根据字段将记录分组
用itertools.groupby()方法


```python
from operator import itemgetter
from itertools import groupby

# 字符串压缩，面试经常会出现这样的问题
l = 'aabbbbcccddeddff'
for key, group in groupby(l):
    print(key)
    print(list(group))
```

    a
    ['a', 'a']
    b
    ['b', 'b', 'b', 'b']
    c
    ['c', 'c', 'c']
    d
    ['d', 'd']
    e
    ['e']
    d
    ['d', 'd']
    f
    ['f', 'f']



```python
friends = [
    {'name': 'jlan', 'age': 27, 'gender': 'm'},
    {'name': 'lann', 'age': 25, 'gender': 'f'},
    {'name': 'bob', 'age': 23, 'gender': 'm'},
    {'name': 'herry', 'age': 28, 'gender': 'f'},
    {'name': 'dairy', 'age': 26, 'gender': 'm'}
]
# 对friends按性别分组
friends.sort(key=itemgetter('gender'))
for key, group in groupby(friends, key=itemgetter('gender')):
    print(key)
    print(list(group))
```

    f
    [{'gender': 'f', 'age': 25, 'name': 'lann'}, {'gender': 'f', 'age': 28, 'name': 'herry'}]
    m
    [{'gender': 'm', 'age': 27, 'name': 'jlan'}, {'gender': 'm', 'age': 23, 'name': 'bob'}, {'gender': 'm', 'age': 26, 'name': 'dairy'}]


### 说明
groupby()方法通过扫描序列找出拥有相同值（或是由参数key指定的函数返回的值）的序列项，并将它们分组。groupby()创建了一个迭代器，每次迭代都返回一个值（分组的key）和一个子迭代器（属于该分组的group）。对与friends这样序列如果要按照性别分组，需要先对friends按性别进行排序，把相同性别的项放在一块，然后才能按groupby()进行分组。
