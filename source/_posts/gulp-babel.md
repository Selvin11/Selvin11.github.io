---
title: 基于Gulp的babel自编译任务流
categories: Gulp
tags: 
  - Gulp
  - Babel
---

## Package.json

```json
"devDependencies": {
  "babel-core": "^6.14.0",
  "babel-preset-es2015": "^6.14.0",
  "gulp": "^3.9.1",
  "gulp-babel": "^6.1.2"
}
```
<!-- more -->


### gulpfile.babel.js

``` javascript
'use strict';

import gulp from 'gulp';
import babel from 'gulp-babel';

const paths = {
  src: 'src',
  dest: 'lib'
};


gulp.task('babel-js',()=>{
  return gulp.src(`${paths.src}/*.js`)
    .pipe(babel({
      presets:['es2015']
    }))
    .pipe(gulp.dest(`${paths.dest}/`));
})

gulp.task('watch',()=>{
  gulp.watch('./src/*.js',['babel-js']);
})

```


