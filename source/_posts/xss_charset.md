---
title: XSS 编码的一些基础知识
date: 2017-09-08 01:01:01
layout: post
comments: true
reward: true
tags:
    - XSS
---

<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width=330 height=86 src="//music.163.com/outchain/player?type=2&id=786262&auto=0&height=66"></iframe>

>记得那天，Windows xp 桌面上的天很蓝。  
这是第几次远远的望着已经记不清了，走上前试试看吧。  
我表现的不是很熟络，交谈的过程却出奇的愉快，好感度的 Level 似乎在不经意间也升了几级。  
伴随着逐渐深入的交流，难免会擦出些共鸣的火花。  
我是个对待这种事情比较认真的人。 
<!--more -->了解的时候，走走心就好了。  
真到了认定了的时候，再走……走四方，路迢……<br>
可能那一天来的快了些，仓促间暴露出了还没准备好的胆怯。  
不知道有没有被看到，不过想想也却是没什么丢人的。  
说来也不怕笑话，平时会刻意让自己多留意一些有关体位的姿势。  
在得到“请开始你的表演”的暗示时，脑海中的各种体位交错在一起，心还不争气的砰砰跳个不停。    
这短暂的瞬间突然很慌张，毕竟没有实际操作的经验，这些体位有没有效果我心里也没底。<br>
我动起来的那一刻，我完全不知道下一秒会得到什么样的反应。  
毕竟是个没有经验的年轻人，当发现没有回应的时候，内心那个轻声呢喃的质疑声现在都还记得。  
有点慌张，还有些不知所措。  
不知从哪里涌出来的不甘让我顾不及反应如何就把脑中的体位一个接一个的用了出来。  
渐渐的有了回应，因某些姿势恰到好处弹出回应的 body 给了我一些信心。  
可有时任凭我姿势如何，那依然沉寂的如没有音乐的骨灰盒的 body，却成了挥不去的阴影。<br>
那天事后，我点了支烟。  
脑海中回忆着前一刻自己操作的慌张，眼神透过迷蒙的烟雾望着前方。  
我想通了一件事情。  
XSS 这样各种编码瞎特么插，迟早药丸。

---

前几天有人看了我前面的博客后问我「什么样的情况下使用什么编码方式?」  
当时我就扇了自己一嘴巴子，确实心路历程描述的不够饱满……

### 正文之前
编码这件事如果瞎j8操作那真是犹如「加藤老师扣人嘴」，还是写一写吧。便有了这篇。

其实该使用什么编码插入，要根据实际情况而定，单一的编码，混合编码都有可能。重要的是清楚**哪种编码**在**哪个时刻**被**哪种规则**解析。 

简单点来说就是：URL 请求时会对百分号等 URL 的编码方式进行转码；浏览器接收到页面数据后，会对 HTML 实体编码进行转码；执行 JS 时会对 JSUnicode 等 JS 支持的方式进行转码。

复合编码的情况稍微复杂了些，比如这样：  
```html
<html>
<body>
    <a href="javascript:ShowSomething('&#37&#53&#99&#37&#55&#56&#37&#51&#52&#37&#51&#49');">click</a>
</body>
<script>
    function ShowSomething(x){
        alert(x);
    }
</script>
</html>
```
这个例子中，从浏览器开始加载页面到你点击「click」弹出内容，`ShowSomething()` 的参数经过了三次转码。

如果看不懂上面的例子没关系，等看完这篇文章，你还是看不懂上面的例子，再跳楼也不迟：）

