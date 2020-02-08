---
title: "Nodejs学习笔记10 将Node应用上云 Heroku"
date: 2020-02-08T16:43:39+08:00
draft: true
---

# 将Node.js应用上云(heroku)


*阅读这篇blog大约需要10分钟*

前面学习了Node.js开发的基本概念。
也了解了最流行的虚拟化技术Docker本地部署的方法。

今天尝试一下把Node REST API服务上云。用一个很简单但是又非常先进的方法，那就是heroku。网址：www.heroku.com

Heroku是一个支持多种编程语言的云平台即服务。在2010年被Salesforce.com收购。Heroku作为最开始的云平台之一，从2007年6月起开发，当时它仅支持Ruby，但后来增加了对Java、Node.js、Scala、Clojure、Python以及（未记录在正式文件上）PHP和Perl的支持。

业界有很多云平台（PaaS），SAP也提供了SAP云平台,可以提升企业级开发创新能力。PaaS为开发者提供了很多便利的服务，屏蔽了IaaS的复杂操作，能够让开发人员最短的时间内专注于业务，进行创新。

选择Heroku，主要是它提供了一些免费的服务，可以把相对简单的服务/app/网站快速上云，然后也可以对比一下它和SAP云平台的差异。

> Good code is its own best documentation. As you’re about to add a comment, ask yourself, “How can I improve the code so that this comment isn’t needed?” --Steve McConnell

主要通过以下简单的几个步骤，就可以快速把服务上云：

1. 准备工作
2. Node.js项目准备
3. 上传到GitHub
4. 安装Heroku命令工具和部署
5. 测试云服务

## 准备工作

- Git

电脑上需要安装git，并熟悉git的基本操作

- Heroku

首先需要注册一个账号。推荐通过Gmail注册即可，立即生效。

## Node.js项目准备
借用上一个项目的代码。很简单的通过JSON返回一个用户的数据。

稍微做一下小小的调整：修改端口号的代码。

server.js:
```
const data = require('./data');
const express = require('express');

const PORT = process.env.PORT || 3000;

const app = express();

app.get('/user', (req, res) => {
    res.send(data);
});

app.listen(PORT);
```
端口号码使用了环境变量，运行的时候就是云服务器上的端口号。

## 上传到github

在github上创建一个项目，然后上传：
```
echo "# heroku01" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/rangwei/heroku01.git
git push -u origin master
```

## 安装Heroku命令工具和部署

1. 在Mac下安装heroku命令工具：
```
brew install heroku/brew/heroku
```
2. 登录heroku：

```
heroku login
```
3. 创建一个应用:

```
heroku create
```
很快就可以看到创建完毕，输出信息包含了app名称、应用地址以及代码地址：

```
Creating app... done, ⬢ damp-temple-40624
https://damp-temple-40624.herokuapp.com/ | https://git.heroku.com/damp-temple-40624.git
```
4. 同步代码到heroku:

```
git push https://git.heroku.com/damp-temple-40624.git
```
然后就是自动上传代码编译运行，输出如下：

```
Enumerating objects: 8, done.
Counting objects: 100% (8/8), done.
Delta compression using up to 8 threads
Compressing objects: 100% (6/6), done.
Writing objects: 100% (8/8), 5.16 KiB | 5.16 MiB/s, done.
Total 8 (delta 0), reused 0 (delta 0)
remote: Compressing source files... done.
remote: Building source:
remote: 
remote: -----> Node.js app detected
remote:        
remote: -----> Creating runtime environment
remote:        
remote:        NPM_CONFIG_LOGLEVEL=error
remote:        NODE_ENV=production
remote:        NODE_MODULES_CACHE=true
remote:        NODE_VERBOSE=false
remote:        
remote: -----> Installing binaries
remote:        engines.node (package.json):  unspecified
remote:        engines.npm (package.json):   unspecified (use default)
remote:        
remote:        Resolving node version 12.x...
remote:        Downloading and installing node 12.15.0...
remote:        Using default npm version: 6.13.4
remote:        
remote: -----> Installing dependencies
remote:        Installing node modules (package.json + package-lock)
remote:        added 50 packages from 37 contributors and audited 126 packages in 1.68s
remote:        found 0 vulnerabilities
remote:        
remote:        
remote: -----> Build
remote:        
remote: -----> Caching build
remote:        - node_modules
remote:        
remote: -----> Pruning devDependencies
remote:        audited 126 packages in 0.98s
remote:        found 0 vulnerabilities
remote:        
remote:        
remote: -----> Build succeeded!
remote: -----> Discovering process types
remote:        Procfile declares types     -> (none)
remote:        Default types for buildpack -> web
remote: 
remote: -----> Compressing...
remote:        Done: 22.4M
remote: -----> Launching...
remote:        Released v3
remote:        https://damp-temple-40624.herokuapp.com/ deployed to Heroku
remote: 
remote: Verifying deploy... done.
To https://git.heroku.com/damp-temple-40624.git
 * [new branch]      master -> master
```

## 测试云服务

可以通过命令打开网址：
```
heroku open -a damp-temple-40624
```
打开浏览器，这个Node.js服务在短短几时间里就上云了：

https://damp-temple-40624.herokuapp.com/user

## 小结

Heroku是一个很不错的paas服务，除了Node.js也支持其它的技术栈，大家有空也可以尝试一下。

## 项目代码
- [nodejs-blogs/a10](https://github.com/rangwei/nodejs-blogs/tree/master/a10)
 
## 参考阅读

https://devcenter.heroku.com/articles/getting-started-with-nodejs

    
    
