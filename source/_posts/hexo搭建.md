---
title: 使用hexo搭建个人博客
date: 2017-01-26 16:20:08
tags: 
- js
categories:
- 基础知识
---

### 使用hexo搭建github pages
前言：
@(个人博客)[github-pages|hexo|博客]
以前使用印象笔记或者博客园来写一些学习笔记，一直都没能上传到自己的个人仓库里，现在开始慢慢将原来所做所学内容放在个人github博客中，温故而知新。

#### github pages的注册
一. 首先，你得有一个github的账号，这个注册过程就不详细描述了，有账号，然后建立与自己账号名相同的仓库名： yourname/yourname.github.io。

##### 本地安装环境
二. 本地安装环境（以windows为例）：

1. git安装： 去官网下载git工具，基本上一路next，也可根据自己需要是否安装其他以及快捷方式等等选择，git常用操作git add * --> git commit -m '提交备注’； 
2. node环境： 去[node官网](https://nodejs.org/en/) 下载node，安装node环境。
    > ![node官网目前显示版本](../images/node.png) 最好使用稳定版本6.x，最新的不一定稳定哈，注意检查环境变量(一般情况下node安装过程中会自动将安装路径添加到环境变量中)，若因不知名原因无，在环境变量的path的值中手动加入node的安装目录。
3. npm、cnpm： 现在的node包下载之后，已经集成了npm，所以无需再做别的操作，因npm链接国外不稳定，可以设置淘宝镜像cnpm： `$ npm install -g cnpm --registry=https://registry.npm.taobao.org ` 
----详细信息可自行查询[官网链接](https://npm.taobao.org/)。
4. 使用npm install -g hexo全局安装hexo 或者 npm install hexo --save。
目前我个人安装环境版本如图：版本

#### SSH的生成
三. 注意github使用过程中，别忘记生成ssh！

$ git config --global user.name "jellyhui"
$ git config --global user.email xx@example.com
$ssh-keygen -t rsa -C 'xxx@example.com'
输入命令回车之后会提示你输入一些东西，不用管。一直回车到底就好了。然后你的c盘下的user中找到 ~/.ssh 文件下就会生成两个文件 id_rsa 和 id_rsa.pub 。然后点击个人头像去个人信息的settings中找到 SSH and GPG keys， title主要是为了方便自己识别是哪台PC，在SSH一栏，复制id_rsa.pub内的内容即可。

#### deploy到repo
四. 当环境均安装就绪后，就可以尽情使用hexo啦！
主要过程分为三步：
1> . 创建github的repository； new repository -不详细说，注意repository的名字必须与用户名一样即可。
2>. 使用hexo生成pages

    1. hexo init  <文件夹名> : hexo生成文件夹并初始化；
    2. cd <文件夹> 并且  npm install： 安装所需环境配置；新建完成后，文件夹下的目录如下：
            .
            ├── _config.yml---网站的配置文件，可以在其中配置网站的大部分参数
            ├── package.json ---应用程序的信息
            ├── scaffolds ---资源文件夹，是用来存放用户资源的地方
            ├── source ---资源文件夹，是用来存放用户资源的地方
            |   ├── _drafts
            |   └── _posts
            └── themes ---主题文件夹，Hexo会根据主题来生成不同的静态页面

    3. 生成之后，主要配置_config.yml文件，常见配置等说明后续再进一步说明。目前最需要的是 
        title是博客标题，author为作者名
     `deploy:
      type: git 
      repo: git@github.com/yourname:yourname.github.io.git或者https://github.com/yourname/yourname.github.io.git
      branch: master`
    4.   在source文件夹下，有_posts文件夹，存放用户自己的md文件，也就是需要上传的文件，需要注意的是，自己的md文件中开头需要用

```
        ---
            title: md文件的名字
            date: 书写时间
            tags: [mysql,php,redis]---标签的名字s
            categories: [mysql] ---分类的名字
        ---

```来表明这篇文章应该所属的分类以及标签【注意：距离最左边不能有空格】。
5.  >  hexo g ---生成静态页面，生成的内容在public文件夹下
    >  hexo s ---启动本地服务localhos://127.0.0.1:4000，进行文章预览调试；
    >  hexo s --debug 命令可以用来调试
    >  hexo d 推送代码到自己在config.yml中配置的repo上。
3>. 推送代码：

1. hexo deploy ：推送对象是在_config.yml中配置的repo对象（一般是SSH对象）；
2. 推送时若提示git not found之类的错误，注意检查是否安装插件： $`npm install hexo-deployer-git --save`；
3. 打开 yourname.github.io就可以看到在效果啦~~
4. 后续推送代码时，记得先clone仓库到本地，然后再编辑更新哟~~