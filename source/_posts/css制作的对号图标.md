---
title: css制作的「对号」图标
date: 2018-04-23 00:00:00
tags:
  - css
  - css
---

抛砖引玉
之前看到网上一些帖子用css制作的图标制作的类似于「打勾」☑️的效果
是用我们视觉看到的那样写出，比如撇捺，进行旋转就组成对号了。
至少要写两个旋转
transform
。
而今天的这种方法放在伪元素
border
当作「对号」的厚度；
宽高就是「对号」的撇捺；
愉快的写各种「对号」再也不用去切图了;

html

```

```

css

```
.input{
width: 40px;
height: 40px;
border-radius: 50%;
position: relative;
border: 1px solid #dcdfe6;
display: inline-block;
}
.check{
background: #ff7848;
border-color: #ff7848;
}
.check::after{
content: '';
display: block;
border: 1px solid #fff;
position: absolute;
border-top: 0;
border-left: 0;
height: 22px;
left: 11px;
width: 14px;
top: 2px;
transform: rotate(45deg) scaleY(1);
```

效果

http://runjs.cn/code/jdotyefi

html

css

效果

FEATURED TAGS

css

FRIENDS

miumiu