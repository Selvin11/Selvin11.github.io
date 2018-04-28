---
title:  vuex入门到实践
date: 2017-03-17
categories:  Vue
tags: 
  - vuex
---

vuex一探到底，vuex是为了解决什么而存在的？究竟何时需要用到它？应该如何使用？伴随这三个问题，逐步深入。

<!-- more -->

### vuex解决了什么?

在使用`Vue`开发web应用程序时，每个组件如果都有各自的状态，而且这些状态之间是有关联，这时仅靠`Vue`是无法合理的管理这些状态的，会导致状态和代码难以维护。

因此vuex诞生了，说简单点，它充当了所有组件的容器，并为各个组件提供了调用状态，改变状态的接口。

### 何时需要用到它？

一般都会说，就像眼镜，你自然知道什么时候使用。

但这句话不免太过程序员语气，通俗来讲，当你的应用组件之间共用的状态出现较多时，这时vuex能够以它提供的规则，会避免你的代码和逻辑混乱，如果只有一两个共有的，可以用`global event bus`方案解决，本质是生成一个独立的`vue`实例，将共有的状态以及改变状态的方法挂载在上面，让你组件中使用的`vue`实例进行调用。

### vuex该如何使用？

上述两个问题的答案，可以看出最重要的是要理解vuex状态管理的模式，理解它提供的接口

1. vuex状态管理的模式：

  先来看看vue状态管理模式：单向数据流，状态（state）为视图（view）的数据源，操作（actions）可以改变状态，从而改变了视图。

  `view -> actions -> state ->view ...`

  但涉及多个组件之间的状态共享时，这个模式将难以维护，因此vuex的状态管理模式改为：

  `vuex -> actions -> 提交（commit）mutations ->state -> view -> dispatch 触发 actions ->...`

  vuex集中管理组件的状态，组件dispatch actions，然后actions才能提交改变状态的方法，状态改变了，视图也就更新了

  **应用中的状态不是所有都要放入vuex中，将组件之间共享的状态放入即可，像单个组件自己的状态，继续使用局部状态，避免vuex中的代码冗余**

2. vuex提供的接口 (实例代码为vue单文件组件)​


* State （状态）

  状态存储了整个应用的数据，而且是唯一的，这个接口提供的是让组件获取状态数据。vue提供了vuex挂载在根节点的接口store，因此组件获取state的方式如下：

  ```javascript
  // ./store/index.js  存放vuex中的接口及数据的文件
  import Vue from 'vue'
  import Vuex from 'vuex'

  Vue.use(Vuex)

  const counter = {
    count : 0
  }

  // 1. 将store挂载在根节点的vue实例上，因此所有子组件均能访问到vuex中的数据
  import store from './store'
  const app = new Vue({
    el: '#app',
    // 把 store 对象提供给 “store” 选项，这可以把 store 的实例注入所有的子组件
    store,
    components: { Counter }
  })
  // 2. 在组件中获取vuex中的state
  <template>
     <h1>{{count}}</h1>
  </template>
  <script>
     import {mapState} from 'vuex'
     // 引入vuex /src/helper.js中的辅助函数，将多个状态生成计算属性，供组件获取状态数据state
     
       // 下面有几种写法，其实这不是重点，只是在es6,es7的语法问题
       // 第一种：纯粹的调用$store对象上挂载的vuex接口数据
       computed:{
         count(){
           return this.$store.state.count
         }
       }
      //第二种：使用vuex提供的mapState辅助函数
  	computed:mapState({
        count: state => state.count,
        //或者
        countAlias: 'count', // 同箭头函数
        
        // 为了能够使用 `this` 获取局部状态，必须使用常规函数
        countLocalState(state){
          return state.count + this.localCount
        }
  	})
      //第三种：计算属性名称与state提供的数据名称相同，直接提供字符串数组即可
      computed:mapState([
        'count' // 这里不但组件模板中可以直接调用count，其它methods中可直接以this.count调用，它实质映射为store.state.count
      ])
      //第四种写法：使用...扩展运算符，mapState返回的是对象，里面有属性方法，...可以取出参数对象中所有可遍历属性，浅拷贝至当前对象中
      computed:{
        ...mapState([
          'count'
        ])
      }
  </script>
  ```

* Getters （在store中预处理state的方法）

     如需要对状态事先进行筛选，可以将此操作封装为函数方法，赋予getters，这样做的好处是所有组件共用此函数，不必在每个组件中重复写

     ```javascript
     // ./store/index.js store 文件中
     const store = new Vuex.Store({
       state: {
         todos: [
           { id: 1, text: '...', done: true },
           { id: 2, text: '...', done: false }
         ]
       },
       getters: {
         doneTodos: state => {
           return state.todos.filter(todo => todo.done)
           // [{ id: 1, text: '...', done: true }]
         },
         // 可以接受其他getters方法作为第二参数
         doneTodosCount: (state, getters) => {
           return getters.doneTodos.length
           // 1
       	}
       }
     })

     // 在组件中使用vuex中的getters
     <template>
        <h1>{{count}}</h1>
     </template>
     <script>
        import {mapGetters} from 'vuex'
        // 引入vuex /src/helper.js中的辅助函数，将多个状态生成计算属性
          // 第一种：纯粹的调用$store对象上挂载的vuex接口数据
          computed:{
            doneTodos () {
         	return this.$store.getters.doneTodos
       	   }
          }
         //第二种：计算属性名称与getters提供的数据名称相同，直接提供字符串数组即可 使用...扩展运算符，mapGetters返回的是对象，里面有属性方法，...可以取出参数对象中所有可遍历属性，浅拷贝至当前对象中
         computed:{
           ...mapGetters([
             'doneTodos'
           ])
         }
     	// 第三种：如果在组件中想用其他属性名替代getters提供的属性名，使用对象形式即可
     	computed:{
           ...mapGetters({
             mydoneTodos: 'doneTodos'
           })
         }
     </script>
     ```

