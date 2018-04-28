---
title: React学习之基础篇（一）
categories: React
tags: 
  - React
---


学习`React`基于`ES6`的基本写法。
<!-- more -->


1. 第一章

```javascript
//虚拟DOM  &&  真实DOM
ReactDOM.render(virtual dom , real dom);
//组件语法
//组件内部通过this.props对象获取参数
class MyTitle extends React.Component {
    constructor(...args) {
      super(...args);
      this.state = {
        text: 'World'
      };
    }

    handleClick() {
      this.setState({
        text: 'Clicked'
      });
    }

    render() {
      return <h1 onClick={this.handleClick.bind(this)}>
        {'Hello ' + this.state.text}
      </h1>;
    }
  };
```