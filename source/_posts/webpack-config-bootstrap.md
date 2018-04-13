---
title: webpack中打包bootstrap css 资源
date: 2018-02-20 10:44:14
tags: [webpack,bootstrap]
categories: [html]
---

使用npm安装bootstrap 

```shell
$ npm install --save bootstrap
```

webpack 入口文件中配置:
```shell
$ import 'bootstrap/dist/css/bootstrap.css'
```

webpack 打包文件中配置:

```javascript

....

loaders: [
    { test: /\.css$/, loader: 'style-loader!css-loader' },
    { test: /\.eot(\?v=\d+\.\d+\.\d+)?$/, loader: "file" },
    { test: /\.(woff|woff2)$/, loader:"url?prefix=font/&limit=5000" },
    { test: /\.ttf(\?v=\d+\.\d+\.\d+)?$/, loader: "url?limit=10000&mimetype=application/octet-stream" },
    { test: /\.svg(\?v=\d+\.\d+\.\d+)?$/, loader: "url?limit=10000&mimetype=image/svg+xml" }
]

....



```
