---
title: 《Python CookBook》读书笔记-数据结构和算法（一）
date: 2017-04-21 10:03:40
categories: IT技术
tags: python
---
## 1. 把一个序列L拆分为包含n(n<=len(l))个单独的变量

### 用法


```python
data = ['ACME', 50, 90, (2017, 3, 8)]
name, shares, price, date = data
print(name, date)
name, _, _, (year, *_) = data    # 只取某几个元素而忽略其他元素
print(name, year)
```

    ACME (2017, 3, 8)
    ACME 2017


### 说明
做分解操作，有时可能需要丢弃某些值，通常可以选一个一般用不到的变量名来表示这些要被丢弃的元素，如可以用 '**_**'，'***_**', '**ign(ignored)**', '***ign**'。对于分解未知或者任意长度的可迭代对象时，使用这种*式的语法特别有用。

### 例子


```python
# 计算平均分时一般要去掉最高和最低分
def drop_first_last(grades):
    first, *middle, last = grades
    return avg(middle)
```


```python
# 实现递归
def sum(items):
    head, *tail = items
    return head + sum(tail) if tail else head
```

## 2. 通过heapq模块来找最大或最小的N个元素

### 用法


```python
import heapq
import random

nums = random.sample(range(-20, 50), 10) # 从range(-20, 50)中随机取出10个元素
print(nums)
print(heapq.nlargest(3, nums))
print(heapq.nsmallest(3, nums))

dict_nums = [dict.fromkeys('a', i) for i in nums]
print(dict_nums)
print(heapq.nlargest(3, dict_nums, key=lambda s: s['a'])) # 通过接受参数key，可以在更复杂的数据结构上进行操作
print(heapq.nsmallest(3, dict_nums, key=lambda s: s['a']))

heap = list(nums)
heapq.heapify(heap) # 把nums以堆的顺序进行排列
print(heap)
print(heapq.heappop(heap)) # heap[0]总是最小那个元素，heappop取出最小的元素，取出最小元素后会重新构造堆
print(heapq.heappop(heap))
```

    [-1, 43, 16, 47, 31, -11, 4, 49, 13, 28]
    [49, 47, 43]
    [-11, -1, 4]
    [{'a': -1}, {'a': 43}, {'a': 16}, {'a': 47}, {'a': 31}, {'a': -11}, {'a': 4}, {'a': 49}, {'a': 13}, {'a': 28}]
    [{'a': 49}, {'a': 47}, {'a': 43}]
    [{'a': -11}, {'a': -1}, {'a': 4}]
    [-11, 13, -1, 43, 28, 16, 4, 49, 47, 31]
    -11
    -1


### 说明
- 堆最重要的特性是heap[0]总是最小那个元素
- 当要找的元素数量相对较小时，适用nlargest()和nsmallest()，注意nlargest()和nsmallest()的实际实现会根据适用他们的不同方式而有所不同，如N仅仅集合大小时会先排序
- 如果只是简单的找到最小或最大的元素，则用min()和max()合适
- 如果N和集合本身大小差不多，通常更快的方法是先对集合排序，然后做切片操作

## 3. 实现优先级队列

### 问题
实现一个队列，它能以给定的优先级来对元素进行排序，每次pop操作都会返回优先级最高的那个元素

### 解决方案


```python
import heapq

class PriorityQueue:
    def __init__(self):
        self._queue = []
        self._index = 0
        
    def push(self, item, priority):
        heapq.heappush(self._queue, (-priority, self._index, item))
        self._index += 1
        
    def pop(self):
        return heapq.heappop(self._queue)[-1]
    
pq = PriorityQueue()
pq.push('jlan', 5)
pq.push('hua', 2)
pq.push('lann', 4)
pq.push('bob', 1)
pq.push('han', 3)
print(pq._queue)
print(pq.pop())
print(pq._queue)
print(pq.pop())
```

    [(-5, 0, 'jlan'), (-3, 4, 'han'), (-4, 2, 'lann'), (-1, 3, 'bob'), (-2, 1, 'hua')]
    jlan
    [(-4, 2, 'lann'), (-3, 4, 'han'), (-2, 1, 'hua'), (-1, 3, 'bob')]
    lann


### 说明（详解见P10）
1. 这个方案的核心在于heapq模块的使用。heappush()和heappop()分别从_queue中插入和移除，且保证列表中第一个元素的优先级最低。heappop()总是返回‘最小’元素，因此这是队列能弹出正确元素的关键。
2. 在这段代码中，队列以元组(-priority, index, item)的形式组成，priority取负值是为了让队列能够按优先级从高到底排序（正常情况下，堆是按从小到大排序的）,构造堆时按-priority排序，当priority相同时按index排序。
3. 变量index的作用是为了将具有相同优先级的元素以适当的顺序排列（以入队顺序）。

## 字典操作

- 将键映射到多个值上


```python
from collections import defaultdict

d = defaultdict(list)
d['a'].append(1)
d['a'].append(2)
print(d)
```

    defaultdict(<class 'list'>, {'a': [1, 2]})


- 有序字典
使用collections.OrderedDict可以保持字典有序，OrderedDict内部维护了一个双向链表，会根据元素加入的顺序来排列键的位置。OrderedDict的大小是普通字典的2倍多。

- 在字典上进行求最大值，最小值等操作


```python
prices = {
    'apple': 2,
    'banada': 3,
    'orange': 1,
    'peach': 4
}

# 在字典上执行常见的操作，只会处理键，而不是值
min(prices) # 返回'apple'
max(prices) # 返回'peach'

# 可以用dict.values()来处理值
min(prices.values()) # 返回 1
max(prices.values()) # 返回 4

min(prices, key=lambda x: prices[x]) # 返回'orange'
max(prices, key=lambda x: prices[x]) # 返回'peach'

# 对字典进行求最大值，最小值等操作一般用下边的方法
min(zip(prices.values(), prices.keys())) # 返回(1, 'orange')
max(zip(prices.values(), prices.keys())) # 返回(4, 'orange')
# zip()创建了一个迭代器，其内容只能被消费一次
# 涉及(value, key)对的比较时，如果多个条目有相同的value，此时将根据key进行判定
```




    (4, 'peach')



- 在字典中寻找相同点


```python
# 找出两个字典可能相同的地方（相同的键或者值）
a = {'x': 1, 'y': 2, 'z': 3}
b = {'w': 11, 'y': 2, 'z': 13}

# a和b相同的键
a.keys() & b.keys() # 返回 {'y', 'z'}

# 在a中但是不在b中的键
a.keys() - b.keys() # 返回 {'x'}

# a和b中相同的{key: value}
a.items() & b.items() # 返回 {('y', 2)}
```




    {('y', 2)}




```python

```