---
title: "Nodejs学习笔记7 服务器编程"
date: 2020-02-08T16:41:11+08:00
draft: true
---
# 服务器编程

*阅读这篇blog大约需要5分钟*

传统的后端语言有java, php等，如今Node.js也凭借其强大的JS，广泛的生态，非阻塞I/O，高并发的特性，占据了一席之地。今天快速学习了解一下Node服务器编程。

- Hello Node HTTP
- Hello Express

> KISS: Keep it simple and stupid. 

## 介绍

通过几个小例子来感受一下Node服务器的极简风格:

1. 安装nodemon
2. 最简单的http服务器
3. Hello Express


## 安装nodemon

npm install nodemon -g

nodemon是一个非常有用的小工具。每次只要修改完代码，都需要手工重新运行程序。有了nodemon以后，它能够自动监测代码如果有变化就会自动重启运行。

## 最简单的http服务器

通过Node的标准HTTP模块用一句话就能构建一个简单的服务器：

app.js:

```
const http = require('http');

http.createServer(function (req, res) {
    res.writeHead(200, { 'Content-Type': 'text/plain' });
    res.end('Hello, World!\n');
}).listen(3000, '127.0.0.1');

console.log('Server running at http://127.0.0.1:3000');
```

通过nodemon 运行:
```
nodemon app.js
```


## Hello Express

1. 介绍

Express 是一个保持最小规模的灵活的 Node.js Web 应用程序开发框架,为 Web 和移动应用程序提供一组强大的功能。

2. 为项目安装Express模块

```
npm init -y

npm install express --save
```

3. 示例

server.js:

```
const express = require('express');

const app = express();

app.get('/', (req, res) => {
    res.send('Hello Express!');
});

app.listen(3000);
```

nodemon server.js

可以通过浏览器访问测试:

    http://localhost:3000

让express返回html:


```
const express = require('express');

const app = express();

app.get('/', (req, res) => {
    res.send('<h1>Hello Express!</h1>');
});

app.listen(3000);
```
## 小结

node程序不仅代码非常简单，而且消耗资源很少，支持高并发。这是它非常大的优势。

但是每一种语言和环境都有自己的优势和劣势。如果大量CPU计算的任务，node就不太适合了。下次我们可以看看这一点。

## 项目代码
- [nodejs-blogs/a07](https://github.com/rangwei/nodejs-blogs/tree/master/a07)

## 参考阅读
 - http://www.expressjs.com.cn
 - http://expressjs.com

    
    

