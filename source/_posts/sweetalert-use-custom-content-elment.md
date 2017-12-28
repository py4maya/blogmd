---
title: SweetAlert 使用 自定义弹出框内容
date: 2017-12-26 19:42:05
tags: [SweetAlert]
categories: 开发
---

[说明地址:(https://sweetalert.js.org/guides/#installation)](https://sweetalert.js.org/guides/#installation)
wget  [https://unpkg.com/sweetalert/dist/sweetalert.min.js](https://unpkg.com/sweetalert/dist/sweetalert.min.js)

自定义内容方法:

```javascript
     swal({
       content: {
         element: "div",
         attributes: {
             innerHTML:"<b>ONE</b>"
         }
       }
     });
```
