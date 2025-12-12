---
title: git与github
date: 2018-04-19 00:00:00
tags:
  - 笔记
  - 笔记
---

设置作者

```
git config --global user.name "miumiu"
git config --global user emial "mozzie-@hotmail.com"
```

git命令

```
//查看状态
git status
//添加到暂存区
git add index.html
//所有修改的文件提交
git add .
//添加到版本库
git commit
git commit -m "i am 注释"
//工作区直接添加到版本库的简写 -a add -m 注释
git commit -a -m "add script"
//拉取远程分支并创建本地分支
git checkout -b 本地分支名 origin/远程分支名
```

对比

在index.js文件里写入一个名字叫做a的function，
并git commit -a -m “add function”到版本库
紧接着在js里写入var obj = document.getElementById(id);
并 git add .
最后在js里写入obj.onmousedown = function () {}
执行一些命令对比效果如下：

```
//工作区与暂存区的对比
git diff

:![](img/1.jpg)
```

```
//暂存区与版本库之间的对比
git diff --cached
```

```
//工作区与版本库之间的对比
git diff master（分支名字
```

撤销

```

```

删除

```
//删除暂存区
git rm 

//删除暂存区同时删除工作区
git rm -f 

//删除暂存区保留工作区
git rm -canched 

```

设置作者

git命令

对比

撤销

删除

FEATURED TAGS

笔记

FRIENDS

miumiu