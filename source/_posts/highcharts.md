---
title: highcharts 一些小点
date: 2018-04-11 19:30:28
tags: 
- vue
categories:
- vue
---


1. 关于highcharts： 由纯JavaScript编写的Html5图表库，全部源码开放，个人以及非商业用途可以任意使用及源代码编辑。
2. highcharts系列软件包含highchairs JS/ highstock JS/ highmaps JS 共三款软件。
    1. highcharts是纯JavaScript编写的图表库；
    2. highstock是纯JavaScript编写的股票图标控件，可以开发股票走势或大数据量的时间轴突变，包含多个高级导航组件： 预设置数据时间范围，日期选择器，滚动条，平移，缩放功能。
    3. highmaps是基于HTML5的优秀地图组件。继承了highcharts简单易用的特性，利用它可以方便快捷的创建用于展现销售/选举结果等其他与地理位置关系密切的交互性地图图表。
3. 可以在js文件中引入官方提供的cdn服务，也可以从官方下载资源包引入使用，也可以与vue等结合使用，npm安装，在main.js文件中import或者require后，直接使用。具体vue使用操作步骤： 
    1. npm install highcharts -（参数自己看情况选择，-g全局，—save上线后依赖，--save-dev仅在开发时依赖）；
    2. 在需要的文件中import Highcharts from ‘highcharts’引入， 就可以在mounted时 使用 Highcharts.chart（图表唯一的id名， 图表的option）；
    3. option可以在上述文件中写入，也可以创建option组件，专门export 需要编辑的option之类。然后在父组件中引用调用。
4. 注意： highstock是完全包含了highcharts的，所以没必要完全将两个文件都引入；highmaps 可以单独使用也可以通过地图模块来引用，如果只需要 highmaps 功能，那么只需要引入 highmaps.js 即可。
5. 图表组成：标题 title， 坐标轴axis， 数据列series， 数据提示框tooltip，图例legend， 版权标签credits等。另外还可以包含导出功能按钮exporting， 标识线 plotLines， 标示区域 plotBands，数据标签 dataLabels。
    1. highcharts实例化中绑定容器的方式有很多种方式： 
        1. 通过构造函数： Highcharts.chart（‘id名字’， { 配置 }）；
        2. 通过chart.renderTo来指定： Highcharts.chart（  { chart : {  renderTo： ‘id名 ’} }  ）；
        3. 如果页面已经引入了jquery，还可以用jquery插件的形式调用 $(‘id 名’).highcharts( {  配置} )；
    2. 设置容器的min- width可以让highcharts自适应宽度





















































