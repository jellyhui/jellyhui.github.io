---
title: widget学习笔记
date: 2017-02-06 23:20:09
tags: 
- js | 框架 | 打包工具
categories:
- 基础知识
---

### 一、 原理：
 现在webapp已经全面升级为单页系统，单页系统因为是局部刷新页面，在切页时仍会保留前一个页面的部分状态，比如声明的变量，在某些元素上绑定的事件，导致相关内存不能自动回收，存在内存泄露。
1. widget组件中，el指向是切片的单位，也就是说，每个widget组件，只作用于该区域：---可以使用事件代理机制，更小范围的寻找dom元素（不用满document选择元素了）； 分配模块管理，便于重用和维护。
2. widget通过事件代理机制配置的events，充当了controller的角色，在view里触发各式各样的事件时，触发controller的代理方法，在处理方法中，又可以修改view的HTML。
3. 由于大规模的拼接HTML会造成各种维护以及效率问题，所以引入了BaiduTemplate。
4. widget中并没有单独分离出model层，而是将所有的逻辑交给controller，灵活性很高。

### 二、 解决方案
1. 规范widget实现：提供一个windows.Widget基类，windows.widget基类有一个静态方法extend，所有widget均由extend方法继承自该基类，extend方法返回的是widget的一个子类构造函数，基类中统一进行事件委托，在widget中提供统一的事件绑定方式。
2. 在widget中提供对页面加载各个节点的统一接口，都是页面加载的节点。
3. 切页面时回收内存： 切页时解除各引用关系，解绑各事件处理函数，使内存能自动回收。
4. 对外提供destroy接口，各人实现自己widget时可以重写destroy接口，实现切页时要做的操作。

### 三、 API
1. extend： windows.widget的静态方法，接受一个对象作为参数，返回值是window.Widget的子类，extend接受的参数对象说明： 
2.   参数对象属性以及属性值说明：    
     1.  el：属性值为字符串，包含该组件的容器，可以是selector---组件所有事情都会委托在该元素上，若不写，会委托在body上，但是建议写，一般是写class或者id值或其他selector带上widget名，避免跟同页面其他widget冲突。如策略监控系统的两个大id为widget-policy-find何widget-policy-alarm，分别对应两个文件夹命名为policyFind和policyAlarm。
     2.  init：属性值为函数,初始化操作，会再createWidget()方法内部调用，可接受参数，参数即为createWidget()方法接收的参数。
     3.  events： 属性值为对象，浏览器事件统一绑定，对象的每个属性值是匿名函数或者函数名的字符串，注意：函数名外引号不要去掉，格式如下：
        ```
            events： {
                "click[data-node='search'": 'handleForm',
                "submit[data-action=create_account]":'createAccount'
            }
        ```

     4 .  channels:属性值为对象
```
    channels: {
        'common.page pagexxx': function () {},
        'loading show': loadingHandler ---调用方法名
    }
```
     补充说明： loadingHandler是一个方法名，在此处调用
5.     destroy： 属性值为函数，切页时要做的操作可以在destroy总实现，会在切页时由基类window.Widget自动调用，预留Api，提供了一些方法： 
    1. _bindCustomerEvent: 绑定广播事件：
        * param {string} eventName--事件触发的频道，如common.page；
        * param {string} selector 自定义事件名称，如get和show等
        * param {funciton} method --事件处理函数
        * _unbindBrowserEvent:解绑浏览器事件
        * _io--封装ajax请求操作，通过Deferred对象的pass和reject两种状态作为处理ajax成功和失败的操作入口。
6. extend方法返回值说明： 
    1. 类型： window.Widget的子类，有静态方法widgetcreateWidget，接受一个任意类型的参数，该参数会作为实参传给extend参数对象的init方法。
    2. 总结：调用window.Widget.extend得到widget构造函数，调该构建函数的createWidget方法实例化widget。
7. 实例： 
    1. 先依赖： 
        ```在fengkong的fe_front_qishan中，依赖入口在page下的antispampalt中：所有页面扩展自layout.tpl。{%require name=“id名： 地址”%}；```
    2. 实例中：
    ```
          module.export = Widget.extend （{
                el: "#大的id名";
                events:{ "click[data-node=xxx]: "ssss"} ;
                channels: { } ;
                init: 
                urls: 
           }）
   ```
8.     解析： 
    1. 在init中，拿到$el(根据配置中的选择器)；
    2. 解析DOM，将$el下拥有data-node的元素节点保存在me.$dom下，方便以后引用；
    3. 如果配置中存在init函数，立即调用，传入```pageName( Widget.extend().createWidget({a:'b',c:'dddd'}) )```，pageName实际为tpl配置的数据；
    4. 执行事件绑定：检测是否存在events配置项，用jQuery事件代替进行逐个绑定；
    5. 执行事件总线绑定：检测是否存在channels存在，用listener进行逐个处理；
    6. 最后绑定事件总线common.page，如果执行到，执行销毁，也就说：如果想要将一个组件的所有事件和事件总线进行解绑，就listener.trigger(' common.page ' , ' pagexxxx ');
          

### 四、 结合当前策略监控平台绑定部分（一）： 
     前三条为总结现有代码以及查阅相关资料所整理，本条结合实例：

关于widget_data传递数据的过程：其实就是：tpl是一种后端模板，后端会将其处理转换成对应的前端的html相关。
```
{%script%}
    window.widgetData = {%json_encode($widget_data)%};
    require('./App.es6');
```

其实就是等于

```
<!-- require('./App.es6')({%json_encode($widget_data)%}); -->
```
分解意思，在require引用es6文件时，将其看作一个函数，后边的( )表示函数立即执行，括号内的参数是解析的$widegt_data，后端在解析时（不管$widget_data）在什么地方，只要碰到了，在解析的时候就能转为前端可以处理的函数，使用最多的 data = $widget_data，也是一个意思，后端会解析$widget_data为对应的格式。

在vue中挂载：
```
new Vue({
    data: {
        bus,
        tplData //--  也就是widget_data，可以像平常一样挂载在window上，也可以挂载在vue的初始化实例时。
    },
    router
}).$mount('.page-main');
```
然后在子组件中调用方法： 可以参考下图的调用方法

![](/images/widget.png)

<!-- {% asset_img widget.png widget%} -->
 说明：尝试后发现此方法不可取，具体原因还会再进一步分析，但发现绑定在window上之后，可以定义为const变量类型，然后在组件中直接访问变量。


































































