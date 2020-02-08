---
title: "Nodejs学习笔记4 完善删除 读取 列表功能"
date: 2020-02-08T15:39:31+08:00
draft: true
---

# 完善删除、读取、列表功能

*阅读这篇blog大概需要10分钟*

之前通过文件操作，实现了最基本的增加一个Note的功能，下面会继续完善这个小例子，增加删除、读取、列表功能。

- 数组的filter。
- 通过callback来设计函数。

> “People think that computer science is the art of geniuses but the actual reality is the opposite, just many people doing things that build on each other, like a wall of mini stones.”  —Donald Knuth

## 介绍

废话不多说，直接上干货。
今天主要内容包括：

1. 重复标题的记录
2. 删除
3. 读取
4. 列表
5. 功能演示
5. 小结

## 重复标题的记录

之前的代码有个小问题，没有对重复的标题做检查。

可以通过数组的filter方法，来查询传入的新标题是否已经存在，如果不存在的话才会新增一条记录。

代码如下:
```
const duplicateNote = notes.filter(
                note => note.title === title
            );

            if (duplicateNote.length === 0) {
                notes.push(note);
            }

            saveNotes(notes);
```

注意传入filter()的是一个匿名函数，这是闭包的写法，简洁明了。
注意闭包如果只有一个参数的话，可以省略括号。

## 删除

删除记录的逻辑也可以利用filter方法，过滤掉查询记录，然后保存：


```
const filterNotes = notes.filter(
                note => note.title != title
            );
            
            saveNotes(filterNotes);
```

## 读取

对于读取的getNote方法，由于readFile是异步函数。所以在参数里用了一个callback。通过readFile将结果传递给callback：

notes.js:
```
const getNote = (title, callback) => {

    fs.readFile('notes.json', (err, data) => {
        if (err) {
            console.log('read file error');
            callback(err);
        } else {
            const notes = JSON.parse(data);

            const note = notes.filter(
                note => note.title === title
            );
            
            callback(undefined, note);
            
        }
    });
};
```

那么对于调用getNote的话，在app.js中就是这样：


```
notes.getNote(argv.title, (err, result) => {
        if (err) {
            console.log('get note err!');
        } else {
            if (result.length === 1) {
                console.log(result[0]);
            } else {
                console.log(`could not find ${argv.title}!` );
            }
        }
    });
```

这种传递 callback的写法在Node中非常常见。

## 列表
也是通过一个callback获取结果，和刚刚类似：

```
const getAll = (callback) => {
    fs.readFile('notes.json', (err, data) => {
        if (err) {
            console.log('read file error');
            callback(err);
        } else {
            const notes = JSON.parse(data);

            callback(undefined, notes);
            
        }
    });
}
```
## 功能演示

1. 增加：

```
node app.js add --title 'abc3' --body 'this is test'
```
notes.json:

    [{"title":"abc1","body":"this is test"},{"title":"abc2","body":"this is test"},{"title":"abc3","body":"this is test"}]

2. 删除:

```
node app.js remove --title 'abc3' --body 'this is test'
```
notes.json:

    [{"title":"abc1","body":"this is test"},{"title":"abc2","body":"this is test"}]

3. 读取:
```
node app.js read --title 'abc1'

```
输出:

```
Starting notes.js
Starting app
{ title: 'abc1', body: 'this is test' }
```

4. 列表:

```
node app.js list
```

输出:

```
Starting notes.js
Starting app
[
  { title: 'abc1', body: 'this is test' },
  { title: 'abc2', body: 'this is test' }
]
```

## 小结
通过一个非常简单的小例子，通过文件读写JSON对象的方式，完成了对Note的简单增加、删除、查询、列表的功能。

在文件读写的操作中，都是通过了标准异步的方式来完成的。通过这个例子，了解到了闭包、常见的callback使用方式。

## 项目代码
[nodejs-blogs/a04](https://github.com/rangwei/nodejs-blogs/tree/master/a04)

## 参考阅读
- [Understand JavaScript Callback Functions and Use Them](http://javascriptissexy.com/understand-javascript-callback-functions-and-use-them/)
- [Callback Hell](http://callbackhell.com)
- [eloquentjavascript](https://eloquentjavascript.net/)

