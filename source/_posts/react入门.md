---
title: react入门
date: 2018-04-22 00:00:00
tags:
  - react
  - react
---

React-JXS-Style

```
// JXS转换库
// 引入https://cdn.bootcss.com/react/0.14.0-beta3/JSXTransformer.js
```

```
//基础写法

/*
React React.createClass创建组件,变量名开头大写字母;
sytle 两种写法
1) className='css名'
2) style={{样式}}
*/
var Hello = React.createClass({
render:function () {
var styleName = {
fontSize:'30px'
};
return 
hello {this.props.name}

}
});
/*
参数1，要渲染的components
参数2，要插入容器的元素
*/
React.render(
,
document.getElementById('container'))

```

react生命周期

```
var Hello = React.createClass({
getInitialState:function () {
alert('init')
return{
opacity:1,
fontSize:'12px'
}
},

render:function () {
return 
hello {this.props.name}

},

componentWillMount:function () {
alert('will')
},
componentDidMount:function () {
alert('did')
var _self = this;
window.setTimeout(function () {
_self.setState({
opacity:0.5,
fontSize:'44px'
})
},1000);
}
})
React.render(
,
document.getElementById('container'))
```

文档

cdn:

http://www.bootcdn.cn/react/

org：

https://reactjs.org/

中文翻译

http://react.yubolun.com/

React-JXS-Style

react生命周期

文档

FEATURED TAGS

react

FRIENDS

miumiu