---
date: 2016-10-28 09:48
status: public
title: 2016-10-28
---

#面试题准备2.0版 
第一版现在看，感觉有的东西没准备到，有的东西过于简略，所以来给他们升个级吧


##CSS
###水平垂直居中
####flex
父元素设置
```css
			display:flex;
			justify-content:center;
			align-items:center;
```
###设置div height:100%生效 即占满屏幕高度
父元素html body的高度为auto 也就是没有defined的值，所以子元素height:100%就无法生效，必须设置html body的高度
```css
html{height:100%;}
body{height:100%;}
div{height:100%;}
```
##js
###数组去重
####set
果断用我们大ES6的Set啊
Set ES6的新数据结构 成员的值都是唯一的 类数组
`Array.from()` 方法可以将一个类数组对象或可遍历对象转换成真正的数组
```js
var a = [1,1,3,4,5,5,6]
function unique(arr){
return Array.from(new Set(arr))
}
console.log(unique(a))//[1, 3, 4, 5, 6]
```
参考:
[求一个javascript数组去重方法？](https://www.zhihu.com/question/29558082)
[Array.from()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/from)

###事件委托
####回调
与事件委托绑定在一起的回调会有作用域问题
```js
var user = {
 firstname: 'Bob',
 greeting: function(){
   alert('My name is ' + this.firstname);
 }
};


//回调函数的作用域为当前元素 element
element.addEventListener('click', user.greeting)
//包裹匿名函数
//匿名函数的作用域被指向事件触发元素，但执行的内容就像直接调用一样，不会影响其作用域。
//如果想使用 removeEventListener 解除绑定，还需要再创建一个函数引用
element.addEventListener('click', function() {
  user.greeting();
});

//bind
user.greeting = user.greeting.bind(user);
element.addEventListener('click', user.greeting);

```
[事件委托](http://yujiangshui.com/javascript-event/)
###for in 、for of等
方法	适用范围	描述
for...in	数组、对象	获取可枚举的实例和原型属性名
Object.keys()	数组，对象	返回可枚举的实例属性名组成的数组
Object.getPropertyNames()	数组、对象	返回除原型属性以外的所有属性（包括不可枚举的属性）名组成的数组
for...of	可迭代对象(Array, Map, Set, arguments等)	返回属性值
```JS
var obj = {
    'x': 1,
    'y': 2,
    'z': 3
}
for (prop in obj) {
    console.log(prop) // x ,y, z
    console.log(obj[prop]); // 输出1，2，3
}
$.each(obj,function(prop){console.log(obj[prop])})
```

###debounce && throttle
>电梯超时
想象每天上班大厦底下的电梯。把电梯完成一次运送，类比为一次函数的执行和响应。假设电梯有两种运行策略 throttle 和 debounce ，超时设定为15秒，不考虑容量限制。
throttle 策略的电梯。保证如果电梯第一个人进来后，15秒后准时运送一次，不等待。如果没有人，则待机。
debounce 策略的电梯。如果电梯里有人进来，等待15秒。如果又人进来，15秒等待重新计时，直到15秒超时，开始运送。
[高阶函数 debounce 和 throttle](http://www.cnblogs.com/ambar/archive/2011/10/08/throttle-and-debounce.html)
[浅谈 Underscore.js 中 _.throttle 和 _.debounce 的差异](https://blog.coding.net/blog/the-difference-between-throttle-and-debounce-in-underscorejs)
[debouncing&throttling](http://codesky.me/archives/javascript-debouncing-throttling.wind)


###百度面试
好细好琐碎
```js
var a = []
if(a){
console.log(1)
}
```
if([])的结果是什么
判断一个数组是不是空  如果是空 就会输出1 true
if({}) 结果为 true
真值判断什么的
== 与===
比如 null==undefined的结果 undefined NaN null
img是行内元素！！！
不定宽元素水平居中
清除浮动
回调函数的this指向哪里

神马的常规问题
百度的题目总让我觉得是为了出题而出题 答的时候感觉好难受 orz

-------------------------------第一版
题目主要参考自[hawx1993](https://github.com/hawx1993/Front-end-Interview-questions)和[markyun](https://github.com/markyun/My-blog)
##HTML

###Doctype
告诉浏览器解析器以什么标准解析文档。
###文档模式：标准(严格)模式 混杂模式(兼容模式)
文档模式决定了可以使用什么功能（能够使用哪个级别的css，js中哪些api，以及如何对待文档类型Doctype）
```html
<!--强制浏览器以某种模式渲染页面，可以使用 HTTP 头部信息 X-UA-Compatible，或通过等价的<meta>标签来设置-->
<meta http-equiv="X-UA-Compatible" content="IE=IEVersion">
```
混杂模式会让 IE 的行为与（包含非标准特性的）IE5 相同，而标准模式则让 IE 的行为更接近标准行为。
严格模式的排版和 JS 运作模式是 以该浏览器支持的最高标准运行。
在混杂模式中，页面以宽松的向后兼容的方式显示。模拟老式浏览器的行为以防止站点无法工作。
DOCTYPE**不存在**或**格式不正确**会导致文档以混杂模式呈现。

###行内、块级、空元素
（1）行内元素有：a span  input select strong（强调的语气）
（2）块级元素有：div img  ul ol li dl dt dd h1 h2 h3 h4… p
（3）常见的空元素：br  hr img input link  meta

###浏览器内核
**渲染引擎**(layout engineer或Rendering Engine)和**JS引擎**。
**渲染引擎**：负责取得网页的内容（HTML、XML、图像等等）、整理讯息（例如加入CSS等），以及计算网页的显示方式，然后会输出至显示器或打印机。
浏览器的内核的不同对于网页的语法解释会有不同，所以渲染的效果也不相同。所有网页浏览器、电子邮件客户端以及其它需要编辑、显示网络内容的应用程序都需要内核。

**JS引擎**：解析和执行javascript来实现网页的动态效果。

最开始渲染引擎和JS引擎并没有区分的很明确，后来JS引擎越来越独立，内核就倾向于只指渲染引擎。

**常见的浏览器内核**

Trident内核：IE,MaxThon,TT,The World,360,搜狗浏览器等。[又称MSHTML]
Gecko内核：Netscape6及以上版本，FF,MozillaSuite/SeaMonkey等
Presto内核：Opera7及以上。      [Opera内核原为：Presto，现为：Blink;]
Webkit内核：Safari,Chrome等。   [ Chrome的：Blink（WebKit的分支）]

###HTML语义化
用正确的标签做正确的事情。
1. 去掉或者丢失样式的时候能够让页面呈现出清晰的结构 
2. 有利于SEO(搜索引擎最佳化)：和搜索引擎建立良好沟通；
3. 有助于爬虫抓取更多的有效信息：爬虫依赖于标签来确定上下文和各个关键字的权重
4. 便于团队开发和维护，语义化更具可读性

###HTML5离线存储
```html
<!DOCTYPE HTML>
<html manifest = "cache.manifest">
...
</html>
```
```
CACHE MANIFEST
#v0.11
CACHE:
js/app.js
css/style.css
NETWORK:
resourse/logo.png
FALLBACK:
/ /offline.html
```
>离线存储的manifest一般由三个部分组成:
1.**CACHE（需要离线存储的）**：表示需要离线存储的**资源列表**，由于包含manifest文件的页面将被自动离线存储，所以不需要把页面自身也列出来。
2.**NETWORK（不需要离线存储的）**：表示在它下面列出来的资源只有在在线的情况下才能访问，他们**不会被离线存储**，所以在离线情况下无法使用这些资源。不过，如果在CACHE和NETWORK中有一个相同的资源，那么这个资源还是会被离线存储，也就是说CACHE的优先级更高。
3.**FALLBACK（替换资源）**：表示如果访问第一个资源失败，那么就使用第二个资源来替换他，比如上面这个文件表示的就是如果访问根目录下任何一个资源失败了，那么就去访问offline.html。


**浏览器怎么解析manifest**
在线：
1. 浏览器发现html头部有manifest属性-->
2. 请求manifest文件-->
3. 是否第一次访问？-->
4. 是：下载资源离线存储（浏览器就会根据manifest文件的内容下载相应的资源并且进行离线存储）
5. 否：使用离线的资源加载页面 -->

 浏览器对比新旧manifest文件 -->
 文件是否发生改变？-->
 是：重新下载文件中的资源并进行离线存储。
 否：不做任何操作。

离线：浏览器直接使用离线资源

注意：
1. **资源manifest文件都要更新**：服务器对离线的资源进行了更新，那么必须更新manifest文件之后这些资源才能被浏览器重新下载。
2. **manifest文件不设缓存**：对manifest文件进行了更新，但是http的缓存规则告诉浏览器本地缓存的manifest文件还没过期，这个情况下浏览器还是使用原来的manifest文件，所以对于manifest文件最好**不要设置缓存**。
3. **一个失败全部失败**：浏览器在下载manifest文件中的资源的时候，它会一次性下载所有资源，如果某个资源由于某种原因下载失败，那么这次的所有更新就算是失败的，浏览器还是会使用原来的资源。
4.在更新了资源之后，新的资源需要到下次再打开app才会生效，如果需要资源马上就能生效，那么可以使用window.applicationCache.swapCache()方法来使之生效，

[https://segmentfault.com/a/1190000000732617](https://segmentfault.com/a/1190000000732617)
[https://developer.mozilla.org/zh-CN/docs/Web/HTML/Using_the_application_cache](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Using_the_application_cache)
###Label的作用
label标签来定义和表单控件的关系,当用户选择该标签时，浏览器会自动将焦点转到和标签相关的表单控件上。
```HTML
<label for="Name">Number:</label>
<input type=“text“name="Name" id="Name"/>

<label>Date:<input type="text" name="B"/></label>
```
###如何实现浏览器内多个标签页之间的通信  ???
调用localstorge、cookies等本地存储方式

###webSocket
WebSocket是Web应用程序的传输协议，它提供了双向的，按序到达的数据流。他是一个HTML5协议，WebSocket的连接是持久的，他通过在客户端和服务器之间保持双工连接，服务器的更新可以被及时推送给客户端，而不需要客户端以一定时间间隔去轮询。
适用于服务器主动推送消息的场景

###cookie，sessionStorage 和 localStorage  ***
| 特性    |     Cookie |   localStorage   |   	sessionStorage  |
| :-------- | --------:| :------: | :------: |
| 数据的生命期   |   可设置失效时间，默认是关闭浏览器后失效 |  除非被清除，否则永久保存 |仅在当前会话下有效，关闭页面或浏览器后被清除  |
|存放数据大小   |   4K左右 | 一般为5MB |一般为5MB |
| 与服务器端通信   |   每次都会携带在HTTP头中，如果使用cookie保存过多数据会带来性能问题 |  仅在客户端（即浏览器）中保存，不参与和服务器的通信  |仅在客户端（即浏览器）中保存，不参与和服务器的通信 |
| 易用性  |  	需要程序员自己封装，源生的Cookie接口不友好 |  源生接口可以接受，亦可再次封装来对Object和Array有更好的支持 |源生接口可以接受，亦可再次封装来对Object和Array有更好的支持  |

[https://segmentfault.com/a/1190000002723469](https://segmentfault.com/a/1190000002723469)
[https://developer.mozilla.org/zh-CN/docs/Web/Guide/API/DOM/Storage](https://developer.mozilla.org/zh-CN/docs/Web/Guide/API/DOM/Storage)
[http://www.zhangxinxu.com/wordpress/2011/09/html5-localstorage%E6%9C%AC%E5%9C%B0%E5%AD%98%E5%82%A8%E5%AE%9E%E9%99%85%E5%BA%94%E7%94%A8%E4%B8%BE%E4%BE%8B/](http://www.zhangxinxu.com/wordpress/2011/09/html5-localstorage%E6%9C%AC%E5%9C%B0%E5%AD%98%E5%82%A8%E5%AE%9E%E9%99%85%E5%BA%94%E7%94%A8%E4%B8%BE%E4%BE%8B/)
###data-* 
js访问：用dataset获取自定义属性
css访问：attr和属性选择器如article[data-columns='3']

[Using data attributes](https://developer.mozilla.org/zh-CN/docs/Web/Guide/HTML/Using_data_attributes)
###html5新特性、移除了那些元素？如何处理HTML5新标签的浏览器兼容问题？如何区分 HTML 和 HTML5？
  HTML5 现在已经不是 SGML 的子集，主要是关于图像，位置，存储，多任务等功能的增加。
>新特性
拖拽释放(Drag and drop) API
语义化更好的内容标签（header,nav,footer,aside,article,section）
音频、视频API(audio,video)
画布(Canvas) API
地理(Geolocation) API
本地离线存储 localStorage 长期存储数据，浏览器关闭后数据不丢失；
sessionStorage 的数据在浏览器关闭后自动删除
表单控件，calendar、date、time、email、url、search
新的技术webworker, websocket, Geolocation


###js执行 window.onload document.ready 
**window.onload**必须等到页面内包括图片的所有元素加载完毕后才能执行。
**$(document).ready()**是DOM结构绘制完毕后就执行，不必等到加载完毕。
head里的 script标签会阻塞后续资源的载入以及整个页面的生成。可以使用window.onload或是docmuemt ready来防止阻塞
>js载入后马上执行，执行时会阻塞页面后续的内容（包括页面的渲染、其它资源的下载）




[Javascript 装载和执行](http://coolshell.cn/articles/9749.html)
[JavaScript 的性能优化：加载和执行](https://www.ibm.com/developerworks/cn/web/1308_caiys_jsload/)

----

##CSS
###盒模型
（1）有两种， IE 盒子模型、标准 W3C 盒子模型；IE的content部分包含了 border 和 padding;
（2）盒模型： 内容(content)、内边距(padding)、外边距(margin)、 边框(border).
###position  ***
1. static：默认，元素在标准文档流。元素不会被特殊的定位。一个 static 元素表示它不会被“positioned”
2. relative：相对于元素原来的位置（在标准文档流的位置）。设置 top 、 right 、 bottom 和 left 属性会使其偏离其正常位置。其他的元素则**不会**调整位置来**弥补**它偏离后剩下的**空隙**。
3. absolute：绝对定位，相对于最近一个不是static的父元素定位。脱离标准文档流，其他元素**会**调整位置来**弥补**它偏离后剩下的**空隙**。
4. fixed：绝对定位，相对于窗口定位。top 、 right 、 bottom 和 left 属性都可用。
5. sticky：在屏幕中时按常规流排版，当卷动到屏幕外时则表现如fixed。
6. center(不支持)：水平垂直居中
7. page(不支持)：
[http://zh.learnlayout.com/position.html](http://zh.learnlayout.com/position.html)

###position:absolute 和 float 属性的异同  ***
共同点：对**内联元素**设置 float 和 absolute 属性，可以让元素脱离文档流，并且可以设置其宽高。
不同点： float 仍会占据位置， position 会覆盖文档流中的其他元素。
###display:none 和 visibility:hidden 的区别
display:none 隐藏对应的元素，在文档布局中不再给它分配空间，就当他从来不存在。**不保留空间**
 visibility:hidden 隐藏对应的元素，但是在文档布局中仍保留原来的空间。**保留空间**
###link  和 @import 的区别
1. link属于HTML标签，而@import是CSS提供的
2. 当一个页面被加载的时候（就是被浏览者浏览的时候），link引用的CSS会同时被加载，而@import引用的CSS 会等到页面全部被下载完再被加载。
3. import只在IE5以上才能识别，而link是HTML标签，无兼容问题
4. 使用dom控制样式时的差别。当使用javascript控制dom去改变样式的时候，只能使用link标签，因为**@import不是dom可以控制**的。
```html
<!--@import放在其他除了@charset规则以外的CSS规则的前面-->
<style type=”text/css”>
@import url(Path To stylesheet.css)
</style>

<link rel=”stylesheet” type=”text/css” href=“Path To stylesheet.css” />
```
[http://stackoverflow.com/questions/1022695/difference-between-import-and-link-in-css](http://stackoverflow.com/questions/1022695/difference-between-import-and-link-in-css)
[http://www.daqianduan.com/2417.html](http://www.daqianduan.com/2417.html)

###选择符优先级
!important >内联 > id >伪类 >属性选择 >类class >元素选择器tag >通用*
优先级的算法：
每个规则对应一个初始"四位数"：0、0、0、0
若是 行内选择符，则加1、0、0、0
若是     ID选择符，则加0、1、0、0
若是 类选择符/属性选择符/伪类选择符，则分别加0、0、1、0
若是 元素选择符/伪元素选择符，则分别加0、0、0、1
算法：将每条规则中，选择符对应的数相加后得到的”四位数“，从左到右进行比较，大的优先级越高。

需注意的是：
①!important的优先级是最高的，但出现冲突时则需比较”四位数“；
②优先级相同时，则采用就近原则；
③继承得来的属性，其优先级最低；

###伪类伪元素***
 伪类 -- 指定某一元素被选中后的特殊状态时,一个 CSS 伪类 会被作为一个关键词添加到选择器上。        
  :link、:visited、:hover、:active、:focus、:first-child
 
 [https://developer.mozilla.org/zh-CN/docs/Web/CSS/Pseudo-classes](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Pseudo-classes)
 伪元素 -- 代表了某个元素的子元素，这个子元素虽然在逻辑上存在，但却并不实际存在于文档树中。
  :before、:after、:first-line、:first-letter、:selection、:backdrop
  
>比如我们设置:hover、:first-child，是不是就相当于给指定的元素添加了class="hover first-child",而::before，相当于在指定元素前定义一个元素，等效的效果我们可以通过在HTML添加实际元素来实现。  
  注意：有时你会发现伪元素使用了两个冒号 (::) 而不是一个冒号 (:). 这是 CSS3 规范中的一部分要求,目的是为了区分伪类和伪元素 .
  
```html
伪类
  p>i:first-child {color: red}
<p>
    <i>first</i>
    <i>second</i>
</p>
等价于
<p>
    <i class="first-child">first</i>
    <i>second</i>
</p>

伪元素
p:first-letter{color: red;}
<p>I Love You</p>
等价于
.first-letter {color: red}
<p><span class='first-letter'>I</span>Love You.</p>
```
 [https://developer.mozilla.org/zh-CN/docs/Web/CSS/Pseudo-elements](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Pseudo-elements)
 [http://www.haorooms.com/post/css_wl_wys](http://www.haorooms.com/post/css_wl_wys)    
 [http://codesky.me/archives/css-fake-class-and-element-understand.wind](http://codesky.me/archives/css-fake-class-and-element-understand.wind)

###box-sizing
值
**content-box：**
默认值，标准盒模型。 width 与 height 只包括内容的宽和高， 不包括边框（border），内边距（padding），外边距（margin）。 
布局宽度
Width = width + padding-left + padding-right + border-left + border-right
布局高度
Height = height + padding-top + padding-bottom + border-top + border-bottom padding-box
**border-box：**
Height = height(包含padding-top + padding-bottom+ border-top + border-bottom）
Height = height(包含padding-top + padding-bottom + border-top + border-bottom)

###居中***
####水平居中
#####行内元素--父元素为块级元素设置 text-align:center;
#####块级元素
**定宽块级**元素--margin-left 和 margin-right 设置为 auto
不定宽块级元素--
1. dispaly 设置为 inline 类型，然后使用 text-align:center 来实现居中效果。
缺点：丢失盒子模型的特性
2. relative相对定位 父元素设置` float:left; position:relative; left:50%;`子元素设置`position:relative; left:-50%;`
3. absolute绝对定位
```css 
 .main{position: absolute;
          left: 50%;
          margin-left: -480px;}/*将margin-left设置为负的div宽度的一半*/                         
```
4. flex-box 父元素设置
```css   
.container {
    height: 300px;
    display:flex;
    justify-content: center;//项目在主轴上的对齐方式。
 }
 [http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)
```
####垂直居中
#####行内元素
单行 -- 对于单行行内或者文本元素，只需为它们添加等值的 padding-top 和 padding-bottom 就可以实现垂直居中
#####块级元素
定高
```css
.parent {
  position: relative;
}
.child {
  position: absolute;
  top: 50%;
  height: 100px;
  margin-top: -50px; /* account for padding and border if not using box-sizing: border-box; */
}
```
不定高
1.
```css
.parent {
  position: relative;
}
.child {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
}
```
2.flexbox
```css
.parent {
  display: flex;
  flex-direction: column;//主轴方向
  justify-content: center;//项目在主轴上的对齐方式。
}
```
[http://www.w3cplus.com/css/centering-css-complete-guide.html](http://www.w3cplus.com/css/centering-css-complete-guide.html)

####不定宽高居中***
#####flex

###position absolute后设置left right均为0块会在哪里
答左边
[https://www.w3.org/TR/css3-positioning/#abs-non-replaced-width](https://www.w3.org/TR/css3-positioning/#abs-non-replaced-width)

###flexbox 弹性盒状布局***

[http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html)



###CSS3有哪些新特性？
CSS3实现圆角（border-radius），阴影（box-shadow），
对文字加特效（text-shadow、），线性渐变（gradient），旋转（transform）
transform:rotate(9deg) scale(0.85,0.90) translate(0px,-30px) skew(-9deg,0deg);//旋转,缩放,定位,倾斜
增加了更多的CSS选择器  多背景 rgba
在CSS3中唯一引入的伪元素是::selection.
媒体查询，多栏布局
border-image

###浮动&&清除浮动***
浮动元素脱离文档流，不占据空间。浮动元素碰到包含它的边框或者浮动元素的边框停留。
浮动元素引起的问题：
（1）父元素的高度无法被撑开，影响与父元素同级的元素
（2）与浮动元素同级的非浮动元素（内联元素）会跟随其后
（3）若非第一个元素浮动，则该元素之前的元素也需要浮动，否则会影响页面显示的结构
清除浮动
1.使用空标签清除浮动。
   这种方法是在所有浮动标签后面添加一个空标签 定义css clear:both. 弊端就是增加了无意义标签。
```css
<div style="clear:both;"></div>
```
2.父标签使用overflow
   给包含浮动元素的父标签添加css属性 overflow:auto; zoom:1; zoom:1用于兼容IE6。

3.使用after伪对象清除浮动。
使用中需注意以下几点。一、该方法中必须为需要清除浮动元素的伪对象中设置 height:0，否则该元素会比实际高出若干像素；
```css
   #parent:after{
        content:".";
        height:0;
        visibility:hidden;
        display:block;
        clear:both;
        }

```
   
###CSS的clear
定义了元素的哪边上不允许出现浮动元素。这个规则只能影响使用清除的元素本身，不能影响其他元素。   

###圣杯布局 双飞翼布局***
1. 搭建 head content(main left right) foot, content 内部的三个元素全部左浮动，然后给content清除浮动防止影响 foot
2. 给 main 100% 的宽度让他占满一行
3.  给 left元素 -100% 的margin-left 让他移动到最左边，给 right元素 和他宽度一样的负 margin-left 让他移动到最右边
4.  针对移动后 main 的两边会被 left 和 right 重合覆盖掉做出不同的改变，这儿也就是两个布局的本质区别
-- 圣杯布局会给 content 内边距，左右分别为 left 和 right的宽度，然后再利用相对定位移动 left (position:relative; left:负元素宽度；)和 right(position:relative;right:负元素宽度;)
-- 双飞翼布局会在 main 里面再加一层 wrap ，然后把内容都写在 wrap 里面，正对 wrap 设置他的 margin, 左右外边距和 left 与 right 一样即可
 
[https://segmentfault.com/a/1190000004579886](https://segmentfault.com/a/1190000004579886) 

###负边距margin
static元素
* 负top/left  把元素向上/左拉
* 负bottom/right时，它并不会把元素向下或右拉，相反，它会把后面的元素往里面拉，从而覆盖自己。
* 如果宽度没有设置，左右负边距会把元素向两个方向拉以增加宽度。在这里margin的作用相当于padding

浮动中
* 如果对一个浮动的元素使用负边距，它会产生一个空白，其他元素就可以覆盖这一部分。这个技巧可以很好地用户流式布局。比如有一列宽度100%，另一列有固定的宽度，比如说100px。

简单的两列布局--IE6适配
```
  	#mydiv1 {float:left;width: 100%; background-color: red; margin-right:-100px;}
  	#mydiv2 {float:left;width: 100px; background-color: blue;}

	<div id="mydiv1">First</div>
	<div id="mydiv2">Second</div>
```	

￼

* 如果两个元素都使用了左浮动并且设置margin-right:-20px。#mydiv2会把#mydiv1看成宽度缩小20px（所以会覆盖一部分），但是有趣的是#mydiv1并不会有任何变化，而是依然保持原先的宽度。
* 如果负边距和宽度一样大的话，它就会被完全覆盖掉。因为外边距，内边距，边框和内容加起来等于元素的宽度。如果负外边距等于元素的宽度的话，那么该元素的宽度就会变成0px。
[https://segmentfault.com/a/1190000003942591](https://segmentfault.com/a/1190000003942591)

###baseline和vertical-align

###BFC
BFC化的元素，内部的样式不会影响外部其他元素……两个BFC元素垂直方向的margin会重叠

-----
##JavaScript
###同步异步 Promise
同步：顺序执行
异步：每一个任务有一个或多个回调函数（callback），前一个任务结束后，不是执行后一个任务，而是执行回调函数，后一个任务则是不等前一个任务结束就执行，所以程序的执行顺序与任务的排列顺序是不一致的、异步的。
>异步的过程：主线程发起一个异步请求，相应的工作线程接收请求并告知主线程已收到(异步函数返回)；主线程可以继续执行后面的代码，同时工作线程执行异步任务；工作线程完成工作后，通知主线程；主线程收到通知后，执行一定的动作(调用回调函数)。
异步的主要目的是处理非阻塞，在和HTML交互的过程中，会需要一些IO操作（典型的就是Ajax请求，脚本文件加载），如果这些操作是同步的，就会阻塞其它操作，用户的体验就是页面失去了响应。
####回调函数
```js
//setTimeout直接返回，匿名函数会在1000毫秒（不一定能保证是1000毫秒）后异步触发并执行，完成打印控制台的操作
setTimeout(function(){
    console.log("this will be exectued after 1 second!");
},1000);
```

```js
function f1(callback){
    console.log('f1')
    setTimeout(function(){
        console.log('f1 again')
        // some code
        callback();
    },1000);
}
function f2(){
    console.log('f2')
}
function f3(){
    console.log('f3')
}
// exec functions
f1(f2);
f3();
```
执行结果
f1
f3
f1 again //1s后
f2


####事件监听
[js异步编程](http://www.15yan.com/story/38LyDwz8Lwc/)
[Javascript异步编程的4种方法](http://www.ruanyifeng.com/blog/2012/12/asynchronous%EF%BC%BFjavascript.html)
[彻底理解同步异步](https://segmentfault.com/a/1190000004322358)
###变量
基本数据类型：Undefined、 Null、 Boolean、 Number 和 String。按值访问。**typeof** 操作符是确定一个变量是字符串、数值、布尔值，还是 undefined 的最佳工具。
```js
var s = "Nicholas";
alert(typeof s); //string
```
引用类型：Object、Array、Date、RegExp、Function
如果变量是给定引用类型（根据它的原型链来识别；第 6 章将介绍原型链）的实例，那么
**instanceof **操作符就会返回 true。
```js
alert(person instanceof Object); // 变量 person 是 Object 吗？
alert(colors instanceof Array); // 变量 colors 是 Array 吗？
alert(pattern instanceof RegExp); // 变量 pattern 是 RegExp 吗？
```
###json
JSON格式及其简单，它只是数组和对象字面量的混合写法
{"name": "value", "some": [1, 2, 3]}
JSON和对象字面量在语法上的唯一区别是，合法的JSON属性名均需要用**引号包含**。而在对象字面量中，只有属性名是非法的标识符时才使用引号包含，比如，属性名中包含空格{"first name": "Dave"}。

在JSON字符串中，不能使用**函数**和**正则表达式字面量**。
出于安全考虑，不推荐使用eval()来粗暴地解析JSON字符串。最好是使用JSON.parse()方法
[https://github.com/TooBug/javascript.patterns/blob/master/chapter3.markdown](https://github.com/TooBug/javascript.patterns/blob/master/chapter3.markdown)
###==(转换类型比较运算符)和===(严格比较运算符)
==为两个不同类型的操作数转换类型，然后进行严格比较。
当两个操作数都是对象时，JavaScript会比较其内部引用，当且仅当他们的引用指向内存中的相同对象（区域）时才相等，即他们在栈内存中的引用地址相同。
===不会进行类型转换，仅当操作数严格相等时返回true
```js
 1   ==  1     // true
"1"  ==  1     // true
 1   == '1'    // true
 0   == false  // true

3 === 3   // true
3 === '3' // false
```
[https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Comparison_Operators](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Comparison_Operators)

###null和undefined
null表示"没有对象"，即该处不应该有值，转为数值时为**0**；
使用 typeof 操作符检测 null 值时会返回**"object"**
**如果定义的变量准备在将来用于保存对象，那么最好将该变量初始化为 null。**

undefined表示"缺少值"。
对**未初始化**和**未声明**的变量执行 typeof 操作符都返回了 undefined 值
```js
var message; // 这个变量声明之后默认取得了 undefined 值
// 下面这个变量并没有声明
// var age
alert(typeof message); // "undefined"
alert(typeof age); // "undefined"
```
转为数值时为**NaN**。

>如果你本来就声明了一个变量，那么给这个变量赋予 Undefined 还是 Null 来**表示初始化与否**，则你个人的编程习惯（一般用 Null ）。
如果你自己严格遵守“声明了但是未初始化”就用 Null，那么 Undefined 就可以被用来确实判断变量是否声明与否，或者对象属性生命与否的标记。
[链接](https://www.zhihu.com/question/20164319/answer/20246104)
null 与 undefined 的不同点：
```js
typeof null        // object (bug in ECMAScript, should be null)
typeof undefined   // undefined
null === undefined // false
null  == undefined // true
```
[https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/null](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/null)
[http://www.ruanyifeng.com/blog/2014/03/undefined-vs-null.html](http://www.ruanyifeng.com/blog/2014/03/undefined-vs-null.html)
[https://leohxj.gitbooks.io/front-end-database/content/javascript-basic/difference-between-undefined-and-null.html](https://leohxj.gitbooks.io/front-end-database/content/javascript-basic/difference-between-undefined-and-null.html)
###NaN--Not a Number

NaN用于表示一个本来要返回数值的操作数未返回数值的情况（这样就不会抛出错误了）。例如，在其他编程语言中，任何数值除以 0 都会导致错误，从而停止代码执行。但在 ECMAScript 中，任何数值除以 0 会返回 NaN。
与 JavaScript 中其他的值不同，NaN不能通过相等操作符（== 和 ===）来判断 ，因为 NaN == NaN 和 NaN === NaN 都会返回 false。 因此，isNaN 就很有必要了。

###回调函数


###事件代理*** 怎么区分子元素不同button
当我们需要对很多元素添加事件的时候，可以通过将事件添加到它们的父节点而将事件委托给父节点来触发处理函数。
主要得益于浏览器的事件冒泡机制.

任何可以冒泡的事件都不仅仅可以在事件目标上进行处理，目标的任何祖先节点上也能处理。使用这个知识，就可以将事件处理程序附加到更高层的地方负责多个目标的事件处理。
[http://www.cnblogs.com/owenChen/archive/2013/02/18/2915521.html](http://www.cnblogs.com/owenChen/archive/2013/02/18/2915521.html)
###Ajax --js高程21章
AJAX的工作原理
Ajax的工作原理相当于在用户和服务器之间加了—个中间层(AJAX引擎),使用户操作与服务器响应异步化。并不是所有的用户请求都提交给服务器,像—些数据验证和数据处理等都交给Ajax引擎自己来做, 只有确定需要从服务器读取新数据时再由Ajax引擎代为向服务器提交请求。

Ajax其核心有JavaScript、XMLHTTPRequest、DOM对象组成，通过XmlHttpRequest对象来向服务器发异步请求，从服务器获得数据，然后用JavaScript来操作DOM而更新页面

###跨域 --js高程21章
同源 --协议 端口 主机名相同
相对http://store.company.com/dir/page.html同源检测的示例:
https://store.company.com/secure.html	失败	协议不同
http://store.company.com:81/dir/etc.html	失败	端口不同
http://news.company.com/dir/other.html	失败	主机名不同

对于XMLHttpRequest，只能访问同源对象的内容
	* 允许同源会导致敏感数据泄露，如CSRF的token
####设置document.domain 的值为当前域的一个后缀--让子域访问其父域--主域相同时使用
在同源策略中有一个例外，脚本可以设置 document.domain 的值为当前域的一个后缀，如果这样做的话，短的域将作为后续同源检测的依据。
例如，假设在 http://**store.company.com**/dir/other.html 中的一个脚本执行了下列语句：
```js
document.domain = "company.com";
```
本来两个页面主机名不同源，这条语句执行之后，页面将会成功地通过对 http://**company.com**/dir/page.html 的同源检测。而同理，company.com 不能设置 document.domain 为 othercompany.com.
注意：这两个域名必须属于同一个基础域名!而且所用的协议，端口都要一致，否则无法利用document.domain进行跨域
[https://developer.mozilla.org/zh-CN/docs/Web/Security/Same-origin_policy](https://developer.mozilla.org/zh-CN/docs/Web/Security/Same-origin_policy)
####CORS Cross-site HTTP request 跨站http请求
CORS：发起请求的资源所在域不同于该请求所指向资源所在的域的 HTTP 请求。域名A请求域名B的资源
设置```Access-Control-Allow-Origin```
```js
//如果服务器端仅允许来自 http://foo.example 的跨站请求，它可以返回：
Access-Control-Allow-Origin: http://foo.example
```
[https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Access_control_CORS](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Access_control_CORS)
####jsonp
使用jsonp进行跨域 --json+padding（内填充），顾名思义，就是把JSON填充到一个盒子里
缺点：只支持GET请求。

####window.name
只要不关闭浏览器，window.name可以在不同页面加载后依然保持。
子页面bar.com/b.html向父页面foo.com/a.html传数据。
```html
<!--父页面  foo.com/a.html -->
<iframe id="ifr" src="http://bar.com/b.html"></iframe>
<script>
function callback(data) {
    console.log(data)
}
</script>

<!--子页面 bar.com/b.html -->
<input id="txt" type="text">
<input type="button" value="发送" onclick="send();">
<script>
var proxyA = 'http://foo.com/aa.html';    // foo.com下代理页面
var proxyB = 'http://bar.com/bb.html';    // bar.com下代理空页面

var ifr = document.createElement('iframe');
ifr.style.display = 'none';
document.body.appendChild(ifr);

function send() {
    ifr.src = proxyB;
}

ifr.onload = function() {
    ifr.contentWindow.name = document.querySelector('#txt').value;
    ifr.src = proxyA;
}
</script>

<!-- foo.com/aa.html -->
top.callback(window.name)

[https://segmentfault.com/a/1190000000702539](https://segmentfault.com/a/1190000000702539)
```
####window.postMessage
.postMessage(message, targetOrigin)参数说明

message: 是要发送的消息，类型为 String、Object (IE8、9 不支持)
targetOrigin: 是限定消息接收范围，不限制请使用 '*'
'message', function(e)回调函数第一个参数接收 Event 对象，有三个常用属性：

data: 消息
origin: 消息来源地址
source: 源 DOMWindow 对象
假设window A主动向window B post消息（B可以是A的iframe或frame或A open出来的） 同时B中的代码也是你可以修改的。
```html
<!--A 中代码, http://www.exmaple.com-->
B.postMessage("move!","http://chat.example.com");
A.addEventListener("message",function(e){
  console.log(e.data);
});
<!--B 中代码，http://chat.example.com-->
B.addEventListener("message",function(e){
  console.log(e.data);
  e.source.postMessage("rager that!");
});

```
上面代码，A向B发送 'move!'，B收到消息后返回'rager that!'
[https://developer.mozilla.org/zh-CN/docs/Web/API/Window/postMessage](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/postMessage)
[https://github.com/aztack/Reading-Notes/blob/master/web/interview/cross-domain.md](https://github.com/aztack/Reading-Notes/blob/master/web/interview/cross-domain.md)
###闭包 
当函数可以记住并访问所在的词法作用域时， 就产生了闭包， 即使函数是在当前词法作用域之外执行。
使用了回调函数就是使用了闭包

**函数内部可以直接读取全局变量，在函数外部自然无法读取函数内的局部变量。**
函数内部声明变量的时候，一定要使用var命令。如果不用的话，你实际上声明了一个全局变量！

从外部读取局部变量 -->函数内部再定义一个函数 闭包
一个持有外部环境变量的函数就是闭包。 

闭包的作用：一个是前面提到的可以**读取函数内部的变量**，另一个就是让这些**变量的值始终保持在内存中**。
最简单的理解就是瞬间执行一个函数 而这个函数的内部返回的是另一个函数 在外部函数的变量被内部函数使用了 作用域链不能访问到内部函数的东西 所以调用有一定的安全性 .

```js
function foo() {
var a = 2;
function bar() {
console.log( a );
}
return bar;
}
var baz = foo();
baz(); // 2 —— 朋友， 这就是闭包的效果。
```

####循环和闭包
```js
for (var i=1; i<=5; i++) {
setTimeout( function timer() {
console.log( i );
}, i*1000 );
}
```
正常情况下， 我们对这段代码行为的预期是分别输出数字 1~5， 每秒一次， 每次一个。
但实际上， 这段代码在运行时会以每秒一次的频率输出五次 6。
延迟函数的回调会在循环结束时才执行。 事实上，当定时器运行时即使每个迭代中执行的是 setTimeout(.., 0)， 所有的回调函数依然是在循环结束后才会被执行， 因此会每次输出一个 6 出来。
```js
for (var i=1; i<=5; i++) {
(function(j) {
setTimeout( function timer() {
console.log( j );
}, j*1000 );
})( i );
}
```

参考资料：
《你不知道的JavaScript》
[http://www.ruanyifeng.com/blog/2009/08/learning_javascript_closures.html](http://www.ruanyifeng.com/blog/2009/08/learning_javascript_closures.html)
###cookie session
 1、cookie数据存放在客户的**浏览器**上，session数据放在**服务器**上。
 2、cookie不是很安全，别人可以分析存放在本地的COOKIE并进行COOKIE欺骗考虑到安全应当使用session。
 3、session会在一定时间内保存在服务器上。当访问增多，会比较占用你服务器的性能考虑到减轻服务器性能方面，应当使用COOKIE。
 4、单个cookie保存的数据不能超过4K，很多浏览器都限制一个站点最多保存20个cookie。
 5、将登陆信息等重要信息pfrrr存放为SESSION，其他信息如果需要保留，可以放在COOKIE中


每个 HTTP 请求和响应都会带有相应的头部信息，其中有的对开发人员有用，有的也没有什么用。
XHR 对象也提供了操作这两种头部（即请求头部和响应头部）信息的方法。
默认情况下，在发送 XHR 请求的同时，还会发送下列头部信息。
 Accept：浏览器能够处理的内容类型。
 Accept-Charset：浏览器能够显示的字符集。
 Accept-Encoding：浏览器能够处理的压缩编码。
 Accept-Language：浏览器当前设置的语言。
 Connection：浏览器与服务器之间连接的类型。
 Cookie：当前页面设置的任何 Cookie。
 Host：发出请求的页面所在的域 。
 Referer：发出请求的页面的 URI。注意， HTTP 规范将这个头部字段拼写错了，而为保证与规
范一致，也只能将错就错了。（这个英文单词的正确拼法应该是 referrer。）
 User-Agent：浏览器的用户代理字符串。

###作用域链
作用域链的作用是保证执行环境里有权访问的变量和函数是有序的，作用域链的变量只能向上访问，变量访问到window对象即被终止，作用域链向下访问变量是不被允许的。

###onmousemove和onmouseover的区别
时间上：onmousemove事件触发后，再触发onmouseover事件。
按钮上：不区分鼠标按钮。
动作上：onmouseover只在刚进入区域时触发，onmousemove除了刚进入区域触发外，在区域内移动鼠标，也会触发
###创建js对象的两种方法 以及字面量
```js
//1.使用 new 操作符后跟 Object 构造函数
var person = new Object();
//2.对象字面量表示法 大括号
var person = {
name : "Nicholas",
age : 29
};
```
字面量写法的一个明显优势是，它的代码更少。它可以强调对象就是一个简单的可变的散列表，而不必一定派生自某个类。
另外一个使用字面量而不是Object()构造函数创建实例对象的原因是，对象字面量**不需要“作用域解析”**（scope resolution）。因为存在你已经创建了一个同名的构造函数Object()的可能，当你调用Object()的时候，解析器需要顺着作用域链从当前作用域开始查找，直到找到全局Object()构造函数为止。
[https://github.com/TooBug/javascript.patterns/blob/master/chapter3.markdown](https://github.com/TooBug/javascript.patterns/blob/master/chapter3.markdown)
###创建数组
```js
//1.使用 Array 构造函数
var arr = new Array();
//2.数组字面量表示法
var arr = [];
```
###变量提升
变量提升指的是，无论是哪里的变量在一个范围内声明的，那么JavaScript引擎会将这个声明移到范围的顶部。
```js
a = 2;
var a;
console.log( a );//2
//------------------------------------------
console.log( b );//undefined
var b = 2;
```
只有声明本身会被提升， 而赋值或其他运行逻辑会留在原地。 

###全局变量的风险
“全局变量”是**不在任何函数体内部声明的变量**，或者是**直接使用而未声明**的变量。
全局变量的危险之处在于其他人可以创建相同名称的变量，然后覆盖你正在使用的变量。
全局变量在整个JavaScript应用或者是整个web页面中是始终被所有代码共享的。它们存在于同一个命名空间中，因此命名冲突的情况会时有发生。
假设某一段第三方提供的脚本定义了一个全局变量result。随后你在自己写的某个函数中也定义了一个全局变量result。这时，第二个变量就会覆盖第一个，会导致第三方脚本工作不正常。

--减少全局变量最有效的方法还是坚持使用var来声明变量。 ？？？

####隐式全局变量
JavaScirpt中具有“隐式全局对象”的概念，也就是说**任何不通过var声明的变量都会成为全局对象的一个属性**（可以把它们当作全局变量）。(译注：在ES6中可以通过let来声明块级作用域变量。)

####隐式全局变量和显式的区别
隐式创建的全局变量和显式定义的全局变量之间有着细微的差别，就是通过delete来删除它们的时候表现不一致。
* 通过var创建的全局变量（在任何函数体之外创建的变量）不能被删除。
* 没有用var创建的隐式全局变量（不考虑函数内的情况）可以被删除。
也就是说，隐式全局变量并不算是真正的变量，但它们却是全局对象的属性。属性是可以通过delete运算符删除的，而变量不可以被删除：
```js
// 定义三个全局变量
var global_var = 1;
global_novar = 2; // 反模式
(function () {
    global_fromfunc = 3; // 反模式
}());

// 尝试删除
delete global_var; // false
delete global_novar; // true
delete global_fromfunc; // true

// 测试删除结果
typeof global_var; // "number"
typeof global_novar; // "undefined"
typeof global_fromfunc; // "undefined"
```
[https://github.com/TooBug/javascript.patterns/blob/master/chapter2.markdown](https://github.com/TooBug/javascript.patterns/blob/master/chapter2.markdown)
###如何通过JavaScript对象中的成员变量迭代？???
```js
for(var prop in obj){
    // bonus points for hasOwnProperty
    if(obj.hasOwnProperty(prop)){
        // do something here
    }
}
```
###内存泄露？？
###垃圾回收？？
标记清除
引用计数 reference counting
###js继承
原型链继承
构造函数继承
组合式继承
###js设计模式

####构造函数模式
使用构造函数的方法 ，即解决了重复实例化的问题 ，又解决了对象识别的问题，该模式与工厂模式的不同之处在于：
1.构造函数方法没有显示的创建对象 (new Object());
2.直接将属性和方法赋值给 this 对象;
3.没有 renturn 语句。
####工厂模式

主要好处就是可以消除对象间的耦合，通过使用工程方法而不是new关键字。将所有实例化的代码集中在一个位置防止代码重复。
工厂模式解决了重复实例化的问题 ，但还有一个问题,那就是识别问题，因为根本无法 搞清楚他们到底是哪个对象的实例。
```js
function createObject(name,age,profession){//集中实例化的函数var obj = new Object();
    obj.name = name;
    obj.age = age;
    obj.profession = profession;
    obj.move = function () {
        return this.name + ' at ' + this.age + ' engaged in ' + this.profession;
    };
    return obj;
}
var test1 = createObject('trigkit4',22,'programmer');//第一个实例var test2 = createObject('mike',25,'engineer');//第二个实例
```
### DOM操作***
document是每个文档的根节点

![](~/14-41-35.jpg)

```js
//创建
document.createElement(name)
document.createTextNode()
element.cloneNode(true/false)
//节点查找
element.parentNode
element.childNodes
element.nextSibling
element.previousSibling
//--jQuery 节点查找
element.parent
element.children
element.siblings
//获取节点
docuemnt.querySelector//接收一个 CSS 选择符，返回与该模式匹配的第一个元素，如果没有找到匹配的元素，返回 null。
document.getElementById(id)
document.getElementsByTagName(name)
document.getElementsByName(name)
document.getElementsByClassName(classname)
//插入删除节点
element.appendChild()
element.insertBefore
element.removeChild
element.replaceChild
//jq 插入删除节点
a.insertAfter(b)//a插到b后面
a.after(b)//a插入到b前面
a.insertBefore()
.before()
.appendTo()
.append()
.prependTo()
.prepend()
//元素属性
element.setAttribute
element.getAttribute
textNode.textContent
//事件监听
element.addEventListener
element.removeEventListener
//XML事件监听
event.dispatch//给节点分派一个合成事件。
document.createEvent

//Css
getComputedStyle
getClientBoundingRect
getClientRects
```
插入元素 createElement appendChild或者innerHTML（这种方法插入的script脚本不会执行，不支持col head table 等，使用时最好手工删除要被替换的元素的所有事件处理程序和js对象属性）
```js
for (var i=0, len=values.length; i < len; i++){
ul.innerHTML += "<li>" + values[i] + "</li>"; //要避免这种频繁操作！！
}
//这个例子的效率要高得多，因为它只对 innerHTML 执行了一次赋值操作。
var itemsHtml = "";
for (var i=0, len=values.length; i < len; i++){
itemsHtml += "<li>" + values[i] + "</li>";
}
ul.innerHTML = itemsHtml;
```
[jquery设计思想](http://www.ruanyifeng.com/blog/2011/07/jquery_fundamentals.html)

###阻止默认行为和阻止冒泡 区别？***
　event.preventDefault() 阻止事件的默认行为（比如点击链接，会自动打开新页面）
　event.stopPropagation()阻止目标元素的事件冒泡到父级元素。

![](~/16-08-10.jpg)

输出结果 前三个是事件捕获，后面body和html事件冒泡
div callback
UL
LI
BODY
HTML
去掉event.stopPropagation() 呢？
答案：由于事件同时有捕获和冒泡时先执行捕获，捕获到div之后，事件被阻止，后面就不在继续传播了。所以只输出div callback.


----

##网络
###HTTP状态码***
    **100  Continue  继续，一般在发送post请求时，已发送了http header之后服务端将返回此信息，表示确认，之后发送具体参数信息
    **200  OK   正常返回信息
    201  Created  请求成功并且服务器创建了新的资源
    202  Accepted  服务器已接受请求，但尚未处理
   ** 301  Moved Permanently  请求的网页已永久移动到新位置。
    302 Found  临时性重定向。
    303 See Other  临时性重定向，且总是使用 GET 请求新的 URI。
    304  Not Modified  自从上次请求后，请求的网页未修改过。
    400 Bad Request  服务器无法理解请求的格式，客户端不应当尝试再次使用相同的内容发起请求。
    401 Unauthorized  请求未授权。
    **403 Forbidden  禁止访问。
    **404 Not Found  找不到如何与 URI 相匹配的资源。
    500 Internal Server Error  最常见的服务器端错误。
    **503 Service Unavailable 服务器端暂时无法处理请求（可能是过载或维护）。
    **502 
    
    why有302 
###Etag

>当浏览器向服务器发送一个请求时，浏览器首先会进行缓存过期判断。浏览器根据缓存过期时间判断缓存文件是否过期。
情景一：若没有过期，则不向服务器发送请求，直接使用缓存中的结果，此时我们在浏览器控制台中可以看到 200 OK(from cache) ，此时的情况就是完全使用缓存，浏览器和服务器没有任何交互的。
情景二：若已过期，则向服务器发送请求，此时请求中会带上①中设置的**文件修改时间**，和**Etag**

然后，进行资源更新判断。服务器根据浏览器传过来的文件修改时间，判断自浏览器上一次请求之后，文件是不是没有被修改过；根据Etag，判断文件内容自上一次请求之后，有没有发生变化
情形一：若两种判断的结论都是文件没有被修改过，则服务器就不给浏览器发index.html的内容了，直接告诉它，文件没有被修改过，你用你那边的缓存吧—— 304 Not Modified，此时浏览器就会从本地缓存中获取index.html的内容。此时的情况叫协议缓存，浏览器和服务器之间有一次请求交互。
情形二：若修改时间和文件内容判断有任意一个没有通过，则服务器会受理此次请求，之后的操作同①  
① 只有get请求会被缓存，post请求不会  

###OSI七层模型
应用层：包括HTTP,FTP,SMTP等协议，对应应用程序的通信服务
表示层：包括加密,ASCII等，定义数据格式及加密
会话层：包括RPC,SQL等，定义如何开始、控制、结束一个对话
传输层：包括TCP,UDP等，选择协议，对数据流进行复用，对数据包排序
网络层：包括IP协议等，端对端的包传输
链路层：包括ATM,FDDI等，主要定义如何在一个链路上进行数据传输
物理层：包括RJ45,802.3等标准，主要是有关传输介质的标准
###tcp/ip五层
物理层
数据链路层
网络层
传输层
应用层
###四层
网络接口层
网际层
传输层
应用层

###TCP三次握手
[http://www.nowcoder.com/discuss/1778](http://www.nowcoder.com/discuss/1778)
##安全
###xss 跨站脚本攻击 输入检查 转义
###CSRF 跨站请求伪造
##编程
###页面内拖拽一个div

###页面内实现右键菜单组件功能

###页面内有一个input输入框，实现在数组arr查询命中词以及autocomplete效果

###实现一个弹出层
###数组去重
```js
function uniqArray(array){
    var newarr =[];
    for(var i=0 ,len=array.length;i<len;i++){
        if(newarr.indexOf(array[i]) ==-1){
            newarr.push(array[i]);        
        }    
    }
    return newarr;
}
var a = [1, 3, 5, 7, 5, 3];
var b = uniqArray(a);
console.log(b); // [1, 3, 5, 7]
```
##其他
### 搜索的智能提示如何实现 
###从输入 URL 到页面加载完成的过程中都发生了什么事情？***
[从输入 URL 到页面加载完成的过程中都发生了什么事情？](http://fex.baidu.com/blog/2014/05/what-happen/)
[2](http://www.guokr.com/question/554991/)

###ES6
promise let map set
>新增模板字符串（为JavaScript提供了简单的字符串插值功能）、箭头函数（操作符左边为输入的参数，而右边则是进行的操作以及返回的值Inputs=>outputs。）、for-of（用来遍历数据—例如数组中的值。）arguments对象可被不定参数和默认参数完美代替。ES6将promise对象纳入规范，提供了原生的Promise对象。增加了let和const命令，用来声明变量。增加了块级作用域。let命令实际上就增加了块级作用域。ES6规定，var命令和function命令声明的全局变量，属于全局对象的属性；let命令、const命令、class命令声明的全局变量，不属于全局对象的属性。。还有就是引入module模块的概念
###前端模块化
代码复用，降低耦合，提高维护性
CommonJS

AMD(Asynchronous Module Definitio异步模块定义)  是 RequireJS 在推广过程中对模块定义的规范化产出。
CMD (Common Module Definition通用模块定义)是 SeaJS 在推广过程中对模块定义的规范化产出。

AMD是提前执行，CMD 是延迟执行。

AMD推荐的风格通过返回一个对象做为模块对象，CommonJS的风格通过对module.exports或exports的属性赋值来达到暴露模块对象的目的。

CMD模块方式
    define(function(require, exports, module) {

      // 模块代码

    });
[http://www.cnblogs.com/dolphinX/p/4381855.html](http://www.cnblogs.com/dolphinX/p/4381855.html)