好进入正题前先说明（anli）一下：
下文中不会刻意说明某些字符转码前后分别是什么，大家请自行对照。  
如果有转码需求，可以使用 Cos 他们这个开源工具 「[xssor.io](http://xssor.io/)」 自行转码。  

另外，本篇会面向基础薄弱的读者。会写的相对浅显，例如不会提及 `DOM` 、 `JS 解释器` 等这类需要一定知识积累的概念。老司机可以下车了～

### URL 编码
这里要说的 URL 编码指的是 **URI 的百分号编码**。
我们在进行 URL 请求时，浏览器会自动帮我们把部分符号转换成 `'%' + '十六进制数字'` 的形式，用 windows 时间长的朋友们可能会遇见自己的安装软件时显示的路径中有一些`字符`或者`中文`会被显示为这种形式。   

服务器接收到提交来的请求时，会先把 `%` 编码转换成正常的字符。
我们借助一个简单的 php web 应用来说明这个问题，代码很简单：
```html
<html>
<body>
    < a href=' "URL 请 求：" + location.search.substr(1) +
                "\n" + 
                "服务器接受：" + "<?php echo 'para='.$_GET['para']?>"
            );'
    >click</ a>
</body>
</html>
```
上面这段代码可以自己放到自己的服务器上试一下。  
当我们带着这样的参数请求时：
```
http://www.xxx.com/xxx.php?para=%3Cscript%3E%61lert();%3C/script%3E
```
![url_encode](/Image/url_encode.png)

从 `alert()` 的内容和 Chrome 的控制台都能看出我们的「百分号编码」被转义为正常内容了。  
其中 `alert()` 中的 `a` 被我转成了 `%61`，想说明除了符号，数字和字母也是可以做转换的。

URL 这里其实还有一种 **Base64** 编码, 常用来做简单加密传输数据。  
比如我们在 「[xssor.io](http://xssor.io/)」 中使用 「Base64EN」转换下面的代码  
`<script>alert(/xss/);</script>` 得到： `PHNjcmlwdD5hbGVydCgveHNzLyk7PC9zY3JpcHQ+`  
我们可以这样试试效果：
```html
<a href="data:text/html;base64,PHNjcmlwdD5hbGVydCgveHNzLyk7PC9zY3JpcHQ+">XSS</a>
```
点击链接后，就会通过 data 协议去加载后面的内容。其中，如果声明了 Base64 的话，就会做相应的 Base64 解码操作。对了，这个例子在 Chrome 里会被拦截，点击没反应的话，按 F12 你会看到红色的警告，控制台里点击一下链接就看到效果了。  
其实还有……这里就不讲了。想拓展的可以去 「[xssor.io](http://xssor.io/)」看看上面都有什么，这也是一种学习方式：）。
### HTML 实体编码
有些时候，我们想在页面上显示的东西会跟 HTML 本身的标记冲突。  
比如我们只想在页面中显示 `<script>alert();</script>` 而不是执行这串代码的时候，我们如下这样写是不行的：
```html
<html>
<body>
    <script>alert();</script>
</body>
</html>
```
其中的 `<script>alert();</script>` 会被当作 JS 脚本进行解析。  
但如果我们把这些字符用另一种方式表示，只要不出现类似 `<script>` 这种内容的话，问题似乎就解决了。  
HTML 编码就是用来解决这件事情的，我们的 ‘<’ 字符可以用 HTML编码中的 `&lt;` 来表示。  
那我们的代码可以变成：
```html
<html>
<body>
    &lt;scirpt>alert();&lt;/script>
</body>
</html>
```
上面的代码可以自己拷贝出来，保存到文本文件，起名 xxx.html 然后用浏览器打开试试看。  
上面用到的是 `'&' + '约定名称' + ';'` 的形式，其实还有 `'&#' + '十进制数字' + ';'` 和 `'&#x' + '十六进制数字' + ';'` 的形式。比如上文的 `'<'` 这个字符可以表示为 `&#0060;` 和 `&#x003c;`。  
其中约定名称是为了便于记忆，后面的十进制和十六进制编码中 `#` 后面的连续的零多几个或者少几个都可以，类似`1`和`00001`的数值是相等的感觉。  
  
我们都知道这样是可以触发弹窗的（不知道的话，要自行补一补哦）：
```html
<html>
<body>
    <img src='' onerror='alert();'/>
</body>
</html>
```
我们还可以应用 HTML 实体编码这样：
```html
<html>
<body>
    <img src='' onerror='&#97;lert&#40;&#41;&#59'/>
</body>
</html>
```
但有个要求：HTML 实体编码不能干扰到标签自身的属性和声明性内容。
比如：
```html
<html>
<body>
    <im&#103; src='' onerror='alert();'/>
    <img sr&#99;='' onerror='alert();'/>
</body>
</html>
```
都是不行的。

关于 HTML 实体编码，十进制与十六进制编码后面的 ';' 是可以省略的。

### JS 编码
到这里，可能大家面对各种编码已经比较淡定了，不就是不同的编码规则嘛。
首先说说 JS Unicode 编码
规则是 `'\u' + '四位十六进制数字'` 不够四位前面补0。  
我们来看看例子（通过 「[xssor.io](http://xssor.io/)」转码）：
```html
<html>
<body>

</body>
<script>
    //进行 JS Unicode 转码前的样子：
    //document.body.innerHTML="<img src='' onerror='alert(/xss/)'/>";
    document.body.innerHTML="\u003c\u0069\u006d\u0067\u0020\u0073\u0072\u0063\u003d\u0027\u0027\u0020\u006f\u006e\u0065\u0072\u0072\u006f\u0072\u003d\u0027\u0061\u006c\u0065\u0072\u0074\u0028\u002f\u0078\u0073\u0073\u002f\u0029\u0027\u002f\u003e";
</script>
</html>
```
这段代码在执行 innerHTML 操作前，因为字符串符合 Unicode 编码规则，会被先解码再输出。
另外，也可以这样玩：
```html
<script>
    \u0061lert(/xss/);
</script>
```
好，下面说说 「8进制」和「16进制」的转义字符。  
「8进制」由 `'\' + '8进制数字'` 组成。  
「16进制」由 `'\x' + '16进制数字'` 组成。  

我们来看看例子。  额……嗯……  
「[xssor.io](http://xssor.io/)」上并没有直接提供这两种转码功能……  在网上简单搜索了一下，没找到什么合适的。  
「卧槽，没有地方能转怎么办？」『**自己写啊**』  
#### 自己写一个「定制版转换工具」
其实自己写的难度不大，为什么这么说？因为 「[xssor.io](http://xssor.io/)」 已经实现了 10进制 与 16进制 的 encode、decode功能。  
嗯，我们去「借鉴一下」。

通过项目的页面代码，我们能能够找到我们需要的代码所在位置：
![xssor_github_ende](/Image/xssor_github_ende.png)

在页面的文件中找到对应方法，看具体实现：
![xssor_github_ende_function](/Image/xssor_github_ende_function.png)

作为一个老司机，最重要的还是「心中无码」，我们只要看它的实现思想就好。
核心思想是通过 `charCodeAt()` 把我们输入的内容转成 Unicode 编码的数字，然后使用 `toString(x)` 再转成对应 x 进制的数字，然后定制我们的样式。
下面来根据我们的需求，定制我们自己的代码：
```js
<html>
<body>

</body>
<script>
    function xss_js_to8or16(x, txt){
        var _a="";
        for(i=0; i<txt.length; i++){
            // 这里用到了三目运算符。如果不知道自行充能哦。
            s = txt.charCodeAt(i).toString(x == 8 ? 8 : 16);
            // '\\' 转义后变为 '\'   
            _a += (x == 8 ? "\\" : "\\x") + s;
        }
        return _a;
    }
    var str = "<img src='' onerror='alert(/xss/)'/>";
    console.log(xss_js_to8or16(8, str));
    console.log(xss_js_to8or16(16, str));
</script>
</html>
```
好了，我们把代码保存为 `xx.html` ，并通过浏览器打开，F12 下看 Console 标签内的内容，就是我们想要的 8进制 与 16进制的内容了。

把拿到的字符串放入我们的例子中，8进制：
```html
<html>
<body>

</body>
<script>
    //进行 JS Unicode 转码前的样子：
    //document.body.innerHTML="<img src='' onerror='alert(/xss/)'/>";
    document.body.innerHTML="\74\151\155\147\40\163\162\143\75\47\47\40\157\156\145\162\162\157\162\75\47\141\154\145\162\164\50\57\170\163\163\57\51\47\57\76";
</script>
</html>
```
16进制：
```html
<html>
<body>

</body>
<script>
    //进行 JS Unicode 转码前的样子：
    //document.body.innerHTML="<img src='' onerror='alert(/xss/)'/>";
    document.body.innerHTML="\x3c\x69\x6d\x67\x20\x73\x72\x63\x3d\x27\x27\x20\x6f\x6e\x65\x72\x72\x6f\x72\x3d\x27\x61\x6c\x65\x72\x74\x28\x2f\x78\x73\x73\x2f\x29\x27\x2f\x3e";
</script>
</html>
```
如果你仔细观察下，会发现， JS 8进制编码的长度要短一些，如果遇到插入长度有所限制的情况就可以用到这个特点。
还有一种叫做 [JSFuck](http://www.jsfuck.com/) 的编码，长什么样呢？
```
[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]][([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+([][[]]+[])[+!+[]]+(![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[+!+[]]+([][[]]+[])[+[]]+([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+(!![]+[])[+!+[]]]((![]+[])[+!+[]]+(![]+[])[!+[]+!+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]+(!![]+[])[+[]]+(![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[!+[]+!+[]+[+[]]]+(![]+[+![]])[([![]]+[][[]])[+!+[]+[+[]]]+(!![]+[])[+[]]+(![]+[])[+!+[]]+(![]+[])[!+[]+!+[]]+([![]]+[][[]])[+!+[]+[+[]]]+([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(![]+[])[!+[]+!+[]+!+[]]]()[+!+[]+[+[]]]+(+(+!+[]+[+[]]+[+!+[]]))[(!![]+[])[+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+(+![]+([]+[])[([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+([][[]]+[])[+!+[]]+(![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[+!+[]]+([][[]]+[])[+[]]+([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+(!![]+[])[+[]]+(!![]+[])[+!+[]]+([![]]+[][[]])[+!+[]+[+[]]]+([][[]]+[])[+!+[]]+(+![]+[![]]+([]+[])[([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+([][[]]+[])[+!+[]]+(![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[+!+[]]+([][[]]+[])[+[]]+([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+(!![]+[])[+!+[]]])[!+[]+!+[]+[+[]]]](!+[]+!+[]+!+[]+[!+[]+!+[]+!+[]+!+[]])[+!+[]]+(![]+[])[!+[]+!+[]+!+[]]+(![]+[])[!+[]+!+[]+!+[]]+(![]+[+![]])[([![]]+[][[]])[+!+[]+[+[]]]+(!![]+[])[+[]]+(![]+[])[+!+[]]+(![]+[])[!+[]+!+[]]+([![]]+[][[]])[+!+[]+[+[]]]+([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(![]+[])[!+[]+!+[]+!+[]]]()[+!+[]+[+[]]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[!+[]+!+[]+[+[]]])()
```

我们在 Chrome 的控制台中输入：  
![jsfunck_chrome](/Image/jsfuck_chrome.png)

如果想详细了解浏览器、解释器之间到底是怎么配合的，可以看看这篇文章学习一下。  
[How browsers work - Behind the scenes of modern web browsers](http://taligarsiel.com/Projects/howbrowserswork1.htm)

现在再来看看开头提到的混合编码：
```html
<html>
<body>
    <a href="javascript:ShowSomething('&#37&#53&#99&#37&#55&#56&#37&#51&#52&#37&#51&#49');">click</a>
</body>
<script>
    function ShowSomething(x){
        alert(x);
    }
</script>
</html>
```
页面加载时，会把 a 标签中的 HTML 实体编码做解码。  
当点击「click」时，先把 href 的内容当作整体做 URL 解码。  
最后因为 javascript 伪协议，把其中的 8进制转义字符做解码。  
至于每一步，由什么转成什么，就不用我说了吧：）
### 后记
本篇的目的在于科普基础的编码，为了让非科班出身的朋友看起来不那么晦涩，理论知识介绍较少。  
但这不代表理论知识不重要，我的初衷是希望大家能把我的例子拿出来修修改改，尝试不同的可能。在摸索中遇到疑惑，然后自己解决困惑，渐渐积累起一定的概念后，会有想知道原理的冲动。相信这个时候，你积累的东西已经能让你阅读通顺了。  
写这篇文章的过程中，自己也查阅了很多的文章与维基百科，写了大量的测试代码来验证想法和探究原理，与老司机交流解惑。但最后只把其中比较直观的拿出来当作例子。后面再写写一些我了解到的姿势和一些需要搞清楚的原理性知识点也说不定（上面的那篇 How browsers work 中有此类知识点）。  
不过到时候你可能已经比我强了，记得带带我：）