---
title: 记python的一个坑
date: 2016-12-01 16:55:10
layout: post
comments: true
reward: true
tags:
    - python
    - 反人类
---

之前 python 的 server 出过一次问题，总是异常丢失数据。
查日志也分析不出问题。

![heirenwenhao](/Image/hei1.jpeg)

<!--more-->
很困扰，最后一层一层找，发现是数据库返回的数据有重复列，到python端因为采用的是 index + data 分开存的方式（index: {name:1, age:2}, data:{'zhangsan', 23} 这样的两个集合），两个重复列名会导致index的对应出问题（字典无法存在两个相同 key）。我的问题是 index 的索引值超出了 data 的长度。

问题找到后，数据库端存储过程修改正确后这个奇怪的现象被我忽略了。
后来今天看到了这么一段：
> 以下代码将输出什么?
```python
list = ['a', 'b', 'c', 'd', 'e']
print list[10:]
```
> 答案

> 以上代码将`输出 []`，并且不会导致一个 IndexError。

![heirenwenhao](/Image/hei2.jpeg)

> 正如人们所期望的，试图访问一个超过列表索引值的成员将导致 IndexError（比如访问以上列表的 list[10]）。尽管如此，试图访问一个列表的以超出列表成员数作为开始索引的切片将不会导致 IndexError，并且将仅仅返回一个空列表。

> **一个讨厌的小问题是它会导致出现 bug ，并且这个问题是难以追踪的，因为它在运行时不会引发错误。**

这原来是面试题里的送分题……
这就尴尬了……