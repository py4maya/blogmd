---
title: golang 编写测试用例 
date: 2019-03-06 10:22:11
tags: [golang,test]
categories: 开发
---

golang 写测试用例，非常简单。如下几步即可:

* 普通的包，文件命名必须以 test.go 结尾
* 包内必须包含 指定的包，指定的方式定义函数用例:

```golang
import "testing"

func TestHello(t *testing.T) {

}

```

* 打印日志,通过或者失败:

```golang

t.Logf("Hello Log Message %s ","<something>")
t.Errorf("One Error Happend %s","<badthing>") // 只要出现Error 那么就认为该测试用例失败

```

* 运行测试:

```shell
$ go test  # 默认只包含测试不通过的用例，但是成功的也回执行,如果想看所有的日志，那么请用 go test -v 

➜  info go test
--- FAIL: TestArraySlice (0.00s)
	info_test.go:28: 2 7
	info_test.go:29: 3 5
	info_test.go:30: End!
FAIL
exit status 1
FAIL	info	0.002s
```

go test 会执行当前包内定义的所有的以 Test 开头的方法定义

* 把go test 编译成一个可执行文件(不执行)

可以使用这个命令生成可以灵活扩展的多入口程序

```shell
$ go test -c 
```

* 如下是一个编写好的简单测试用例文件:
```golang

package info

import "testing"

func TestSomething(t *testing.T) {
	t.Log("Hello test something!")
}

func TestGoAround(t *testing.T) {
	for i := 1; i < 10; i++ {
		t.Log(i)
	}
}

func TestSlice(t *testing.T) {
	var s []int
	for i := 0; i < 12; i++ {
		s = append(s, i+3)
		t.Log(len(s), cap(s))
	}
}

func TestArraySlice(t *testing.T) {
	var items = [...]int{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}
	var a = items[3:5]
	var b = items[5:8]

	t.Log(len(a), cap(a))
	t.Log(len(b), cap(b))
	t.Errorf("End!")
}


```
