---
title: 国内无备案网站使用畅言评论系统
date: 2017-5-13 07:50:28
layout: post
comments: true
reward: true
tags:
    - 工具
    - 音乐
---

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=5133273&auto=0&height=66"></iframe>

> 本来朋友圈说昨晚把教程发出来，结果……到家后衣服都没脱就睡了。
早上五点恢复意识，磨蹭到7点才打开电脑。

<!--more-->

# 背景

畅言评论系统是搜狐搞出来的一个免费的评论系统，对于小站不想花精力去做评论系统的人确实很方便。  
类似的评论系统常用的有：多说、网易云跟帖、友言等。  
多说团队宣布2017年6月1日正式关闭服务，不巧的是我用的也是多说，所以开始了辗转之旅。  
多番体验后选择使用畅言，网易云跟帖的风格让我感觉跟blog格格不入也是一方面因素。 

适合人群：
* github建站未购买私人域名无法备案
* 已拥有国内域名但因种种原因没有备案

# 原理

利用畅言网站备案审核后的白名单机制让自己的网站也能使用畅言。  
是的，如果你比较聪明的话，看到这里你应该知道该怎么搞了。


# 细节

### 正常注册畅言用户

这一步跳过了，确实没什么可说的

### 提交申请

![beianshenqing](/Image/beianshenqing.png)

因为我已经审核通过，所以上面显示已通过，不用纠结这一点。  
关于备案，我当初用baidu站点尝试，完全是想法的尝试没想那么多。  
不过若畅言团队封掉同一站点网址多个实例注册的话，审核很容易就不通过了。  
建议去使用一些其他站点的备案信息。  
备案信息获取可百度：备案信息查询。随便找域名，拿来备案信息就用~
然后提交审核，耐心等待审核通过。  

### 修改域名白名单

![xiugaibaimingdan](/Image/xiugaibaimingdan.png)

审核通过完毕后，按照图中方式修改域名白名单，然后等待官方确认修改的通知。  
收到通知后，你的就可以开心的使用畅言了~

### 已知小问题

无法在本地调适时，即 localhost 访问时显示评论，不过不影响使用。如果大家有解决方案欢迎告诉我~