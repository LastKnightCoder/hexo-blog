---
title: PicGo图床上传问题
date: 2021-06-19
categories: PicGo
tags:
  - 图床	
  - PicGo
---



## 问题描述

使用 PicGo 上传图片报错

<img src="https://cdn.jsdelivr.net/gh/LastKnightCoder/ImgHosting3/20210619103831.png" alt="image-20210619101835151" style="zoom:50%;" />

<img src="https://cdn.jsdelivr.net/gh/LastKnightCoder/ImgHosting3/20210619103837.png" alt="image-20210619102314780" style="zoom:50%;" />

## 查看日志

相关日志如下

```
------Error Stack Begin------
RequestError: Error: tunneling socket could not be established, cause=connect ECONNREFUSED 127.0.0.1:1080
    at new RequestError (D:\PicGo\resources\app.asar\node_modules\request-promise-core\lib\errors.js:14:15)
    at Request.plumbing.callback (D:\PicGo\resources\app.asar\node_modules\request-promise-core\lib\plumbing.js:87:29)
    at Request.RP$callback [as _callback] (D:\PicGo\resources\app.asar\node_modules\request-promise-core\lib\plumbing.js:46:31)
    at self.callback (D:\PicGo\resources\app.asar\node_modules\request\request.js:185:22)
    at Request.emit (events.js:200:13)
    at Request.onRequestError (D:\PicGo\resources\app.asar\node_modules\request\request.js:881:8)
    at ClientRequest.emit (events.js:200:13)
    at ClientRequest.onError (D:\PicGo\resources\app.asar\node_modules\tunnel-agent\index.js:179:21)
    at Object.onceWrapper (events.js:288:20)
    at ClientRequest.emit (events.js:200:13)
-------Error Stack End------- 
```

上网搜索，发现是设置了代理，去掉代理设置即可。

## 解决办法

打开配置文件

<img src="https://cdn.jsdelivr.net/gh/LastKnightCoder/ImgHosting3/20210619103847.png" alt="image-20210619102143684" style="zoom:50%;" />

发现设置了代理，将改行删掉即可(所以我当时为什么要设置代理)

## 参考文章

- [PicGo RequestError: Error: tunneling socket could not be established, cause=connect ECONNREFUSED 127.0.0.1:36677](https://www.cnblogs.com/masterchd/p/14230191.html)

