---
date: 2021-11-22 19:17
title: 博客部署问题
categories: 
- 技术
---

好久没有给博客除草，结果增加了新文章本地显示OK，但是部署后发现页面空白，在尝试了各种方式（比如切换分支，切换主题）之后，搜到了一篇文章。

[解决hexo generate 生成的时候index.html为0kb空白的问题](https://blog.tcs-y.com/2020/04/26/hexo-index-0kb/)

发现和我一样的问题，居然是 node 版本过高的问题，导致无法部署，我先切换成了`sudo n stable
`变成 v16.13.0，发现 generate后还是空白，换成博主一样的 12.16.2就ok了