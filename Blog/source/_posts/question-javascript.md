##  iife 闭包
```js
//1. 
for (var i = 0; i < 5; i++) {
  console.log(i);
}
// 0 1 2 3 4

//2. setTimeout
for (var i = 0; i < 5; i++) {
  setTimeout(function() {
    console.log(i);
  }, 1000 * i);
}
// 5 5 5 5 5 
//setTimeout 延迟执行，那么执行到 console.log 的时候，其实 i 已经变成 5 

//3. 闭包 输出0 1 2 3 4
for (var i = 0; i < 5; i++) {
  (function(i) {
    setTimeout(function() {
      console.log(i);
    }, i * 1000);
  })(i);
}

//4. 去掉闭包的i
for (var i = 0; i < 5; i++) {
  (function() {
    setTimeout(function() {
      console.log(i);
    }, i * 1000);
  })(i);
}
//内部没有对 i 保持引用，输出 5遍5

//5. 给setTimeout传入立即执行函数
for (var i = 0; i < 5; i++) {
  setTimeout((function(i) {
    console.log(i);
  })(i), i * 1000);
}

```
继承

[Excuse me？这个前端面试在搞事！](https://zhuanlan.zhihu.com/p/25407758)

## 作用域

词法分析：
分析参数,分析变量声明,分析函数声明

eg1:
```js
	function t(userName) {
        console.log(userName);//这里输出什么?
        function userName() {
            console.log('tom');
        }
    }
    t('toby');
//打印结果
	function userName() {
        console.log('tom');
    }
```
执行t('toby')的时候,会开始两个阶段,一个是分析阶段,分析完就到执行阶段

>函数运行的瞬间,会生成一个Active Object对象(以下简称AO对象),一个函数作用域内能找到的所有变量,都在AO上,此时用代码表示为: t.AO = {}

分析参数: 接收参数,以参数名为属性,参数值为属性值,因此分析结果用代码表示为: t.AO = {userName : toby}

分析var声明: t函数内没有var声明,略过

分析函数声明: 这个函数声明有个特点,AO上如果有与函数名同名的属性,则会被此函数覆盖,因为函数在JS领域,也是变量的一种类型,因此用代码表示为: t.AO = { userName : function userName() {console.log('tom');}}

```js
//这个叫函数声明
function userName() {
    console.log('tom');
}

//这个叫函数表达式
var userName = function () {
    console.log('tom');
}
```

eg2:
```js
  function t(userName) {
        console.log(userName);//这里输出什么?

        var userName = function () {
            console.log('tom');
        }
    }
    t('toby'); //toby

    创建AO对象,t.AO = {}

分析参数: t.AO = {userName : toby}

分析var声明: 在AO上,形成一个属性,以var的变量名为属性名,值为undefined,(因为是先分析,后执行,这只是词法分析阶段,并不是执行阶段,分析阶段值都是undefined,如果执行阶段有赋值操作,那值会按照正常赋值改变),也就是说代码应该表示为:t.AO = {userName : undefined},但是还有另外一个原则,那就是如果AO有已经有同名属性,则不影响(也就是什么事都不做),由于分析参数时,AO上已经有userName这个属性了,所以按照这个原则,此时什么事都不做,也就是说,此时按照分析参数时的结果t.AO = {userName : toby}

分析函数声明: 此时没有函数声明
```
eg3:
```js
    t();
    t2();

    function t() {
        console.log('toby');//这里会输出什么? toby
    }

    var t2 = function () {
        console.log('hello toby');//这里会输出什么? 报错
    };
```
eg4：
```js
 function t(userName) {
        console.log(userName);//这里输出什么? function的内容
        function userName() {
            console.log(userName);//这里输出什么? function的内容
        }
        userName();
    }
    t('toby');
```
作用域链的作用是保证执行环境里有权访问的变量和函数是有序的，作用域链的变量只能向上访问，变量访问到window对象即被终止，作用域链向下访问变量是不被允许的。

[套公式让你不再害怕JavaScript中的作用域](http://www.jianshu.com/p/43bf4f2e0d57)
## 原型链
>
typeof obj: 返回obj对象的类型的全小写的字符串,只针对基本类型有效,如果作用在引用类型上,都返回object字符串,无法判断变量的真实类型

obj instanceof Type : 判断obj是否属于Type类型,返回boolean,只能作用在引用类型上,如果作用在基本类型上,都是返回false的结果.

__proto__（隐式原型）
prototype（显式原型）
隐式原型指向创建这个对象的函数(constructor)的prototype
__proto__: 这个属性是JS对象上的隐藏属性,这个属性指向的是该对象对应类型的prototype对象
普通对象没有prototype,但有_proto_属性。

>Js中一切皆为对象（Object），但是Js中并没有类（class）；Js是基于原型（prototype-based）来实现的面向对象（OOP）的编程范式的，但并不是所有的对象都拥有prototype这一属性：prototype是每个function定义时自带的属性


JS中对象被创建的方式 （1）对象字面量的方式 （2）new 的方式 （3）ES5中的Object.create()

[套公式让你不再害怕JavaScript中的原型链](http://www.jianshu.com/p/a81692ad5b5d)

## 构造函数
>在构造函数内部 - 也就是被调用的函数内 - this 指向新创建的对象 Object。 这个新创建的对象的 prototype 被指向到构造函数的 prototype。
```js
function Foo() {
    this.bla = 1;
}

Foo.prototype.test = function() {
    console.log(this.bla);
};

var test = new Foo();
console.log(test.prototype) //Object  
```

## == && === 的区别
== 会进行强制类型转换，可能带来问题
=== 也就是所谓的严格比较，关键的区别在于=== 会同时比较类型与值，而不是仅比较值。

// Example of comparators
0 == false; // true
0 === false; // false

2 == '2'; // true
2 === '2'; // false

>比较对象
虽然 == 和 === 操作符都是等于操作符，但是当其中有一个操作数为对象时，行为就不同了。
{} === {};                   // false
new String('foo') === 'foo'; // false
new Number(10) === 10;       // false
var foo = {};
foo === foo;                 // true
这里等于操作符比较的不是值是否相等，而是是否属于同一个身份；也就是说，只有对象的同一个实例才被认为是相等的。 这有点像 Python 中的 is 和 C 中的指针比较。


[js秘密花园--等于操作符](https://bonsaiden.github.io/JavaScript-Garden/zh/#types.equality)
## this
setTimeout
```JS
setTimeout(function(){console.log(this)},1000) //WINDOW
```


## 数组去重
```js
//Set ES6的新数据结构 成员的值都是唯一的 类数组
//Array.from() 将一个类数组对象或可遍历对象转换成真正的数组
//只对基本数据类型效果
function unique(arr){
	return Array.from(new Set(arr))
}
```
对象数组去重
上面的方案对对象数组等引用数据类型无效
//引用数据类型
[数组去重](http://www.jstips.co/zh_cn/javascript/deduplicate-an-array/)
[Immutable](https://facebook.github.io/immutable-js/)

## 判断对象是否没属性

## 判断对象是否在数组中

## 判断数组 isArray



## preventDefault stopPropagation
　event.preventDefault() 阻止事件的默认行为（比如点击链接，会自动打开新页面）
　event.stopPropagation()阻止目标元素的事件冒泡到父级元素。

## import require
require CommonJS规范的一部分
import ES6模块规范
>过去将每个文件内的代码都包在一个自执行函数中：(function() { ... })();。这种方法创建了一个本地作用域，于是最初的模块化的概念产生了。

## 立即执行函数 IIFE
IIFE (立即调用函数表达式) 是一个 JavaScript 函数 ，它会在定义时立即执行。
```js
(function(){…})()
(function (){…}())
```
## 闭包

## this
this的指向在函数定义的时候是确定不了的，只有函数执行的时候才能确定this到底指向谁。

1.函数调用 指向window
```js
function a(){
    var user = "追梦";
    console.log(this.user); //undefined
    console.log(this); //Window
}
a();
```
2.对象的方法 this永远指向的是最后调用它的对象
```js
var o = {
    user:"追梦子",
    fn:function(){
        console.log(this.user);  //追梦子
    }
}
o.fn();
```
函数中包含多个对象，尽管这个函数是被最外层的对象所调用，this指向的也只是它上一级的对象
```js
var o = {
    a:10,
    b:{
         a:12,
        fn:function(){
            console.log(this.a); //12
        }
    }
}
o.b.fn();
//------------------------------------
var o = {
    a:10,
    b:{
        // a:12,
        fn:function(){
            console.log(this.a); //undefined
        }
    }
}
o.b.fn();

//------------------------------------------
var o = {
    a:10,
    b:{
        a:12,
        fn:function(){
            console.log(this.a); //undefined
            console.log(this); //window
        }
    }
}
var j = o.b.fn;
j();
```

3.构造函数
指向新生成的对象
4.apply call
this指向第一个参数,后面为传入的参数
```js
var a = {a:1111}
function b(){
	console.log(this)
}
b.apply(a) //this指向a
```

[call apply bind区别](http://www.jianshu.com/p/56a9c2d11adc)
```js
xw.say.call(xh,"实验小学","六年级");
xw.say.apply(xh,["实验小学","六年级"]);
xw.say.bind(xh,"实验小学","六年级")();
//or
xw.say.bind(xh)("实验小学","六年级");
```
>call和apply都是对函数的直接调用，而bind方法返回的仍然是一个函数，因此后面还需要()来进行调用才可以。

回调函数的this指向 window或者

## jquery $的原理

## querySelector 

## let var区别？
尽管 JavaScript 支持一对花括号创建的代码段，但是并不支持块级作用域； 而仅仅支持 函数作用域。
ES6新增的let  let声明的变量拥有块级作用域。 
let声明的全局变量不是全局对象的属性。。
形如for (let x...)的循环在每次迭代时都为x创建新的绑定。
[var、let、const 区别？](http://www.jianshu.com/p/4e9cd99ecbf5)
[JavaScript ES6 var VS let 区别](http://www.jianshu.com/p/767bceb95bef)

## setTimeout 原理 
js单线程 
setTimeout告诉js引擎，在指定时间点将回调函数插入 `待处理任务队列` 末尾
如果队列为空，则是在指定时间执行，否则要等前面的代码全执行完再执行
因此 js的setTimeout并不能保证一定在你需要的时间点执行

setTimeout的this会指向window

[从setTimeout说事件循环模型](http://www.alloyteam.com/2015/10/turning-to-javascript-series-from-settimeout-said-the-event-loop-model/)

## 函数节流throttle 函数去抖debounce

函数防抖debounce就是让某个函数在上一次执行后，满足等待某个时间内不再触发此函数后再执行，而在这个等待时间内再次触发此函数，等待时间会重新计算。
固定的时间间隔内, 如果事件再次被触发, 则重置时间, 直到大于等于时间间隔才执行方法.


[浅谈js函数节流](http://www.alloyteam.com/2012/11/javascript-throttle/)

## HTTPcode
403 why
304 过程

    100  Continue  继续，一般在发送post请求时，已发送了http header之后服务端将返回此信息，表示确认，之后发送具体参数信息

    200  OK   正常返回信息

    201  Created  请求成功并且服务器创建了新的资源

    202  Accepted  服务器已接受请求，但尚未处理

    301  Moved Permanently  请求的网页已永久移动到新位置。

    302 Found  临时性重定向。

    303 See Other  临时性重定向，且总是使用 GET 请求新的 URI。

    304  Not Modified  自从上次请求后，请求的网页未修改过。


    400 Bad Request  服务器无法理解请求的格式，客户端不应当尝试再次使用相同的内容发起请求。

    401 Unauthorized  请求未授权。

    403 Forbidden  禁止访问。

    404 Not Found  找不到如何与 URI 相匹配的资源。

    500 Internal Server Error  最常见的服务器端错误。

    503 Service Unavailable 服务器端暂时无法处理请求（可能是过载或维护）

## Network 能看到的问题
http 时间 和页面时间 以哪个为准 ？  
## 如何看DOM数量
一般别超过1000？
控制台

js拿到页面节点数量  
```js
document.getElementsByTagName('*').length //拿不到文本节点
document.querySelectorAll('*').length //拿不到文本节点
$.('*').length
```

## 性能方面的问题 

页面优化 如何看页面渲染时间
[Chrome渲染分析之Timeline工具的使用](http://www.ghugo.com/chrome-timeline/)


## 五个js 代码压缩合并带来的问题？
改一处代码就要打包一次？？？
类库
公共脚本
业务

## 判断对象为空

## 排序
[聊聊前端排序的那些事](http://efe.baidu.com/blog/talk-about-sort-in-front-end/)

## 快速排序的js实现
快速排序使用 分治法(Divide and conquer)策略(即分而治之，各个击破)把一个序列(list)分为两个子序列(sub-lists)。其基本步骤如下:

从数列中挑出一个元素，称为 基准(pivot)。
重新排序数列，所有小于基准的元素，都移到基准的左边；所有大于基准的元素都移到基准的右边。这个分区结束之后，该基准处于数列的中间位置，称为 分区(partition)操作。
对基准左边和右边的两个子集，进行递归操作，即不断重复第一步和第二步。直到所有子集只剩下一个元素为止。

```js
function quickSort(arr){
	if(arr.length<=1){
		return arr
	}
	var pivotIndex =Math.floor(arr.length/2),
		pivot = arr.splice(pivotIndex,1)[0],
		left = [],
		right = []
	for(let i=0;i<arr.length;i++){
		if(arr[i]<pivot){
			left.push(arr[i])
		}else{
			right.push(arr[i])
		}
	}
	return quickSort(left).concat([pivot],quickSort(right))
}

var numArr = [2,3,4,1,5]
console.log(quickSort(numArr))




function quickSort(arr,key){
	if(arr.length<=1){
		return arr
	}
	var pivotIndex =Math.floor(arr.length/2),
		pivot = arr.splice(pivotIndex,1)[0],
		left = [],
		right = []
	for(let i=0;i<arr.length;i++){
		if(arr[i][key]<pivot[key]){
			left.push(arr[i])
		}else{
			right.push(arr[i])
		}
	}
	return quickSort(left,key).concat([pivot],quickSort(right,key))
}

var arr = [{ "eventId": 2498858, "timestamp": 1463569008327, "user": "auto" }, { "eventId": 2498859, "timestamp": 1463569008326, "user": "auto" }, { "eventId": 2498850, "timestamp": 1463569008328, "user": "auto" }, { "eventId": 2498851, "timestamp": 1463569008321, "user": "auto" }];

console.log(quickSort(arr,'timestamp'))

```

[JavaScript算法详解——快速排序](https://www.w3cplus.com/javascript/quicksort-in-javascript.html)
## 从输入url到页面加载发生了什么

>
浏览器向本地DNS服务器发送一个查询报文，本地DNS服务器转发查询报文到 根服务器--> 根服务器返回com的顶级域名服务器的ip地址 -->本地服务器再向com的服务器发送查询请求， com的的服务器返回baidu.com的域名的服务器的ip地址 --> 本地域名服务器将ip地址返回给浏览器-->
通过ip找到服务器 浏览器打开TCP连接到服务器--》浏览器发送HTTP请求-->应用程序响应，返回数据 -->浏览器得到数据  -->TCP连接释放 -->浏览器渲染显示

[在浏览器地址栏输入一个URL后回车，背后会进行哪些技术步骤？](https://www.zhihu.com/question/34873227)
[前端经典面试题: 从输入 URL 到页面加载发生了什么？](https://juejin.im/entry/57f10284da2f60004f5f2e5e)

## 正则表达式
```js
var str = 'o3052202501;uin=o3052202501;'
var regex =  /uin=[\w+]([*0-9]+)/
 
console.log(regex.exec(str))

```

## tree

```js
                    var locationList = [
            { id: 0, name: "中国" },
            { id: 1, pid: 0, name: "广东省" },
            { id: 2, pid: 1, name: "深圳市" },
            { id: 3, pid: 1, name: "广州市" }
            ]

            function locationNode(node){
                return {
                    id: node.id,
                    pid:node.pid,
                    name:node.name,
                    subLocations:[]
                }
            }
            function findRoot (node){
                return !node.pid
            }

            function buildLocationTree(list){
                var root = list.find(findRoot)
                    root.subLocations=[]
                var temp = list
                var tree= {}
                tree[root.id] = root
                var length = temp.length
                for(let i=0; i<length ; i++){
                    let item = temp[i]
                    if(item.pid>=0){
                        let node = locationNode(item)
                        tree[item.pid].subLocations.push(node)
                        tree[item.id] = node
                    }
                }
                return tree[root.id]
            }
            var showTree = buildLocationTree(locationList)
            console.log(JSON.stringify(showTree))
        }
```



## 发布-订阅者模式 

[理解 javascript 观察者模式 (订阅者与发布者)](https://juejin.im/entry/580b5553570c350068e6c2d6)

单向链表 是啥数据结构
二叉树
TCP
原型链
call apply
vue 机制 
前端模块化

webserver node 控制台 技术输出 

session 服务器和客户端 ？？？
缓存相关的协议头
网络攻击
装饰者模式

同源 相同的协议 域名 端口号
禁止跨域：防止不同域名的网页之间共享 cookie
跨域的方式 
jsonp 动态插入script
在js中，我们直接用XMLHttpRequest请求不同域上的数据时，是不可以的。但是，在页面上引入不同域上的js脚本文件却是可以的，jsonp正是利用这个特性来实现的。
jsonp 的本质其实是请求一段 js 代码，是对静态文件资源的请求，所以并不遵循同源策略。但是因为是对静态文件资源的请求，所以只能支持 GET请求，对于其他方法没有办法支持。
document.domain 子和父属于同一个基础域名 设置成同一个基础域名
window.name 在一个窗口(window)的生命周期内,窗口载入的所有的页面都是共享一个window.name的
window.postMessage
CORS
[js跨域](http://www.cnblogs.com/2050/p/3191744.html)
get post区别

拥塞机制


三次握手
SYN  发送端发出同步信号给接收端 客户端to服务器
SYN/ACK 接收端收到SYN报文并给发送端发出SYN/ACK报文
ACK 发送端发出确认报文告诉接收端我
若在握手过程中某个阶段莫名中断，TCP协议会再次以相同的顺序发送相同的数据包。

客户端 —> 服务器，客户端请求连接
服务器 —> 客户端，服务器确认连接信息
客户端 —> 服务器，客户端确认连接信息，开始连接

四次挥手
FIN 主to被 告诉对方我要关闭数据传送
ACK 被to主 发送确认信号 表示自己收到
FIN 被to主 告诉对方我也不会再发送数据了
ACK 主to被 发送确认信号 表示自己收到
A —> B，A请求断开连接
B —> A，B确认请求并准备断开连接
B —> A，B关闭连接并通知A
A —> B，A确认关闭


TCP 面向连接 可靠
UDP 面向无连接 不可靠

xss 
CSRF 验证码 
XSS是获取信息，不需要提前知道其他用户页面的代码和数据包。CSRF是代替用户完成指定的动作，需要知道其他用户页面的代码和数据包。

WebSocket是Web应用程序的传输协议，它提供了双向的，按序到达的数据流。
他是一个HTML5协议，WebSocket的连接是持久的，他通过在客户端和服务器之间保持双工连接，服务器的更新可以被及时推送给客户端，而不需要客户端以一定时间间隔去轮询。

HTTP协议通常承载于TCP协议之上，在HTTP和TCP之间添加一个安全协议层（SSL或TSL），这个时候，就成了我们常说的HTTPS。

性能优化
代码层面：避免使用css表达式，避免使用高级选择器，通配选择器。
缓存利用：缓存Ajax，使用CDN，使用外部js和css文件以便缓存，添加Expires头，服务端配置Etag，减少DNS查找等
请求数量：合并样式和脚本，使用css图片精灵，初始首屏之外的图片资源按需加载，静态资源延迟加载。
请求带宽：压缩文件

##　缓存
ETag
向服务器发送请求 浏览器进行缓存过期判断
1. expires与当前时间比较 没过期 200 ok(from cache) 命中缓存
Cache-Control : max-age=3600代表缓存资源有效时间为1小时，即从第一次获取该资源起一小时内的请求都被认为可命中强缓存。
这两个都是强缓存 
因为 Expires 是 HTTP 1.0 定义的字段，而 Cache-Control 是 HTTP 1.1 的字段，万一客户端只支持 HTTP 1.0，那么 Cache-Control 有可能就会不工作，所以一般为了兼容会都写上。 cache-control会覆盖expires
2. 过期 向服务器发请求（强缓存未命中） 带上ETag 文件修改时间Last-Modified
2.1 未修改 协商缓存命中304 not modified 服务器与浏览器有一次交互
2.2 

## http请求
### request headers
cookie
host
### response headers
cache-control
expires
last-modified
[Last-Modified , If-Modified-Since]和[ETag , If-None-Match]一般同时启用，这是为了处理Last-Modified不可靠的情况。
[浏览器缓存](https://segmentfault.com/a/1190000006672573)

## js计算数组内元素出现的次数
```js
// reduce() 方法对累加器和数组的每个值 (从左到右)应用一个函数，以将其减少为单个值。
let origin = ['mark','lily','mike'],
	 data ={}
//accumulator上一次调用回调返回的值，或者是提供的初始值（initialValue）
//currentValue 数组中正在处理的元素
//initialValue可选项，其值用于第一次调用 callback 的第一个参数。

data = origin.reduce((p, k) => (p[k]++ || (p[k] = 1), p), {});
//逗号表达式，先求逗号前面的 然后返回逗号后面的

data = origin.reduce(function(p,k){
  if(p[k]){
    p[k]++
  }else{
    p[k]=1
  }
  return p
},{})

//时间复杂度n
data = origin.reduce(function(p,k){
  p[k] = p[k] ?  p[k]+1 :1;
  return p
},{}) 
//先传入{} 最后返回这个字典即可
```
## 找出出现次数最多的元素
```js
//找最大的，将上面的{}遍历，即可拿到 n
var maxString ="",maxCount=0
for( key in data){
	if(data[key]>maxCount){
		maxString = key
		maxCount = data[key]
	}
}
```
for...of与for...in的区别

for...in循环会遍历一个object所有的可枚举属性。

for...of语法是为各种collection对象专门定制的，并不适用于所有的object.它会以这种方式迭代出任何拥有[Symbol.iterator] 属性的collection对象的每个元素。

下面的例子演示了for...of 循环和 for...in 循环的区别。for...in 遍历（当前对象及其原型上的）每一个属性名称,而 for...of遍历（当前对象上的）每一个属性值:

Object.prototype.objCustom = function () {}; 
Array.prototype.arrCustom = function () {};

let iterable = [3, 5, 7];
iterable.foo = "hello";

for (let i in iterable) {
  console.log(i); // logs 0, 1, 2, "foo", "arrCustom", "objCustom"
}

for (let i of iterable) {
  console.log(i); // logs 3, 5, 7
}

