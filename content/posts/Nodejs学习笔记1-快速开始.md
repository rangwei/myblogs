---
title: "Nodejs学习笔记1 快速开始"
date: 2020-02-08T15:38:24+08:00
draft: true
---

# 快速开始

*阅读这篇Blog大概需要5分钟*

Node.js 是一个基于 Chrome V8 引擎的 JavaScript 运行环境。 Node.js 使用了一个事件驱动、非阻塞式 I/O 的模型。

对于云应用的快速开发，学习并熟练掌握Node已经是一项全栈开发工程师的必备技能。

我计划用一个月时间快速学习Node.js。这是第一篇学习笔记。主要内容：

- Hello World.

> 天下武功，唯快不破。

## 介绍

这篇笔记主要包含的内容：
1. 安装环境
1. 创建项目
1. 创建并运行程序
2. 小结

## 安装环境

在mac下安装node.js环境非常简单：

```
brew install node
node -v
```
可以看到安装后的node版本，像这样:

```
v12.11.1
```

## 创建项目

快速创建一个node.js项目，通过npm init初始化一个项目，用-y的参数默认全部yes：

```
mkdir a01
cd a01
npm init -y
```

## 创建并运行程序
1. 在当前目录下创建app.js

```
console.log('Hello world!');
```


2. 运行代码
在命令行下输入：

```
node app.js
```

可以看到输出：
```
Hello world!
```

## 小结
创建并运行一个Node app非常快速和简单。

node程序是基于Javascript的，JS是目前最流行的编程语言，学习了解JS的基本语法对学习了解Node有很大的帮助。

## 参考阅读

- [初识Node.js](https://www.cnblogs.com/lixiao0703/p/lixiao03.html)

- [来，告诉你什么是nodejs](https://segmentfault.com/a/1190000019854308?utm_source=tuicool&utm_medium=referral)

- [nodejs是什么？为什么选择它？](https://segmentfault.com/a/1190000019968791)

- [现代JavaScript教程](https://zh.javascript.info/)
    
    
