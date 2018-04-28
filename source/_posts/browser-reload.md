---
title:  "全栈前端热更新，后端服务重启，浏览器自动刷新"
date: 2017-03-20
categories: "全栈"
tags: 
  - Webpack 
  - Gulp 
  - Node.js
  - nodemon
---

详解浏览器自刷新环境和热更新（热重载/热替换）环境搭建

<!-- more -->

### 第一类，文件变动 ，浏览器内容同步更新

1. 第一种方案：浏览器刷新，BrowserSync结合gulp
2. 第二种方案：webpack热更新



### 第二类，等待sass -> css 、babel -> es5 、 ... 的编译完成后，实现浏览器内容同步

1. 第一种方案：通过gulp的任务流，顺序执行编译以及浏览器刷新
2. 第二种方案：webpack热更新就是在编译之后实现的，无需其它配置

### 第三类，文件变动 => 编译 => 重启服务器，浏览器实现内容同步

需要有监听服务器重启完成后，通知浏览器实现内容同步的机制

`server`部分的自动刷新，会面临一个问题：自动刷新的消息通知依靠的是浏览器和服务器之间的web socket连接，但在`server`部分修改代码的话，一般都要重启服务器来使变更生效（比如修改接口路由`routes`），这就会断开web socket连接。

所以，这需要一个变通的策略：浏览器这边增加一个对web socket断开的处理，如果web socket断开，则开启一个稍长于服务器重启时间的定时任务（`setTimeout`），相当于等到服务器重启完毕后，再进行一次整页刷新。

方案如下：

* 使用`supervisor/nodemon`对服务器端文件监控（服务端路由，api接口等文件），
* 当文件变动时，`supervisor/nodemon`使得服务器服务重启，并且gulp也对改文件监听改动，然后执行browser-sync的reload的延时函数，使得满足上述需求

理解要点：（以下具体参数只作为参考）

**理解三个端口！！！**

1. app应用端口为：8080 （webpack-dev提供的服务端口）
2. 服务端端口为：3000 （express/koa/http ，node提供的服务端端口）
3. browser-sync的端口：4000 （browser-sync配置代理proxy，代理8080端口）


**理解两个启动！！！**

1. node build/dev-server.js即npm run dev （vue-cli中提供的webpack启动命令）
2. gulp default (gulp实现gulp-nodemon以及browser-sync的服务)

webpack 只对应用（端口为8080）中的js，css等实施编译打包等，然后将打包形成的bundle文件插入app应用的index.html中，从而实现了热更新，并且在webpack-dev中设置proxy（代理），使得应用中的axios可以跨域调用服务端（3000）的接口数据，因此npm run dev => node build/dev-server.js ，此命令启动了应用的8080端口

 gulp 的存在是为了实现第三类的刷新任务，browser-sync中设置proxy（8080），及代理了应用的端口，同时需要设置browser-sync的端口 （除3000之外的如4000，默认为3000），然后设置nodemon的任务，监听的服务器文件（服务端的server.js中设置了3000端口），因此gulp default命令便启动了服务端3000端口以及浏览器的4000端口（及代理了应用的8080端口）

**浏览器刷新无论是热更新或者browser-sync ，都是在html文件中加入监控脚本**


实践案例：[vue + webpack + express + gulp](https://github.com/Selvin11/vue-express-webpack-gulp)。






> 启动文件babel语法问题

koa2 中采用了es7的async/await语法，因此采用的是babel-preset-stage-3标准，目前node最新版7.7.2可以在node中直接使用，但如果版本较低的话，得采用babel编译后执行，

* 第一种方法：使用node最新版，支持最新语法，直接启动文件即可

* 第二种方法：全局安装babel-cli，使用babel-node 启动文件，即会编译改文件并执行

* 第三种方法：在文件头部，引入babel-register，遇到require便会编译
