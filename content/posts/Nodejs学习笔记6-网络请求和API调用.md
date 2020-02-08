---
title: "Nodejs学习笔记6 网络请求和API调用"
date: 2020-02-08T16:40:53+08:00
draft: true
---

# 网络请求和API调用

*阅读这篇blog大约需要10分钟*

网络编程是非常重要的一块内容，JS作为web开发最紧密的一部分，在网络编程方面本来就是很强大的，然后Node生态也提供了各种便利的工具来发送和接收网络请求。

我们最基本的工具是Request模块，然后第三方也提供基于Promise的工具，常用的有axios,node-fetch等。

- Request
- axios
- node-fetch

> Resources for developers, by developers.

## 介绍

先通过几个小例子来coding体验一下不同：

1. Request
2. Web RESTful API
3. axios
4. node-fetch

## Request
1. 介绍
Request是一个超级简单使用的HTTP Client工具。
它的目标是通过最简单的方法来发送http请求。

安装Request模块:

```
npm install request --save
```
2. 最基本的用法：

```
var req = require('request');

req('http://www.bing.com', function(error, response, body){
    if (!error && response.statusCode == 200) {
        console.log(body) // Show the HTML for the baidu homepage.
      }
});
```
Request通过callback来异步传输数据，现在有很多其它更强大的工具来帮助完成工作。

## Web RESTful API

1. RESTful

rest是REpresentational State Transfer三个单词的缩写，由Roy Fielding于2000年论文中提出，它代表着分布式服务的架构风格。

2. Demo API:

接下来使用这个示例api来demo应用：

    https://reqres.in/api/users

查询某一个用户的数据：

    https://reqres.in/api/users/1

返回结果：

```
{
    "data": {
        "id": 1,
        "email": "george.bluth@reqres.in",
        "first_name": "George",
        "last_name": "Bluth",
        "avatar": "https://s3.amazonaws.com/uifaces/faces/twitter/calebogden/128.jpg"
    }
}
```
假设我们需要先通过用户id查询到基本数据，然后根据avatar这个字段保存的url来获取用户头像。

下面看看如何通过axios和node-fetch来完成这个功能。

## axios

1. 介绍
Node.js里面非常有名的基于Promise的HTTP Client，简单易用高效。
主要的特性有支持Promise，自动转化JSON数据等、支持拦截器、丰富的配置项。目前使用非常广泛。

安装axios模块:

```
npm install axios --save
```


2. 通过aysnc/await

axios_demo.js:

```
const axios = require('axios');
const fs = require('fs');
const url = 'https://reqres.in/api/users';

const getPicByUser = async user => {

    try {
        const userResponse = await axios.get(url + '/' + user);

        const picResponse = await axios(
            {
                url: userResponse.data.data.avatar,
                method: 'GET',
                responseType: 'stream'
            }
        );

        const writer = fs.createWriteStream('./avatar.jpg');
        picResponse.data.pipe(writer);

    } catch (error) {
        console.log(error)
    }
}

getPicByUser(1);
```
看起来非常简单，和同步代码的语法差不多了。首先获取某个用户的数据，然后根据avatar查询到头像地址，最后写入文件保存。

3. 通过promise.then的语法
也可以通过Promise链的方式来完成刚刚的功能：

```
axios.get(url + '/1')
    .then(userResponse => axios({
        url: userResponse.data.data.avatar,
        method: 'GET',
        responseType: 'stream'
    }), err => console.log(err))
    .then(picResponse => {
        const writer = fs.createWriteStream('./avatar.jpg');
        picResponse.data.pipe(writer);
    }, err => console.log(err));

```


## node-fetch

1.介绍
Node.js版本的Fetch工具。

安装node-fetch:

```
npm install node-fetch --save
```


2. 通过aysnc/await

fetch_demo.js:
```
const fetch = require('node-fetch');
const fs = require('fs');


const url = 'https://reqres.in/api/users';

const getPicByUser = async(user) => {

    let userResponse = await fetch(url + '/' + user);

    let userData = await userResponse.json();

    let picResponse = await fetch(userData.data.avatar);

    const writer = fs.createWriteStream('./avatar.jpg');
    
    picResponse.body.pipe(writer);

}

getPicByUser(1);
```
大致和上面的代码很像，由于通过await关键字，所以没有了callback嵌套的格式。

3. 通过promise.then的语法

```
fetch(url + '/1')
.then(respone => respone.json())
.then(userData => fetch(userData.data.avatar))
.then(picResponse => {
    const writer = fs.createWriteStream('./avatar.jpg');
    picResponse.body.pipe(writer);
});
```


## 小结
可以看到通过Node.js来实现网络请求的发送接收是超级方便的，在不同的系统之间调用RESTFul API也很简单。这也是个人最喜欢的一方面。

## 项目代码
- [nodejs-blogs/a06](https://github.com/rangwei/nodejs-blogs/tree/master/a06)

## 参考阅读

- https://www.npmjs.com/package/request
- https://www.npmjs.com/package/axios
- https://www.npmjs.com/package/node-fetch
- https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch

    
    
