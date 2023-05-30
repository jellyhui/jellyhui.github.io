---
title: js面试题
date: 2017-02-26 13:28:29
tags: 
- js 面试
categories:
- 基础知识 面试
---

javascript
#### 例子1
```
var scope=”global”,c=5;
function t(){
console.log(scope);
console.log(c);
var scope=”local”
console.log(scope);
}
t();
```

结果：
undefined
5
local
#### 例子2
```
var kkk=”global”,c=5;
function t(){
console.log(kkk);
console.log(c);
var scope=”local”
console.log(scope);
}
t();
```

结果：
global
5
local

问题1：将例子1的scope改为例子2的kkk后，结果就不一样。作用域是一样的，scope不是关键字也不是保留字，为何会这样？

#### 例子3：
```
var b = 44;
alert(delete b); //false
alert(b); //44

var name = “ddd”;
alert(delete name); //true
alert(name); //出错
```
问题2：例子3中将name,改成其他变量名，就会正常了。
上面3个例子均是在firefox46.0.1得到的结果

解答一
JS在解析一段代码的时候，会先使用var给声明某个变量，然后才执行代码。在例子一当中，你在函数t声明了scope变量，这时scope变量的作用域在函数范围内。所以你在第一次打印scope时值为undefined，直到赋值为”local”后，scope才有值。
MDN var描述
变量声明无论出现在代码的任何位置，都会在任何代码执行之前处理。使用var语句声明的变量的作用域是当前执行位置的上下文：一个函数的内部（声明在函数内）或者全局（声明在函数外）。
解答二
使用var声明的变量，默认的configurable属性是false，所以你是无法删除的，而name是window的一个属性，configurable是true。
MDN delete描述
delete 操作符用来删除一个对象的属性。
在严格模式中，如果属性是一个不可配置（non-configurable）属性，删除时会抛出异常，非严格模式下返回 false。其他情况都返回 true。

```
var scope=”global”,c=5;
function t(){
    console.log(scope);
    console.log(c);
    var scope=”local”
    console.log(scope);
}
t();
```
相当于
```
var scope=”global”,c=5;
function t(){
    var scope;//前置，值默认undefined
    console.log(scope);
    console.log(c);
    scope=”local”
    console.log(scope);
}
t();
```
问题2类似，只是kkk访问的是父作用域的kkk，值当然是global
```
var b = 44;
alert(delete b); //false
alert(b); //44

var name = “ddd”;
alert(delete name); //true
alert(name); //出错
```
相当于
```
var b,name;//name将被忽略，因为window本身有name属性，而且configurable为true
b = 44;
alert(delete b); //false
alert(b); //44

name = “ddd”;
alert(delete name); //true
alert(name); //出错
```
在全局作用域下使用var声明变量，相当于给window对象加属性，并且加的属性的configurable为false，意思就是不能被删除。

