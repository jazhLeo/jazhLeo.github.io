---
title: 「从入门到入狱系列」 - AWS+SS 0成本徒手翻墙
date: 2017-08-26
layout: post
comments: true
reward: true
tags:
    - 从入门到入狱
    - 工具
---
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=1077483&auto=0&height=66"></iframe>   

## 开篇日常BB 
前几天写了个「从入门到入狱系列」的文章，介绍了 xssgame 的通关经验，心里美滋滋。 

分享出来后，萌新 DaLao 们纷纷表示「这他妈根本打不开，看他妈的棒棒鸡」
难道求学的欲望没有让你的内心躁动不安吗。  
![askwiki](/Image/askwiki.jpg)  
<!--more-->
话说这一切的起因，要从古老的**「GFW(Great Firewall)」**说起。
那是一个……  自！己！去！查！ 
![xiangbudaoba](/Image/xiangbudaoba.jpg)
好，我们终于费劲笔墨终于把 GFW 讲完了，接下来我们说说：翻墙干嘛。
自！己！去！…… 还是说两句吧，要被打。
起初呢，是因为听说「w\*w.s\*x.c\*m」「w\*w.pr\*nh\*b.com」这些网站有一些「不可描述的东西」，后来呢，就出不来了……
![laosiji](/Image/laosiji.jpg)

## 关于免费与付费的 vpn
**说来真是惭愧，人类始终对于繁衍后代有着执着且坚定的信念。「墙挡上了不让看？墙在哪呢？把墙扒了，我要看人儿！」**
萌新在日常生活中也会因为各种各样的原因接触到「翻墙」这个词汇，有点行动力的小伙伴可能会下载各种免费 vpn 软件来排解内心的寂寞。
但是呢，免费 vpn 的坑实在是太多了，萌新还是记住一点，天下没有免费的早餐、午餐、晚餐还有夜宵。
```
┓┏┓┏┓┃
┛┗┛┗┛┃＼○／
┓┏┓┏┓┃  /      STOP
┛┗┛┗┛┃ノ)
┓┏┓┏┓┃         USE
┛┗┛┗┛┃ 
┓┏┓┏┓┃         FREE
┛┗┛┗┛┃ 
┓┏┓┏┓┃         VPN
┃┃┃┃┃┃
```
这是余弦 DaLao 曾经发过的很经典的一个……一个……这叫啥？算了，就那么个玩意。

那对萌新来讲，不使用「free vpn」应该换成哪些姿势呢。首先想到的可能是「付费 vpn」，付费的其实也一样有风险。  
我们使用 vpn 时是把网络请求发给一个中间人，中间人帮你去请求，请求完以后再把得到的回复发给你。  
这个过程中，你的风险成本是不随着「付费」和「免费」的转换而有所降低或上升的。 
只能说付费的可能收了钱会相对的良心一点，但是我们不能把良心当作理所应当，你觉得呢。

## AWS+SS 的姿势
前面说到付费与免费都会在服务商的胯下颤抖  
![buranlie](/Image/burranlie.jpg)
那我们就自己搭建一个自己的梯子啊～
自己搭建服务时选择服务器要满足几个条件：  
>1.有一台属于我们自己的服务器。  
2.服务器要有「公网 ip」便于我们去访问服务器。  
3.服务器要能正常访问诸如~~「www.google.com」~~「w\*w.s\*x.c\*m」「w\*w.pr\*nh\*b.com」这些网站。  
4.最好不花钱～

然后我们发现「亚马逊AWS(Amazon Web Services) 云服务」很适合我们:  
>1.给你一台属于你的服务器。  
2.给你分配一个「公网 ip」  
3.有很多海外服务器可供选择，海外服务器不受 GFW 的限制可以访问「不可描述的网站」。  
4.免费一年～  

![memeda](/Image/memeda.jpg)  
当然，美中不足就是每个月15G的流量限制，超出要花钱 QAQ。

有了服务器以后呢，我们需要在上面跑一个24小时供你使用的翻墙程序「shadowsocks 服务端」。  
有了服务器上随时待命的程序以后，我们只需要在本地安装一个相应的客户端软件与服务端链接，然后就能顺利通过你自己的服务器访问「不可描述的网站」了。  
绿色环保无虫害，重点是除了你，没人知道你在看些奇怪的东西。

