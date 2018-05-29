## 基于Hexo／Next主题／Travis CI，实现自动构建集成发布的博客系统

[![Build Status](https://travis-ci.org/Selvin11/Selvin11.github.io.svg?branch=master)](https://travis-ci.org/Selvin11/Selvin11.github.io)


> 写作

```bash
$ hexo new [layout] <title文章名> 
```

layout默认为post，文章将储存到 source/_posts 文件夹

> 生成静态文件

```bash
$ hexo generate 或者 hexo g

```

> push 

```bash
git add .
git commit -m 'xxx'
git push
```

> 具体实现

在hexo／next的基础上，集成Travis CI，当使用`git push`后，将发布博文的工作自动化。


## hexo基本流程

1. 本地初始化一个hexo项目`yourgithubname.github.io`
2. hexo new post // 新建文章
3. hexo g // 将文章生成静态文件
4. git push // 将hexo项目push到远端，同时自动将静态文件发布项目的master分支

## 方案思路

1. github上有一个存放`yourgithubname.github.io`的hexo项目，source分支存放hexo代码，master分支存放静态文件

2. 在更新source中的文件后，`git push`时通过Travis CI的`Continuous Deployment`提供的`providers`，配置push的路径为项目的master分支


## 实现细节

1. 直接在Github中创建一个`yourgithubname.github.io`空项目

2. 参照[hexo官方网站](https://hexo.io/zh-cn/docs/index.html)和[next官方网站](http://theme-next.iissnan.com/getting-started.html)，在本地克隆`yourgithubname.github.io`，并使用hexo生成项目

3. 集成Travis CI，使用你的Github账号登录Travis CI官网，具体可以参考[持续集成服务 Travis CI 教程](http://www.ruanyifeng.com/blog/2017/12/travis_ci_tutorial.html)

4. `.travis.yml`文件中有详细注释

## 参考

* [使用 Travis 自动部署 Hexo 到 Github 与 自己的服务器](https://segmentfault.com/a/1190000009054888)
