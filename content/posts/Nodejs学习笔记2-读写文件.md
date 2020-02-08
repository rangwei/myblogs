---
title: "Nodejs学习笔记2 读写文件"
date: 2020-02-08T15:38:49+08:00
draft: true
---

# 读写文件功能

*阅读这篇blog大概需要10分钟*

通过require来引入标准的模块功能来实现一些高级功能。

- 如何读写文件
- 如何使用模块来组织代码

> 方法在JavaScript中属于一等公民，和字符串、数字等类型是完全一样的。可以通过变量传递。

## 介绍

学习用文件模块来进行读写操作，了解callback方法的使用。

1. 使用文件模块FileSystem
2. module变量
3. 通过module.export导出方法
4. 通过命令来写入文件内容
5. 小结

## 准备
创建一个新的项目a02，并用npm初始化。

```
npm init -y
```

## 使用FileSystem模块

可以通过require('fs')语法来引用标准模块。

关于FileSystem模块的文档：
[fs.appendFile](https://nodejs.org/api/fs.html#fs_fs_appendfile_path_data_options_callback)

app.js代码:

```
const fs = require('fs');

console.log('Starting app');

fs.appendFile('message.txt', 'Hello world!', err => {
    if (err) {
        console.log('write file error!');
    }
});

console.log('Finishing app');
```

fs.appendFile()方法第三个参数是callback的异步函数,这里使用了箭头函数的语法格式。在Node应用中大部分方法都会是这样的风格，这就是Node强大的地方。通过异步I/O的方式极大的提高了效率。

读取文件的方法是fs.readFile(path[, options], callback),使用方法类似。


## module变量
我们的代码可以访问module这个变量，查看module：

```
console.log('Starting app');

console.log(module);

```
可以看到类似像这样的输出：

```
Starting app
Module {
  id: '.',
  path: '/Users/user/Documents/projects/apps/node/blogs/a02',
  exports: {},
  parent: null,
  filename: '/Users/user/Documents/projects/apps/node/blogs/a02/module.js',
  loaded: false,
  children: [],
  paths: [
    '/Users/user/Documents/projects/apps/node/blogs/a02/node_modules',
    '/Users/user/Documents/projects/apps/node/blogs/node_modules',
    '/Users/user/Documents/projects/apps/node/node_modules',
    '/Users/user/Documents/projects/apps/node_modules',
    '/Users/user/Documents/projects/node_modules',
    '/Users/user/Documents/node_modules',
    '/Users/user/node_modules',
    '/Users/node_modules',
    '/node_modules'
  ]
}
```

## 通过module.export导出方法
创建一个新的文件notes.js:


```
console.log('Starting notes.js');

module.exports.addNote = () => {
    console.log('add note');
    return 'new note';
};
```
然后app.js可以通过require来引入：

```
const fs = require('fs');
const notes = require('./notes');

console.log('Starting app');

console.log(notes.addNote());

```
运行以后的输出：

```
Starting notes.js
Starting app
add note
new note
```
可以看到require()会在应用最开始自动运行。
然后运行addNote()


## 通过命令来写入文件内容

process变量包含了命令参数。可以通过下面的代码测试：

```
console.log(process.argv);
```

通过命令行运行app.js,并加入两个参数。

```
node app.js add test
```
'add'和'test'分别可以通过process.argv[2]和process.argv[3]读取。

修改相应代码，可以完成通过命令来写入文件的功能。

app.js:

```
const command = process.argv[2];

const body = process.argv[3];

if (command == 'add') {
    notes.addNote(body);
}
```


notes.js:

```
module.exports.addNote = (body) => {

    console.log('add note');

    fs.appendFile('notes.txt', body, err => {
        if (err) {
            console.log('write file error!');
        }
    });
    return 'new note';
};
```
测试程序:

```
node app.js add test
```
可以看到内容写入了文件。

## 小结
Node通过标准和第三方模块提供了各种强大的功能，让编写代码变得非常简单，JavaScript最新的语法用起来也非常方便。后面我会使用Json对象来保存数据。

## 项目代码
[nodejs-blogs/a02](https://github.com/rangwei/nodejs-blogs/tree/master/a02)

## 参考阅读
- [Node.js 文档](https://nodejs.org/api/index.html)

- [First-Class Functions in JavaScript](https://nick.scialli.me/first-class-functions-in-javascript/)

    
    
