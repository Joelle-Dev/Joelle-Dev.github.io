---
title: es6一些
date: 2017-11-21 00:00:00
tags:
  - 笔记
  - 笔记
---

let

1.使用let声明的变量，只在命令所在的代码块内以有效；
2.使用let命令声明的变量在域解析的时候不会被提升；
3.let不允许在同一个作用域下声明已经存在的变量。

```
console.log(a) //a is not defined
let a = 1
```

```
console.log(a) //undefined
var a = 1
```

```
//在循环语句之内是一个父作用域，在循环体之中是一个子作用域
for(let i=0;i

const

1.使用let声明的变量，只在命令所在的代码块内以有效
2.使用let命令声明的变量在域解析的时候不会被提升。
3.let不允许在同一个作用域下声明已经存在的变量。
4.声明的时候必须赋值。
5.储存简单的数据类型不可改变其值，储存的是对象，对象引用不可以被改变，对象里面的数据如何变化，是没有关系的。

```
const a = 1;
a = 2; //报错

const obj = {a:10};
obj.a = 20;
console.log(obj) //{a:20}
```

变量的结构赋值

本质上是一种匹配模式，只要等号两边的模式相同，那么左边的变量就可以被赋予对应的值。

```
let[a,b,c] = [1,2,3];
console.log(a,b,c) //1 2 3
```

主要分为：
1.数组的结构赋值

```
let [a,[[b],c]] = [1,[[2],3]]
console.log(a,b,c) //1 2 3

let [,,c] =[1,2,3]
console.log(c) //3

let [x] = [];
console.log(x) //undefined (结构不成功

let [x =1 ] = []
console.log(x)  //1 (执行默认值
```

2.对象的结构赋值

```
let {a,b} = {b:'bbb',a:'aaa'}
console.log(a,b) //aaa bbb

let {a:b} = {a:1}
console.log(b) //1
```

3.基本类型的结构赋值

```
let [a,b,c,d] = '1234'
console.log(a,b,c,d) //1 2 3 4

let {length:len} = 'miumiu'
console.log(len) //6
```

4.null和undefined 不能进行结构赋值

数据结构Set

集合是由一组无序且唯一（不能重复）的项组成，这个数据结构使用了有限集合相同的数据概念，应用在计算机的数据结构中。

特点：key和value相同，没有重复的value
1.创建一个Set

```
const s = new Set([1,2,3])
console.log(s)
```

2.Set类的属性

```
console.log(s.size) //3
```

3.Set类的方法

1.Set.add(value) 添加一个数据，返回Set结构本身

```
s.add('a').add('b')
console.log(s)
```

2.Set.delete(value) 删除一个数据，返回布尔值，表示删除是否成功

```
s.delete('a')
```

3.Set.has(value) 判断该值是否为Set的成员，返回一个布尔值

```
console.log(s.has('a')) //false
```

4.Set.clear(value) 清除所有数据，没有返回值

5.key() 返回键名的遍历器

6.value() 返回键值的遍历器

7.entries() 返回键值对的遍历器

```
console.log(s.entries()) //{[1,1],[2,2]}
```

8.forEach() 使用回调函数遍历每个成员

```
s.forEach(function (value,kye,set) {
console.log(value)
})
```

利用特性去重

```
var arr = [1,2,3,4,54,54,3,4]
var b = new Set(arr);
console.log(b)
```

数据结构map

用来储存不重复key和hash结构。不同于集合（set）的是，所使用的是 [ 键，值]的形式来储存数据的。

1.创建一个Map

```
const map = new Map([
['a',1],
['b',2]
])
console.log(map)
```

2.Map类的属性

```
console.log(map.size) //2
```

3.Map类的方法

1.set(key,value) 

```
map.set('miumiu','m')
```

2.get(key) 读取key对应的键值

```
map.get('a') //1
```

3.delete(key) 删除成功返回true

```
map.delete('a') //true
```

4 has(key)

```
map.has('a')
```

5 clear() 清除

```
map.clear()
```

6 同上5678

tip:map里面的key的排列顺序是按照添加顺序进行排列的。

…………待更

let

const

变量的结构赋值

数据结构Set

数据结构map

FEATURED TAGS

笔记

FRIENDS

miumiu