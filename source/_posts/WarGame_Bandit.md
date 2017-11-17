---
title: 「WarGame 系列」 Bandit 
date: 2017-11-17 21:29:00
layout: post
comments: true
reward: true
tags:
    - Wargame
    - Linux 
---
这次安利一个游戏。
```sh
 _                     _ _ _   
| |__   __ _ _ __   __| (_) |_ 
| '_ \ / _` | '_ \ / _` | | __|
| |_) | (_| | | | | (_| | | |_ 
|_.__/ \__,_|_| |_|\__,_|_|\__|
```
游戏呢是给初学者设计的，新手刚好可以一边过关一边学习一些 Linux 的基本操作。
先说说这个游戏怎么玩吧：
我们这个游戏有27关，对应27对账号密码组合。开始的时候送我们一个第一关的账号密码：「bandit0」-「bandit0」。
我们需要使用这个账号登陆服务器并找到「bandit1」的密码，然后使用得到的密码登陆「bandit1」，再找到「bandit2」的密码……

咦，等一下，这游戏怎么有种……「你追我，如果你追到我，我就让你嘿嘿嘿」的感觉呢……
<!--more-->
然而通关以后冰冷的现实告诉我：  
![](/Image/bandit_resources/27CB93E553A707C15C1AB4DCFA52EFBA.jpg)
……

游戏呢，是需要自己动手玩的，这篇文章只是记录了我的通关思路和过程，可以参考，但也别无脑拷。
下面的通关记录是在玩游戏的过程中边玩边记录的，所以比较正经，想上车的同学可以下车了～
前几关写的比较细，也是考虑到新手一上来就看不懂很闹心，然后又要浪费蛋白质，嗯你懂的：）

如果有好的思路，欢迎留言。

# Bandit 游戏规则
强盗战争是针对绝对的初学者。它会教授需要能够玩其他战争游戏的基础知识。

……（剩下不翻译了，懒～



Level 0 

两种姿势：1.终端直接 ssh 。2.看下面

Chrome 插件 「Secure Shell」

![](/Image/bandit_resources/851073B8A9CDFA81ECB4F7719C7BDA39.jpg)

按上图配置好各项后按「Enter」或点击「连接」

![](/Image/bandit_resources/608E268171D41C5563D62EA2F9F30FDE.jpg)

输入页面中告诉我们的密码：bandit0

（输入密码时是隐式输入，光标不会动，不要以为卡了，正常输入后按回车等待反馈信息就好）

![](/Image/bandit_resources/BE3283D120238F44FADC24C8EA3F5CB8.jpg)

见到这个东西就证明我们使用 SSH 连接服务器成功了。我们就可以用手中的这台电脑操作这台服务器了。

---
Level 0 - 1

描述：下一级别的密码存储在位于主目录中的名为readme的文件中。使用此密码 SSH 登录到bandit1。
密码：boJ9jbbUNNfktd78OOpsqOltutMc3MY1

```sh
bandit0@bandit:~$ ls
readme
bandit0@bandit:~$ cat readme
boJ9jbbUNNfktd78OOpsqOltutMc3MY1
```

心路历程：借上图中的红框，「~」表明我们登陆后的当前位置是该用户的主目录，`ls` 列出当前目录下的文件，发现确实有这个「readme」文件。使用 `cat` 命令打印出文件的内容。
拿到这个所谓的密码之后，我们要使用 exit 命令退出这次登陆（直接刷新网页更快～）。

```sh
bandit0@bandit:~$ exit 
logout
Connection to bandit.labs.overthewire.org closed.
NaCl 插件已退出，状态代码为：0。
重新连接(&R)、选择其他连接(&C)或退出(&E)
```

这时我们输入 C，然后配置新的连接，使用新的用户名「bandit1」和密码「boJ9jbbUNNfktd78OOpsqOltutMc3MY1」登陆就ok了。
提示：如果大家不知道去哪里查找命令的话，可以在终端中 `man 命令`，例如 `man ls`。天生英文抗性为负的朋友可以来这里查👉[Linux 命令大全](http://man.linuxde.net/)，但并不能完全取代 man 手册。

---

Level 1 - 2

描述：下一级别的密码存储在一个名为 - 位于主目录中的文件中。

密码：CV1DtqXWVFXTvM2F0k09SHz0YwRINYA9

```sh
bandit1@bandit:~$ ls
-
bandit1@bandit:~$ cat ./-
CV1DtqXWVFXTvM2F0k09SHz0YwRINYA9
```

心路历程：文件名是「-」，如果是其他名字直接 `cat filename` 就好了，然而如果是 `cat -` 的话，就有一些小问题，因为 bash 中会用「-」来接受命令行参数，未避免冲突，我们使用「./」来表示当前目录，那「-」文件就表示为「./-」。

---

Level 2 - 3

描述：下一级别的密码存储在位于主目录中的文件名含有「空格」的文件中

密码：UmHadQclWmgdLOKQ3YNgjWxGoRMb5luK

```sh
bandit2@bandit:~$ ls
spaces in this filename
bandit2@bandit:~$ ll
total 28
drwxr-xr-x  3 bandit2 bandit2 4096 Nov 10 15:23 ./
drwxr-xr-x 30 root    root    4096 Nov 10 15:23 ../
-rw-r--r--  1 bandit2 bandit2  220 Apr  9  2014 .bash_logout
-rw-r--r--  1 bandit2 bandit2 3637 Apr  9  2014 .bashrc
drwx------  2 bandit2 bandit2 4096 Nov 10 15:23 .cache/
-rw-r--r--  1 bandit2 bandit2  675 Apr  9  2014 .profile
-rw-r-----  1 bandit3 bandit2   33 Sep 28 14:04 spaces in this filename
bandit2@bandit:~$ cat spaces\ in\ this\ filename 
UmHadQclWmgdLOKQ3YNgjWxGoRMb5luK
```

心路历程：这关使用 `ls` 后没办法确定文件的数量（1～4个），使用 `ll` 命令（`ls -alF`）查看一下发现是一个单独的文件（「.」开头的文件为隐藏文件，故之前没有显示）。这里的空格面临的情况跟之前的「-」差不多，避免混淆，使用「\ 」（斜线后跟空格表示空格）。
提示：`ll` 并不是 linux 的基本命令，而是「.bashrc」这个文件中的
```bash
alias ll='ls -alF'
```
这句话定义的。相当于对 `ls -alF` 这个命令起了一个别名。
因为这个别名的定义较为普遍，所以我没看这个文件内容之前就习惯性的试了下。

---



```sh
bandit3@bandit:~$ ls
inhere
bandit3@bandit:~$ cd inhere 
bandit3@bandit:~/inhere$ ls
bandit3@bandit:~/inhere$ ll
total 12
drwxr-xr-x 2 root    root    4096 Sep 28 14:04 ./
drwxr-xr-x 4 bandit3 bandit3 4096 Nov 10 15:42 ../
-rw-r----- 1 bandit4 bandit3   33 Sep 28 14:04 .hidden
bandit3@bandit:~/inhere$ cat .hidden 
pIwrPrtPN36QITSp3EQaw936yaFoFgAB
```

心路历程：进入「inhere」目录后使用 `ls` 命令看不到非隐藏文件，我们使用 `ll` 就ok了。

---

Level 4 -5

描述：下一级别的密码存储在inhere目录中唯一的人类可读文件中。提示：如果你的终端搞砸了，试试“重置”命令。

密码：koReBOKuIDDepwhWk7jZC0RTdopnAYKh

```sh
bandit4@bandit:~$ ls
inhere
bandit4@bandit:~$ cd inhere
bandit4@bandit:~/inhere$ ls
-file00  -file01  -file02  -file03  -file04  -file05  -file06  -file07  -file08  -file09
bandit4@bandit:~/inhere$ file ./*
./-file00: Non-ISO extended-ASCII text, with CR line terminators, with escape sequences
./-file01: data
./-file02: data
./-file03: data
./-file04: data
./-file05: data
./-file06: data
./-file07: ASCII text
./-file08: data
./-file09: data
bandit4@bandit:~/inhere$ cat ./-file07
koReBOKuIDDepwhWk7jZC0RTdopnAYKh
```

心路历程：因为描述中提到了这个文件是唯一人类可读的文件，大概率会与其他文件不太一样，我们选择使用 `file` 命令查看文件类型后发现只有一个 「ASCII text」文件，那基本就是它了。

---

Level 5 - 6

描述：下一级别的密码存储在inhere目录下的某个文件中，并具有以下所有属性： 人类可读，大小为1033字节，不可执行。

密码：DXjZPULLxYr17uwoI01bNLQbtFemEgo7

```sh
bandit5@bandit:~$ ls
inhere
bandit5@bandit:~$ cd inhere; ll
total 88
drwxr-x--- 22 root    bandit5 4096 Sep 28 14:04 ./
drwxr-xr-x  4 bandit5 bandit5 4096 Nov 11 06:01 ../
drwxr-x---  2 root    bandit5 4096 Sep 28 14:04 maybehere00/
drwxr-x---  2 root    bandit5 4096 Sep 28 14:04 maybehere01/
drwxr-x---  2 root    bandit5 4096 Sep 28 14:04 maybehere02/
drwxr-x---  2 root    bandit5 4096 Sep 28 14:04 maybehere03/
drwxr-x---  2 root    bandit5 4096 Sep 28 14:04 maybehere04/
drwxr-x---  2 root    bandit5 4096 Sep 28 14:04 maybehere05/
drwxr-x---  2 root    bandit5 4096 Sep 28 14:04 maybehere06/
drwxr-x---  2 root    bandit5 4096 Sep 28 14:04 maybehere07/
drwxr-x---  2 root    bandit5 4096 Sep 28 14:04 maybehere08/
drwxr-x---  2 root    bandit5 4096 Sep 28 14:04 maybehere09/
drwxr-x---  2 root    bandit5 4096 Sep 28 14:04 maybehere10/
drwxr-x---  2 root    bandit5 4096 Sep 28 14:04 maybehere11/
drwxr-x---  2 root    bandit5 4096 Sep 28 14:04 maybehere12/
drwxr-x---  2 root    bandit5 4096 Sep 28 14:04 maybehere13/
drwxr-x---  2 root    bandit5 4096 Sep 28 14:04 maybehere14/
drwxr-x---  2 root    bandit5 4096 Sep 28 14:04 maybehere15/
drwxr-x---  2 root    bandit5 4096 Sep 28 14:04 maybehere16/
drwxr-x---  2 root    bandit5 4096 Sep 28 14:04 maybehere17/
drwxr-x---  2 root    bandit5 4096 Sep 28 14:04 maybehere18/
drwxr-x---  2 root    bandit5 4096 Sep 28 14:04 maybehere19/
bandit5@bandit:~/inhere$ find . -type f -size 1033c
./maybehere07/.file2
bandit5@bandit:~/inhere$ cat ./maybehere07/.file2
DXjZPULLxYr17uwoI01bNLQbtFemEgo7
```

心路历程：第二个命令 `cd inhere; ll` 两个命令通过分号间隔，会分别执行。我们发现 inhere 目录下还有好多目录（文件夹）。这如果一个一个找就累死了。我们使用 `find` 命令查找一下。`-type f`指定文件类型为普通文件，`-size 1033c` 指定文件大小为 1033 bytes。具体可参考我上面给出的 [Linux 命令大全](http://man.linuxde.net/)
提示：`ls -l` 后每行开头的字符串中第一个字母是「d」表示为「directory」- 目录。我们使用的 `ll` 在 .bashrc 文件中被定义为 `ls -alF` 其中 `a` 是显示隐藏文件， `l` 是详细信息， `F` 是每条后面追加文件类型标识符，我们输出的每行内容尾部都有的「／」就表示这些都是目录。

---

Level 6 - 7

描述：下一级别的密码存储在服务器的某个位置，具有以下所有属性： 所属用户bandit7，所属用户组bandit6，拥有 33个字节的大小

密码：HKBPTKQnIay4Fw76bEy8PVxKEDQRKTzs

```sh
bandit6@bandit:~$ find / -group bandit6 -user bandit7 -size 33c 2>/dev/null
/var/lib/dpkg/info/bandit7.password
bandit6@bandit:~$ cat /var/lib/dpkg/info/bandit7.password
HKBPTKQnIay4Fw76bEy8PVxKEDQRKTzs
```

心路历程：描述中说「存储在服务器的某个位置」，我们使用「／」这个 linux 根目录作为我们搜索的最顶层。其中我们还用到了 Linux 下的「黑洞」，把错误输入全扔了进去。这样就只返回了正确结果。
如果把错误输出重定向到「黑洞」中，就是酱紫滴：  
![IMAGE](/Image/bandit_resources/6672B54C1537E1423C2D857DA346FF5B.jpg)
太多干扰信息不好观察。
提示：「2>/dev/null」中「2」表示错误输出，「>」是重定向符号表示把信息送到哪里，「/dev/null」是「Linux黑洞」。这里有篇文章供参考👉[shell中>/dev/null 2>&1是什么鬼？](http://www.kissyu.org/2016/12/25/shell%E4%B8%AD%3E%20:dev:null%202%20%3E%20&1%E6%98%AF%E4%BB%80%E4%B9%88%E9%AC%BC%EF%BC%9F/)

---

Level 7 - 8

描述：下一级的密码存储文件data.txt中 “millionth” 的下一个单词。

密码： cvX2JJa4CFALtqS87jk27qwqGhBM9plV

```sh
bandit7@bandit:~$ ll
total 4112
drwxr-xr-x  3 bandit7 bandit7    4096 Nov 11 07:48 ./
drwxr-xr-x 30 root    root       4096 Nov 11 07:48 ../
-rw-r--r--  1 bandit7 bandit7     220 Apr  9  2014 .bash_logout
-rw-r--r--  1 bandit7 bandit7    3637 Apr  9  2014 .bashrc
drwx------  2 bandit7 bandit7    4096 Nov 11 07:48 .cache/
-rw-r--r--  1 bandit7 bandit7     675 Apr  9  2014 .profile
-rw-r-----  1 bandit8 bandit7 4184396 Sep 28 14:04 data.txt
bandit7@bandit:~$ grep millionth data.txt
millionth       cvX2JJa4CFALtqS87jk27qwqGhBM9plV
```

心路历程：我们查看文件详细内容时发现这个文件比较大，光靠肉眼找是没戏了。所以我们借助命令 `grep` 找到 「milionth」这个单词所在行的内容，后面跟着下一关的密码。（其实最开始我惯性的`ls` 然后 `cat` 了一下……发觉不太对赶紧 `ctrl+c` 终止， `ll` 看了眼大小……）

---

Level 8 - 9

描述：下一个级别的密码存储在文件data.txt中，并且是仅出现一次的文本行。

密码：UsvVyFSfZZWbi6wgC7dAFyFuR6jQQUhR

```sh
bandit8@bandit:~$ ls
data.txt
bandit8@bandit:~$ sort data.txt | uniq -u
UsvVyFSfZZWbi6wgC7dAFyFuR6jQQUhR
```

心路历程：又描述得知这是一个文本文件，里面有很多行字符串，我们需要找到其中仅出现一次的行。使用 `sort` 命令对文件排序后再对其结果进行 `uniq -u` 只显示其中的单一行。
提示：uniq -u 是上下相邻两行对比得到是否为单一行：
```bash
bandit8@bandit:~$ echo -en "1\n1\n2\n1\n" >test.txt
bandit8@bandit:~$ cat test.txt 
1
1
2
1
bandit8@bandit:~$ uniq -u test.txt 
2
1
```
如例子中的前两行被认定为重复行，第二行与第三行不同，第三行与第四行不同，所以「2」被当作了唯一行。第四行中的「1」同理。
所以我们这里要先对文件排序：
```bash
bandit8@bandit:~$ sort test.txt > sorted.txt; cat sorted.txt
1
1
1
2
```
然后再筛选唯一行：
```bash
bandit8@bandit:~$ uniq -u sorted.txt
2
```

---

Level 9 - 10

描述：下一个级别的密码存储在文件data.txt中的几个人类可读字符串之一，从几个“=”字符开始。

密码：truKLdjsbJ5g7yyJ2X2R0o3a5HQJFuLk

```sh
bandit9@bandit:~$ ls
data.txt
bandit9@bandit:~$ strings data.txt | grep ==
|========== the
========== password
========== is
========== truKLdjsbJ5g7yyJ2X2R0o3a5HQJFuLk
```

心路历程：描述中提到了几个人类可读字符，那我们用 strings 筛选一下（strings是在文件中查找可打印字符串并输出长度为4个或更多的字符串，遇到换行或空字符结束）。其中也提到从几个「=」符号开始，那我们用 grep 命令筛选 含有「==」的字符串看看。

---

Level 10 - 11

描述：下一级别的密码存储在data.txt文件中，该文件包含base64编码数据

密码：IFukwKGsFW8MOq3IRFqrxE1hxTNEbUPR

```sh
bandit10@bandit:~$ ls
data.txt
bandit10@bandit:~$ cat data.txt
VGhlIHBhc3N3b3JkIGlzIElGdWt3S0dzRlc4TU9xM0lSRnFyeEUxaHhUTkViVVBSCg==
bandit10@bandit:~$ base64 -d data.txt 
The password is IFukwKGsFW8MOq3IRFqrxE1hxTNEbUPR
```

心路历程：题目告诉我们使用 base64 编码了数据，那我们解码就好了。

---

Level 11 - 12

描述：下一级的密码存储在文件data.txt中，其中所有小写（a-z）和大写（A-Z）字母已经被旋转了13个位置

密码：5Te8Y4drgCRfCx8ugdwuEX8KFC6k2EUu

```sh
bandit11@bandit:~$ ls
data.txt
bandit11@bandit:~$ cat data.txt
Gur cnffjbeq vf 5Gr8L4qetPEsPk8htqjhRK8XSP6x2RHh
bandit11@bandit:~$ cat data.txt | tr 'a-zA-Z' 'n-za-mN-ZA-M'
The password is 5Te8Y4drgCRfCx8ugdwuEX8KFC6k2EUu
```

心路历程：这里我们用到了 `tr` 命令，参数为两个字符集，把第一个字符集中的字符替换为第二个字符集中的对应字符。题目中说旋转了13个位置，相当于26个字母前十三个和后十三个换了个位置。按照这样的对应关系，调整给出的字符集。
提示：有人会写程序来做替换，但是对于这道题来讲 tr 会方便很多。虽然这个命令不常用，用起来还是很爽的。

---

Level 12 - 13

描述：下一级的密码存储在data.txt文件中，该文件是一个已被重复压缩的文件的十六进制转储文件。对于这个级别，可以在 `/tmp` 下使用 `mkdir` 创建一个工作的目录。例如：`mkdir /tmp/myname123` 。然后使用 `cp` 复制数据文件，并使用 `mv` 重命名（阅读manpages！）  
密码：8ZjyCRiBWFYkneahHwxCv3wb2a1ORpYL

```sh
bandit12@bandit:~$ ls
data.txt
bandit12@bandit:~$ mkdir /tmp/GeekaLeo123 ; cp data.txt /tmp/GeekaLeo123/data_1 ; cd /tmp/GeekaLeo123
bandit12@bandit:/tmp/GeekaLeo123$ cat data_1 ; file data_1
0000000: 1f8b 0808 5601 cd59 0203 6461 7461 322e  ....V..Y..data2.
0000010: 6269 6e00 013f 02c0 fd42 5a68 3931 4159  bin..?...BZh91AY
0000020: 2653 5914 13ca ff00 001b ffff faef 7fff  &SY.............
0000030: f9fb a79e de5b efbb ffff fd7f cf7b fbff  .....[.......{..
0000040: ff7f afbd 8ddb ff77 f752 ffff b001 3b56  .......w.R....;V
0000050: 6100 01a3 d400 0000 0068 d0d0 69a0 0000  a........h..i...
0000060: 007a 867a 9034 0340 6401 a340 0000 f280  .z.z.4.@d..@....
0000070: 01ea 0d1a 1a1a 0d01 9034 0e40 0000 0686  .........4.@....
0000080: 8d00 00c8 6819 3406 8d1e a003 d400 0c80  ....h.4.........
0000090: f534 0034 309a 0006 83d4 0000 01a6 80c8  .4.40...........
00000a0: 1900 6103 a01b 5034 69a1 a1a0 6868 3403  ..a...P4i...hh4.
00000b0: 4341 a340 64f5 3400 0680 6801 a340 6800  CA.@d.4...h..@h.
00000c0: 000d 0068 0340 00d0 01a1 a068 3430 3516  ...h.@.....h405.
00000d0: 1543 1355 0d26 5d39 505d 970e fcac 9c37  .C.U.&]9P].....7
00000e0: 0ec4 62b1 05bc 607b 68e4 c4f4 efa3 32f8  ..b...`{h.....2.
00000f0: 6d9e 9c52 9d50 36a0 5598 b734 a0c4 7683  m..R.P6.U..4..v.
0000100: 04e3 7cbd ec15 ea5d 1db8 1283 ea8b 4318  ..|....]......C.
0000110: 0358 207a a12c 554f 4a2d 5428 eb47 6e7c  .X z.,UOJ-T(.Gn|
0000120: ffdc 4018 60fc 0690 28ec 12b9 5d02 eecb  ..@.`...(...]...
0000130: 11d4 e987 eb36 d574 e87c 2e67 f803 2cdf  .....6.t.|.g..,.
0000140: b465 9110 302d a9c0 0c33 3e55 573c 8818  .e..0-...3>UW<..
0000150: 76cf 6c6a 5efd c51e 20ec 2358 f5a8 694e  v.lj^... .#X..iN
0000160: bc7a bc91 0376 ebc8 61a2 33c1 97e9 936d  .z...v..a.3....m
0000170: df2b ceef 0a4f 6039 8cb5 b9cc d490 607b  .+...O`9......`{
0000180: ff20 e253 1875 489a 0465 3643 497e 8348  . .S.uH..e6CI~.H
0000190: 51dd d85e 5038 9c31 fcc3 bb2b 6157 0413  Q..^P8.1...+aW..
00001a0: 7b90 6633 f706 0005 3dc0 7d9b f4ba b026  {.f3....=.}....&
00001b0: 1a91 eca8 8423 7d1b 0401 d150 0c14 1fc5  .....#}....P....
00001c0: ef57 ef39 3e53 dfc5 c2ce 29de 871f dce8  .W.9>S....).....
00001d0: 2f85 3ff8 1f16 a894 6677 d26e a7b6 2550  /.?.....fw.n..%P
00001e0: bc05 d2e6 51f8 d799 52f1 2783 a642 db4e  ....Q...R.'..B.N
00001f0: 344f b1a4 608c 4249 20f6 549e 64db e2e8  4O..`.BI .T.d...
0000200: 55da 10b5 adfc 28fd 1a8c 7e81 4188 5028  U.....(...~.A.P(
0000210: 29ec ddf4 4bef 8de6 9a0b 0d49 14e7 d30e  )...K......I....
0000220: 48a4 55b8 5729 7484 2900 e001 e451 7290  H.U.W)t.)....Qr.
0000230: 057c f004 bb85 0788 0139 d730 8a08 0448  .|.......9.0...H
0000240: 4a45 0565 243c 7017 9906 e644 ff8b b922  JE.e$<p....D..."
0000250: 9c28 480a 09e5 7f80 c978 5ff9 3f02 0000  .(H......x_.?...
data_1: ASCII text
bandit12@bandit:/tmp/GeekaLeo123$ xxd -r data_1 > data_2 ; file data_2
data_2: gzip compressed data, was "data2.bin", from Unix, last modified: Thu Sep 28 14:04:06 2017, max compression
bandit12@bandit:/tmp/GeekaLeo123$ mv data_2 data_3.gz ; gzip -d data_3.gz ; ls
data_1  data_3
bandit12@bandit:/tmp/GeekaLeo123$ file data_3
data_3: bzip2 compressed data, block size = 900k
bandit12@bandit:/tmp/GeekaLeo123$ mv data_3 data_4.bz2 ; bzip2 -d data_4.bz2 ; ls
data_1  data_4
bandit12@bandit:/tmp/GeekaLeo123$ file data_4
data_4: gzip compressed data, was "data4.bin", from Unix, last modified: Thu Sep 28 14:04:06 2017, max compression
bandit12@bandit:/tmp/GeekaLeo123$ mv data_4 data_5.gz ; gzip -d data_5.gz ; ls
data_1  data_5
bandit12@bandit:/tmp/GeekaLeo123$ file data_5
data_5: POSIX tar archive (GNU)
bandit12@bandit:/tmp/GeekaLeo123$ mv data_5 data_6.tar ; tar -xvf data_6.tar ; ls
data5.bin
data5.bin  data_1  data_6.tar
bandit12@bandit:/tmp/GeekaLeo123$ file data5.bin 
data5.bin: POSIX tar archive (GNU)
bandit12@bandit:/tmp/GeekaLeo123$ mv data5.bin data_7.tar; tar -xvf data_7.tar ; ls 
data6.bin
data6.bin  data_1  data_6.tar  data_7.tar
bandit12@bandit:/tmp/GeekaLeo123$ file data6.bin
data6.bin: bzip2 compressed data, block size = 900k
bandit12@bandit:/tmp/GeekaLeo123$ mv data6.bin data_8.bz2 ; bzip2 -d data_8.bz2 ; ls
data_1  data_6.tar  data_7.tar  data_8
bandit12@bandit:/tmp/GeekaLeo123$ file data_8
data_8: POSIX tar archive (GNU)
bandit12@bandit:/tmp/GeekaLeo123$ mv data_8 data_9.tar ; tar -xvf data_9.tar ; ls
data8.bin
data8.bin  data_1  data_6.tar  data_7.tar  data_9.tar
bandit12@bandit:/tmp/GeekaLeo123$ file data8.bin
data8.bin: gzip compressed data, was "data9.bin", from Unix, last modified: Thu Sep 28 14:04:06 2017, max compression
bandit12@bandit:/tmp/GeekaLeo123$ mv data8.bin data_10.gz ; gzip -d data_10.gz ; ls
data_1  data_10  data_6.tar  data_7.tar  data_9.tar
bandit12@bandit:/tmp/GeekaLeo123$ file data_10
data_10: ASCII text
bandit12@bandit:/tmp/GeekaLeo123$ cat data_10
The password is 8ZjyCRiBWFYkneahHwxCv3wb2a1ORpYL
```

心路历程：这关比较恶心了……按照对应的加密方式解密就好，用 `file` 查看文件类型，`mv` 命令修改名字，还有`bzip2 -d`、`gzip -d`、`tar -xvf`以及`xxd -r` 这些解密解压方法。

---

Level 13 - 14

描述：下一级的密码存储在`/etc/bandit\_pass/bandit14` 中，只能由用户bandit14读取。对于这个级别，你不会得到下一个密码，但你会得到一个私人的SSH密钥，可以用来登录到下一个级别。注意：localhost是指您正在使用的机器的主机名  
密码：4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e

```sh
bandit13@bandit:~$ ls
sshkey.private
bandit13@bandit:~$ ssh -i sshkey.private bandit14@bandit.labs.overthewire.org -p 2220
The authenticity of host '[bandit.labs.overthewire.org]:2220 ([0.0.0.0]:2220)' can't be established.
ECDSA key fingerprint is ee:4c:8c:e7:57:2c:bc:63:24:b8:e6:23:27:63:72:9f.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '[bandit.labs.overthewire.org]:2220,[0.0.0.0]:2220' (ECDSA) to the list of known hosts.
...
Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.
bandit14@bandit:~$ cat /etc/bandit_pass/bandit14  
4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e
```

心路历程：告诉我们需要 ssh 登陆进去，自己去找密码，我们就登录一下好了。

---

Level 14 - 15
描述：通过将当前级别的密码提交到localhost上的端口30000，可以检索到下一级别的密码。
密码：BfMYroe26WYalil77FoDi9qh59eK5xNr

```sh
bandit14@bandit:~$ nc localhost 30000 < /etc/bandit_pass/bandit14
Correct!
BfMYroe26WYalil77FoDi9qh59eK5xNr
```

心路历程：我们使用 `nc` 命令来发送我们的密码，因为上一关我们已经知道密码在那放着了，刚好可以用到。
提示：什么？你想直接查看其他关的密码？不存在的……（权限啊老铁）

---

Level 15 -16

描述：可以通过使用SSL加密将当前级别的密码提交到本地主机上的端口30001来检索下一级别的密码。

密码：

```sh
bandit15@bandit:~$ openssl s_client -connect localhost:30001 -ign_eof                          
CONNECTED(00000003)
depth=0 CN = 8f75dc271013
verify error:num=18:self signed certificate
verify return:1
depth=0 CN = 8f75dc271013
verify return:1
---
Certificate chain
 0 s:/CN=8f75dc271013
   i:/CN=8f75dc271013
---
Server certificate
-----BEGIN CERTIFICATE-----
MIICvjCCAaagAwIBAgIJALADbwWQ0u9aMA0GCSqGSIb3DQEBCwUAMBcxFTATBgNV
BAMTDDhmNzVkYzI3MTAxMzAeFw0xNzA5MTYwNzAyMjRaFw0yNzA5MTQwNzAyMjRa
MBcxFTATBgNVBAMTDDhmNzVkYzI3MTAxMzCCASIwDQYJKoZIhvcNAQEBBQADggEP
ADCCAQoCggEBALmjBUTlmjROJUssm+rAlFADFfzrz+xCH0qUXryou5/wW8pnQ6nG
HbdeRIBwTVGFiDIKRbFdWQU4BbrfjEhyGn9d7eh/3GV09ZdvLDYRoLmJ4tDF8CiC
wGl9GufcWr3zeaNYa8CwVdtWam8umhMICrsv7B5iV9RdSQfudUtVbr26SBVyuhBm
m0t7Su6rLCrrGtshdIihjk4k67bBMpSNAOduhpp79UgIPKcwJUhRJHTcji3m/IQ8
O9zNS25oL8KhMn7e/Xe70kztstq0ShMsx8feutONnGulUOlaEMMqW+HSWgnVeG/r
mU9Nzwn++4qxe16OvvmXAzctH2RlDx7XbcsCAwEAAaMNMAswCQYDVR0TBAIwADAN
BgkqhkiG9w0BAQsFAAOCAQEADHODX5CcMLI5fdumzly5FAVg5Yc22eDGNhmyhi/N
kDhP6QYw+HW5nWEYapc9m/ZQGEEoxr+wj6qeEhscxRxpuEIcunZsLKcoAmToyXeO
ANMslQugRcGqN57Pt0h5VuctLMa3ickeVPFvV6gxJSHBNRK1iN8nrfsy+zR+stzI
xcjIuakDDxMKFtb/1TMKf4/EsimSQLS0WXLjbxfQ/J510O4/Of0tmZI0ZIG+cKmM
V5hAOtuuAk6jREfWYJQ3DB+phv7PO9s2FpofVJss5PK4NWDS7UQOv359ZOJ85ZpJ
ihGxDqV7IAHJZNM9lvFXz/+EOn1oTGW9V8bAwt34OVYoPw==
-----END CERTIFICATE-----
subject=/CN=8f75dc271013
issuer=/CN=8f75dc271013
---
No client certificate CA names sent
---
SSL handshake has read 1682 bytes and written 637 bytes
---
New, TLSv1/SSLv3, Cipher is DHE-RSA-AES256-SHA
Server public key is 2048 bit
Secure Renegotiation IS supported
Compression: NONE
Expansion: NONE
SSL-Session:
    Protocol  : SSLv3
    Cipher    : DHE-RSA-AES256-SHA
    Session-ID: C193C0A99C7335DB7D7B7B89367CF5E6A514E4FF58066E86A47070BCD02F7BC5
    Session-ID-ctx: 
    Master-Key: 5D0EB4E12667C302D7D9AAB88855BF67DA51124248593FC4B5613C6BBCF69C145BF0B37DCA5A3765A8DFD2EBDC84B248
    Key-Arg   : None
    PSK identity: None
    PSK identity hint: None
    SRP username: None
    Start Time: 1510423808
    Timeout   : 300 (sec)
    Verify return code: 18 (self signed certificate)
---
BfMYroe26WYalil77FoDi9qh59eK5xNr
Correct!
cluFn7wTiGryunymYOu4RcffSxQluehd

read:errno=0
```

心路历程：我们使用 `openssl` 的 `s\_client` SSL/TSL 客户端程序连接服务器，成功后复制粘贴我们本关的密码就能得到下一关的密码了。多看手册哦。
提示：
```bash
bandit15@bandit:~$ man openssl
bandit15@bandit:~$ man s_client
```

---

Level 16 -17
描述：可以通过将当前级别的密码提交到本地主机上31000到32000范围内的端口来检索下一级别的凭证。首先找出哪些端口有服务器正在侦听它们。然后找出哪些人说SSL和哪些不。只有一个服务器会提供下一个凭据，其他的只是发回你发给它的内容。
密码：xLYVMN9WE5zQ5vHacb0sZEVqbrp7nBTn

```sh
# 先扫描开放端口
bandit16@bandit:~$ nmap -p 31000-32000 localhost

Starting Nmap 6.40 ( http://nmap.org ) at 2017-11-12 09:08 UTC
Nmap scan report for localhost (127.0.0.1)
Host is up (0.00047s latency).
Other addresses for localhost (not scanned): 127.0.0.1
Not shown: 996 closed ports
PORT      STATE SERVICE
31046/tcp open  unknown
31518/tcp open  unknown
31691/tcp open  unknown
31790/tcp open  unknown
31960/tcp open  unknown

# 依次给5个端口发送消息,其他端口只会返回你发给它的内容。
bandit16@bandit:~$ echo "Hello World!" | nc localhost 31046    
Hello World!
bandit16@bandit:~$ echo "Hello World!" | nc localhost 31518
ERROR
140737354053280:error:1408F10B:SSL routines:SSL3_GET_RECORD:wrong version number:s3_pkt.c:351:
bandit16@bandit:~$ echo "Hello World!" | nc localhost 31691
Hello World!
bandit16@bandit:~$ echo "Hello World!" | nc localhost 31790
ERROR
140737354053280:error:1408F10B:SSL routines:SSL3_GET_RECORD:wrong version number:s3_pkt.c:351:
bandit16@bandit:~$ echo "Hello World!" | nc localhost 31960
Hello World!

# 我们发现有两个端口是SSL的，分别给这两个端口发送数据测试。
bandit16@bandit:~$ echo "Hello World" | openssl s_client -quiet -connect localhost:31518
depth=0 CN = 8f75dc271013
verify error:num=18:self signed certificate
verify return:1
depth=0 CN = 8f75dc271013
verify return:1
Hello World
# 因为回复了Hello World，确定这个属于「其他端口」这里直接发送密码给 31790 端口
bandit16@bandit:~$ cat /etc/bandit_pass/bandit16 | openssl s_client -quiet -connect localhost:31790
depth=0 CN = 8f75dc271013
verify error:num=18:self signed certificate
verify return:1
depth=0 CN = 8f75dc271013
verify return:1
Correct!
-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ
imZzeyGC0gtZPGujUSxiJSWI/oTqexh+cAMTSMlOJf7+BrJObArnxd9Y7YT2bRPQ
Ja6Lzb558YW3FZl87ORiO+rW4LCDCNd2lUvLE/GL2GWyuKN0K5iCd5TbtJzEkQTu
DSt2mcNn4rhAL+JFr56o4T6z8WWAW18BR6yGrMq7Q/kALHYW3OekePQAzL0VUYbW
JGTi65CxbCnzc/w4+mqQyvmzpWtMAzJTzAzQxNbkR2MBGySxDLrjg0LWN6sK7wNX
x0YVztz/zbIkPjfkU1jHS+9EbVNj+D1XFOJuaQIDAQABAoIBABagpxpM1aoLWfvD
KHcj10nqcoBc4oE11aFYQwik7xfW+24pRNuDE6SFthOar69jp5RlLwD1NhPx3iBl
J9nOM8OJ0VToum43UOS8YxF8WwhXriYGnc1sskbwpXOUDc9uX4+UESzH22P29ovd
d8WErY0gPxun8pbJLmxkAtWNhpMvfe0050vk9TL5wqbu9AlbssgTcCXkMQnPw9nC
YNN6DDP2lbcBrvgT9YCNL6C+ZKufD52yOQ9qOkwFTEQpjtF4uNtJom+asvlpmS8A
vLY9r60wYSvmZhNqBUrj7lyCtXMIu1kkd4w7F77k+DjHoAXyxcUp1DGL51sOmama
+TOWWgECgYEA8JtPxP0GRJ+IQkX262jM3dEIkza8ky5moIwUqYdsx0NxHgRRhORT
8c8hAuRBb2G82so8vUHk/fur85OEfc9TncnCY2crpoqsghifKLxrLgtT+qDpfZnx
SatLdt8GfQ85yA7hnWWJ2MxF3NaeSDm75Lsm+tBbAiyc9P2jGRNtMSkCgYEAypHd
HCctNi/FwjulhttFx/rHYKhLidZDFYeiE/v45bN4yFm8x7R/b0iE7KaszX+Exdvt
SghaTdcG0Knyw1bpJVyusavPzpaJMjdJ6tcFhVAbAjm7enCIvGCSx+X3l5SiWg0A
R57hJglezIiVjv3aGwHwvlZvtszK6zV6oXFAu0ECgYAbjo46T4hyP5tJi93V5HDi
Ttiek7xRVxUl+iU7rWkGAXFpMLFteQEsRr7PJ/lemmEY5eTDAFMLy9FL2m9oQWCg
R8VdwSk8r9FGLS+9aKcV5PI/WEKlwgXinB3OhYimtiG2Cg5JCqIZFHxD6MjEGOiu
L8ktHMPvodBwNsSBULpG0QKBgBAplTfC1HOnWiMGOU3KPwYWt0O6CdTkmJOmL8Ni
blh9elyZ9FsGxsgtRBXRsqXuz7wtsQAgLHxbdLq/ZJQ7YfzOKU4ZxEnabvXnvWkU
YOdjHdSOoKvDQNWu6ucyLRAWFuISeXw9a/9p7ftpxm0TSgyvmfLF2MIAEwyzRqaM
77pBAoGAMmjmIJdjp+Ez8duyn3ieo36yrttF5NSsJLAbxFpdlc1gvtGCWW+9Cq0b
dxviW8+TFVEBl1O4f7HVm6EpTscdDxU+bCXWkfjuRb7Dy9GOtt9JPsX8MBTakzh3
vBgsyi/sN3RqRBcGU40fOoZyfAMT8s1m/uYv52O6IgeuZ/ujbjY=
-----END RSA PRIVATE KEY-----

read:errno=0

# 存到文件中
bandit16@bandit:~$ echo "-----BEGIN RSA PRIVATE KEY-----
> MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ
> imZzeyGC0gtZPGujUSxiJSWI/oTqexh+cAMTSMlOJf7+BrJObArnxd9Y7YT2bRPQ
> Ja6Lzb558YW3FZl87ORiO+rW4LCDCNd2lUvLE/GL2GWyuKN0K5iCd5TbtJzEkQTu
> DSt2mcNn4rhAL+JFr56o4T6z8WWAW18BR6yGrMq7Q/kALHYW3OekePQAzL0VUYbW
> JGTi65CxbCnzc/w4+mqQyvmzpWtMAzJTzAzQxNbkR2MBGySxDLrjg0LWN6sK7wNX
> x0YVztz/zbIkPjfkU1jHS+9EbVNj+D1XFOJuaQIDAQABAoIBABagpxpM1aoLWfvD
> KHcj10nqcoBc4oE11aFYQwik7xfW+24pRNuDE6SFthOar69jp5RlLwD1NhPx3iBl
> J9nOM8OJ0VToum43UOS8YxF8WwhXriYGnc1sskbwpXOUDc9uX4+UESzH22P29ovd
> d8WErY0gPxun8pbJLmxkAtWNhpMvfe0050vk9TL5wqbu9AlbssgTcCXkMQnPw9nC
> YNN6DDP2lbcBrvgT9YCNL6C+ZKufD52yOQ9qOkwFTEQpjtF4uNtJom+asvlpmS8A
> vLY9r60wYSvmZhNqBUrj7lyCtXMIu1kkd4w7F77k+DjHoAXyxcUp1DGL51sOmama
> +TOWWgECgYEA8JtPxP0GRJ+IQkX262jM3dEIkza8ky5moIwUqYdsx0NxHgRRhORT
> 8c8hAuRBb2G82so8vUHk/fur85OEfc9TncnCY2crpoqsghifKLxrLgtT+qDpfZnx
> SatLdt8GfQ85yA7hnWWJ2MxF3NaeSDm75Lsm+tBbAiyc9P2jGRNtMSkCgYEAypHd
> HCctNi/FwjulhttFx/rHYKhLidZDFYeiE/v45bN4yFm8x7R/b0iE7KaszX+Exdvt
> SghaTdcG0Knyw1bpJVyusavPzpaJMjdJ6tcFhVAbAjm7enCIvGCSx+X3l5SiWg0A
> R57hJglezIiVjv3aGwHwvlZvtszK6zV6oXFAu0ECgYAbjo46T4hyP5tJi93V5HDi
> Ttiek7xRVxUl+iU7rWkGAXFpMLFteQEsRr7PJ/lemmEY5eTDAFMLy9FL2m9oQWCg
> R8VdwSk8r9FGLS+9aKcV5PI/WEKlwgXinB3OhYimtiG2Cg5JCqIZFHxD6MjEGOiu
> L8ktHMPvodBwNsSBULpG0QKBgBAplTfC1HOnWiMGOU3KPwYWt0O6CdTkmJOmL8Ni
> blh9elyZ9FsGxsgtRBXRsqXuz7wtsQAgLHxbdLq/ZJQ7YfzOKU4ZxEnabvXnvWkU
> YOdjHdSOoKvDQNWu6ucyLRAWFuISeXw9a/9p7ftpxm0TSgyvmfLF2MIAEwyzRqaM
> 77pBAoGAMmjmIJdjp+Ez8duyn3ieo36yrttF5NSsJLAbxFpdlc1gvtGCWW+9Cq0b
> dxviW8+TFVEBl1O4f7HVm6EpTscdDxU+bCXWkfjuRb7Dy9GOtt9JPsX8MBTakzh3
> vBgsyi/sN3RqRBcGU40fOoZyfAMT8s1m/uYv52O6IgeuZ/ujbjY=
> -----END RSA PRIVATE KEY-----" > ssh.private

# 修改文件权限，确保他人不被允许访问这个文件，不然不会被认证端认同
bandit16@bandit:~$ chmod 600 ssh.private

# 连接
bandit16@bandit:~$ ssh -i ssh.private bandit17@localhost -p 2220
 _                     _ _ _   
| |__   __ _ _ __   __| (_) |_ 
| '_ \ / _` | '_ \ / _` | | __|
| |_) | (_| | | | | (_| | | |_ 
|_.__/ \__,_|_| |_|\__,_|_|\__|
                               
a http://www.overthewire.org wargame.

...
bandit17@bandit:~$ cat /etc/bandit_pass/bandit17
xLYVMN9WE5zQ5vHacb0sZEVqbrp7nBTn
```

心路历程：这关把过程注释在了上面的代码中，帮助大家路顺思路，这里就不赘述了。

---

Level 17 -18

描述：homedirectory中有两个文件：passwords.old和passwords.new。下一级的密码是passwords.new中跟.old唯一不同的一行

密码：kfBf3eYk5BPBRzwjqutbbfE887SVc5Yd

```sh
bandit17@bandit:~$ ll
total 44
drwxr-xr-x  4 bandit17 bandit17 4096 Nov 12 10:42 ./
drwxr-xr-x 30 root     root     4096 Nov 12 10:42 ../
-rw-r-----  1 bandit17 bandit17   33 Sep 28 14:04 .bandit16.password
-rw-r--r--  1 bandit17 bandit17  220 Apr  9  2014 .bash_logout
-rw-r--r--  1 bandit17 bandit17 3637 Apr  9  2014 .bashrc
drwx------  2 bandit17 bandit17 4096 Nov 12 10:42 .cache/
-rw-r--r--  1 bandit17 bandit17  675 Apr  9  2014 .profile
drwxr-xr-x  2 root     root     4096 Sep 28 14:04 .ssh/
-rw-r-----  1 bandit17 bandit17 1704 Sep 28 14:04 .ssl-cert-snakeoil.key
-rw-r-----  1 bandit18 bandit17 3300 Sep 28 14:04 passwords.new
-rw-r-----  1 bandit18 bandit17 3300 Sep 28 14:04 passwords.old
bandit17@bandit:~$ diff passwords.old passwords.new
42c42
< R3GQabj3vKRTcjTTISWvO1RJWc5sqSXO
---
> kfBf3eYk5BPBRzwjqutbbfE887SVc5Yd
```

心路历程：描述说的很清晰，这里直接 diff 就好了。

---

Level 18 -19

描述：下一级别的密码存储在家庭目录中的 readme 文件中。不幸的是，当你用SSH登录时，有人修改了.bashrc将你注销。

密码：IueksS7Ubh8G3DCwVzrTd8rAVOwq3M5x

这里就厉害了……
![IMAGE](/Image/bandit_resources/5C57A9F0545D1CDEA58442EEDC952A51.jpg)
输入密码后，跟我说了句拜拜然后给我一脚……
卧槽忍不了。
常年看 man 手册的老司机表示年轻人要低调，密码我还是能拿到。
先看 `man ssh` 说了什么：
![IMAGE](/Image/bandit_resources/5750971C77CDE09AD8C9ED4D020D4701.jpg)
Secure Shell 中的配置页面也预留了 Command 配置:
![IMAGE](/Image/bandit_resources/AD0B25D055288B2474EDD87D747BCF9F.jpg)
配置好之后点连接，输入密码：
```bash
 _                     _ _ _   
| |__   __ _ _ __   __| (_) |_ 
| '_ \ / _` | '_ \ / _` | | __|
| |_) | (_| | | | | (_| | | |_ 
|_.__/ \__,_|_| |_|\__,_|_|\__|
                               
a http://www.overthewire.org wargame.

bandit18@bandit.labs.overthewire.org's password: 
IueksS7Ubh8G3DCwVzrTd8rAVOwq3M5x
```

心路历程：如果事先不知道，卡住是正常的，但是如果之前用到 `ssh` 命令的时候仔细看看 man 手册，不知道吗？不存在的……
该反省的反省。

--- 

Level 19 - 20

描述：要访问下一个级别，您应该使用homeu目录中的setuid二进制文件。 不带参数的执行它找出如何使用它。 在使用setuid二进制文件后，可以在通常的地方（/etc/bandit\_pass）找到这个级别的密码。

密码：GbKksEFF4yrVs6il55v6gwY5aVje5f0j

说实话我没抬读懂这个 setuid binary，不过不影响。大致意思就是告诉你 Home 目录下有个文件，先不带参数执行一下，他会告诉你怎么用。

```sh

bandit19@bandit:~$ ll
total 32
drwxr-xr-x  3 bandit19 bandit19 4096 Nov 12 11:30 ./
drwxr-xr-x 30 root     root     4096 Nov 12 11:30 ../
-rw-r--r--  1 bandit19 bandit19  220 Apr  9  2014 .bash_logout
-rw-r--r--  1 bandit19 bandit19 3637 Apr  9  2014 .bashrc
drwx------  2 bandit19 bandit19 4096 Nov 12 11:30 .cache/
-rw-r--r--  1 bandit19 bandit19  675 Apr  9  2014 .profile
-rwsr-x---  1 bandit20 bandit19 7378 Sep 28 14:04 bandit20-do*
# 看起来就是bandit20-do了
bandit19@bandit:~$ ./bandit20-do 
Run a command as another user.
  Example: ./bandit20-do id
bandit19@bandit:~$ ./bandit20-do
# 人家已经告诉你了，会使用另一个用户执行命令，再返回去看一眼她的权限 -rwsr-x---
bandit19@bandit:~$ ./bandit20-do cat /etc/bandit_pass/bandit20
GbKksEFF4yrVs6il55v6gwY5aVje5f0j
```

心路历程：嗯，有种报复社会的感觉。

---

Level 20 -21

描述：homedirectory中有一个setuid二进制文件，它执行以下操作：它将连接 localhost:[命令行参数的上的端口]。然后从连接中读取一行文本，并将其与上一级（bandit20）中的密码进行比较。如果密码正确，将传送下一级密码（bandit21）。

注意：基础设施的变化使这一层面更加困难。您将需要找出在同一个Docker实例中启动多个命令的方法。

密码：gE269g2h3mw3pwgrj0Ha9Uoqen1c9DGr

这关的大意呢是告诉你，你的 Home 目录下有一个二进制文件（setuid又出现了……），你执行它，并提供给它端口号，它会请求这个端口并获取这个端口提供给他的数据，如果数据和 bandit20 的密码相同，他就会告诉你下一关的密码。
并且给了注意事项:[Changes to the infrastructure](http://overthewire.org/help/sshinfra.html)。
注意事项中提到了他们的游戏是运行在 Docker 容器上的，默认会给每一次连接分配一个全新的环境，如果当你完成游戏需要开启两个终端 并会产生交互的话，可能需要一些特殊的手段（ssh -L localport:host:port），我们后面再讲这个操作，先分析下他要我们干什么。

他提到：这个程序会访问 localhost 的[你提供的端口号]来获取数据。这里我们需要处理两件事情：1.运行这个程序。2.创建一个监听事件并会回复这个程序当前关的密码。
监听端口与访问端口有可能用到两个终端，而且是存在交互的，这也是他给我们提示的初衷。
我们做这关呢，有两个解法。一个是本着学习的态度，走提示流程，一个是日常野路子解法。我们先说说提示的思路。

### 思路一：
这个跑在 Docker 容器内的游戏，每一次连接都是全新的环境，那我们一个监听，一个请求，需要保证两个终端能够通信，监听的一段正常监听，请求的一端再去请求。所以我们现在要创建两个能交互的终端。方法是借助 `ssl -L` 这个命令，先查一下这个命令。然后再看会好很多。
嗯……先给你3分钟查一查，不够的话再来3分钟也行……

![IMAGE](/Image/bandit_resources/9D5E08F5F93CA44BC4287F54BD6661FC.jpg)
拿实际例子来说，当我们这样配置并连接的时候，我们请求连接本地主机的 1234 端口时这个请求会转发到 `bandit.labs.overthewire.org`，它会来访问 `localhost` (此时，对于 `bandit.labs.overthewire.org` 来讲 `localhost` 是它自身)的 22 端口。
注意事项中告诉我们，这样就可以在提供 -L 参数的这个连接不关闭的情况下，通过本地端口转发，建立另一条连接并能够与前一个连接搞基。

那我们的第二个连接就是这样的：
![IMAGE](/Image/bandit_resources/CA9CFE5E31145E899BB4D6AD535E8B21.jpg)

我们在其中一个终端中创建一个发送本关密码的监听：
```bash
bandit20@bandit:~$ nc -l 23333 < /etc/bandit_pass/bandit20
 
```
此时光标会在第二行等待，监听命令没有执行完毕退出。
我们在第二个终端中，我们来看看他所说的这个程序：
```bash
bandit20@bandit:~$ ll
total 28
drwxr-xr-x  2 root     root     4096 Nov 13 15:58 ./
drwxr-xr-x 29 root     root     4096 Nov 13 15:57 ../
-rw-r--r--  1 root     root      220 Sep  1  2015 .bash_logout
-rw-r--r--  1 root     root     3771 Sep  1  2015 .bashrc
-rw-r--r--  1 root     root      655 Jun 24  2016 .profile
-rwsr-x---  1 bandit21 bandit20 8044 Nov 13 15:58 suconnect*
bandit20@bandit:~$ ./suconnect 
Usage: ./suconnect <portnumber>
This program will connect to the given port on localhost using TCP. If it receives the correct password from the other side, the next password is transmitted back.
# 嗯 上面的报道基本没什么偏差…… 我们来运行一下，端口号是我们上面自定义的监听端口「23333」
bandit20@bandit:~$ ./suconnect 23333
Read: GbKksEFF4yrVs6il55v6gwY5aVje5f0j
Password matches, sending next password
# 程序告诉我们，他读到了我们发送给他的密码「GbKksEFF4yrVs6il55v6gwY5aVje5f0j」，密码抱对成功，下一关的密码已发送。
```
那我们再来看一眼监听的那一边：
```bash
bandit20@bandit:~$ nc -l 23333 < /etc/bandit_pass/bandit20 # 这条是我们之前的
gE269g2h3mw3pwgrj0Ha9Uoqen1c9DGr #收到了密码
bandit20@bandit:~$ # 出现了它，表示 nc 命令结束了执行。
```

### 思路二：
这个思路其实我之前叙述的时候已经透露出一些味道了，细心的应该已经不用我说了。
关键词「&」。
这道题创建两个终端去连接的根本原因是因为监听和执行会占用两个终端，那我们把监听扔到后台去跑不就解决了吗。
连接姿势为正常姿势不多说……算了还是多说个j8（图）吧：
![IMAGE](/Image/bandit_resources/7BBE68BB3C0BC4EC581DB35ABD35E039.jpg)

```bash
# 放在后台执行
bandit20@bandit:~$ nc -l 23333 < /etc/bandit_pass/bandit20 & 
# 后台运行进程代号[1]，PID 为 1135 
[1] 1135
bandit20@bandit:~$ ./suconnect 23333
Read: GbKksEFF4yrVs6il55v6gwY5aVje5f0j
Password matches, sending next password
# 后台进程返回了得到的信息（密码）到标准输出（你当前的终端）
gE269g2h3mw3pwgrj0Ha9Uoqen1c9DGr
# 后台进程运行结束，命令为：nc -l 23333 < /etc/bandit_pass/bandit20
[1]+  Done                    nc -l 23333 < /etc/bandit_pass/bandit20
bandit20@bandit:~$ 
```

心路历程：嗯，先说思路一的理由是我怕我先写思路二，再写的思路一可能没人看了。

Level 21 - 22

描述：一个程序从cron（基于时间的作业调度程序）定期自动运行。查看/etc/cron.d/中的配置并查看正在执行的命令。

密码：Yk7owGAcWjwMVRwrTesJEwB7WVOiILLI

```sh
# 进入描述中的文件夹
bandit21@bandit:~$ cd /etc/cron.d/
bandit21@bandit:/etc/cron.d$ ll
total 28
drwxr-xr-x   2 root root 4096 Nov 13 15:58 ./
drwxr-xr-x 101 root root 4096 Nov 13 15:58 ../
-rw-r--r--   1 root root  102 Apr  5  2016 .placeholder
-rw-r--r--   1 root root  120 Nov 13 15:58 cronjob_bandit22
-rw-r--r--   1 root root  122 Nov 13 15:58 cronjob_bandit23
-rw-r--r--   1 root root  120 Nov 13 15:58 cronjob_bandit24
-rw-r--r--   1 root root  190 Oct 31 13:21 popularity-contest
# 不要问我为什么，就是直觉
bandit21@bandit:/etc/cron.d$ cat cronjob_bandit22
@reboot bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
# 他会跑这个脚本，那我们去看看
bandit21@bandit:/etc/cron.d$ cat -n /usr/bin/cronjob_bandit22.sh
     1  #!/bin/bash
     2  chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
     3  cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
# 他会往这个脚本里跑内容
bandit21@bandit:/etc/cron.d$ cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
Yk7owGAcWjwMVRwrTesJEwB7WVOiILLI
```

心路历程：甚至不需要知道他所说的 cron 是干什么的，我们就能找到密码（当然，游戏放水也有很大因素）。不过该知道的还是要知道的，cron 是 Linux 的守护进程。通俗一点就是我们平时接触到的计划任务，按照规定时间安排定期执行动作的。其中的 cronjob\_bandit\* 就是提供给 cron 执行的文件。「\* \* \* \* \* bandit22 /usr/bin/cronjob\_bandit22.sh &\> /dev/null」前面的「\*」是每分钟执行一次，「bandit22」是用它的身份执行，「/usr/bin/cronjob\_bandit22.sh」是被执行脚本，至于「&\> /dev/null」 前面都有分别提到过，不说任性。

---

Level 22 - 23

描述：一个程序从cron（基于时间的作业调度程序）定期自动运行。查看/etc/cron.d/中的配置并查看正在执行的命令。 注意：查看其他人编写的shell脚本是非常有用的技巧。这个级别的脚本是故意易于阅读。如果您在理解它的功能时遇到问题，请尝试执行它以查看打印的调试信息。

密码：jc1udXuA1tiHqjIsL8yaapX5XIAI6i0n

```sh
bandit22@bandit:~$ ll /etc/cron.d
total 28
drwxr-xr-x   2 root root 4096 Nov 13 15:58 ./
drwxr-xr-x 101 root root 4096 Nov 13 15:58 ../
-rw-r--r--   1 root root  102 Apr  5  2016 .placeholder
-rw-r--r--   1 root root  120 Nov 13 15:58 cronjob_bandit22
-rw-r--r--   1 root root  122 Nov 13 15:58 cronjob_bandit23
-rw-r--r--   1 root root  120 Nov 13 15:58 cronjob_bandit24
-rw-r--r--   1 root root  190 Oct 31 13:21 popularity-contest
bandit22@bandit:~$ cat /etc/cron.d/cronjob_bandit23
@reboot bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
bandit22@bandit:~$ cat -n /usr/bin/cronjob_bandit23.sh
     1  #!/bin/bash
     2
     3  myname=$(whoami)
     4  mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)
     5
     6  echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"
     7
     8  cat /etc/bandit_pass/$myname > /tmp/$mytarget

cat /etc/bandit_pass/$myname > /tmp/$mytarget
bandit22@bandit:~$ myname="bandit23"
bandit22@bandit:~$ mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)
bandit22@bandit:~$ echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"
Copying passwordfile /etc/bandit_pass/bandit23 to /tmp/8ca319486bfbbc3663ea0fbe81326349
bandit22@bandit:~$ cat /tmp/8ca319486bfbbc3663ea0fbe81326349
jc1udXuA1tiHqjIsL8yaapX5XIAI6i0n
```

心路历程：这里的定位思路与上一关没什么区别，最后会要你分析这个简单的代码。因为 cronjob\_bandit23 文件中指定的用户是 bandit23，whoami 的结果也是这个，我们就直接赋值然后走流程就行了。

---

Level 23 -24

描述：一个程序从cron（基于时间的作业调度程序）定期自动运行。查看/etc/cron.d/中的配置并查看正在执行的命令。 

注意：这个级别要求你创建你自己的第一个shell脚本。这是非常大的一步，当你击败这个级别时，你应该为自己感到骄傲！ 

注意2：请记住，你的shell脚本一旦执行就会被删除，所以你可能想保留一个副本...

密码：UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ

```sh
bandit23@bandit:/var/spool/bandit24$ ll /etc/cron.d
total 28
drwxr-xr-x   2 root root 4096 Nov 13 15:58 ./
drwxr-xr-x 101 root root 4096 Nov 16 10:18 ../
-rw-r--r--   1 root root  102 Apr  5  2016 .placeholder
-rw-r--r--   1 root root  120 Nov 13 15:58 cronjob_bandit22
-rw-r--r--   1 root root  122 Nov 13 15:58 cronjob_bandit23
-rw-r--r--   1 root root  120 Nov 13 15:58 cronjob_bandit24
-rw-r--r--   1 root root  190 Oct 31 13:21 popularity-contest
bandit23@bandit:/var/spool/bandit24$ cat /etc/cron.d/cronjob_bandit24
@reboot bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
* * * * * bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
bandit23@bandit:/var/spool/bandit24$ cat -n /usr/bin/cronjob_bandit24.sh
     1  #!/bin/bash
     2
     3  myname=$(whoami)
     4
     5  cd /var/spool/$myname
     6  echo "Executing and deleting all scripts in /var/spool/$myname:"
     7  for i in * .*;
     8  do
     9      if [ "$i" != "." -a "$i" != ".." ];
    10      then
    11          echo "Handling $i"
    12          timeout -s 9 60 ./$i
    13          rm -f ./$i
    14      fi
    15  done
bandit23@bandit:/var/spool/bandit24$ echo "cat /etc/bandit_pass/bandit24 > /tmp/bandit24_pass" > /var/spool/bandit24/bandit24_getpwd ; chmod 777 bandit24_getpwd ; chmod 777 /tmp/bandit24_pass
bandit23@bandit:/var/spool/bandit24$ cat chmod 777 /tmp/bandit24_pass
UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ
```

心路历程：我们定位脚本的思路不变，看脚本发现：该脚本会执行 `/var/spool/bandit24` 的脚本，60s 如果还没之行结束会强制kill掉，然后删除。所以我们写了一个把 `/etc/bandit\_pass/bandit24/` 输出到 /tmp/bandit24\_pass 的脚本到这个目录下，然后付了个权限。

---

Level 24 - 25

描述：一个守护进程正在监听端口30002，如果给出了bandit24的密码和一个秘密的数字4位pincode，将给你bandit25的密码。没有办法检索pincode，除非枚举10000个组合，称为蛮力。

密码：uNG9O58gUE7snukf3bvZ0rxhtnjzSGzG

```sh
bandit24@bandit:~$ vim /tmp/boom.py
bandit24@bandit:~$ cat -n /tmp/boom.py
     1  #!/usr/bin/env python
     2
     3  fl = open('/tmp/boom_dict.txt', 'w+')
     4  pwd = 'UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ'
     5  for i in xrange(10000):
     6      fl.write(pwd + ' ' + str(i).zfill(4) + '\n')
     7  fl.close()
     8
bandit24@bandit:~$ python /tmp/boom.py
bandit24@bandit:~$ head /tmp/boom_dict.txt
UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ 0000
UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ 0001
UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ 0002
UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ 0003
UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ 0004
UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ 0005
UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ 0006
UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ 0007
UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ 0008
UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ 0009
bandit24@bandit:~$ nc localhost 30002 < /tmp/boom_dict.txt > /tmp/reply_pwd.txt
bandit24@bandit:~$ sort /tmp/reply_pwd.txt | uniq -u

Correct!
Exiting.
I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line, separated by a space.
The password of user bandit25 is uNG9O58gUE7snukf3bvZ0rxhtnjzSGzG
```

心路历程：这关描述也很清晰（虽然我是机翻）。它开放了一个 30002 端口监听，你给他发 bandit24 的密码 +空格 + 四位数字，如果这个字符串正确它就会返回给你 bandit25 的密码。 Bandit24 的密码我们很容易得到，那问题就是这个4位数了。描述中说我们只能尝试暴力破解，那就从「0001」到「9999」都生成出来，分别与 bandit24的密码+空格连接，把这一万个组合都试一下。于是写了一个 python 脚本生成这个含有 10000 种组合的文件，并提交给 30002 端口，把返回的信息存放在文件中，然后筛选文件内正确信息。

---

Level 25 - 26

描述：从bandit25登录到bandit26应该相当简单...用户bandit26的shell不是/ bin / bash，而是别的。找出它是什么，它是如何工作的，以及如何摆脱它。

密码：5czgV9L3Xx8JPOyRbXh6lQbmIOWvPT6Z

```bash
bandit25@bandit:~$ ls
bandit26.sshkey
bandit25@bandit:~$ ssh -i bandit26.sshkey bandit26@localhost        
Could not create directory '/home/bandit25/.ssh'.
The authenticity of host 'localhost (127.0.0.1)' can't be established.
ECDSA key fingerprint is SHA256:rm2/oZ03et2au9vHBOtBNdgMDGJkbNqdkmHDLPDD32E.
Are you sure you want to continue connecting (yes/no)? yes
 | |                   | (_) | |__ \ / /  
 | |__   __ _ _ __   __| |_| |_   ) / /_  
 | '_ \ / _` | '_ \ / _` | | __| / / '_ \ 
 | |_) | (_| | | | | (_| | | |_ / /| (_) |
 |_.__/ \__,_|_| |_|\__,_|_|\__|____\___/ 
Connection to localhost closed.
bandit25@bandit:~$  
# ssh 连接以后把我提出来了。。。
# 题目提示 bandit26 的 shell 不是 /bin/bash，那我们看看是什么
bandit25@bandit:~$ cat /etc/passwd | grep bandit26
bandit26:x:11026:11026:bandit level 26:/home/bandit26:/usr/bin/showtext
bandit25@bandit:~$ cat -n /usr/bin/showtext
     1  #!/bin/sh
     2
     3  export TERM=linux
     4
     5  more ~/text.txt
     6  exit 0
# more 命令是一次显示一屏文字，然后左下角会显示一个 more 和当前显示了百分之多少
# 我们在回头看看上面登陆后的字符画：
# | |                   | (_) | |__ \ / /  
# | |__   __ _ _ __   __| |_| |_   ) / /_  
# | '_ \ / _` | '_ \ / _` | | __| / / '_ \ 
# | |_) | (_| | | | | (_| | | |_ / /| (_) |
# |_.__/ \__,_|_| |_|\__,_|_|\__|____\___/ 
# 之前在其他登陆中不曾见过，想必就是 more 命令显示的 text.txt 的内容
# 因为行数足以一屏显示完，所以没有显示 more 就没有显示
# 我们的思路就在 more 的这个特点上。我们要让他卡在一屏读不完的位置，也就是让你的终端高度读不下6行（字符画高度）。
# 这样我们可以在 more 的状态下通过一些特性执行命令找到我们下一关的密码。
# 因为上面的字符画有6行，而使用 chrome 没办法变那么窄，我们转而使用系统的命令行 ssh 上去搞。
```
我们先连接上去
![IMAGE](/Image/bandit_resources/9E633BCC3C0A9193B0E8E2C2B65731BA.jpg)
然后我们使用 bandit25 这个用户带着 bandit26 的 privatekey 去连接 bandit26
![IMAGE](/Image/bandit_resources/D9A392F2C38EC45B17ACAAB75A5E3890.jpg)
more 命令像预期一样输出一屏后等待我们的动作：
![IMAGE](/Image/bandit_resources/E02BACB49695839AC213BC250033CBDE.jpg)
我们看看手册，有没有能我们需要的东西：
![IMAGE](/Image/bandit_resources/A821D5D2B3FBEF8E79E26DE69CA97891.jpg)
这个v命令能让我们进入 vi 模式，虽然不常用 vi，但经常 vim 敲代码，经常使用「:sp」「:vsp」这种能打开其他文件的命令。
我们再去看看 vi 的 man 手册：
![IMAGE](/Image/bandit_resources/06367F7F7B9D0A55C16D0B51300F9A4B.jpg)
看起来就是我们需要的命令，那我们去试试。
在 more 的页面按v进入vi
![IMAGE](/Image/bandit_resources/FDDCDF1F1AA31D4A681D965AB9B27E74.jpg)
然后使用命令 `:ex! /etc/bandit_pass/bandit26` 打开我们需要的密码文件：
![IMAGE](/Image/bandit_resources/DDF96BC0FFF6CC6F41E8694DBDC1CE1E.jpg)

心路历程：这关很有意思，刚好 more 与 vi 这两个命令我都不常用，查阅文档与测试了一番。诸如登陆时候的字符画与 more 这种小细节，透过终端似乎能在看到设计者的：）

---

Level 26 - 27

描述：**这时候，27级还不存在**

[Game Over]

---

嗯……游戏是结束了，我最后问一句：
![IMAGE](/Image/bandit_resources/00023D502683EF1B4545C5273153F690.jpg)