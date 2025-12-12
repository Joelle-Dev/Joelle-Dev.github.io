---
title: echarts关于tooltip自定义修改的问题
date: 2018-04-19 00:00:00
tags:
  - 所想
  - 所想
---

背景需求

折线图–
tooltip
的title接口动态数据输出

```
//接口数据
let arr=[
{
d:'2018-04-19'，n:'降水量',v:'1'
},
{
d:'2018-04-24'，n:'降水量',v:'2'
},
{
d:'2018-04-28'，n:'降水量',v:'3'
}
]

//tooltip提示层=>params.name
2018-04-19
降水量 1

//期望
04/19-4/24
降水量 1
```

过程

查找文档和网上资料一般是对
tooltip

从x轴返回的
params
拼接一些符号️操作的自定义改装

实现

费尽心机的想从x轴的数据添加自定义的参数字段希望
formatter能够接收，但无功而返。
params始终是从x轴拿数据的

```
//曲线救国方法series （用于tooltip的显示）
series: [
{
data: (function(){
//arr.d处理 return出去
//tooltip.formatter进行样式修改即可
})(),
type: 'line'
},
{
data: [1,2,3],
name:'降水量',
type: 'line'
}
]
```

文档

http://echarts.baidu.com/option.html#tooltip.formatter

http://echarts.baidu.com/option.html#series-line

背景需求

过程

实现

文档

FEATURED TAGS

所想

FRIENDS

miumiu