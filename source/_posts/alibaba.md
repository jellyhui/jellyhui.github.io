---
title: 阿里巴巴前端面试
date: 2016-06-21 15:30:02
tags: 
- 面试
categories:
- 面试
---

alibaba前端面试

前端面试题
1、对json格式中的某一字段进行排序
eg：var stuJson = [{ name: "daming", age: 21, weight: 66, sex:"boy" },
                   { name: "lisa", age: 19, weight: 45, sex:"girl" },
                   { name: "lili", age: 20, weight: 50, sex:"boy"}];
     //eg:按age升序
   stuJson.sort(function(a,b){
                    return a.age - b.age;
  });
console.log(stuJson);//[ { name: 'lisa', age: 19, weight: 45, sex: 'girl' },
                     //{ name: 'lili', age: 20, weight: 50, sex: 'boy' },
                     //{ name: 'daming', age: 21, weight: 66, sex: 'boy' } ]
2、用一句语句对数组去最小或最大值
eg:var arr = [32,42,12,42,21,23,56,75,3,33,53,23,36];
   var aMin = Math.min.apply(null,arr);
   console.log(min);//3
   var aMax = Math.max.apply(null,arr);//75
   console.log(aMax);
3、如何获取冒泡事件的根节点
4、前端mvc，后端mvc
5、body里面有个div，将div宽度设成100%，并设置padding为10px，问浏览器会不会有滚动条
6、页面布局为左边一个200px宽的块，右边有个200px的块，中间的块宽度设成100%，问有哪些实现方法。
7、在服务器端设置缓存，当访问过一次某个数据后，第二次访问的时候不查询数据库，问怎么实现
8、事件委托