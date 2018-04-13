---
title: 最简单的ES6开发编译环境
date: 2018-01-11 20:53:47
tags: [ES6,node,npm]
categories: 开发
---

* 安装全局的babel-cli,用来转换ES6代码

```shell
$ npm install -g babel-cli
```

* 初始化你的项目目录:
```shell
$ mkdir sample && cd sample
$ mkdir src
$ mkdir dist
$ npm init -y # 一路初始化自己的项目
```

* 创建静态文件:
```shell
$ cat index.html
<!DOCTYPE html>
<html lang="en">
    <head>
        <title></title>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <script src="./dist/index.js"></script>
    </head>
    <body>
        Hello ECMA Script 6
		<script src="./dist/index.js"></script>
    </body>
</html>
```

* npm 安装转换器，及依赖
```shell
$ npm install --save-dev babel-preset-es2015 babel-cli
```

* 创建babel 使用配置
```shell
$ cat .babelrc

{
    "presets":[
        "es2015"
    ],
    "plugins":[]
}
```

* package.json 中添加 转换命令:

```shell
....
    "build": "babel src -d dist"
....
```

*  一个完整版的package.json 如下,理论上说package.json + .babelrc 就可以配置基础的环境了:

```shell
{
  "name": "xes6",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "babel src -d dist --watch"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "babel-cli": "^6.26.0",
    "babel-preset-es2015": "^6.24.1"
  }
}
```

* 试一下吧：
```
npm run build
```
