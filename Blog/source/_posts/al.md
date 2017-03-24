## 算法与数据结构


### 数据结构
#### 栈 先进后出 在一端操作 
新添加的在栈顶  

#### 队列 先进先出 头出尾进
优先队列
循环队列

#### 链表 存储有序的元素集合 元素不连续放置 
添加或删除不需要移动其他元素 需要指针
想访问链表中一个元素必须从起点开始迭代

单向链表 
双向链表 
循环链表 最后一个元素指向第一个元素

#### 集合 Set 里面的项无序且唯一

#### 字典 存储键值对
集合以[值，值]的形式存储元素，字典则是以[键，值]的形式来存储元素。

#### 树
树 每个节点有一个父节点（根节点除外），有0或多个子节点
二叉树 一个节点最多两个子节点
二叉搜索树 BST  左子节点小于父节点 右子节点大于父节点

#### 图
由一条边连接在一起的顶点称为相邻顶点。
一个顶点的度是其相邻顶点的数量。

广度优先遍历 BFS

深度优先遍历 DFS

[js数据结构](https://github.com/wengjq/Blog)

### 排序
#### 冒泡 
#### 选择 找到未排序的元素里最大或最小的放入起始位置，再从剩下的继续选最大或最小的放到之前的后面，直到所有排序完毕
#### 插入 构建有序序列，对于未排序数据，在已排序序列中从后向前扫描，找到相应位置并插入。

#### 归并
采用分治法（Divide and Conquer）
归并排序是一种稳定的排序方法。将已有序的子序列合并，得到完全有序的序列；即先使每个子序列有序，再使子序列段间有序。

#### 快排
分治  选出基准  分区（将比基准小的放到一个数组left，大的放到right）

[排序](https://github.com/wengjq/Blog/issues/10)
[十大经典排序算法js实现](https://segmentfault.com/a/1190000006921369)
### 查找
二分查找 要求数据已经排序

## vue原理
模型（Model）只是普通的 JavaScript 对象，修改它则视图（View）会自动更新。

[深入响应式原理](http://www.imooc.com/article/14466)


## 闭包
内部函数能够访问外部函数的变量
能够访问其他函数变量的函数
```js
var str1 = str2 = "web";
(function () {
var str1 = str2 = "前端";
})();
console.log(str2) //前端
console.log(str1) //web
```
只有函数作用域没有块级作用域
利用函数闭包能有效的封装局部的变量，而不污染全局作用域

闭包的优点是可以避免全局变量的污染，
缺点是闭包会常驻内存，会增大内存使用量，使用不当很容易造成内存泄露。
[从一个小题目谈闭包](https://segmentfault.com/a/1190000007077713)


## 原型链
作用：实现对象的继承

原型链：
JS在创建对象（不论是普通对象还是函数对象）的时候，都有一个叫做__proto__的内置属性，用于指向创建它的**函数对象**的原型对象prototype。
只有函数对象有prototype
```js
function F(){};
F.prototype = {
    hello : function(){}
};
var f = new F();
console.log(f.__proto__) // Object{ hello:function(){}, __proto__}
```
new的时候执行了三步
```js
var f = {}
f.__proto__ = F.prototype
F.call(f)
```
```js
//原型对象prototype上都有个预定义的constructor属性，用来引用它的函数对象。这是一种循环引用。

function F(){};
F.prototype.constructor === F; //true
```
[理解原型链](https://segmentfault.com/a/1190000005363885)


## 设计模式
### 发布-订阅者模式 (观察者模式)
```js
div.onclick  =  function click (){
   alert ( 'click' )
}
```
这里function click 订阅了 div 的click事件，当我们的鼠标点击操作，事件发布，对应的function就会执行。
这里function click 就是一个观察者。
一种一对多的依赖关系，当一个对象的状态发生改变时，所有依赖于它的对象都得到通知并被自动更新。

解耦
mvvm框架应用了这个模式
[AlloyTeam 设计模式-观察者模式](http://www.alloyteam.com/2012/10/commonly-javascript-design-pattern-observer-mode/)

### 单例模式
爆炸一个类只有一个实例，并提供一个访问他的全局访问点
### 装饰者模式
不改变对象自身的基础上，在程序运行期间给对象动态添加职责


工作的难点
一个人搭建系统 


[关于前端面试](https://segmentfault.com/a/1190000005127264)