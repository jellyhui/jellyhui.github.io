---
title: node-blog知识整理
date: 2017-04-05 13:20:10
tags: 
- js | 框架 | 打包工具
categories:
- 基础知识
---

1. node-glob的常用用法说明： 匹配方式与正则类似：
        *  * ： 匹配0或多个除了‘/’以外的字符
        * ？： 匹配单个除了‘/’以外的字符
        *  ** ： 匹配多个字符包括‘/’
        *  {} ： 可以让多个规则用，逗号分隔，起到或者的作用
        *  ！ ：出现在规则的开头，表示取反，即匹配不命中后面规则的文件。
2.  注意的是： fis中的文件都是以/开头的，所以编写规则时，请尽量严格以/开头，
当设置规则时，没有严格的以 / 开头，比如 a.js, 它匹配的是所有目录下面的 a.js, 包括：/a.js、/a/a.js、/a/b/a.js。 如果要严格只命中根目录下面的 /a.js, 请使用 fis.match('/a.js')。
另外 /foo/*.js， 只会命中 /foo 目录下面的所有 js 文件，不包含子目录。 而 /foo/**/*.js 是命中所有子目录以及其子目录下面的所有 js 文件，不包含当前目录下面的 js 文件。 如果需要命中 foo 目录下面以及所有其子目录下面的 js 文件，请使用 /foo/**.js
3. 扩展规则： 
        * 匹配某目录下的所有js文件，fis扩展了node-glob的规则： a/**.js--表示匹配到a目录以及其子目录下的所有js文件。
        * 括号用法扩展： fis.match('a/**.js',{release: ''/b/$1})：让a目录下的所有js发布到b目录下，保留原始文件名。
4. 捕获分组：  
使用 node-glob 捕获的分组，可以用于其他属性的设定，如 release, url, id 等。使用的方式与正则替换类似，我们可以用 $1, $2, $3 来代表相应的捕获分组。其中 $0 代表的是 match 到的整个字符串。
fis.match('/a/(**.js)', {
 release: '/b/$1' // $1 代表 (**.js) 匹配的内容
});
fis.match('/a/(**.js)', {
 release: '/b/$0' // $0 代表 /a/(**.js) 匹配的内容
});

5. 特殊用法：
    1. ::package用来匹配fis的打包过程中；
    2. ::text匹配文本文件；    
默认识别这类后缀的文件。
[
'css', 'tpl', 'js', 'php',
'txt', 'json', 'xml', 'htm',
'text', 'xhtml', 'html', 'md',
'conf', 'po', 'config', 'tmpl',
'coffee', 'less', 'sass', 'jsp',
'scss', 'manifest', 'bak', 'asp',
'tmp', 'haml', 'jade', 'aspx',
'ashx', 'java', 'py', 'c', 'cpp',
'h', 'cshtml', 'asax', 'master',
'ascx', 'cs', 'ftl', 'vm', 'ejs',
'styl', 'jsx', 'handlebars'
]
如果你希望命中的文件类型不在列表中，请通过 fis.set('project.fileType.text') 扩展，多个后缀用 , 分割。
fis.set('project.fileType.text', 'cpp,hhp');
    3.  :: image用来匹配文件类型为图片的文件。 默认识别的后缀为：
默认识别这类后缀的文件。
[
'svg', 'tif', 'tiff', 'wbmp',
'png', 'bmp', 'fax', 'gif',
'ico', 'jfif', 'jpe', 'jpeg',
'jpg', 'woff', 'cur', 'webp',
'swf', 'ttf', 'eot', 'woff2'
]
如果你希望命中的文件类型不在列表中，请通过 fis.set('project.fileType.image') 扩展，多个后缀用 , 分割。
fis.set('project.fileType.image', 'raw,bpg');
    4. *.html:css 用来匹配命中的html文件中内嵌的css部分。fis3 htmllike的文件内嵌的js内容也会走单文件编译流程，默认制作标准化处理，如果想压缩，可以进行如下配置： fis.match('*html:js',{optimizer:fis.plugin('uglify-js')})；
    5. *.html:inline-style用来匹配命中的html文件中的内联样式，可以配置些auto prefix之类的插件。
    6. *.html:scss用来命中html文件中的scss部分。
6. 注意事项：node-glob扩展可能还存在问题，分组() 与{}搭配使用时存在缺陷：如：
/a/({b,c}/**.js) 会拆分成并列的两个规则 /a/(b/**.js) 和 /a/(c/**.js)，当这两个合成一个正则的时候，这个时候问题来了， 一个分组变成了两个分组，分组 1 为 (b/**.js) 分组 2 为 (c/**.js)。那么当希望获取捕获信息时，不能按原来的分组序号去获取了。
// 错误
fis.match('/a/({b,c}/**.js)', {
  release: '/static/$1'
});

// 正确
fis.match('/a/({b,c}/**.js)', {
  release: '/static/$1$2'
});