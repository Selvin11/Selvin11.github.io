---
title: React学习之基础篇（四）———— 事件机制
categories: React
tags: 
  - React
---

学习`React`中的事件机制，理解语法及本质。
<!-- more -->

> React 处理事件的不同之处

- 事件采用驼峰命名

  ```html
  <button onClick={activateLasers}>
    Activate Lasers
  </button>
  ```

- 无法直接使用`return false;`组织事件默认行为

  ```javascript
  function ActionLink() {
    function handleClick(e) {
      e.preventDefault();
      console.log('The link was clicked.');
    }

    return (
      <a href="#" onClick={handleClick}>
        Click me
      </a>
    );
  }
  ```

- React 中的事件都是虚拟的，但兼容性是大大的，不必使用`addEventListner`进行事件监听，相反，只需在初始渲染时提供一个`listener`即可。当使用ES6 class，进行组件定义时，可以采用`handler event`的方法，届时调用即可

  ```javascript
  class Toggle extends React.Component {
    constructor(props) {
      // 继承父级this及其属性(状态)，否则会报错
      super(props);
      //Toggle 私有状态
      this.state = {isToggleOn: true};

      // This binding is necessary to make `this` work in the callback
      // es6 class 方法默认不会自动绑定this
      this.handleClick = this.handleClick.bind(this);
    }

    handleClick() {
      this.setState(prevState => ({
        // prevState 存储着前一个时刻的状态
        isToggleOn: !prevState.isToggleOn
      }));
    }

    render() {
      return (
        <button onClick={this.handleClick}>
          {this.state.isToggleOn ? 'ON' : 'OFF'}
        </button>
      );
    }
  }

  ReactDOM.render(
    <Toggle />,
    document.getElementById('root')
  );
  ```