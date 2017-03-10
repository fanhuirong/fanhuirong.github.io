---
date: 2016-11-08 20:34
title: vue笔记
---

# vue
## 入门
### 开始
因为需要将现有项目用vue重写，所以就去学习了中文文档超级详细的vue
搭建环境各种死于安装webpack  orz 对于墙的存在真是无能为力啊，于是换国内的cnpm进行安装
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



#### Tip
1. 一个组件下只能有一个并列的 div
2. 数据要写在 return 里面

### 组件
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
3. 使用 在<template></template>内加上组件即可
```
<template>
  <div id="app">
    <firstcomponent></firstcomponent>
  </div>
</template>
```
### 路由 vue-router
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
[vue-router文档](http://router.vuejs.org/zh-cn/)
[vue-router 60分钟入门](http://www.cnblogs.com/keepfool/p/5690366.html)
### 动态数据 vue-resource
`cnpm install vue-resource --save`
### 编译
`npm run build`
把 index.html 和整个 dist 目录丢到服务器就可以了

[Vue2.0 新手完全填坑攻略——从环境搭建到发布](http://www.jianshu.com/p/5ba253651c3b)

### vuex
vuex or 全局事件总线

[vuex文档](https://vuex.vuejs.org/zh-cn/intro.html)
[用vue构建一个笔记应用](https://segmentfault.com/a/1190000005015164)
## 一个todoist应用
那就从做一个todoist应用开始吧

### 基础 组件选项
data
methods
watch 监听方法
v-text v-html  
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

### es6
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
### localstorage存储
window.localStorage
store.js 用于fetch save

watch items的变化使用handler 设置deep 
### 组件通信
使用组件记得要注册

#### 父-子 props
####子-父
$.emit
//下面两种vue2中取消了
$.dispatch 需要注册events
$.broadcast

### 进化
利用计算属性来修改todoist
#### slot
#### event dispatcher

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
## 工具神马的
### vue devtools
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

### questions
#### 修改title
[vue2.0一起在懵逼的海洋里越陷越深（六）](http://leenty.com/2016/12/18/vue2-6/)

####按需加载

[vue按需加载](https://www.zhihu.com/question/41147233)
[vue中，组件如何做到按需加载？](https://www.zhihu.com/question/54111689)
[vue-router如何配置](http://zakwu.me/2016/09/18/vue-routerpei-zhi/)
[在webpack中使用code splitting实现按需加载](http://www.alloyteam.com/2016/02/code-split-by-routes/)
[webpack指南](https://www.gitbook.com/book/toobug/webpack-guide)
[如何配置webpack的code-splitting使之不反复打包第三方库？](https://www.zhihu.com/question/31352596/answer/127369675?from=profile_answer_card)

[webpack CommonsChunkPlugin详细教程](https://segmentfault.com/a/1190000006808865)

#### require
关于require('moment')是如何找到对应文件的
reqire会找到node_modules 然后找到moment文件 在文件下找package.json 然后找到main看main对应的js是哪个，然后进行加载

#### lazy load
[menu设置vue-router时，路由懒加载组件时在第二次路由时组件渲染失败 ](https://github.com/ElemeFE/element/issues/2022)

### modal
[modal](http://cn.vuejs.org/v2/examples/modal.html)

[vue源码解析](https://github.com/youngwind/blog)

[vue源码解析](https://wakeupmypig.github.io/jw_blog/html/Vue.js/Vue%E6%8C%87%E4%BB%A4.html)