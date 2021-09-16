---
title: HTML5超详细了解
categories: 
	- HTML
tags:
	- HTML5
	- 基础知识
keywords: HTML5 基础知识
mathjax: true
description:
    讲解HTML5的新增内容，包括语义化标签，表单新属性，和新增的API
date: 2019-12-23
cover: https://gitee.com/lastknightcoder/blogimage/raw/master/img/bg39.png
---

HTML5属于上一代HTML的新迭代语言，设计HTML5最主要的目的是为了在移动设备上支持多媒体，例如：video标签和audio及canvas标记。

## HTML5中语义化标签

在HTML5中新增了很多的语义标签，如

- header
- footer
- nav
- article
- aside
- section
- ... ...

比如以前我们使用以下方式来布局

```html
<div class="header"></div>
```

现在可以替换为

```html
<header></header>
```

HTML5可以让很多更语义化的结构化代码标签代替大量无意义的div标签

1. 这种语义化的特性提升了网页的质量和语义
2. 减少了以前用于CSS 调用的`class`和`id`属性

并且对搜索引擎的友好，新的结构标签带来的是网页布局的改变及提升对搜索引擎的友好。

但是现在碰到一个问题，由于这些具有语义的标签是HTML5新增的，这就意味着在IE8及以下版本的IE浏览器中不支持，如下面的样式在IE8中就不能够正常的显示

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        header {
            background-color: red;
            height: 400px;
            width: 100%;
        }
    </style>

</head>
<body>
    <header></header>
</body>
</html>
```
 解决办法：

1. 在script中创建语义标签header，并且将header的display设置为block

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        header {
            background-color: red;
            height: 400px;
            width: 100%;
            display: block;
        }
    </style>

    <script>
        document.createElement("header");
    </script>
</head>
<body>
    <header></header>
</body>
</html>
```
2. 使用1的方法，意味着对每个语义标签都要创建元素，这样未免比较麻烦，更好的办法是使用插件，引入html5shiv.js文件，该插件的实质还是创建了语义元素

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
    <style>
        header {
            background-color: red;
            height: 400px;
            width: 100%;
        }
    </style>

    <script src="html5shiv.js"></script>
</head>
<body>
    <header></header>
</body>
</html>
```

3. 上面的方法还有需要改进的地方，比如在谷歌浏览器中完全支持HTML5，这就意味着在渲染HTML网页时不需要下载html5shiv.js文件，但是上面的方法是在任何的浏览器中都会下载的，所以再次改进如下

```html
<!--[if lte IE 8]>
    <script src="html5shiv.js"></script>
<![endif]-->
```

上面的代码只有IE浏览器才会识别，意思是如果IE浏览器的版本是IE8及以下的版本，才会下载这个js文件，在其他浏览器中会认为这时注释，自动忽略。

## video和audio

在浏览器中插入视频和音频文件，以往是使用flash来实现，但是在移动端使用flash就会比较慢，HTML5给了两个新的标签，用来插入视频和音频文件。二者的使用相似，现以video为例介绍该标签的属性

| 属性 | 作用|
| --- | --- |
|src | 值为视频文件的路径|
|controls| 显示控制台|
|autoplay| 自动播放|
|loop| 循环播放|
|... ... | ... ...|

还有一些属性没有介绍，以上是较为常用的，剩余的请参考网站[HTML5视频](https://www.w3school.com.cn/html5/html_5_video.asp)。这里给出一个实例

```html
<video src="resources/video.mp4" controls autoplay loop></video>
```

另一个需要注意的是，目前只支持三种格式的视频

- Ogg
- MPEG 4(mp4)
- WebM 

并且不同的浏览器支持的程度也不一样，具体的可以参考上面的链接。那么这个时候怎么办? 我们不能这么写

```html
<video src="test.mp4"></video>
<video src="test.ogg"></video>
<video src="test.WebM"></video>
```

虽然我们的本意是：如果支持.mp4，那么就使用.mp4，否则如果支持.ogg，则用.ogg，以此类推。但是上面的效果是出现3个video，而不是一个，这个时候的解决办法是使用source标签，如下

```html
<video>
    <source src="test.mp4">
    <source src="test.ogg">
    <source src="test.WebM">
</video>
```

这个时候达到的效果就是我们想要的。

## 表单

HTML5在表单这里也做了很多的改进，比如新增了一些属性以进行表单验证(以往这些工作我们都是使用JavaScript进行正则表达式的验证)，以及新的标签和方法。

### 智能表单控件

- emial
- url
- number
- range
- color
- date
- month
- week
- time

首先用法如下

```html
<form action="test" method="get">
    email: <input type="email"> <br>
    url: <input type="url"> <br>
    number: <input type="number"> <br>
    range: <input type="range"> <br>
    color: <input type="color"> <br>
    date: <input type="date"> <br>
    week: <input type="week"> <br>
    time: <input type="time"> <br>
    <button type="submit">提交</button>
