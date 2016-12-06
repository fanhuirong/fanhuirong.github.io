---
date: 2016-03-29 21:24
status: public
title: 'this call apply'
---

##this
###全局 window
在全局运行上下文中（在任何函数体外部），this 指代全局对象，无论是否在严格模式下。
###函数调用 window
非严格模式 window
```js
function f1 () {
    return this;
}
console.log(f1() === window);//true, global object
```
严格模式 undefined
```js
function f2 () {
    "use strict";//使用严格模式
    return this;
}
console.log(f1() === undefined);//true
```
###对象方法中
####对象里有方法 指向调用该方法的对象
当以对象里的方法的方式调用函数时，它们的 this 是调用该函数的对象.
创建字面量对象o， o.f() 被调用时，函数内的this将绑定到o对象。
```js
var o = {
  prop: 37,
  f: function() {
    return this.prop;
  }
};

console.log(o.f()); // logs 37
```
#####对象里无方法
首先定义函数然后再将其附属到o.f。这样做this的行为也一致：
```js
var o = {
    prop: 37
};

function independent() {
    return this.prop;
}
o.f = independent;
console.log(o.f()); // 37
```
this的值只与函数 f 作为 o 的成员被调用有关系。
####原型链中的this
如果该方法存在于一个对象的原型链上，那么this指向的是调用这个方法的对象，表现得好像是这个方法就存在于这个对象上一样。
```js
var o = {
    f: function() {
        return this.a + this.b;
    }
};
var p = Object.create(o);
p.a = 1;
p.b = 2;
console.log(p.f()); //3
```
通过 var p = Object.create(o) 创建对象p，p 是基于原型 o 创建出的对象，p本身没有属于自己的f属性，它的f属性继承自原型o。
p 的原型是 o，调用 f() 的时候是调用了 o 上的方法 f()。
查找过程从p.f开始，所以this指向p。


###构造函数中的this  指向新创建的对象。
一个函数被作为一个构造函数来使用（使用new关键字），它的this与即将被创建的新对象绑定。
```js
function C(){
  this.a = 37;
}
var o = new C();//C的this指向o
console.log(o.a); // logs 37
```
```js
function C2(){
  this.a = 37;
  return {a:38};
}
o = new C2();
console.log(o.a); // logs 38
```
在调用构造函数的过程中，手动的设置了返回对象，与this绑定的默认对象被取消。

###call、apply  this指向函数调用的第一个参数。】
apply 参数以数组的形式存在。
call 参数以列表形式存在。
当一个函数的函数体中使用了this关键字时，通过所有函数都从Function对象的原型中继承的call()方法和apply()方法调用时，它的值可以绑定到一个指定的对象上。
```js
function add(c, d){
  return this.a + this.b + c + d;
}
var o = {a:1, b:3};
add.call(o, 5, 7); // 1 + 3 + 5 + 7 = 16
add.apply(o, [10, 20]); // 1 + 3 + 10 + 20 = 34
```
如果传递的 this 值不是一个对象，JavaScript 将会尝试使用内部 ToObject 操作将其转换为对象。

```js
function bar() {
  console.log(Object.prototype.toString.call(this));
}
bar.call(7); // [object Number]  7 通过new Number(7)被转换为对象
```

###bind方法与this
this将永久地被绑定到了bind的第一个参数，无论这个函数是如何被调用的。
```js
function f(){
  return this.a;
}

var g = f.bind({a:"azerty"});
console.log(g()); // azerty

var o = {a:37, f:f, g:g};
console.log(o.f(), o.g()); // 37, azerty
```

[https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/this](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/this)
[http://bonsaiden.github.io/JavaScript-Garden/zh/#function.this](http://bonsaiden.github.io/JavaScript-Garden/zh/#function.this)