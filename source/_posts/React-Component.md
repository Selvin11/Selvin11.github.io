---
title: React学习之基础篇（二）————组件
categories: React
tags: 
  - React
---

学习`React`组件，从创建，使用，拆分，构建，理解组件化的思维，以及对于组件构建的一些理解，就像造轮子和使用轮子，充斥着哲学味道。

<!-- more -->

> React Component

- 组件的本质

组件本质就是一个对象。

- 创建一个组件

```javascript
// 原生函数创建
function Welcome(props) {
  return <h1>Hello,{props.name}</h1>
}
// 使用es6中的类，继承来创建
class Welcome extends React.Component {
  render() {
    return <h1>Hello,{this.props.name}</h1>
  }
}
```

- 使用组件

```javascript
const element = <Welcome name="selvin" />;
ReactDOM.render(
element,
document.getElementById("root")
);
```

- 组件唯一的根元素

注意：Component返回值必须单一的根元素，即只有一个最外包裹节点，因此加入了空的`<div>`

```javascript
function App() {
  return (
    <div>
      <Welcome name="selvin"/>
      <Welcome name="selvin"/>
      <Welcome name="selvin"/>
      <Welcome name="selvin"/>
    </div>
  )
} 
// use it
ReactDOM.render(
  <App/>,
  document.getElementById("root")
);
```

- 构建组件

注意：一个React app一般只有一个组件在最外层，但如果你是向已经存在的app中添加组件，则是由最底层的节点，例如一个按钮`button`，一步步向最外层进行组件开发，直至这个组件成为最外层节点。

```javascript
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <img className="Avatar"
          src={props.author.avatarUrl}
          alt={props.author.name}
        />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}

 // this component accepts three object . author / text / date
 // first extract author
 function Avatar(props) {
  return (
    <img className="Avatar"
      src={props.user.avatarUrl}
      alt={props.user.name}
    />
  );
}
  // second extract user
 function UserInfo(props) {
  return (
    <div className="UserInfo">
      <Avatar user={props.user} />
      <div className="UserInfo-name">
        {props.user.name}
      </div>
    </div>
  );
}
 // finally
 function Comment(props) {
  return (
    <div className="Comment">
      <UserInfo user={props.author} />
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

- `props`只有一个根节点，只读。

> React 组件核心思想

- 重构和继承，面对一个UI Component，我们可以对其进行近可能的拆分，从而导致我们最终可能会有成百上千个组件，面对新的组件开发，我们是采用重新构建还是继承呢，这里就是理解React组件化的核心思想了。构建是最具可塑性的组件化方式，它十分的弹性，我们只需根据现有的可用的组件完成基本结构的搭建，然后通过特殊标签`props.children`或者`自定义的子组件并重新定义`，就可以构建出任何你所需的组件了，因此大多数情况下采用构建就够了。