</form>
```

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/html501.png"/>
</center>


当`type`设置为`emial`时，如果输入的不是电子邮箱，当点击提交按钮时，不能提交成功，并给出提示信息

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/html502.png"/>
</center>


当`type`设置为`url`时，如果输入的不是`url`地址，那么当点击提交按钮时，也不能提交成功，并给出提示信息

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/html503.png"/>
</center>


正确的`url`地址应该以`http`或者`https`开头，如`http://www.baidu.com`。

当`type`设置为`number`时，这时在控件里面只能输入数字，当你按其他键时没有反应，可以自行实验看看效果。

当`type`设置为`color`时，点击`color`后的颜色，会出现拾色器，可以选择颜色，如下

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/html504.png"/>
</center>


设置`type`设置为`date, week, time`时，显示的是各种格式的时间，这里不多加解释想必可以明白。

### 表单属性

#### form表单的属性

1. autocomplete

直译过来就是自动完成，当我们提交表单后，表单会记录我们提交的内容，当我们再次填写时，它会给出我们已经提交过的内容作为提示信息。有时这种情况下可能会造成信息的泄漏，不安全，我们可以将`autocomplete`设置为`off`，这时就不会出现上面的情况。默认情况下`autocomplete`为`on`

```html
<form action="test" method="get" autocomplete="off">
    ... ...
</form>
```

2. novadilate

上面我们提到，当我们使用智能表单控件时，如果不能满足格式的要求，如`email`，则不能提交成功，当表单添加`novadilate`属性时，那么这时即使所填写的格式不满足要求，那么也可以提交成功。

```html
<form action="test" method="get" autocomplete="off"  novalidate>
    ... ...
</form>
```

#### input的属性

1. autofocus

自动获得焦点，我们先来看一个淘宝的案例

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/html505.gif"/>
</center>


当我们进入淘宝，搜索框会自动的获得焦点，用户可以直接输入，不需要用鼠标先点击搜索框获得焦点才能输入。input添加autofocus的属性即可有这种效果。

2. form

先来看这么一个结构

```html
<form action="test" method="get">
    <input type="text" name="one">
    <button type="submit">提交</button>
</form>
<input type="text" name="two">
```

我们可以知道当提交form表单时，只会提交表单域里面的表单，表单域外的表单不会提交，所以当我们提交时，只会有`one`的数据才会提交

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/html506.png"/>
</center>


但是如果希望当提交时，`two`的数据也能进行提交(别奇怪，真的有这种需求)，这个时候就需要用到`form`属性了

```html
<form id="data" action="test" method="get">
    <input type="text" name="one">
    <button type="submit">提交</button>
</form>
<input type="text" name="two" form="data">
```

`form`属性的值为`form`表单的`id`值。这时再次进行提交

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/html507.png"/>
</center>


这时`two`的数据也得到了提交。

3. list

`list`属性要配合HTML5新添加的表单标签`<datalist>`使用，如下

```html
<input type="text" list="list">
<datalist id="list">
    <option value="One"></option>
    <option value="Two"></option>
    <option value="Three"></option>
</datalist>
```

`list`属性的值为`datalist`标签的`id`值。当我们在`text`中输入时，会有`datalist`中`option`值的提示

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/html508.png"/>
</center>


4. multiple

`multiple`可以实现多选的效果，比如选择多个文件，假设有下面的`input`标签

```html
<input type="file">
```

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/html509.gif"/>
</center>


这时只能选择一个文件，为了选择多个文件，我们为`input`标签添加`multiple`属性

```html
<input type="file" multiple>
```

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/html510.gif"/>
</center>


这时就可以选择多个文件了。

5. placeholder

使用placeholder作为提示信息，假设有如下标签

```html
<input type="text" placeholder="请输入信息">
```

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/html511.gif"/>
</center>


当我们输入文字时，提示信息会消失，当我们将文字消失时，文字又会出现。

6. required

当`input`使用该属性时，表示该`input`控件是必填项，否则无法提交，具体可以自己试验一下，这里就不演示了。

## HTML5 API

### 获取DOM元素

假设有如下的`html`结构

```html
<ul>
    <li>
        <span>span1</span>
    </li>
    <li>
        <a href="#"><span>span2</span></a>
    </li>
</ul>
```

如果我们要改变`span`的样式(通过JS)，我们我们一般要为`span`标签添加`id`属性或者`class`属性，这样才能获取要对应的`DOM`元素，HTML5新增了两个方法

- querySelector()：只能选择一个元素
- querySelectorAll()：可以选择所有符合条件的元素

可以向其中传入选择器(任何CSS支持的选择器)，从而来选择`DOM`元素，如

```javascript
//使用子代选择器选择span元素
document.querySelector("li>span").style.color = "red";
```

### 类名操作

有时候我们需要为某个标签添加或者移除类样式，HTML5为我们提供了API接口