## 可能是最不正经的教程
![aws_welcome](/Image/aws_welcome.jpg)
### 注册
其实呢，按照你的生物本能去注册就可以……
这里需要准备一张信用卡，可能会收到「预授权」费用1美刀消费的提示，先别慌，去查查这个「预授权」的交易类型是什么意思让自己放个心。  
其中有一个身份认证环节，要填写手机号，会有电话打过来要你给出页面中的 pin 码，你可以在手机输入或者一个数字一个数字的读给他听(用英语)，听说好多输入的都有问题，那我这边建议读吧。如果出现点击「Call Me Now」按钮上方会提示你错误，让你联系他们的情况，就点进去提交 `case`，说明好自己的问题：
![aws_case](/Image/aws_case.jpg)  
下面选择 `phone` 或者 `chat`。
客服人员会联系你，不论 `phone` 还是 `chat` 都是英语哦～听不懂的话不用慌，对付对付以后你的邮箱会收到邮件，点击链接进入以后发现这一步已经通过了。

顺利通过注册后，我们点击这个 EC2 进行创建。  
![aws_ec2](/Image/aws_ec2.png)

接着如果出现如下情况，就等几个小时后再来试试，最多不超过24小时。  
![aws_wait](/Image/aws_wait.png)

### 创建
创建的时候呢注意左上角要选择推荐选择东京节点  
![aws_jiedian](/Image/aws_jiedian.jpg)  

为什么推荐东京呢，显然不应该是因为东京的各位老师才选择的，因为东京的节点在物理上离我们国家比较近，能为我们提供较低的延迟，这点在「ssh」（可以理解为远程对我们服务器发送指令进行操作的一种方式）我们的服务器时感受明显，我身边朋友是美国节点，「ssh」远程连接时，终端中打字延迟感明显，很难受。  
当然只是相对来说，比较推荐东京，其他节点也是可以选择的。

接下来就是中规中矩的创建流程了。  
![aws_service](/Image/aws_service.png)

![aws_qidong](/Image/aws_qidong.png)

接下来选择 Amazon 的系统影像
这里选择，Amazon Linux AMI、RedHat、Ubuntu都行。我这边选择的是Ubuntu。 
下面就是选择实例的类型了，也就是选择你的服务器型号，因为我们是抱着不花钱的打算来的，所以我们这里选择 「t2.micro」,注意一下它下面标着「符合条件的免费套餐」。

接下来的步骤暂时不需要配，我们直接点击右下角含有启动字样的蓝色按钮结束这个流程就好。

接着有一点需要注意一下，提示你创建秘钥对时，一定要下载下来保存好，多备份几个地方，没有这个东西，你就没办法连接你的服务器了。

接下来提示你正在启动，并提示你创建账单警告，点进去创建一下，凭借从小锻炼的阅读理解能力进行勾选.

再下一步就是连接我们服务器了，找到自己的实例如果没有启动的话点击启动，然后在上面右键连接我们的服务器。
![aws_connect](/Image/aws_connect.jpg)

