---
title: 搭建基于Travis CI自动化部署的hexo／next主题博客
date: 2017-08-03 16:46:05
tags: 
  - Travis CI
---
 
在hexo／next的基础上，集成Travis CI，当使用`git push`后，将发布博文的工作自动化。

<!-- more -->

## hexo基本流程

1. 本地初始化一个hexo项目
2. hexo new post // 新建文章
3. hexo g -d  // 将文章生成静态文件并发布到博客项目（xxx.github.io）

## 产生的痛点

误删本地项目／换电脑／更新系统，都会导致本地项目丢失，当然也可以存U盘／云盘等，但直接放在Github上保存比较直观，所以我的理想流程是：

1. 本地初始化一个hexo项目
2. hexo new post // 新建文章
3. hexo g // 将文章生成静态文件
4. git push // 将hexo项目push到远端，同时自动将静态文件发布到博客项目（xxx.github.io）

## 方案思路

1. github上有一个存放hexo blog的项目，以及一个存放博客文章的静态资源项目（xxx.github.io）
2. 在更新hexo blog中的文件后，`git push`时通过Travis CI的Continuous Deployment提供的providers，配置push的路径为项目（xxx.github.io），以及目录分支等参数


## 实现细节

1. 参照[hexo官方网站](https://hexo.io/zh-cn/docs/index.html)和[next官方网站](http://theme-next.iissnan.com/getting-started.html)，生成项目（hexo-blog）

2. 直接在Github中创建一个（xxx.github.io）空项目

3. 项目（hexo-blog）集成Travis CI，使用你的Github账号登录Travis CI官网，具体可以参考[持续集成服务 Travis CI 教程](http://www.ruanyifeng.com/blog/2017/12/travis_ci_tutorial.html)

4. 在项目（hexo-blog）根目录下创建.travis.yml文件，进行配置（仅供参考）

  ```yaml
  language: node_js
  node_js:
    - 'stable'
  deploy:
    provider: pages
    skip_cleanup: true
    github_token: $GITHUB_TOKEN # Set in travis-ci.org dashboard
    local_dir: public # 选择push内容的根目录
    name: Selvin11 # 发布名称
    repo: Selvin11/selvin11.github.io # push到的项目名称
    target_branch: master # push到该项目下的分支名称（默认gh-pages）
    project_name: Blog # Deploy Blog to github.com/Selvin11/selvin11.github.io.git:master 构成的commit信息
    on:
      branch: master
  cache:
    directories:
    - node_modules # NPM packages
  ```
