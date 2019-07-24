---
title: golang 生成随机数种子设置
date: 2019-04-19 11:07:38
tags: [golang,random]
categories: 开发
---

```golang
package main

import (
        "fmt"
        "math/rand"
        "time"
       )

func main() {
i := 0
       for i < 10 {
           i++
               r := rand.New(rand.NewSource(time.Now().UnixNano()))
               fmt.Println(r.Intn(100))
       }
}
```
