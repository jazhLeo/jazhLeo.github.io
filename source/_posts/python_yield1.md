---
title: 你需要一个老头，帮助你理解下 Python 中的 yield
date: 2016-12-14 16:01:01
layout: post
comments: true
reward: true
tags:
    - python

---

> **题外话**
> * 最初接触 Python 这门语言的时候，看的是《Python 核心编程》 迷迷糊糊的把 `yield` 这里看过去了，知道这个东西是用来造一个`生成器`用的，就一带而过了，日后的搬砖过程中也不知道怎么用这个家伙事儿。
> * 我比较笨，学东西讨厌文档型说明（看不懂QAQ）。所以总是自己花很长时间去加工适合我的食物。
> * <iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=33211676&auto=0&height=66"></iframe>

----------

### 说起 `yield`，那这到底是个什么玩意？

准备好了，我要开始背课文了：
……
<!--more-->
从小脑袋不好使，记不住……
还是编故事吧：

有个叫斐波那契的老头（也可能是女的，跟我有什么关系），提出了一个（不明白他搞出这东西要干啥的）数列定义（斐波那契数列）：
> 有一串数字，前两个是1，从第三个开始，每个数字都是它前两位数字的和。（1, 1, 2, 3, 5, 8, 13……）

提出定义的人我可以理解为他无聊，但是后来一群抠脚大汉开始对第N个数是多少那么感兴趣是什么鬼？这玩意算出来能干啥？（不知道）

不行，我也要凑热闹。（koujiao……）

```python
def fab(max):
    n, a, b = 0, 0, 1
    while n < max:
        print b
        a, b = b, a + b
        n = n + 1
```
好，跑一下试试。
```python
>>> fab(5)
1
1
2
3
5
```
结果是对的，你以为这就完了？
隔壁王叔叔表示小伙子你这个写的有点问题啊，你这个直接`print`出来的，只能给我看，不能让我直接拿来用啊（你用它能干嘛？[手动黑人问号]）
好，那就听您老的，给你返回一个 List 让你用（？？？）。
```python
def fab(max):
    n, a, b = 0, 0, 1
    list = []
    while n < max:
        list.append(b)
        a, b = b, a + b
        n = n + 1
    return list
```
来，试一下。
```python
>>> for n in fab(5):
...     print n
...
1
1
2
3
5
```
隔壁王叔叔的丈母娘（跟直系亲属姥姥不是一个人，谢谢)表示，小伙子你这个程序太特么占内存了，我输入大一点，这个List占用的内存都够我玩刺客信条大革命了（excuse me??)。

### 性能优化

如果对python有一些了解的话，我们能很自然的想到一个经常拿来装逼的东西`xrange`。
```python
# 1
for i in range(1000): 
    pass

# 2
for i in xrange(1000):
    pass
```
上面的两种写法，第一种是直接生成 1000个元素的List，相当于一次性给你一年的生活费。 而第二种是每次循环用到的时候给你一个，再用到再给你一个，可以理解为他有一个算式，每次你找他要钱他都要当场算算，然后给你一天的生活费。那用第二种方法吧。（看起来很扣的样子）

好，实现一个。
```python
class Fab(object):
    
    def __init__(self, max):
        self.max = max
        self.n, self.a, self.b = 0, 0, 1
        
    def __iter__(self):
        return self
        
    def next(self):
        if self.n < self.max:
            r = self.b
            self.a, self.b = self.b, self.a + self.b
            self.n = self.n + 1
            return r
        raise StopIteration()
```
基础差的看不懂的，其实上面的代码就是定义了一个算法，你每次问我要一个数，我就当场算一个数，不会像上一个版本直接返回给你一个List。
我们来调用一下：
```python
>>> for n in Fab(5):
...     print n
...
1
1
2
3
5
```
嗯，成功了，这个版本性能是有了，可是隔壁王叔叔的侄女看了看代码不愿意了，帅哥（为什么要说出来？）你这个程序看着太复杂了啊，本来那么简单的代码怎么写的这么复杂啊？你这个人好复杂啊。

很好小姑娘，你要是王叔叔，我特么把你家网线煮了。看在你说的挺有道理的份上（其实是长得好看），我看看能不能简单点。

来，我们用一种高逼格的东西，兼顾简洁与性能，嘤嘤嘤。
```python
def fab(max):
    n, a, b = 0, 0, 1
    while n < max:
        yield b
        # print b
        a, b = b, a + b
        n = n + 1
```
跟最初的第一个版本很像，注释掉了`print b`这句话，加上了 `yield b`。这里我们先调用下，看看结果。
```python
>>> for n in fab(5):
...     print n
...
1
1
2
3
5
```
### 吹牛B模式已开启
ok, 现在来回想下我们一路走过来，从最初的版本到使用`yield`之前，经历了哪些改动？
仔细对照第一次的例子和这个使用`yield`的例子，好像差不多？再看看我们测试结果的代码有什么不同。
就是这里了
```python
# 第一个例子
fab(5)

# yield例子
for n in fab(5)
    print n
```
发现问题了吗？
> * fab(max) 函数中同样使用循环
> * 一个使用 yield，另一个使用 print 打印
> * yield 例子，需要自己循环调用才能一个一个打印出来

这里我们推测`yield`使用上的特点：
> * 暂停函数运行（要循环调用才管用）
> * 再次调用能从上次暂停的地方继续执行函数（数列是连续的）
> * 函数的每次运行都是遇到 yield 就停止并返回迭代值，下次再用时继续
> * 对不起我编不下去了

据说还有更强大的使用方式，下次有机会研究研究写个续吧：）

----------
**隔壁王叔叔的儿子的小姨子表示小伙子，你再努努力，我就跟你跑了。**

例子取自：[廖雪峰-Python yield使用浅析](http://www.liaoxuefeng.com/article/001373892916170b88313a39f294309970ad53fc6851243000)