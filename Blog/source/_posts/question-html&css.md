# HTML

## html5拖拽

[MDN拖放操作](https://developer.mozilla.org/zh-CN/docs/DragDrop/Drag_and_Drop)
## 行内 块级元素
（1）行内元素有：a span img input select strong（强调的语气）
（2）块级元素有：div p  ul ol li dl dt dd h1 h2 h3 h4… 

盒模型区别?
块级元素可以设置 width、height 属性，而内联元素设置无效。
他们都可以设置margin padding
块级元素的 width 默认为 100%，而内联元素则是根据其自身的内容或子元素来决定其宽度。

（3）常见的空元素：br  hr  input link  meta


# CSS
## position
position的值， relative和absolute分别是相对于谁进行定位的？
absolute :生成绝对定位的元素， 相对于最近一级的 定位不是 static 的父元素来进行定位。
fixed （老IE不支持）生成绝对定位的元素，通常相对于浏览器窗口或 frame 进行定位。
relative 生成相对定位的元素，相对于其在普通流中的位置进行定位。
static 默认值。没有定位，元素出现在正常的流中
sticky 生成粘性定位的元素，容器的位置根据正常文档流计算得出

## position:absolute和float属性的异同
共同点：对内联元素设置float和absolute属性，可以让元素脱离文档流，并且可以设置其宽高。
不同点：float仍会占据位置，absolute会覆盖文档流中的其他元素。


## img src
<img src=""> 什么都不显示
<img src="#"> 有个裂了的图片
## 浮动 清除浮动

1.使用空标签清除浮动。
   这种方法是在所有浮动标签后面添加一个空标签 定义css clear:both. 弊端就是增加了无意义标签。
```html
<div style="clear:both;"></div>
```
2.父标签使用overflow
   给包含浮动元素的父标签添加css属性 overflow:auto; zoom:1; zoom:1用于兼容IE6。

3.使用after伪元素清除浮动。
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
## 居中
### 水平居中
1.行内元素（文字、链接）
父元素加上 text-align: center;
2.定宽块级元素 
元素自身设置margin:0 auto;

3.flex 
	display: flex;
	justify-content: center;
### 垂直居中
1.行内元素 
 line-height等于父元素高度

2.定高块级元素
```css
.parent { position: relative;
}
.child { 
position: absolute; 
top: 50%; 
height: 100px; 
margin-top: -50px; /* 元素高度的一半 */}

```
3.不定高块级元素 
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

### 水平垂直垂直居中

1、固定宽高的块级元素
```css
.parent {
  position: relative;
}

.child {
  width: 300px;
  height: 100px;
  position: absolute;
  top: 50%;
  left: 50%;
  margin: -50px 0 0 -150px; /* margin-top为-高度一半 margin-left为-元素高度一半*/
}
```
2、宽高未知块级元素
```css
.parent {
  position: relative;
}
.child {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%); /*元素向左向上移动宽度高度的50%*/
}
```
元素进行移动（translate）、旋转（rotate）、缩放（scale）、倾斜（skew）。 
[transform ](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform)
3、flex布局
```css
.parent {
 display: flex; 
 justify-content: center;
 align-items: center;
}
```

## display:none和visibility:hidden的区别？
display:none  隐藏对应的元素，在文档布局中不再给它分配空间，它各边的元素会合拢，就当他从来不存在。
visibility: fde  隐藏对应的元素，但是在文档布局中仍保留原来的空间。


## 介绍一下box-sizing属性？
box-sizing属性主要用来控制元素的盒模型的解析模式。默认值是content-box。

content-box：让元素维持W3C的标准盒模型。元素的宽度/高度由border + padding + content的宽度/高度决定，设置width/height属性指的是content部分的宽/高

border-box：让元素维持IE传统盒模型（IE6以下版本和IE6~7的怪异模式）。设置width/height属性指的是border + padding + content

标准浏览器下，按照W3C规范对盒模型解析，一旦修改了元素的边框或内距，就会影响元素的盒子尺寸，就不得不重新计算元素的盒子尺寸，从而影响整个页面的布局。


## 选择符优先级
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

## 伪类伪元素
伪类(:)：为已有的元素添加样式，但是它只有处于dom树无法描述的状态下才能为元素添加样式，所以将其称为伪类。比如：hover
伪元素(::):用于创建一些不在文档树中的元素，并为其添加样式。
```code
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
[总结伪类伪元素](http://www.alloyteam.com/2016/05/summary-of-pseudo-classes-and-pseudo-elements/)

## 超链接伪类顺序
a:link > a:visited > a:hover > a:active
L V H T love%hate
[CSS中超链接伪类link,visited,hover,active的顺序分析](http://www.dengzhr.com/frontend/css/344)

## CSS中link 和@import的区别是？
(1) link属于HTML标签，而@import是CSS提供的;

(2) 页面被加载的时，link会同时被加载，而@import被引用的CSS会等到引用它的CSS文件被加载完再加载;

(3) import只在IE5以上才能识别，而link是HTML标签，无兼容问题;

(4) link方式的样式的权重 高于@import的权重.


