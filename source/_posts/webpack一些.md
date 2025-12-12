---
title: webpack一些
date: 2017-11-17 00:00:00
tags:
  - 笔记
  - 笔记
---

cnpm init -y

初始化生成package.json

```
{
"name": "webpack-test",
"version": "1.0.0",
"description": "",
"main": "webpack.config.js",
"scripts": {
"test": "echo \"Error: no test specified\" && exit 1",
},
"keywords": [],
"author": "",
"license": "ISC",
"devDependencies": {
"webpack": "^3.8.1"
}
}
```

cnpm i -D webpack  安装webpack

配置dev命令，cnpm run dev 执行打包

```
"scripts": {
"test": "echo \"Error: no test specified\" && exit 1",
"dev":"webpack"
},
```

cnpm i -D html-webpack-plugin 插件

```
//自动生成一个html文件
plugins:[
new htmlWebpackPlugin({
filename:'index.html',
template:'src/index.html'
})
]
```

cnpm i -D babel-loader babel-core

babel:强大的编译器

```

```

cnpm i -D webpack-dev-server 开启服务器

```
devServer:{
open:true,  //自动打开
port:9000   //端口号
}
```

cnpm i -D css-loader 处理css文件

使用css-loader需要安装style-loader 插入到结构里面

```
module:{
rules:[{
test:/\.css$/,
use:['style-loader','css-loader']
}]
},
```

cnpm i -D file-loader

file-loader要做的事：
1.把资源移动到输出目录
2.返回最终引入资源的url

处理图片

```
module:{
rules:[{
test:/\.(png|jpg|gif|jpeg)/,
use:['file-loader']
}]
},
```

引入字体

```
module:{
rules:[{
test:/\.(eot|svg|ttf|woff)/,
use:['file-loader']
}]
},
```

cnpm i -D url-loader

是把图片处理base64位

```
module:{
rules:[{
test:/\.(png|jpg|gif|jpeg)/,
use:[{
loader:'url-loader',
options:{
limit:10000     //大约图片小于10kb对其base64打包
}
}]
}]
},
```

cnpm i -D sass-loader node-sass 处理suss文件

（cnpm i -D less-loader less 处理less文件）
sass-loader插件依赖于node-sass安装

```
module:{
rules:[{
{
test:/\.scss$/,
use:['style-loader','css-loader','sass-loader'] //sass转成css，使用css-loader需要安装style-loader 插入到结构里面
},
}]
},
```

cnpm init -y

cnpm i -D webpack  安装webpack

cnpm i -D html-webpack-plugin 插件

cnpm i -D babel-loader babel-core

cnpm i -D webpack-dev-server 开启服务器

cnpm i -D css-loader 处理css文件

cnpm i -D file-loader

cnpm i -D url-loader

cnpm i -D sass-loader node-sass 处理suss文件

FEATURED TAGS

笔记

FRIENDS

miumiu