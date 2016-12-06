---
date: 2016-04-03 14:17
title: 元素拖拽
---

##页面有有个正方形元素，实现拖拽和放下
###HTML5 drag drop
有一些默认就可以被拖动的元素。它们包括**选中的文本**，**images图片**和**链接**。
其它元素默认是不可拖拽的，为了让其能被拖拽，进行如下设置
1. 为所需拖拽的元素设置属性 draggable属性。
2. 为 dragstart 事件添加侦听--通过使用 ondragstart 属性来添加一个 dragstart 事件监听器
3. 在侦听中设置拖拽数据。
```js
<div draggable="true" ondragstart="event.dataTransfer.setData('text/plain', 'This text may be dragged')">
  This text <strong>may</strong> be dragged.
</div>
```

####拖拽的状态
dragstart --当一个元素开始被拖拽的时候触发。作用在拖拽元素上
dragend --拖拽源在拖拽操作结束将得到dragend事件对象，不管操作成功与否。作用在拖拽元素上
dragover --当拖拽中的鼠标移动经过一个元素的时候触发。作用在目标元素
dragenter --当拖拽中的鼠标第一次进入一个元素的时候触发。作用在目标元素
dragleave --当拖拽中的鼠标离开元素时触发。作用在目标事件
drop 放下 这个事件在拖拽操作结束释放时于释放元素上触发。作用在目标元素
Event.preventDefault() 方法：阻止默认的些事件方法等执行。在dragover和dragenter中一定要执行preventDefault()，否则drop事件不会被触发。
####拖拽事件
拖动某些元素时，将一次触发下列事件：
dragstart -->drag -->dragend
当元素被拖放到一个有效的放置目标上时，下列事件会依次发生：
dragenter -->dragover -->dragleave或drop
####有效拖拽目标
在拖动元素经过某些无效放置目标时，可以看到一种特殊的光标（圆环中有一条反斜线），表示不能放置。
把任何元素变成有效的放置目标，方法是**重写dragenter和dragover事件的默认行为**。
```js
//把任何元素变成有效的放置目标
    document.addEventListener("dragover", function( event ) {
        // 阻止默认动作
        event.preventDefault();
    }, false);

    document.addEventListener("dragenter", function( event ) {
    	   // 阻止默认动作
          event.preventDefault();
    }, false);

```
####dataTransfer--拖放时有效数据交互
所有的拖拽事件都有一个属性——dataTransfer，它包含着拖拽数据。
拖拽数据包含两类信息，类型(type)或者格式(format)或者数据(data)，和数据的值(data value)。
>格式即是一个表示类型的字符串（如，对于文本数据来说，格式为 "text/plain"），数据值为文本字串。当拖拽开始时，你需要通过提供一个类型以及数据来为拖拽添加数据。拖拽过程中，在dragenter 和 dragover 事件侦听中，需根据数据类型检测是否支持 drop（放下）。
使用 setData 方法在 dataTransfer 中设置数据。需要提供两个参数：**数据类型**和**数据值**。
event.dataTransfer.setData("text/plain", "Text to drag");
在这个例子中，数据值是"Text to drag"，它的格式是"text/plain"
####dropEffect与effectAllowed 搭配服用效果更好哦~
利用dataTransfer对象确定被拖动的元素以及作为放置目标的元素能够接受什么操作。为此，需要访问dataTransfer对象的两个属性：dropEffect和effectAllowed。
**DropEffect--被拖动的元素能够执行哪种放置行为。**
dropEfect属性，必须在draggenter事件处理程序中针对放置目标来设置它。
* copy: 复制到新的位置
* move: 移动到新的位置.
* link: 建立一个源位置到新位置的链接.(但拖动的元素必须是一个链接，有URL)
* none: 禁止放置（禁止任何操作）.
**EffectAllowed --允许拖放元素的哪种DropEffect**

