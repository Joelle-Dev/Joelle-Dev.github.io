---
title: Date日期对象
date: 2017-11-17 00:00:00
tags:
  - 笔记
  - 笔记
---

获取当前系统时间并以字符串的方式传给后台，发现了一个toLocaleString方法
之前没有注意过，今天又翻W3上看了看。这样就很棒了。

直接使用

```
let atBefore = new Date();
console.log(atBefore.toLocaleDateString()) // 2017/11/17
```

日期比较

```
let atBefore = new Date('2017,11,17 18:50:20');
//let atBefore = new Date(2017,11,17); 这种写法月份要已加过1的 实则是'2017,12,17'
let today = new Date();

if (atBefore>=today) {
console.log("比当前时间时间大，或者相等");
}
else {
console.log("比当前时间小");
}
```

显示当前时间（带有一个判断数值是否小于 10 的函数）

```
function checkTime(i){
if (i

常用的日期方法

方法

描述

getDate()

返回月份的某一天

getDay()

返回表示星期的某一天的数字

getMonth()

返回表示月份的数字

getFullYear

返回一个表示年份的 4 位数字

getHours()

返回时间的小时字段

getMinutes()

返回时间的分钟字段

getSeconds()

返回时间的秒

getMilliseconds

返回时间的毫秒

getTime

返回距 1970 年 1 月 1 日之间的毫秒数

setDate()

设置一个月的某一天

setMonth()

设置一个月的某一天

setDate()

设置月份

setHours()

设置指定的时间的小时字段

setSeconds()

调整过的日期的毫秒

setMilliseconds

设置指定时间的毫秒字段

setTime()

以毫秒设置 Date 对象

toString()

对象转换为字符串，并返回结果

toLocaleString()

本地时间把 Date 对象转换为字符串

toLocaleTimeString()

本地时间把 Date 对象转换为字符串

toLocaleString()

本地时间把 Date 对象的时间部分转换为字符串，并返回结果

toLocaleDateString

本地时间把 Date 对象的日期部分转换为字符串，并返回结果

w3school：
http://www.w3school.com.cn/jsref/jsref_obj_date.asp

日期比较

显示当前时间（带有一个判断数值是否小于 10 的函数）

常用的日期方法

FEATURED TAGS

笔记

FRIENDS

miumiu