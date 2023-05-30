---
title: npm相关知识
date: 2017-03-01 19:22:20
tags: 
- js | 框架 | 打包工具
categories:
- 基础知识
---

#### 1. 简介：    
  含义一：node的开放式模块登记和管理系统
  含义二：node默认的模块管理器，是一个命令行下的软件，用来安装和管理node模块。
         不需要单独安装，安装node的时候会连带一起安装，但可能不是最新版本，可以使用npm install npm@latest -g更新到最新版本。

#### 2.   npm init 
    --初始化生成一个新的package.json文件。可以加参数 -y 或者 -f 表示跳过向用户提问阶段，直接生成一个新的package.json文件。
#### 3.   npm set  xx -- 设置环境变量
#### 4.   npm config xx --- 设置安装目录以及安装的版本信息等
#### 5.   npm info xx -- 查看每个模块具体信息
#### 6.   npm search 搜索词 -- 搜索npm 仓库，后面的搜索词可以是字符串也可以是正则表达式
#### 7.   npm list -- 以树形结构列出当前项目安装的所有模块，以及它们依赖的模块。参数为单个模块名称或者global（全局安装的）
#### 8.   npm install --  
    * install后可以是package name，也可以是github代码库地址；
    * 全局安装 sudo npm install -g （或-global）<package name>；
    * 全局--安装到系统目录中，各个项目都可以调用；本地安装--将一个模块下载到当前项目的node_modules子目录，只有在项目目录中才能调用；
    * 安装总是会先去检查node_modules中是否已存在指定模块，如果存在，即使目前仓库里有最新版本，也不会再安装，可以加上参数 -- force强制重新安装
    * 安装不同版本 在模块名后加上 @版本号（默认安装最新版本）
    * 参数 --save-exact，会在package.json文件中指定安装模块的确切版本
    * -save：模块名将被添加到dependencies，简化为-S
    * -save-dev：模块名将被添加到devDependencies，可以简化为参数-D
#### 9. 避免系统权限：默认安装在系统目录下（/usr/local/lib）,普通用户没有权限，需要sudo命令，因此为了方便，可以在主目录下新建配置文件.npmrc，然后在文件中将prefix变量定义到主目录下： prefix = /home/yourusername/npm，然后在主目录下新建npm子目录：mkdir ~/npm。此后，全局安装的模块都会安装在这个子目录中，最后将这个路径在.bash_profile文件（或者.bashrc文件）中加入PATH变量： export PATH = ~/npm/bin:$PATH.
#### 10. npm update，npm uninstall --更新和卸载
#### 11. npm run-- 执行脚本，在package.json的script字段，可以用于指定脚本对应命令，供npm直接调用。如下图中，可以直接使用npm run lint表示执行eslint，npm run test命令行则表示执行mocha test/ 。      
    *   npm如果不加参数直接运行，会列出package.json中所有可执行的脚本命令。
    *   npm run 会创建一个shell，执行指定的命令，并临时将node_modules/.bin加入PATH变量，这意味着本地模块可以直接运行。
    * 例如npm i eslint --save-dev会产生两个结果：首先eslint会被安装到当前目录的node_modules子目录，其次，node_modules/.bin目录会生成一个符号链接node_modules/.bin/eslint，指向eslint的可执行脚本，然后就可以在package.json的script属性里边，不带路径的引用eslint这个脚本。然后在执行npm run lint时就会自动执行./node_modules/.bin/eslint . 
    * 还可以自己添加参数
12. 
```
"scripts": {
    "lint": "eslint .",
    "test": "mocha test/"
  }
```
13. pre-和post脚本： npm run 为每一条命令都提供了两个pre-和post-两个钩子，也就是说例如npm run lint执行时，npm会先查看有没有定义pre-和post-，如果有则会先执行 npm run prelint  然后执行npm run lint ，最后是npm run postlint。
14. 
```
{
  "dist:modules": "babel ./src --out-dir ./dist-modules",
  "gh-pages": "webpack",
  "gh-pages:deploy": "gh-pages -d gh-pages",
  "prepublish": "npm run dist:modules",
  "postpublish": "npm run gh-pages && npm run gh-pages:deploy"
  }
  ```

在上图中，当执行npm run publish时，会找prepublish，运行npm run dist ： modules，也就是会去执行babel编译。