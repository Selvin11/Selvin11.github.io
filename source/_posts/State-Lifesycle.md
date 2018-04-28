---
title: React学习之基础篇（三）———— 状态和生命周期
categories: React
tags: 
  - React
---

来学一学`React`的状态和生命周期


<!-- more -->
简单的示例：

```javascript
class Clock extends React.Component {
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }

  componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }

  componentWillUnmount() {
    clearInterval(this.timerID);
  }

  tick() {
    this.setState({
      date: new Date()
    });
  }

  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

- attention  state update

the way of setState()

1. 直接使用 `this.setState({comment: 'Hello'});`;
2. 当同一个组件内部更新`this.state`和`this.props`时为异步的情况，采用如下方法进行状态更新

```javascript
// Correct
this.setState((prevState, props) => ({
  counter: prevState.counter + props.increment
}));
//prevState 保存的是之前状态的值，然后作为参数传递
```

3. component state 只能影响下级的组件，同一层级相同的组件之间是独立的，不会互相影响