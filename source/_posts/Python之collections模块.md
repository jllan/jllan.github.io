---
title: Python之collections模块
date: 2017-05-18 00:21:14
categories: IT技术
tags: python
---
collections是Python内建的一个集合模块，提供了许多有用的集合类。

### defaultdict
我们都知道，在使用Python原生的数据结构dict的时候，如果用d[key]这样的方式访问，当指定的key不存在时，是会抛出KeyError异常的。
但是，如果使用defaultdict，只要你传入一个默认的工厂方法，那么请求一个不存在的key时，便会调用这个工厂方法使用其结果来作为这个key的默认值。


```python
from collections import defaultdict

# 当ｄ[key]不存在时返回默认值
# 默认值是调用函数返回的，而函数在创建defaultdict对象时传入
dd = defaultdict(lambda: 'N/A')
dd['key1'] = 'abc'
print(dd['key1']) # key1存在
print(dd['key2']) # key2不存在，返回默认值
```

    abc
    N/A



```python
# 将键映射到多个值上
members = [
    # Age, name
    ['male', 'John'],
    ['male', 'Jack'],
    ['female', 'Lily'],
    ['male', 'Pony'],
    ['female', 'Lucy'],
]

result = defaultdict(list)
for sex, name in members:
    result[sex].append(name)

print(result)
```

    defaultdict(<class 'list'>, {'female': ['Lily', 'Lucy'], 'male': ['John', 'Jack', 'Pony']})


### OrderedDict
在Python中，dict这个数据结构由于hash的特性，是无序的，这在有的时候会给我们带来一些麻烦，幸运的是，collections模块为我们提供了一个有序的字典对象OrderedDict。

OrderedDict内部维护了一个双向链表，会根据元素加入的顺序来排列键的位置。OrderedDict的大小是普通字典的2倍多。


```python
from collections import OrderedDict

items = (
    ('A', 1),
    ('B', 2),
    ('C', 3)
)

regular_dict = dict(items)
ordered_dict = OrderedDict(items)

print(regular_dict)
print(ordered_dict)
```

    {'A': 1, 'C': 3, 'B': 2}
    OrderedDict([('A', 1), ('B', 2), ('C', 3)])


### namedtuple()
对列表和元组一般是通过下表来访问的，有时这种访问方式有些难以阅读。我们想要通过名字来访问元素以此减少结构中对位置的依赖，这时就可以使用namedtuple()。namedtuple()是一个工厂方法，它返回的是python中标准元组类型的子类。我们提供给它一个类型名称及相应的字段，它返回一个可实例化的类、为你已经定义好的字段传入值等。


```python
records = [
    ('apple', 2.8, 6),
    ('banada', 1.5, 8),
    ('peach', 2.2, 12),
    ('pear', 1.8, 5)
]

def compute_cost(records):
    total = 0
    for rec in records:
        total += rec[1]*rec[2]
    return total

print(compute_cost(records))
```

    64.2



```python
from collections import namedtuple

Fruit = namedtuple('Fruit', ['name', 'price', 'count'])
def compute_cost2(records):
    total = 0
    for rec in records:
        fruit = Fruit(*rec)
        total += fruit.price*fruit.count
    return total

print(compute_cost2(records))
```

    64.2


namedtuple()的一种可能用法是用来代替dict，与普通dict不同的是，namedtuple是不可变的，如果需要改变属性，可以通过namedtuple实例的_replace()方法。该方法创建一个全新的命名组，并对相应值进行替换。


```python
Fruit = namedtuple('Fruit', ['name', 'price', 'count'])
fruit = Fruit('apple', 2.8, 6)
print(fruit)
# fruit.price=3 # 返回AttributeError: can't set attribute
fruit = fruit._replace(price=3)
print(fruit)
```

    Fruit(name='apple', price=2.8, count=6)
    Fruit(name='apple', price=3, count=6)


### Counter
找出序列中出现次数最多的元素可以用collections模块中的Counter类来实现 Counter的底层是一个字典，在元素和它们出现的次数间做了一个映射。Counter对象提供任何可哈希的对象序列作为输入。


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

    Counter({'a': 4, 'e': 3, 'f': 2, 'd': 2, 'b': 1})
    Counter({'a': 5, 'e': 3, 'd': 2, 'f': 2, 'b': 2, 'c': 1})



```python
# Counter对象的各种运算
a = Counter(words)
b = Counter(words_2)
print(a)
print(b)
print(a + b)
print(a - b)
```

    Counter({'a': 4, 'e': 3, 'd': 2, 'f': 1, 'b': 1})
    Counter({'c': 1, 'b': 1, 'a': 1})
    Counter({'a': 5, 'e': 3, 'd': 2, 'b': 2, 'c': 1, 'f': 1})
    Counter({'e': 3, 'a': 3, 'd': 2, 'f': 1})


### deque
使用list存储数据时，按索引访问元素很快，但是插入和删除元素就很慢了，因为list是线性存储，数据量大的时候，插入和删除效率很低。
deque是为了高效实现插入和删除操作的双向列表，deque其实是double-ended queue的缩写，翻译过来就是双端队列。deque除了实现list的append()和pop()外，还支持appendleft()和popleft()，这样就可以非常高效地往头部添加或删除元素。


```python
from collections import deque
q = deque(['a', 'b', 'c'])
q.append('x')
q.appendleft('y')
print(q)
q.popleft()
print(q)
```

    deque(['y', 'a', 'b', 'c', 'x'])
    deque(['a', 'b', 'c', 'x'])


作为一个双端队列，deque还提供了一些其他的好用方法，比如 rotate 等


```python
import sys
import time
from collections import deque

fancy_loading = deque('>--------------------')
n = 0
while n<2*len(fancy_loading):
    print('%s \r' % ''.join(fancy_loading), end='') # python3中print会自动换行，设置end=''可以不换行
#     sys.stdout.write('%s \r' % ''.join(fancy_loading)) # \r表示换行，回到行首
    fancy_loading.rotate(1)
    sys.stdout.flush()
    time.sleep(0.1)
    n += 1
```

    --------------------> 

## 参考
- http://www.zlovezl.cn/articles/collections-in-python/
- http://www.liaoxuefeng.com/wiki/001374738125095c955c1e6d8bb493182103fac9270762a000/001411031239400f7181f65f33a4623bc42276a605debf6000    
- https://docs.python.org/3/library/collections.html#collections.defaultdict    
