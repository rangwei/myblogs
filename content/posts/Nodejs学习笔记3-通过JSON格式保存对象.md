---
title: "Nodejs学习笔记3 通过JSON格式保存对象"
date: 2020-02-08T15:39:05+08:00
draft: true
---

# 通过JSON格式保存对象

*阅读这篇blog大概需要10分钟*

继续完善前面的小例子，通过命令参数来传递内容，并通过JSON格式来保存数据。

- 通过第三方yargs包来处理参数
- JSON格式操作

> any application that can be written in JavaScript, will eventually be written in JavaScript. -- Atwood定律

## 介绍

继续通过文件读写的功能来完善保存数据的功能。通过JSON来操作，这也是Node后端应用的优势，前后端一致、处理方便快捷：

1. 安装使用yargs包
2. 完善add note功能
3. JSON对象

## 安装使用yargs包
在终端输入命令：

```
npm install yargs --save
```

可以看到yargs的module自动下载安装，并且自动更新了package.json配置文件。

yarg的使用很简单，输入参数会保存在argv变量中。

```
const yargs = require('yargs');
...
console.log(yargs.argv);
```

增加参数测试：

```
node app.js add --title 'abc'
```
输出:

```
Starting notes.js
Starting app
{ _: [ 'add' ], title: 'abc', '$0': 'app.js' }
```

## 完善add note功能

通过yargs._[0]获取命令，然后分别通过argv.title和argv.body获取标题和内容:

app.js:

```
const argv = yargs.argv;
const command = argv._[0];
...
notes.addNote(argv.title, argv.body);
```

然后就可以这样来运行程序了：

```
node app.js add --title 'abc' --body 'this is test'
```

## JSON对象

JSON是JavaScript Object Notation的缩写。在Web应用中使用很广泛。
可以通过标准的JSON函数在JSON对象和字符串之间转换：

测试代码：

```
const obj = {name: 'andy'};
const stringObj = JSON.stringify(obj);

console.log(typeof stringObj);// string
console.log(stringObj); //{"name":"andy"}

const person = JSON.parse(stringObj);
console.log(typeof person); //object
console.log(person); //{ name: 'andy' }
```

## 通过JSON格式来保存读取数据

1. 首先通过readFile()读取现有的数据，转化为JSON对象。
2. 通过push()增加一个note。
3. 将JSON对象转化为字符串，然后通过writeFile()写入数据。

notes.js:

```
const fs = require('fs');

console.log('Starting notes.js');

const addNote = (title, body) => {

    fs.readFile('notes.json', (err, data) => {
        if (err) {
            console.log('read file error');
        } else {
            const notes = JSON.parse(data);

            const note = {title, body};

            notes.push(note);

            fs.writeFile('notes.json', JSON.stringify(notes), err => {
                if (err) {
                    console.log('write file error!');
                } else {
                    console.log('add note successfully!')
                }
            });
        }
    });

};

module.exports = {
    addNote
}
```
运行程序，通过参数传递标题和内容：


```
node app.js add --title 'abc' --body 'this is test'
```


然后查看保存的JSON数据notes.json：

    [{"title":"abc","body":"this is test"},{"title":"abc","body":"this is test"}]

注意readFile()和writeFile()都是异步函数，这里通过嵌套的方式来完成。代码可读性不是特别好。

现在JS有很多不同的方式来改进这一点，有Promise，还有async/await，后面可以开专题学习一下。


## 小结
Node有完善的生态，大量的标准和第三方模块可以直接使用，非常方便。所有的I/O都支持通过异步来调用，可以快速高效地响应用户。

## 项目代码
[nodejs-blogs/a03](https://github.com/rangwei/nodejs-blogs/tree/master/a03)

## 参考阅读

- [yargs](https://www.npmjs.com/package/yargs)
- [callback](https://zh.javascript.info/callbacks)
    
