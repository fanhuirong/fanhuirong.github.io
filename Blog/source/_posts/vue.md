---
date: 2016-11-08 20:34
title: vue笔记
---

#vue
##入门
###开始
各种死于安装webpack  orz
于是换国内的cnpm
```
npm install -g cnpm --registry=https://registry.npm.taobao.org
cnpm install webpack -g
npm install vue-cli -g

```
进入目录 命令行进行项目初始化
```
vue init webpack-simple firstVue
或者创建 vue1.0 的项目
vue init webpack-simple#1.0 fistVue

cd firstVue
npm install # 尽量用npm装依赖 cnpm会缺少一些包 
npm run dev
```



###Tip
1. 一个组件下只能有一个并列的 div
2. 数据要写在 return 里面

###组件
1. 引入App.vue中 `import firstcomponent from './component/firstcomponent.vue'`
2. 注册 
```
export default {
  data () {
    return {
      msg: 'Hello Vue!'
    }
  },
  components: { firstcomponent } //增加这行
}
```
3. 使用 在<template></template>内加上
```
<template>
  <div id="app">
    <firstcomponent></firstcomponent>
  </div>
</template>
```
###路由 vue-router
`cnpm install vue-router --save`
1. 配置webpack.config.js
2. 修改main.js 引入vue-router
定义组件
创建路由器
启动应用
```js
import VueRouter from "vue-router";
Vue.use(VueRouter);
```
3.修改App.vue
template里增加
```html
    <ul>
        <li><router-link to="/first">点我跳转到第一页</router-link></li>
        <li><router-link to="/second">点我跳转到第二页</router-link></li>
    </ul>
    <router-view class="view"></router-view>
```
###动态数据 vue-resource
`cnpm install vue-resource --save`
###编译
`npm run build`
把 index.html 和整个 dist 目录丢到服务器就可以了
###tip
```
vue init webpack-simple firstVue

cd firstVue
npm install # 尽量用npm装依赖 cnpm会缺少一些包 

npm run dev
npm run build
```
[Vue2.0 新手完全填坑攻略——从环境搭建到发布](http://www.jianshu.com/p/5ba253651c3b)



##一个todoist应用
就从做一个todoist应用开始吧


###基础 组件选项
data
methods
watch 监听方法
v-text v-html  { { } }
```
<p>{{a}}</p>// <span>aaaaaa</span>
<p v-html="a"></p>//<span>aaaaaa</span>
<p v-html="a"></p>//aaaaaa

a: '<span>aaaaaa</span>'
```
v-if（不渲染） v-show（display：none） 控制模块隐藏
v-for
v-on:click 简写 @click
v-bind 属性绑定 简写 :class

###es6
import、export 是es6语法 
```js
data:function(){
return { a:'111'}
}
//es6写法
data(){
return {a:'111'}
}
```  
###localstorage存储
window.localStorage
store.js 用于fetch save

![](~/21-12-25.jpg)

watch items的变化使用handler 设置deep 
###组件通信
使用组件记得要注册

####父-子 props
![](~/21-26-32.jpg)
####子-父
$.emit
//下面两种vue2中取消了
$.dispatch 需要注册events
$.broadcast

###进化
利用计算属性来修改todoist
####slot
####event dispatcher

```js
window.Event = new Vue();
Vue.component('xxxx',{
  data:'xxxx',
  methods:{
    onClick(){
      Event.$emit('applied');
  }
  }
})
```
##工具神马的
###vue devtools
[vue devtools](https://github.com/vuejs/vue-devtools)

### 用js获取vue元素的内容并更改
```js
var app = new Vue({
  el: '#root',
  data: {
    names:['joe','mary','jack']
  },
  mounted (){
    document.querySelector('#button').addEventListener('click',()=>{
    let name = document.querySelector('#input')
    app.names.push(name.value);
    name.value = ''
    })
  }

})

//等价于写到mounted里dom操作
document.querySelector('#button').addEventListener('click',()=>{
  let name = document.querySelector('#input')
  app.names.push(name.value);
  name.value = ''
})

```

[五个vue2免费教程](https://gold.xitu.io/post/584cc93b8e450a006ac2196d)
[Vue + Webpack 开发实践](https://cinwell.com/post/vue-webpack/)