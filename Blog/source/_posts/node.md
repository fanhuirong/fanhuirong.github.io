# node

## 模块化

## node模块化

```js
//other_module.js
module.exports = variable;

//main.js
var foo = require('other_module');
```

如果要输出一个键值对象{}，可以利用exports这个已存在的空对象{}，并继续在上面添加新的键值；

如果要输出一个函数或数组，必须直接对module.exports对象赋值。

###es6模块化
```js

// profile.js
var firstName = 'Michael';
var lastName = 'Jackson';
var year = 1958;

export {firstName, lastName, year};

// main.js
import {firstName, lastName, year} from './profile';

```

## 模块
global 全局
process 进程
fs 文件系统 同时提供同步异步方法
stream 流
http 
url
path
crypto 加密

### stream
一个Readable流和一个Writable流串起来后，所有的数据自动从Readable流进入Writable流，这种操作叫pipe。
```js
'use strict';

var fs = require('fs');

var rs = fs.createReadStream('sample.txt');
var ws = fs.createWriteStream('copied.txt');

rs.pipe(ws);
```