* Mutations （更改vuex的store中的state唯一的办法就是提交mutations）

     每个 mutation 都有一个字符串的 **事件类型 (type)** 和 一个 **回调函数 (handler)**。

     **mutation 必须是同步函数，分发在组件methods**

     ```javascript
     //使用常量替代 Mutation 事件类型
     // mutation-types.js
     export const SOME_MUTATION = 'SOME_MUTATION'

     // ./store/index.js  文件中
     import Vuex from 'vuex'
     import { SOME_MUTATION } from './mutation-types'

     const store = new Vuex.Store({
       state: {
         count: 1
       },
       mutations:{
         increment(state){
           // 改变状态
           state.count++;
         },
         // 我们可以使用 ES2015 风格的计算属性命名功能来使用一个常量作为函数名
         [SOME_MUTATION] (state) {
           // mutate state
         }
       }
       //可以向 store.commit 传入额外的参数，即 mutation 的 载荷（payload）
       mutations:{
         increment(state,payload){
           // 改变状态
           state.count += payload.num;
         }
       }
     })

     // 在组件中使用vuex中的mutations
     <template>
        <h1>{{count}}</h1>
     </template>
     <script>
        import {mapMutations} from 'vuex'
        // 引入vuex /src/helper.js中的辅助函数，将多个状态生成计算属性
          // 第一种：纯粹的调用$store对象上挂载的vuex接口数据
          methods:{
            increment () {
         	return this.$store.commit('increment')
       	   }
          }
         //第二种：计算属性名称与mutations提供的数据名称相同，直接提供字符串数组即可 使用...扩展运算符，mapMutations返回的是对象，里面有属性方法，...可以取出参数对象中所有可遍历属性，浅拷贝至当前对象中
         methods:{
           ...mapMutations([
           'increment' // 映射 this.increment() 为 this.$store.commit('increment')
          ]),
         }
     	// 第三种：如果在组件中想用其他属性名替代mutations提供的属性名，使用对象形式即可
     	methods:{
           ...mapMutations({
             myincrement: 'increment'
           })
         }
     </script>
     ```

* Actions （提供异步提交mutations的接口）

     ```javascript
     // ./store/index.js  文件中
     import Vuex from 'vuex'

     const store = new Vuex.Store({
       state: {
         count: 1
       },
       mutations:{
         increment(state){
           // 改变状态
           state.count++;
         }
       },
       actions:{
         //Action 函数接受一个与 store 实例具有相同方法和属性的 context 对象，
         //因此你可以调用 context.commit 提交一个 mutation，
         //或者通过 context.state 和 context.getters 来获取 state 和 getters。
         increment (context) {
           context.commit('increment') //这里提交的就是mutations
         }
       }
       // 或者通过参数结构的语法这样写
       actions: {
         increment ({ commit }) {
             commit('increment')
         },
         // 异步提交mutations
         incrementAsync ({ commit }) {
           setTimeout(() => {
             commit('increment')
           }, 1000)
         },
         // 可以使用promise或者async / await 组合使用actions
         actionA ({ commit }) {
           return new Promise((resolve, reject) => {
             setTimeout(() => {
               commit('someMutation')
               resolve()
             }, 1000)
           })
         },
         actionB ({ dispatch, commit }) {
           return dispatch('actionA').then(() => {
             commit('someOtherMutation')
           })
         }
         
       }
     })
     // 组件中使用actions
     <template>
        <h1>{{count}}</h1>
     </template>
     <script>
        import {mapActions} from 'vuex'
        // 引入vuex /src/helper.js中的辅助函数，将多个状态生成计算属性
          // 第一种：纯粹的调用$store对象上挂载的vuex接口数据
          methods:{
            increment () {
         	return this.$store.dispatch('increment')
       	   }
          }
         //第二种：计算属性名称与state提供的数据名称相同，直接提供字符串数组即可 使用...扩展运算符，mapActions返回的是对象，里面有属性方法，...可以取出参数对象中所有可遍历属性，浅拷贝至当前对象中
         methods:{
           ...mapActions([
           'increment' // 映射 this.increment() 为 this.$store.dispatch('increment')
         ]),
         }
     	// 第三种：如果在组件中想用其他属性名替代actions提供的属性名，使用对象形式即可
     	methods:{
           ...mapActions({
             myincrement: 'increment'
           })
         }
     </script>
     ```

* Modules （将store分为不同的模块，更好的管理状态）

     每一个module中都有各自的state,getters,mutations,actions.

     ```javascript
     const moduleA = {
       state: { ... },
       mutations: { ... },
       actions: { ... },
       getters: { ... }
     }

     const moduleB = {
       state: { ... },
       mutations: { ... },
       actions: { ... }
     }

     const store = new Vuex.Store({
       modules: {
         a: moduleA,
         b: moduleB
       }
     })
     ```

     模块内部的 action、mutation、和 getter 注册在**全局命名空间**——这样保证了多个模块能够响应同一 mutation 或 action。

     举例解释：即上述的A和B模块中声明的`mutations`可以被`actions.js`根（全局）文件中调用

     ​