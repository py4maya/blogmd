---
title: 关于javascript this 的几点讨论
date: 2017-12-16 21:31:25
tags: [javascript,this]
categories: 开发
---

javascript 函数中调用的this，一般去寻找对象本身， .语法之前的对象，如果没发现，那么this 即为全局对象。

几个例子如下:

```javascript

function fn(a,b){
	console.log(this,a,b);
}

fn(1,2); // this 输出的值为window

var a = {};
a.method = fn;
a.method(1,2); // this 输出的值为 a 

```

另外，必要的情况下，我们需要改变this的绑定关系,这时候需要函数的call或者apply方法:
```javascript
function fn(a,b){
	console.log(this,a,b);
}

var this_obj = {};
fn.call(this_obj,1,2); // 输出的this,即为this_obj
fn.apply(this_obj,[1,2]); //区别于call，参数可以传递为一个数组

```

创建对象自身的时候包含原型链,可以动态扩展对象的行为,即查找对象属性时，如果自身不存在，会转而查找他的上一层。
并且，上层动态创建的属性，依然会反映到新创建的对象中:
```javascript 
var info = {
	hi:'hi this is world'
}

var ninfo = Object.create(info); //创建原型链关联关系
console.log(ninfo.hi);			//可以获取到数据 "hi this is world"
info.hello = 'hello world';		//扩展原来的对象
console.log(ninfo.hello);		//依然可以获得hello的属性
```

创建对象时，顺路包含相关扩展:
```javascript

//扩展对象的行为包含move方法
var makeMove = function(base){

    var me = Object.create(base);

    me.move = function(){
        me.location++;
        if(me.location >= 30) {
            me.location = 1;
        }
        console.log(me.location);
    }

    me.loop = function(){
        me.move();
    }

    return me;

}
```

最初始的扩展方式等同于构造函数，javascript中构建类的一个基本方式如下:
```javascript

//不调用new的方式
function C(info){
	var o = Object.create(C.prototype);	
	o.info = info;
	return o;
}

C.prototype = {
	hello:function(){
		console.log(this.info);	
	}
};

var c = C("hello");
c.hello()

```
伪类模式，javascript 引入new的关键字，调用的函数直接为构造函数，内部会初始化一个this，并且会自动实现原型链的关联,并且会自动的把this返回，作为对象实例:
```javascript
//简单的实现方式
function C(info){
	this.info = info;
}

C.prototype = {
	hello:function(){
		console.log(this.info);	
	}
}

```

