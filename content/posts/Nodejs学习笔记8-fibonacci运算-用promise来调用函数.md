---
title: "Nodejs学习笔记8 Fibonacci运算,"
date: 2020-02-08T16:41:27+08:00
draft: true
---

# fibonacci运算, 用promise来调用函数

*阅读这篇blog大约需要10分钟*

Node.js有很丰富的生态，有很多有点，其最大的特点就是善于I/O，不善于计算。

今天主要看一下对于大量计算的任务，node.js的表现。然后再对promise做一些补充学习。

> Any fool can write code that a computer can understand. Good programmers write code that humans can understand. -- Martin Fowler

## 介绍

主要包含这些内容：

- fibonacci运算
- util.promisify
- 标准模块的promise用法

## fibonacci运算

1.介绍

斐波那契数列（Fibonacci sequence），又称黄金分割数列、因数学家列昂纳多·斐波那契（Leonardoda Fibonacci）以兔子繁殖为例子而引入，故又称为“兔子数列”，指的是这样一个数列：1、1、2、3、5、8、13、21、34、……

这是一个非常消耗CPU的运算。

2.测试

通过Node服务器来计算fibonacci，然后运行后看看效果。

server.js:
```
const http = require('http');
const url = require('url');

const fibonacci = n => { 
    if (n === 1 || n === 2) return 1; 
    else return fibonacci(n-1) + fibonacci(n-2); 
}

http.createServer(
    (req, res) => {
        const urlP = url.parse(req.url, true);
        let fibo;
        res.writeHead(200, {'Content-Type': 'text/plain'});
        if (urlP.query['n']) {
            fibo = fibonacci(urlP.query['n']);
            res.end(`Fibonacci ${urlP.query['n']} = ${fibo}`);
        }else {
            res.end('USAGE: http://localhost:8124?n=##');
        }
    }
).listen(8124, 'localhost');

console.log('Server is running');

```
可以分别在浏览器输入一些数字来测试：

    http://localhost:8124?n=10
    
    http://localhost:8124?n=20
    
    http://localhost:8124?n=40
    
    http://localhost:8124?n=80

会发现响应逐渐变慢，电脑的CPU很高，然后其它请求也会得不到回应，这是因为Node.js是单线程的。它的优势是I/O非阻塞，但是对于CPU计算多的任务，Node就不适合了。

有一些改进的办法，比如新开线程、对算法进行优化，但是个人觉得还是应该在设计上去避免，尽量发挥Node的强项避免它的弱项

## util.promisify

Node.js生态大多数异步函数都是通过callback的方法来实现的。utilt提供了promisify方法来将传统的callback函数转化为promise写法，示例：

ls.js:
```
const fs = require('fs');
const util = require('util');

const fs_readdir = util.promisify(fs.readdir);

(async () => {
    const files = await fs_readdir('.');
    for (let fn of files) {
        console.log(fn);
    }
})().catch(err => {console.log(err)});
```


## promise module
从Node10开始标准库也提供了promise版本，比如Filesystem，使用方法很简单，只要在引入的时候加上.promise即可，示例:

ls2-promise.js:
```
const fs = require('fs').promises;

(async () => {
    const files = await fs.readdir('.');
    for (let fn of files) {
        console.log(fn);
    }
})().catch(err => {console.log(err)});
```

## 小结

首先通过fibonacci计算的例子，可以看到Node的一些有意思的特性：单线程，对CPU计算高的任务并不擅长。可以尽量应用在那些高并发、I/O密集、少量业务逻辑的场景。

对于一些常见的异步I/O操作，可以通过util.promisify的方法来转化为promise语法。另外现在node10以后很多标准模块都自带了对promise的支持。

## 项目代码
- [nodejs-blogs/a08](https://github.com/rangwei/nodejs-blogs/tree/master/a08)

## 参考阅读
- https://exploringjs.com/es6/index.html#toc_ch_promises
- https://brunoscopelliti.com/new-util-promisify-in-nodejs/
- https://nodejs.org/dist/latest-v12.x/docs/api/fs.html


    
    
