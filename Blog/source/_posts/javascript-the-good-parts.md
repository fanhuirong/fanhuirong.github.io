---
date: 2016-07-27 16:43
status: public
title: js语言精粹笔记
---

读书笔记
js语言精粹
###第三章 对象
检索 : [ ] 、.
引用：对象通过引用传递，永远不会被复制
反射：hasOwnProperty(只检查自身)、 typeof(检查原型链)
枚举： for in 遍历对象的**属性名**
```js
var a ={ name:111,age:22,id:9872317}
for(item in a){
if(typeof a[item]!=='function'){
  console.log(item+":"+a[item]+"\n")
}
}
```
![](~/17-01-19.jpg)
--->属性名以特定顺序出现 使用数组

![](~/20-21-39.jpg)
删除：删除对象属性 delete 不会触及原型链
减少全局变量污染：
1. 使用唯一的全局变量
2. 闭包

###第四章 函数
函数调用
调用一个函数---》暂停当前函数---》传递控制权和参数给新函数
除了声明时定义的**形参**，函数还接受两个附加参数 **this**、** arguments**
this的值---》取决于调用模式
四种调用模式 
1.方法调用
函数保存为对象的属性---》叫方法
方法被调用，this绑定在 该对象 
2.函数调用   that
this绑定到全局对象   一个错误
本应让this绑定到外部函数的this变量上
解决：that
定义变量that 并给that赋值this，内部函数通过that访问本想访问的外部函数的this
```js
var myOb ={
    value:0
}
myOb.double =function(){
    var that =this;
    var helper = function(){
     that.value = that.value+1
    }
helper();//函数形式调用
}
myOb.double(); //方法形式调用
console.log(myOb.value);
```

![](~/21-35-14.jpg)

3.构造器调用 
函数前带new---》创建一个连接到函数prototype的对象，this绑定到这个新对象
```js
var  Q =function(str){
    this.status = str
}
Q.prototype.get_status = function(){
return this.status
}
var myQ= new Q('aaaa')
console.log(myQ.get_status())
```
//Q 构造器函数  一般构造器函数大写开头
![](~/21-44-03.jpg)
4.apply调用

[apply](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Function/apply)
ps: arguments 实际参数，parameters形式参数
arguments 类似数组，只拥有length属性，没有任何数组的方法

作用域：内部函数可以访问定义他们的外部函数的参数和变量（除了this和arguments）
内部函数的生命周期比外部函数长
闭包
回调 收到响应再被调用
模块化
一个定义了死又变了和函数的函数,利用闭包创建可以访问私有变量和函数的特权函数

级联：单独一条语句中依次调用同一对象的不同方法
因为这些方法返回this，因此能够启用级联
`getElement('Ele').move(350,150).width(100)`
柯里化：局部套用
通过创建保存在原始函数和要被套用的参数的闭包来工作
>curry的概念：只传递给函数部分参数，调用它之后返回一个函数去处理剩下的参数；


![](~/20-49-47.jpg)
[不可或缺的柯里化](https://zhuanlan.zhihu.com/p/20787973)
###第五章 继承
js不能让对象从其他对象继承，插入了一个多余的间接层--->构造器函数
一个函数对象被创建-->Function构造器产生的函数对象会运行 this.prototype = {construtor:this};
构造器函数的危害：如果调用构造器函数的时候忘记加new前缀，this会被绑定到window上
###第六章 数组
js的数组是类数组的对象，与对象相比多一个可以用整数作为属性名的特性。
js允许数组包含混合类型的值。
js的数组的length无上界，所以不能用>=当前length，因为此时length会被增大。
length属性的值=数组最大整数属性名+1，不一定等于属性个数
```js
var arr =[]
arr.length //0
arr[1000]=true
arr.length //10001
//arr只包含一个属性
```
length设小会导致下吧大于等于新length的属性被删除
**length设大不会给数组分配更多空间**
删除：delete splice 
delete会给数组留下空洞
![](~/11-35-12.jpg)
枚举： for
使用for in无法保证属性的顺序
判断对象是否为数组：
typeof 对于数组和对象返回都是object
```js
var num=[111,222,333,444,555]
var is_arr = function(value){
return Object.prototype.toString.apply(value)==='[object Array]'
}
console.log(is_arr(num))
```
数组的方法存在Array.prototype中，通过给Array.prototype扩充一个函数，每个数组都继承了这个方法

###第八章 方法