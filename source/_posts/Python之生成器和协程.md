---
title: Python之生成器和协程
date: 2016-12-26 20:47:16
categories: Python
tags: python
---

## 生成器

Python中，为避免创建完整的list，从而节省大量的空间而在循环的过程中不断推算出后续的元素，这种一边循环一边计算的机制，称为生成器：generator。

要创建一个generator，有很多种方法。
### 用列表生成式的方式来创建generator
只要把一个列表生成式的[]改成()，就创建了一个generator：
```
>>> L = [x * x for x in range(5)]
>>> L
[0, 1, 4, 9, 16, 25]
>>> g = (x * x for x in range(5))
>>> g
<generator object <genexpr> at 0x1022ef630>
```
创建L和g的区别仅在于最外层的[]和()，L是一个list，而g是一个generator。
如果要一个一个打印出来，可以通过next()函数获得generator的下一个返回值：
```
>>> next(g)
0
>>> next(g)
1
>>> next(g)
4
>>> next(g)
9
>>> next(g)
16
>>> next(g)
25
>>> next(g)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
StopIteration
```
我们讲过，generator保存的是算法，每次调用next(g)，就计算出g的下一个元素的值，直到计算到最后一个元素，没有更多的元素时，抛出StopIteration的错误。
一般不使用next(g)这种方法，而是使用for循环，因为generator也是可迭代对象：
```
>>> g = (x * x for x in range(5))
>>> for n in g:
...     print(n)
... 
0
1
4
9
16
25
```
### 用yield来创建generator
如果一个函数定义中包含yield关键字，那么这个函数就不再是一个普通函数，而是一个generator。

**用生成器来生成斐波拉契数列**
著名的斐波拉契数列（Fibonacci），除第一个和第二个数外，任意一个数都可由前两个数相加得到：
`1, 1, 2, 3, 5, 8, 13, 21, 34, ...`
斐波拉契数列用列表生成式写不出来，但是，用函数把它打印出来却很容易：
```
def fib(max):
    n, a, b = 0, 0, 1
    while n < max:
        print(b)
        a, b = b, a + b
        n = n + 1
    return 'done'
```
仔细观察，可以看出，fib函数实际上是定义了斐波拉契数列的推算规则，可以从第一个元素开始，推算出后续任意的元素，这种逻辑其实非常类似generator。要把fib函数变成generator，只需要把print(b)改为yield b就可以了：
```
def fib(max):
    n, a, b = 0, 0, 1
    while n < max:
        yield b
        a, b = b, a + b
        n = n + 1
    return 'done'
```
```
>>> f = fib(6)
>>> f
<generator object fib at 0x104feaaa0>
```
这里，最难理解的就是generator和函数的执行流程不一样。函数是顺序执行，遇到return语句或者最后一行函数语句就返回。而变成generator的函数，在每次调用next()的时候执行，遇到yield语句返回，再次执行时从上次返回的yield语句处继续执行。

我们在循环过程中不断调用yield，就会不断中断。当然要给循环设置一个条件来退出循环，不然就会产生一个无限数列出来。
同样的，把函数改成generator后，我们基本上从来不会用next()来获取下一个返回值，而是直接使用for循环来迭代：
```
>>> for n in fib(6):
...     print(n)
...
1
1
2
3
5
8
```
但是用for循环调用generator时，发现拿不到generator的return语句的返回值。如果想要拿到返回值，必须捕获StopIteration错误，返回值包含在StopIteration的value中：
```
>>> g = fib(6)
>>> while True:
...     try:
...         x = next(g)
...         print('g:', x)
...     except StopIteration as e:
...         print('Generator return value:', e.value)
...         break
...
g: 1
g: 1
g: 2
g: 3
g: 5
g: 8
Generator return value: done
```

**用生成器来生成杨辉三角**
```python
def triangles():
    L = [1]
    while True:
        yield L
        L.append(0)
        L = [L[i - 1] + L[i] for i in range(len(L))]
```
下面调用traingles函数
```
n = 0
for t in triangles():
    print(t)
    n = n + 1
    if n == 10:
        break
        
# 运行结果
[1]
[1, 1]
[1, 2, 1]
[1, 3, 3, 1]
[1, 4, 6, 4, 1]
[1, 5, 10, 10, 5, 1]
[1, 6, 15, 20, 15, 6, 1]
[1, 7, 21, 35, 35, 21, 7, 1]
[1, 8, 28, 56, 70, 56, 28, 8, 1]
[1, 9, 36, 84, 126, 126, 84, 36, 9, 1]
```

## 协程
Python对协程的支持是通过generator实现的。
在generator中，我们不但可以通过for循环来迭代，还可以不断调用next()函数获取由yield语句返回的下一个值。
但是Python的yield不但可以返回一个值，它还可以接收调用者发出的参数。

来看例子：
传统的生产者-消费者模型是一个线程写消息，一个线程取消息，通过锁机制控制队列和等待，但一不小心就可能死锁。
如果改用协程，生产者生产消息后，直接通过yield跳转到消费者开始执行，待消费者执行完毕后，切换回生产者继续生产，效率极高：
```
def consumer():
    r = ''
    while True:
        n = yield r
        if not n:
            return
        print('[CONSUMER] Consuming %s...' % n)
        r = '200 OK'

def produce(c):
    c.send(None)
    n = 0
    while n < 5:
        n = n + 1
        print('[PRODUCER] Producing %s...' % n)
        r = c.send(n)
        print('[PRODUCER] Consumer return: %s' % r)
    c.close()
```
`c = consumer()
produce(c)`
执行结果：
```
[PRODUCER] Producing 1...
[CONSUMER] Consuming 1...
[PRODUCER] Consumer return: 200 OK
[PRODUCER] Producing 2...
[CONSUMER] Consuming 2...
[PRODUCER] Consumer return: 200 OK
[PRODUCER] Producing 3...
[CONSUMER] Consuming 3...
[PRODUCER] Consumer return: 200 OK
[PRODUCER] Producing 4...
[CONSUMER] Consuming 4...
[PRODUCER] Consumer return: 200 OK
[PRODUCER] Producing 5...
[CONSUMER] Consuming 5...
[PRODUCER] Consumer return: 200 OK
```
注意到consumer函数是一个generator，把一个consumer传入produce后：
首先调用c.send(None)启动生成器；
然后，一旦生产了东西，通过c.send(n)切换到consumer执行；
consumer通过yield拿到消息，处理，又通过yield把结果传回；
produce拿到consumer处理的结果，继续生产下一条消息；
produce决定不生产了，通过c.close()关闭consumer，整个过程结束。
整个流程无锁，由一个线程执行，produce和consumer协作完成任务，所以称为“协程”，而非线程的抢占式多任务。
最后套用Donald Knuth的一句话总结协程的特点：
“子程序就是协程的一种特例。”