- classList.add()：为`DOM`元素添加指定的类样式
- classList.remove()：为`DOM`元素移除指定的类样式
- classList.toggle()：切换，意思即如果`DOM`元素有这个类样式，则移除这个类样式，如果没有这个类样式，这添加这个类样式
- classList.contains()：判断该`DOM`元素是否包含这个类样式，包含则返回`true`，否则返回`false`。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>

    <style>
        .active {
            width: 100%;
            height: 100px;
            background-color: darkred;
        }
    </style>

</head>
<body>
    <div></div>
    <button class="add">添加类名</button>
    <button class="remove">移除类名</button>
    <button class="toggle">切换类名</button>
    <button class="contains">是否包含类名</button>

    <script>
        document.querySelector(".add").onclick = function () {
            document.querySelector("div").classList.add("active");
        }
        document.querySelector(".remove").onclick = function () {
            document.querySelector("div").classList.remove("active");
        }
        document.querySelector(".toggle").onclick = function () {
            document.querySelector("div").classList.toggle("active");
        }
        document.querySelector(".contains").onclick = function () {
            console.log(document.querySelector("div").classList.contains("active"));
        }
    </script>
</body>
</html>
```

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/html512.gif"/>
</center>


###  自定义属性

HTML5规定自定义属性需要以`data-`开头，如

```html
<div data-test="one"></div>
```

上面自定义了一个叫`test`的属性，我们可以通过`DOM`元素的`dataset`来访问或者修改自定义属性的值，有两种方式

- dataset.属性名
- dataset["属性名"]

```html
<div data-test="one"></div>
<script>
    console.log(document.querySelector("div").dataset.test);
    document.querySelector("div").dataset["test"] = "two";
</script>
```

如果属性名之间使用`-`之间连接，如下

```html
<div data-test-name="one"></div>
```

那么使用`dataset`访问或修改时要使用驼峰命名法获取，如下

```javascript
console.log(document.querySelector("div").dataset.testName);
document.querySelector("div").dataset["testName"] = "two";
```

### 文件读取

`FileReader`是用来读取上传的文件的，它有3个读取的方法

- readAsText()：读取文本文件，返回文本字符串(utf-8)
- readAsBinaryString()：读取任意文件，返回二进制文件
- readAsDataURL()：读取任意文件，得到包含一个data:URL格式的字符串(base64编码)，以表示所读取文件的内容

上面三个方法读取的内容都会放在FileReader对象的result属性中。

现在演示一个案例，选择上传的图片，在上传之后希望有预览的效果

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <input type="file"><br>
    <br>
    <img src="" width="800"></img>

    <script>
        //获得input DOM元素
        let input = document.querySelector("input");
        //获得img DOM元素
        let image = document.querySelector("img");
        
        //当input发生改变时即有文件上传时触发该事件
        input.onchange = function () {
            //获得上传的文件对象
            let file = input.files[0];
            //获得FileReader对象
            let reader = new FileReader();
            //使用FileReader读取该图片，得到一个base64格式的编码
            reader.readAsDataURL(file);
            
            //reader读取完毕后触发该事件
            reader.onload = function() {
                //将读取到的base64编码格式的URL赋值给img标签的src属性
                image.src = reader.result;
            }
        }

    </script>
</body>
</html>
```

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/html513.gif"/>
</center>


上面的代码中，我们用到了`FileReader`对象的`onload`事件，这里列出`FileReader`提供的事件

- onabort：中断时触发
- onerror：出错时触发
- onload：文件读取成功完成时触发
- onloadend：读取完成触发，无论成功或失败
- onloadstart：读取开始时触发
- onprogress：在读取过程中持续触发

### 获取网络状态

HTML5提供了有关网络状态的API，使用`window.navigator.onLine`可以获取当前的网络状态，返回一个布尔值。除此之外，还提供了两个网络事件

- ononline()：连上网络的时候触发
- onoffline()：断开网络的时候触发

```javascript
window.ononline = function () {
    console.log("online");
}
window.onoffline = function () {
    console.log("offline");
}
```

<center>
<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/html514.gif"/>
</center>


### 本地存储

随着互联网的快速发展，基于网页的应用越来越普遍，同时也变的越来越复杂，为了满足各种各样的需求，会经常性在本地存储大量的数据，传统方式我们以`document.cookie`来进行存储的，但是由于其存储大小只有4k左右，并且解析也相当的复杂，给开发带来诸多不便，HTML5规范则提出解决方案，使用`sessionStorage`和`localStorage`存储数据。

`sessionStorage`的大小大约为5M左右，它的生命周期为当前页面，即当关闭当前页面时，存储在本地的数据会被清除。并且不同页面之间的`sessionStorage`是独立的，不能互相访问。`sessionStorage`的方法有

- setItem(key, value)：存储键值对
- getItem(key)：根据key获取对应的value
- removeItem(key)：删除key所对应的键值对
- clear()：清除`sessionStorage`本地缓存

`localStorage`的大小为20M左右，它的生命周期为当前浏览器。关闭浏览器不会清除数据，数据存储在硬盘上，只能手动清除。`localStorage`的方法同`sessionStorage`。