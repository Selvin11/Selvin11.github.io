---
title: 获取input上传图片，实现裁剪功能
date: 2016-10-10
categories: 常见功能
tags: 
  - FileReader
---


使用FileReader对象实现web异步读取计算机上的文件，同时可以使用file对象来指定的处理的文件或数据

<!-- more -->

- readAsDataURL()

    开始读取指定的File对象中的内容,**result属性中将包含一个data: URL格式的字符串**以表示所读取文件的内容.

```html
<!DOCTYPE HTML>
<html>

<head>
</head>

<body>
    <input id="file" type="file">
    <img src="" id="image">
</body>
<script type="text/javascript" src="jquery.min.js"></script>
<script>
$('#file').on('change', function() {
    var reader = new FileReader();
    reader.onload = function(e) {
        $('#image').attr('src', e.target.result);
    }
    //readAsDataURL 将选取的文件以data:形式的数据存储在e.target.result中
    //result属性的返回值是任意类型，返回本次事件触发执行的上一个事件处理函数的返回值
    reader.readAsDataURL(this.files[0]);
})
</script>

</html>
```


```html
multiple属性可以让用户能选择多个文件

<input id="myfiles" multiple type="file">
```

> 图片裁剪并上传

- 插件cropbox , 将canvas得到的base64格式图片传至后端，进行png格式转化，并存入文件目录