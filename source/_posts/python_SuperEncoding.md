---
title: 用 python 写一个exe应用程序 - SuperEncoding
date: 2017-6-28 17:41:27
layout: post
comments: true
--reward: true
tags:
    - 工具
    - python
---

**作为程序员，能写个程序解决日常需求还是很开心的。
这边把应用程序和代码分享下。
代码简单到仅仅实现了功能，方便小伙伴们阅读。**

<!--more-->

### 程序诞生背景
公司女同事在传文件到服务器时总会因为编码问题导致上传或者显示出错。
有一次找到我，说起出错这件事，我写了个python的脚本帮她测了下文件编码，发现出错的文件大部分都是编码不正确导致的。
这件事过后我抽了个时间。把之前的脚本改成了一个exe可执行程序，这样就省得来找我测编码了（对，没错，注孤生）。

### 程序简介
选择文件，显示文件编码信息。
听起来很简单对吧，嗯，写起来也很简单。

### 实现思路
我们需要干三件事
1.python 获取文件编码信息的代码（其实就是几行代码~）
2.通过 GUI 实现选择文件显示编码信息（我这里选择的 Tkinter）
3.打包成 exe（我这里选择的PyInstaller）

### 资源
|获取方式|获取地址|备注|
|:--------:|:--------:|:--------:|
|github|[SuperEncoding_github](https://github.com/jazhLeo/SuperEncoding)||
|百度云|[SuperEncoding_百度云](https://pan.baidu.com/s/1geM8i8z)|提取码: bcyj|