连接操作官方文档很详细，可以根据你的系统型号进行选择这里不详细讲了，大家去看文档吧。[链接文档](https://docs.aws.amazon.com/zh_cn/AWSEC2/latest/UserGuide/AccessingInstances.html?icmpid=docs_ec2_console)  
### 安装 shadowsocks
接下来就不要自信一波了，老老实实的跟着步骤来吧，免得踩坑。
下面的「#」代表是我写的注释，不用在终端中输入，不带「#」的就照着样子在终端中敲出来后按回车，然后等结果，如果报错了的话，可以先留言，等不及自己去查一波也是可以的～
```
# 获取root权限
sudo -s

# 更新apt-get
apt-get update

# 安装 python 的 包管理工具 pip
apt-get install python-pip

# 安装shadowsocks
pip install shadowsocks
```
### 创建 shadowsocks 的配置文件
我们安装好以后需要自己创建一下我们的配置文件，启动的时候再指定这个配置文件就可以了。
配置文件的位置可以自由选择，不过建议放在`/etc/shadowsocks/`下面,这个目录还没有，我们要创建一下。
```
# 创建 shadowsocks 目录
mkdir /etc/shadowsocks
```
接下来我们把配置文件的内容写好。
```
# 创建并编辑我们的配置文件 
vim /etc/shadowsocks/config.json
```
如果有没使用过`vim`的小伙伴不用担心。  
我们使用`vim`命令进入编辑模式以后，确定自己的键盘输入法为英文大小写切换为小写。  
按下`i`键，观察左下方是否出现`INSERT`字样，如果有，证明我们进入了编辑模式。  
接下来我们图省事的话可以把我下面(的代码)直接复制粘贴进去。粘贴的姿势有的是右键就可以粘贴，有的是右键菜单中有粘贴，大家自己研究，如果不行那就只能手动敲进去了。我写的注释记得要删掉哦。
如果是手动敲进去，注意请保持自己的输入法维持在英文状态。  
写好以后按`esc`键，再按下`:`键，左下角出现`:`字样，然后输入`wq`,这两个字母分别对应保存和退出，然后回车就ok了。
```
{
# 配置服务器 ip，这里填写 0.0.0.0
"server":"0.0.0.0",
# 配置端口号及各自的密码
# 这里我用的多用户配置，大家也可以采用单用户配置
# 端口号如果不懂可以选择跟我相同的，密码修改为自己想设置的密码就好
"port_password":{
     "20610":"MII**********QEA"
     },
# 超时时间 这个不用修改
"timeout":300,
# 加密算法
"method":"aes-256-cfb",
# true 或者 false 开启后能降低延迟
"fast_open":false
}
```
关于上面的 `fast_open` 感兴趣的可以来这里看[TCP-Fast-Open](https://github.com/shadowsocks/shadowsocks/wiki/TCP-Fast-Open) 我们继续。
### 启动 shadowsocks
```
# 启动
ssserver -c /etc/shadowsocks/config.json -d start
# 停止
ssserver -c /etc/shadowsocks/config.json -d stop
# 重新启动
ssserver -c /etc/shadowsocks/config.json -d restart
```
我们这里使用启动命令启动一下试试。
如果启动后不确定自己是否成功，可以使用
```
ps -aux | grep ssserver
```
`ps`是 linux 查看系统进程的工具，感兴趣可以看看这个命令：[ps命令](http://man.linuxde.net/ps)  
如果像下图一样能查到相关结果，证明这个进程已经启动成功了。  
![aws_cmd_ps](/Image/aws_cmd_ps.jpg)  
我们接下来把 shadowsocks 服务端进程设置为开机启动：
```
vim /etc/rc.local
```
按`i`进入`Insert`模式，在`exit 0`上面的空白行加入：
```
sudo ssserver -c /etc/shadowsocks/config.json -d start
```
然后`esc`接着`:`接着`wq`

然后我们可以去控制台右键我们的实例重启一下，再通过之前连接服务器的姿势连进去检查一下我们的`ssserver`进程是否在运行。

光配置了还不够，aws 还不允许正常访问我们刚刚配置的端口，我们回到 aws 的 web 控制台。
配置好我们的出入站规则：  
![aws_secure](/Image/aws_secure.jpg)

`端口范围`这里填上我们前面设置的端口，如果是多用户配置需要把设置的端口都添加进去。因为我配置的是多用户，所以端口就开放的我使用的端口，单用户直接开放对应端口就好。
`来源`下拉框中选择“任何位置”。

到这里，我们服务器端的操作就结束了。

### 客户端
客户端相对简单了很多
来这里下载自己操作系统的客户端：[shadowsocks 客户端](http://shadowsocks.org/en/download/clients.html)  
下载好以后，配置一下，mac 和 windows 下大同小异，以 mac 的客户端作为讲解：  
![aws_ss_client](/Image/aws_ss_client.jpg)  

其中的`地址`填写的为 aws 控制台中 ipv4 字段中的 ip：
![aws_ip_config](/Image/aws_ip_config.jpg)
端口号呢，就是你`config.json`文件中配置的端口号，密码也是你配置的密码，加密方式也选择配置文件中配置的那个。  
关于我们客户端的使用，平时用自动代理就好了，如果访问不了，就切全局，简单粗暴。
![aws_ss_client_config](/Image/aws_ss_client_config.jpg)
为了防止流量超出或者此悲剧发生，需要设置账单警报。  
还有，到期前把你账户下所有AWS实例关闭并销毁，如果使用了 ebs， ebs 也要记得释放，都是辛酸泪...

好了，裤腰带解开这么半天，左手的纸巾准备好，音量自觉调低，我们准备出发～。
本次列车途经 ~~「www.google.com」~~「w\*w.s\*x.c\*m」「w\*w.pr\*nh\*b.com」还没有上车的旅客请抓紧时间上车。
![xingfu](/Image/xingfu.jpg)