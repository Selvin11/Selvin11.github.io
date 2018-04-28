---
title: React学习之基础篇（五）———— 条件、列表循环
categories: React
tags: 
  - React
---

条件渲染

- 同原生JS一致，采用条件语句进行判断并渲染DOM。

  条件通过setState()来进行更新，从而产生判断内容，然后根据UI上的数据来执行渲染过程。

- 简化判断语句

  `true && expression`  `condition ? true : false`

- 阻止组件渲染

  符合条件时`return null`(条件判断放在render()之前)

<!-- more -->

> 列表和键

- 主要通过map()方法来遍历数组，并为每一个列增加key即id，以便区分

  ```javascript
  //simple example
  function NumberList(props) {
    //定义数组常量
    const numbers = props.numbers;
    //定义遍历列表常量
    const listItems = numbers.map((number) =>
      <li key={number.toString()}>{number}</li>
    );
    return (
      <ul>{listItems}</ul>
    );
  }

  const numbers = [1, 2, 3, 4, 5];
  ReactDOM.render(
    <NumberList numbers={numbers} />,
    document.getElementById('root')
  )
  ```


- key一般只需在相邻的list中保持自己的唯一性，不用在全局中保持唯一，这样通过不同的数据遍历出来的内容，可以根据相同的key，在同一数组中传递，例如遍历文章，文章中的标题、时间等都是一一对应的，配合mongodb写博客系统更是酸爽。

  ```javascript
  function Blog(props) {
    const sidebar = (
      <ul>
        {props.posts.map((post) =>
          <li key={post.id}>
            {post.title}
          </li>
        )}
      </ul>
    );
    const content = props.posts.map((post) =>
      <div key={post.id}>
        <h3>{post.title}</h3>
        <p>{post.content}</p>
      </div>
    );
    return (
      <div>
        {sidebar}
        <hr />
        {content}
      </div>
    );
  }

  const posts = [
    {id: 1, title: 'Hello World', content: 'Welcome to learning React!'},
    {id: 2, title: 'Installation', content: 'You can install React from npm.'}
  ];
  ReactDOM.render(
    <Blog posts={posts} />,
    document.getElementById('root')
  );
  ```

> React 中的表单

- 在原生HTML中，表单元素自身拥有状态以及更新状态的能力，但在React中，状态存在于组件中，状态的改变只能通过setState()函数进行改变，因此也称为控制组件。