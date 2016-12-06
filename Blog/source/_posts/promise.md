
---
date: 2016-10-11 10:10
title: Promise
---

###Promise
创建流程
1、new Promise(fn) 返回一个promise对象
2、在fn 中指定异步等处理
处理结果正常的话，调用resolve(处理结果值)
处理结果错误的话，调用reject(Error对象)
```js
//XHR的promise对象
function getURL(URL) {
    return new Promise(function (resolve, reject) {
        var req = new XMLHttpRequest();
        req.open('GET', URL, true);
        req.onload = function () {
            if (req.status === 200) {
                resolve(req.responseText);
            } else {
                reject(new Error(req.statusText));
            }
        };
        req.onerror = function () {
            reject(new Error(req.statusText));
        };
        req.send();
    });
}
// 运行示例
var URL = "http://httpbin.org/get";
getURL(URL).then(function onFulfilled(value){
    console.log(value);
}).catch(function onRejected(error){
    console.error(error);
});
```
[Promise](http://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000/0014345008539155e93fc16046d4bb7854943814c4f9dc2000)
[Promise迷你书](http://liubin.org/promises-book/#chapter1-what-is-promise)
