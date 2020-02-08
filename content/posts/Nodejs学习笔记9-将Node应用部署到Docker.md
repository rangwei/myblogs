---
title: "Nodejs学习笔记9 将Node应用部署到Docker"
date: 2020-02-08T16:42:57+08:00
draft: true
---

# 将Node应用部署到Docker

*阅读这篇blog大约需要10分钟*

Docker 是一个开源的应用容器引擎，基于 Go 语言 并遵从 Apache2.0 协议开源。

Docker 可以让开发者打包他们的应用以及依赖包到一个轻量级、可移植的容器中，然后发布到任何流行的 Linux 机器上，也可以实现虚拟化。

Docker的优点:
- 快速一致地交付应用。
- 响应式部署和扩展。
- 在同一硬件上运行更多工作负载。

> Good code is short, simple, and symmetrical—the challenge is figuring out how to get there. --Sean Parent

## 介绍

Node.js本身就是一个快速构建应用的环境，和Docker可以完美地搭配。主要内容包括两部分：

1. 通过Express创建一个简单的REST应用
2. 将服务部署到Docker

## 通过Express创建一个简单的REST应用

1. 准备
```
mkdir a09

npm init -y

npm install express --save
```
创建一个data.json:

    {"name": "Hello", "likes": ["wolrd", "node"]}

2. 服务器代码

server.js

```
const data = require('./data');
const express = require('express');

const PORT = 3000;
const HOST = '0.0.0.0';

const app = express();

app.get('/user', (req, res) => {
    res.send(data);
});

app.listen(PORT, HOST);

console.log(`Running on http://${HOST}:${PORT}`);
```

3. 测试:
运行程序，通过浏览器访问：
```
http://localhost:3000/user
```
可以查看到返回的JSON数据。


## 将服务部署到Docker

1. 构建Dockerfile

新建一个Dockerfile:


```
FROM node:latest

# Create app directory
WORKDIR /usr/src/app

# Install app dependencies
# A wildcard is used to ensure both package.json AND package-lock.json are copied
# where available (npm@5+)
COPY package*.json ./

RUN npm install
# If you are building your code for production
# RUN npm ci --only=production

# Bundle app source
COPY . .

EXPOSE 3000
CMD [ "node", "server.js" ]
```

2. 新建一个.dockerignore文件：

```
node_modules
npm-debug.log
```


3. 构建Docker镜像

通过docker build来构建镜像：
```
docker build -t <your username>/<your imagename> .
```
在命令行输入：
```
docker build -t rang/a09 .
```
输出：

```
Sending build context to Docker daemon  20.99kB
Step 1/7 : FROM node:latest
 ---> 2f1ff44a8bb5
Step 2/7 : WORKDIR /usr/src/app
 ---> Using cache
 ---> e57bfbf7a84e
Step 3/7 : COPY package*.json ./
 ---> a820cfd03931
Step 4/7 : RUN npm install
 ---> Running in 50a219d625ad
npm WARN exp01@1.0.0 No description
npm WARN exp01@1.0.0 No repository field.

added 50 packages from 37 contributors and audited 126 packages in 97.618s
found 0 vulnerabilities

Removing intermediate container 50a219d625ad
 ---> 419121cda361
Step 5/7 : COPY . .
 ---> 16db3371aff5
Step 6/7 : EXPOSE 3000
 ---> Running in 05b89cf2b4ff
Removing intermediate container 05b89cf2b4ff
 ---> 96359e1075c8
Step 7/7 : CMD [ "node", "server.js" ]
 ---> Running in 95cb9f195d76
Removing intermediate container 95cb9f195d76
 ---> bcf82dfe8b41
Successfully built bcf82dfe8b41
Successfully tagged rang/a09:latest
```
大概几秒钟就构建完毕了。

4. 运行容器：

```
docker run -p 3000:3000 -d rang/a09
```
容器也是秒级启动。

5. 测试：

通过浏览器可以访问到服务：

```
http://localhost:3000/user
```

## 小结

可以看到很快地开发了一个REST API，然后通过Docker本地部署。有了Node.js和Docker这两大武器以后，就可以超级快速的开发部署云应用。

## 项目代码
- [nodejs-blogs/a09](https://github.com/rangwei/nodejs-blogs/tree/master/a09)

## 参考阅读

https://hub.docker.com/_/node/

https://nodejs.org/en/docs/guides/nodejs-docker-webapp/

https://www.runoob.com/docker/docker-tutorial.html
