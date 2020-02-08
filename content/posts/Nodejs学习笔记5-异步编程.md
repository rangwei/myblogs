---
title: "Nodejs学习笔记5 异步编程"
date: 2020-02-08T16:40:31+08:00
draft: true
---

# 异步编程

*阅读这篇blog大约需要10分钟*

Node.js的关键词就是异步、非阻塞、事件、单线程。所以理解并熟练掌握异步编程非常重要:

- callback概念
- Promise
- aysnc/await

> “Write code that is easy to delete, not easy to extend.”
—Tef, Programming is Terrible

## 介绍

今天的主要内容其实都是JavaScript的语法：

1. 最简单的异步函数
2. callback
3. Promise
4. aysnc/await
5. 小结

## 最简单的异步函数

首先来看一个最简单的异步函数，用setTimeout()来模拟一个两秒的操作：

aysnc-simple.js:

```
console.log('Starting app');

setTimeout(()=>{
    console.log('hello!');
}, 2000); //异步操作

console.log('Finish app');
```

代码输出:

```
Starting app
Finish app
hello!
```
可以看到代码是非阻塞的，异步操作会放到一个callback队列中，后面的代码会继续执行。

## callback
在前面的小例子中，其实已经接触了不少关于callback的用法，文件读写操作都用了异步callback的方式来完成。这里看一个最简单的callback写法：(callback.js)

```
const getUser = (id, callback) => {
    const user = {id, name: 'mockname'};
    callback(user);
};

getUser(123, user => {console.log(user)});
```
首先创建了一个getUser函数，它用来模拟获取用户的数据，然后通过callback返回结果。

调用getUser需要传递两个参数，分别是用户id和一个callback函数。

代码输出：

```
{ id: 123, name: 'mockname' }
```

## Promise
callback的编程方式如果用的太多会造成代码嵌套太多，代码维护困难。于是有了Promise的设计。

可以通过promise.then(...)这种写法来执行异步代码，多个异步代码可以通过promise链的形式。

一个简单的Promise例子:(promise.js)



```
let promise = new Promise((resolve, reject) => {
    setTimeout(() => resolve("done!"), 1000);
  });
  
promise.then(result => console.log(result), error => console.log(error));

```
这里传递给promise的函数是一个异步代码，如果代码正常运行后，那么会把结果放入result这个变量。在promise后面的then()里可以获取。

现在看一个更有意思的Promise链，链条里都是异步方法，它们会依次按顺序执行:(promise-chain.js)

```
new Promise((resolve, reject) => {
    setTimeout(() => resolve(1), 1000);
}).then(
    result => {
        console.log(result); //1
        return new Promise(
            (resolve, reject) => {
                setTimeout(() => resolve(result * 2), 1000);
            }
        );
    }
).then(
    result => {
        console.log(result);//2
        return new Promise(
            (resolve, reject) => {
                setTimeout(() => resolve(result * 2), 1000);
            }
        );
    }
).then(
    result => {
        console.log(result);//4
        return new Promise(
            (resolve, reject) => {
                setTimeout(() => resolve(result * 2), 1000);
            }
        );
    }
);
```
代码输出：

```
1
2
4
```

## aysnc/await
aysnc/await其实也是在使用promise，但是一种更加简单的语法。通过aysnc/await关键字，可以让异步代码看起来像同步代码一样。

示例:(aysnc.js)

```
async function f() {
    let promise = new Promise((resolve, reject) => {
        setTimeout(() => resolve("done!"), 1000)
      });

    let result = await promise;//代码会等待执行完毕

    console.log(result);
    
}
```
在这里添加await关键字以后，代码会等待执行完毕才继续。注意await只能在aync方法里执行。如果想在主程序里使用await的话，可以把它放到一个匿名函数里，就像这样：

```
(async() => {
    await new Promise((resolve, reject) => setTimeout(resolve, 3000));
    console.log('after 3 s done.');
})();
```

## 小结

也有很多第三方库来帮助我们通过promise的方式来编写代码。

对于简单的情况，直接使用callback也是很直接的，但是在一些复杂的比如涉及多个异步操作的时候，通过最新的async/await方式会让代码看起来极其简单。

接下来我把这些概念通过网络请求代码的方式来实践一下。

## 项目代码
[nodejs-blogs/a05](https://github.com/rangwei/nodejs-blogs/tree/master/a05)

## 参考阅读

- [JavaSciprt现代教程: Promise, async/await](https://zh.javascript.info/async)    
