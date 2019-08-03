---
date: 2019-03-03 14:17
title: 新浪微博自动发微博脚本的实现
categories: 
- 技术
---

## 注册应用
1. 在[新浪开放平台](https://open.weibo.com/)注册一个自己的应用，获取自己的 ***App Key*** 和 ***App Secret*** 。
2. 在【我的应用】|【应用信息】|【高级信息】里设置授权回调页为`https://api.weibo.com/oauth2/default.html`。

## 获取access_token
1. 安装廖雪峰老师的微博库 `sudo pip install sinaweibopy`。
2. 代码实现
```python

from weibo import APIClient 
import webbrowser 

APP_KEY = "" # App Key 

APP_SECRET = "" # App Secret 

CALLBACK_URL = 'https://api.weibo.com/oauth2/default.html' # callback 

client = APIClient(app_key=APP_KEY, app_secret=APP_SECRET, redirect_uri=CALLBACK_URL) 

url = client.get_authorize_url() 

webbrowser.open_new(url) # 浏览器会自动打开页面要求用户授权，点击授权然后在新页面即可获取用户access_token

```

## 发送微博

```python

# -*- coding: utf-8 -*-

import requests

url = "https://api.weibo.com/2/statuses/share.json"

payload={
"access_token": "", # Access Token
"status": "机器人测试的第一条微博～～～http://weibo.com"
}

r = requests.post(url, data=payload)

```

### 报错处理

`{"error":"appkey not bind domain!","error_code":10017,"request":"/2/statuses/share.json"}` 

解决方案：在新浪开放平台设置安全域名。

路径：【我的应用】|【应用信息】|【基本信息】|【安全域名】


`{"error":"text not find domain!","error_code":10017,"request":"/2/statuses/share.json"}`

解决方案：在每一条分享内容上加上安全域名的链接,需要以 `http` 开头。

```js
payload={
"access_token": "", # Access Token
"status": "test～～～http://weibo.com"
}
```

[weibo error api](https://open.weibo.com/wiki/Help/error)

## 定时执行脚本
使用 crontab 在服务器上定时执行脚本
```

crontab -e

# write your command
21 23 * * * python /Users/kunkun/code/weiborobot/weibo.py  

crontab -l # 查看是否配置成功

sudo launchctl list | grep cron  # mac查看 crontab 是否启动

ls -al /etc/crontab # 检查需要的文件

sudo touch /etc/crontab  # 如果 crontab 文件不存在则创建

```
[任务不执行排查](https://segmentfault.com/a/1190000017493725)
http://linux.vbird.org/linux_basic/0430cron.php#cron