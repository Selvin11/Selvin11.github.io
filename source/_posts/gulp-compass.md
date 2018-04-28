---
title: 基于Gulp的Compass任务流
categories: Gulp
tags: 
  - Gulp
  - Compass
---

## Package.json

```json
"devDependencies": {
    "browser-sync": "^2.16.0",
    "gulp": "^3.9.1",
    "gulp-compass": "^2.1.0",
    "gulp-notify": "^2.2.0",
    "gulp-plumber": "^1.1.0"
  }
```
<!-- more -->


### Gulpfile.js

``` javascript
// Load plugins
var gulp = require('gulp'),
    compass = require('gulp-compass'),
    // gulp弹窗任务完成提醒
    notify = require('gulp-notify'),
    // 解决编译中断进程
    plumber = require('gulp-plumber'),
    browserSync = require('browser-sync').create();



// Styles
gulp.task('compass', function() {
    return gulp.src('./sass/*.scss')
      //输出编译错误 
      .pipe(plumber({errorHandler: notify.onError('Error: <%= error.message %>')}))
      // 编译
      .pipe(compass({
        config_file: './config.rb',
        sourcemap: true,
        css: 'stylesheets',//compass 输出路径
        sass: 'sass'//sass 来源路径
      }))
      .pipe(browserSync.reload({stream:true}))//触发自刷新
      .pipe(notify({ message: 'compass task complete' }));

      
});

//html自刷新
gulp.task('html', function() {
    return gulp.src('*.html')
        .pipe(browserSync.reload({stream:true}));
});


// Watch
gulp.task('watch', function() {
  // browserSync 配置
  browserSync.init({
      server: {
          baseDir: "./"
      }
  });
  // Watch .scss files
  gulp.watch('./sass/*.scss', ['compass']);
  gulp.watch( '*.html', ['html']);
  // Watch *.* files -> reflash
  gulp.watch(['*/**/*.*']).on('change', browserSync.reload);

});

```


