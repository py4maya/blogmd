---
title: golang 使用flag 传递参数
date: 2018-07-02 14:59:45
tags: [golang,flag]
categories: 开发
---

```golang
package main

import (
	"flag"
	"fmt"
	"os"
)

func main() {
	ok := flag.Bool("ok", false, "is ok")
	flag.CommandLine.Parse(os.Args[2:])
	action := os.Args[1]
	fmt.Println("Action: ", action)
	fmt.Println(*ok)

}
```

## 运行参数:
```golang
$ ./flag <action> --ok
```

## 传递参数的方法:
```golang
./flag <action> -ok
./flag <action> --ok
./flag <action> --ok=true
```