*copy: 复制到新的位置.
*move:移动到新的位置 .
*link:建立一个源位置到新位置的链接.
*copyLink: 允许复制或者链接.
*copyMove: 允许复制或者移动.
*linkMove: 允许链接或者移动.
*all: 允许所有的操作.
*none: 禁止所有操作.
*uninitialized: 缺省值（默认值）, 相当于 all.


#####clientX  screenX pageX
screenX:鼠标位置相对于用户屏幕水平偏移量，参照点是原点是屏幕的左上角。
clientX:跟screenX相比就是将参照点改成了浏览器内容区域的左上角，该参照点会随之滚动条的移动而移动。
pageX：参照点是浏览器内容区域的左上角，但它不会随着滚动条而变动
####Demo
父元素设置relative
要拖拽的元素absolute 
？？？left top必须设置初始值，不然获取的是auto，无法变成数字--暂未解决的问题
```css
	    body{
	    	position: relative;
	    }
		.box{
			background-color: red;
			width: 100px;
			height: 100px;
			left: 0px;
			top: 0px;
			position: absolute;
		}
```
div设置draggable

```html
	<div class="box" id="box" draggable="true"></div>

	<script type="text/javascript">
	var box =document.getElementById("box"); 
    
    box.addEventListener("dragstart", function( event ) {
        // 保存拖动元素的CSS
        var style = window.getComputedStyle(event.target, null);
        //设置dataTransfer
        event.dataTransfer.setData("text/plain",(parseInt(style.getPropertyValue("left"),10) - event.clientX) + ',' + (parseInt(style.getPropertyValue("top"),10) - event.clientY));

    }, false);

    //把任何元素变成有效的放置目标
    document.addEventListener("dragover", function( event ) {
        // 阻止默认动作
        event.preventDefault();
        return false; 
    }, false);

	document.addEventListener("drop", function( event ) {
        // 移动拖动的元素到目标位置
        var offset = event.dataTransfer.getData("text/plain").split(',');
        box.style.left = (event.clientX + parseInt(offset[0],10)) + 'px';
        box.style.top = (event.clientY + parseInt(offset[1],10)) + 'px';
         // 阻止默认动作（如打开一些元素的链接）
        event.preventDefault();
        return false;
  }, false);
	</script>

```
[https://developer.mozilla.org/zh-CN/docs/DragDrop/Drag_and_Drop90](https://developer.mozilla.org/zh-CN/docs/DragDrop/Drag_and_Drop)
[https://www.w3cmm.com/html/drag.html](https://www.w3cmm.com/html/drag.html)
[https://developer.mozilla.org/zh-CN/docs/Web/API/DataTransfer](https://developer.mozilla.org/zh-CN/docs/Web/API/DataTransfer)
###非HTML5
参考js高程22章
原理：创建一个绝对定位的元素，使其可以用鼠标移动。
由 mousedown mousemove mouseup 三部分组成。
mousedown：鼠标按键被按下时发生
mouseup：鼠标松开时发生
####鼠标指针保持在点击点
通过将 event的 clientX 和 clientY 属性与该元素的 offsetLeft 和 offsetTop 属性进行比较，就可以算出水平方向和垂直方向上需要多少空间

![](~/15-37-31.jpg)
####Demo
```html
<div class="box draggable" id="box" ></div>

<script type="text/javascript">
    var dragging =null,
    	diffx =0,
    	diffy =0;
	document.addEventListener("mousedown",function(event){
        if (event.target.className.indexOf("draggable") > -1){
            dragging = event.target;
            diffx =event.clientX-dragging.offsetLeft;
            diffy =event.clientY-dragging.offsetTop;
        }
	},false);

	document.addEventListener("mousemove",function(event){
		if(dragging !==null){
            dragging.style.left = event.clientX-diffx + "px";
            dragging.style.top = event.clientY-diffy + "px";
		}
	},false);

	document.addEventListener("mouseup",function(event){
		dragging =null;
	},false);
</script>
```