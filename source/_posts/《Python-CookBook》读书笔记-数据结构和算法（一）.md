---
title: 《Python CookBook》读书笔记-数据结构和算法（一）
date: 2017-04-21 10:03:40
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