---
title: jQuery系统教程
categories: 
	- JavaScript
tags:
	- jQuery
	- JavaScript
keywords: jQuery 教程
description:
    jQuery系统教程
date: 2020-04-07
---

本篇文章是在完整学习 `jQuery` 以后，系统整理的教程，大纲如下

- `jQuery` 的核心函数 `$, jQuery`
- 操作 `jQuery` 集合
- 操作 `DOM` 元素的属性和类属性
- 操作样式
- 操作 `DOM` 元素(添加，删除...)
- `jQuery` 事件
- `jQuery` 显示与隐藏
- `jQuery` 动画
- `jQuery` 扩展及实用函数
- `Ajax`

## jQuery核心函数

`jQuery` 这个库中最重要的就是它提供的核心函数 `jQuery()` ，它提供了非常强大的功能，因为这个函数用的十分的频繁，所以提供了 `jQuery()` 的别名，那就是 `$()` ，在后面的演示中都将会以 `$` 为例，现在我们来看一下该核心函数提供了哪些功能

- 选择多个 `DOM` 元素
- 将 `HTML` 字符串得解析为一个 `DOM` 元素
- 作为入口函数
- 将 `DOM` 元素转换为 `jQuery` 集合

上面有些名词可能没有听过，这里只是感性了解一下，下面就具体讲解。

### 选择元素

考虑下面的 `DOM` 结构

```html
<div id="box"></div>
<div class="container"></div>
<p></p>
```

如果使用原生的 `JavaScript` 选择上面的元素的写法如下

```javascript
document.getElementById("box");
document.getElementsByClassName("container")[0];
document.getElementsByTagName("p")[0];
```

而现在使用 `$()` 函数，为其传入 `CSS` 的选择器即可选中元素

```javascript
$("#box");
$(".container");
$("p");
```

使用 `$()` 得到的是一个集合，即使使用了 `id` 选择器 `#box` ，得到也是一个只有一个元素的集合，如下

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20201028155129.jpg" alt="2020-10-28_154955" style="zoom:50%;" />


我们把它称为 `jQuery` 对象或者 `jQuery` 集合，在下面我将不会区分二者，可能会混合使用，请注意。由上可见， `jQuery` 对象的结构像是一个数组，但它不是真正的数组，因为他不是 `Array` 的实例，只是使用对象来模拟数组， `jQuery` 对象中的元素(借用数组的概念)是被选择的 `DOM` 元素。 `jQuery` 的核心函数支持几乎所有的 `CSS` 选择器，如

- 属性选择器

  ```css
  input[value] /* 选择有 value 属性的 input 标签 */
  input[type='text'] /* 选择 type 为 text 的 input 标签 */
  a[href^='http://'] /* 选择 href 以 http:// 开头的 a 标签 */
  a[href$='pdf'] /* 选择 href 以 pdf 结尾的 a 标签 */
  a[href*='jquery'] /* 选择 href 属性包含 jquery 的 a 标签 */
  ```

- 伪类选择器

  ```css
  a:first
  p:odd
  p:even
  ul li:last-child
  ```

- 过滤器

  ```css
  :animated /* 处于动画状态的元素 */
  :checkbox /* 相当于 input[type='chexkbox'] */
  :checked /* 选择处于选中状态的多选框或单选框 */
  :disabled
  ...
  ```

- ...

选择器十分的多，由于重点不在于选择器，另外这个也没有什么原理可讲，也就是 `API` 的使用，随着使用 `jQuery` 的增多，自然会掌握，所以这里就点到即止。

### 将 HTML 字符串转化为 jQuery 对象

这里首先解释一下什么叫做 HTML 字符串，不给出具体的定义，很好理解，形如下面的字符串就叫做 HTML 字符串

```javascript
"<p>123</p>"
"<h1>一级标题</h1>"
```

当我们向 `jQuery` 的核心函数传递这样的字符串时，核心函数会将该字符串解析得到一个 `jQuery` 对象，如

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20201028155257.jpg" alt="2020-10-28_155232" style="zoom:50%;" />


### 作为入口函数

因为我们一般会在 `JavaScript` 中操作 `DOM` ，所以我们一般会等到 `DOM` 树渲染完毕之后再去执行 `JavaScript` 代码，所以我们一般把 `script` 标签放在 `body` 元素的最后面，或者将代码放在 `window.onload` 中，如下

```javascript
window.onload = function() {
    // 要执行的代码
}
```

`onload` 方法会在 `DOM` 树渲染完毕，并且外部资源如图片等等加载完毕后会执行，虽然能够满足我们的需求，但是在等待静态资源加载的时候，用户是不能与页面进行交互的，这时用户就会感觉页面卡死了，体验感十分不好。我们只是希望在 `DOM` 树渲染完毕后就执行代码，那么可以使用 `ready()` 函数，如下

```javascript
$(document).ready(function() {
    // 要执行的代码
})
```

这里要执行的代码在 `DOM` 树渲染完毕后即可执行，而不必等到资源加载完毕。上面的写法可以简写为

```javascript
$(function() {
    // 要执行的代码
})
```

### 将 DOM 元素转化为 jQuery 对象

为核心函数传入 `DOM` 元素，会返回一个 `jQuery`对象，该对象包含了这个 `DOM` 元素，如下

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20201028155424.jpg" alt="2020-10-28_155403" style="zoom:50%;" />


将 `DOM` 元素转化为 `jQuery` 对象可以使用 `jQuery` 对象原型上的方法，这些方法想较于原生方法方便很多。

> 如何将 `jQuery` 对象转化为DOM元素呢? 我们知道 `jQuery` 对象的结构是类似于数组，其中的每一个元素都是 `DOM` 对象，我们可以使用下标的方式获取 `DOM` 元素，如下两种方法都是可以的
>
> - `jQuery对象[index]`
> - `jQuery对象.get(index)`，该方法可以接受负数，表示倒数

## 操作 jQuery 集合

在本节会使用 `jQuery` 集合这一叫法，因为往里面添加元素或者删除元素等等操作，集合的叫法比对象的叫法更加的符合。如果不做说明，下面操作的`DOM`为以下

```html
<div id="box"></div>
<div class="container"></div>
<p></p>
```

### 添加元素

使用 `jQuery` 集合调用 `add` 方法为集合添加元素， `add` 方法可以接收不同类型的参数，如下

- `jQuery` 选择器：将与 `jQuery` 选择器匹配的所有 `DOM` 元素添加到集合中
- `DOM` 元素或 DOM 元素组成的数组：将 `DOM` 元素或数组中的元素添加到集合中
- `HTML` 字符串：将 `HTML` 字符串解析为 `DOM` 元素后添加到集合中

该方法会返回一个新的 `jQuery` 集合，例子如下

```javascript
let collection = $("#box");
collection = collection.add(".container"); // 添加一个 jQuery 选择器
collection = collection.add(document.getElementsByTagName("p")[0]); // 添加一个 DOM 元素
collection = collection.add("<h1>123</h1>"); // 添加一个 HTML 字符串
console.dir(collection);
```

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20201028155628.jpg" width="100%">


### 过滤元素

过滤集合中的元素有两种方法，一个是 `not`，一个是 `filter`，两个方法的侧重点不同， `not` 方法会删除掉集合中符合条件的元素，返回新集合；而 `filter` 的语义是保留集合中符合条件的元素，返回一个新集合。二者接收的参数同 `add` 方法一样，除此之外，还可以接受一个函数，集合中的每一个元会素调用该函数，根据函数的返回值来决定元素的去留，比如对于 `not` 方法，如果函数返回 `true`，则删除此元素，对于 `filter` 则相反，如果返回 `true`，则保留此元素。这些传入的函数中的上下文(也就是 `this`)指向调用该函数的元素。

```javascript
let collection = $("#box");
collection = collection.add(".container"); // 添加一个 jQuery 选择器
collection = collection.add(document.getElementsByTagName("p")[0]); // 添加一个 DOM 元素
collection = collection.add("<h1>123</h1>"); // 添加一个 HTML 字符串
collection = collection.not(function() {
    return $(this).html() === "123"; // 删除掉 html 内容为 123 的，也就是 h1 标签
})
console.dir(collection);
```

根据上面的代码，此时集合中只有三个元素，将 `h1` 元素滤除了

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20201028155826.jpg" alt="2020-10-28_155805" style="zoom:50%;" />


```javascript
let collection = $("#box");
collection = collection.add(".container"); // 添加一个 jQuery 选择器
collection = collection.add(document.getElementsByTagName("p")[0]); // 添加一个 DOM 元素
collection = collection.add("<h1>123</h1>"); // 添加一个 HTML 字符串
collection = collection.filter(function() {
    return $(this).html() === "123"; // 保留 html 内容为 123 的，也就是 h1 标签
})
console.dir(collection);
```

将上面的 `not` 改为 `filter`，这时集合里面就只有一个元素，那就是 `h1` 元素

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20201028155934.jpg" alt="2020-10-28_155917" style="zoom:50%;" />


> 通过上面几个方法的学习，我们知道这些方法返回的都是一个新的 `jQuery` 集合，这意味着可以接着`.`，如下
>
> ```javascript
> let collection = $("#box")
>                     .add(".container")
>                     .add(document.getElementsByTagName("p")[0])
>                     .add("<h1>123</h1>")
>                     .filter(function() {
>                         return $(this).html() === "123";
>                     });
> ```
>
> 这样写起来明显比上面写起来爽多了，这种写法叫做链式编程，像链子一样，一个接着一个。

### 遍历集合

使用 `each` 方法遍历集合，该方法接收一个函数，集合中的每一个元素都会调用 `each` 中传入的函数，该函数的上下文会被设定为该元素，如

```html
<div>1</div>
<div>2</div>
<div>3</div>
<div>4</div>
<div>5</div>
```

```javascript
$("div").each(function() {
    console.log($(this).html())
})
```

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20201028160134.jpg" alt="2020-10-28_160115" style="zoom:50%;" />


### 集合查找

- `is(selector)`：判断是否集合中是否包含与选择器匹配的元素，包含返回 `true`，否则返回 `false`

- `has(test)`：`test` 可以为选择器，也可以为 `DOM` 元素，它判断的是集合中的元素的后代中是否包含与 `test` 匹配的元素，如果集合中某元素 `e` 的后代包含与 `test` 匹配的元素，则将元素 `e` 添加到新的集合中，最后返回该集合，如

  ```html
  <div>
      <ol>
          <li></li>
      </ol>
  </div>
  <div>
      <ul>
          <li></li>
      </ul>
  </div>
  ```

  ```javascript
  // $("div")中有两个元素，只有第一个div元素的后代有ol元素
  // 所以返回的新的集合中包含一个元素，是第一个div元素
  $("div").has("ol"); 
  // 两个元素的后代都有li元素，所以新集合中的元素与原集合相同
  $("div").has("li"); 
  ```

- `find(selector)`：该方法会遍历集合中的所有元素，在这些元素的后代找到匹配 `selector` 的元素，然后将这些匹配的元素添加到一个新的集合中，并返回该新的集合

  ```javascript
  $("div").find("img") // 在所有div标签中找到img标签，返回这些img标签组成的集合
  ```

### 转换集合

`jQuery` 对象有一个 `map` 方法，该方法与数组的 `map` 方法一样，接收一个回调函数，集合中的每一个元素都会调用该函数，该函数的返回值会被添加到新的集合中并返回(如果返回的是 `null` 或者 `undefined` 则不会添加到新的集合中)，该函数有两个参数，第一个参数是元素在集合中的下标，第二个参数是调用该函数的元素。

```html
<div>1</div>
<div>2</div>
<div>3</div>
<div>4</div>
<div>5</div>
```

```javascript
$("div").map(function(index, element) {
    return index;
    // get()方法会将集合转变为一个真正的数组
}).get().join(", "); //"0, 1, 2, 3, 4"
```

## 操作 DOM 元素的属性和类属性

### 操作属性

我们可以使用`attr()`方法方便的操作 `DOM` 元素的属性

- `attr(name, value)`：为名字为 `name` 的属性设置值为 `value`

  ```javascript
  $("div").attr("id", "box"); // 将所有 div 标签的 id 设置为 box
  ```

  <img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20201028160335.jpg" width="40%"/>
  
- `attr(name)`：获取集合中第一个元素中属性名字为 `name` 的值

- `attr(attributes)`：以对象的形式来设置属性值

  ```javascript
  $(".attr").attr({
      title: "cat", // 设置 title 属性为 cat
      id: "cat" // 设置 id 属性为 cat
  })
  ```
  
- `removeAttr(name)`：删除指定名称的属性

> 经过测试，发现 `attr()` 方法可以设置任意属性的值(我的意思是指即使这些属性是不存在的，比如  `dog` 也可以设置，而这些不存在的属性称为自定义属性)，比如
>
> ```javascript
> attr("dog", "dog"); // 为 dog 属性设置值为 dog
> ```
>
> 上面的 `dog` 就是自定义的属性名。在 `HTML5` 中建议自定义属性名以 `data-` 开头，如 `data-dog`， `HTML5` 会将 `data-` 开头的属性名添加到 `dataset` 的对象中，我们可以通过
>
> - `dataset["属性名"]`
>
> - `dataset.属性名`
>
> 的方式访问，这时的属性名不需要加上 `data-` 前缀，如
>
> - `dataset["dog"]`
>
> - `dataset.dog`
>
> 如果属性名之间使用 `-` 连接，如 `data-my-name`，则访问时需要使用驼峰命名法访问，即
>
> - `dataset["myName"]`
>
> - `dataset.myName`

#### 在新的页面打开标签

现在使用 `attr()` 方法来做几个例子，第一个要求页面内的链接都要在新的页面打开，做法就是将 `a` 标签的 `target` 属性设置为 `_blank` ，这种事情当然可以在写的时候就写好，但是有两个问题

- 首先这个工作是重复性的工作，每写一个 `a` 都要重复同样的事情，既累又不够优雅

- 如果页面内的链接是用户加入的呢，比如百科，这时打开的链接就不一定是在新的页面打开了

所以我们可以使用 `attr()` 批量设置 `a` 标签的 `target` 属性为 `_blank` ，但是并不是所有的 `a` 标签，因为有的 `a` 标签是在页面内跳转的，我们并不需要在新的页面内打开，所以应该设置 `href` 属性以 `http://` 开头的 `a` 标签在新的页面打开

```javascript
$("a[href^='http://']").attr("target", "_blank")
```

#### 解决双重提交

在提交表单时，有的时候由于不能够及时给予用户反馈，导致用户不确定是否提交成功，用户会多次点击提交按钮，这样会给服务器端带来麻烦，我们在捕获 `submit` 事件时，当用户第一次提交时，我们接着将 `disabled` 属性设置为 `true` (或者为 `disabled` )，如下

```javascript
// 绑定表单的提交事件
$("form").submit(function() {
    // :submit 表示 type 为 submit 的元素
    // 提交之后将submit按钮设置为disabled
    $(":submit", this).attr("disabled", "disabled") 
})
```

> 在上面中设置 `disabled` 属性时，`disabled` 属性的值不重要，重点是设置了 `disabled` 这个属性，所以即使将它设置为空字符串 `""`，它也是生效的，如果想 `disabled` 属性设置为不生效，那么就将它的值设置为 `false` (注意不要设置为字符串 `"false"` )，下面的写法作用是一样的，都是使得 `disabled` 生效
>
>- `attr("disabled", "disabled")`
>
>- `attr("disabled", "")`
>
>- `attr("disabled", "false")`
>
>- `attr("disabled", true)`
>
>只有将 `disabled` 设置为布尔值 `false` 才会使得 `disabled` 失效
>
>- **`attr("disabled", false)`**

### 操作类属性

为什么类属性要单独拿出来呢? 假设有这么一个需求，为所有的 `div` 标签添加一个类 `box`，那下面这样的写法对不对

```javascript
$("div").attr("class", "box"); 
```

上面的写法是错误的，因为上面的写法是为 `class` 属性赋值为 `box`，但是之前的类属性就会被覆盖掉，我们想做的仅仅是添加的操作，所以我们需要知道之前的类属性是什么，所以正确的写法如下

```javascript
// 获得原来的类名列表 转化为数组
const classList = $("div").attr("class").split(" ");
// 将要添加的类名添加到数组中
classList.push("box");
// 将数组中的元素以空格拼接为一个字符串
const newClassList = classList.join(" ");
// 设置class属性
$("div").attr("class", newClassList);
```

经过千辛万苦终于添加了一个类，考虑到这样的操作很频繁，所以最好将上面的操作抽象为一个方法，而 `jQuery` 为我们做了这件事情：

- `addClass(names)`

  - `names` 可以是单个类名，也可以是一个以空格分隔的字符串表示的多个类名

  - `names` 还可以是一个函数，函数的返回值作为单个类名或者多个类名，这个函数会传入两个参数，元素的下标(元素在 `jQuery` 集合中的下标)和当前类名的值

- `removeClass(names)`

  - 参数同 `addClass` ，作用是删除单个或多个类名

- `toggleClass(names)`

  - 该函数的作用是切换类名，什么意思呢? 比如如果该元素存在 `names` 指定的类，则删除掉指定的类，如果不存在指定的类，则添加指定的类

  - 参数同上

```javascript
addClass("box"); // 为元素添加 box 类
addClass(function(index, value) {
    // index 是元素在 jQuery 中的下标
    // value 是元素当前类名的值
    // return "active big" 
    return "box"; // 返回值将会被作为类名添加
})

removeClass("box"); // 删除该元素 box 类

toggleClass("box"); // 如果存在 box 类，则删除 box 类，如果不存在 box 类，则添加 box 类
```

- `hasClass(name)`
  - 判断是否包含类名为 `name` 的类，包含则返回 `true`，否则返回 `false`


>`HTML5` 其实也提供了原生的操作类的方法，每个元素都有一个 `classList` 对象，它的原型对象提供了操作类名的方法
>
>- `classList.add()`：为元素添加**一个**类
>
>- `classList.remove()`：为元素删除指定的类
>
>- `classList.toggle()`：切换指定的类，存在则移除，不存在则添加
>
>- `classList.contains()`：判断是否存在指定的类，存在返回 `true`，反之返回 `false`
>
>据我试验所知，上面的每次操作只能对一个类操作，意思就是不能同时添加多个类或删除。

## 操作样式

使用 `css()` 方法来设置样式，使用的方法与 `attr()` 类似

- `css(name, value)`：设置样式 `name` 为 `value`
- `css(properties)`：传入一个对象，通过对象来设置样式
- `css(name)`：获得样式，如果是含有多个元素的 `jQuery` 集合调用此方法，那么返回的是第一个元素的计算样式值

看一个例子，有下面的 `div` 标签

```html
<div></div>
```

现为它设置高度和宽度

```javascript
$("div").css({
    width: "100px",
    height: "100px"
})
```

> css(name, value)：其中 `value` 可以是一个函数，该函数的返回值将作为 `value` 值，这个函数会接收两个参数，第一个参数是元素在 `jQuery` 集合中的位置，第二个参数是样式当前的值，同时函数的内部上下文 `this` 指向当前元素

### 获取元素宽度

下面我们来获取某元素的 `width` 值

```javascript
$("div").css("width"); // 100px
```

得到的结果是一个一个字符串，而不是数字，很多时候我们是希望得到数字用来计算或者其他用途，所以如果想得到数字的话就需要对字符串进行解析，但是 `jQuery` 有考虑这一方面，我们可以很方便的使用 `width(), height()` 方法获得数字的结果，如

```javascript
$("div").width(); // 100
```

> 这里的 `width` 的大小是设置样式时的大小吗? 比如
>
> ```css
> div{
>     width: 100px;
> }
> ```
>
> 使用 `width()` 方法得到的数字是100吗? 为什么在这里我会提出这么一个疑问，因为这里设置的 `width` 不一定是标签的内容宽度，也可能包括 `padding, border` (当 `box-sizing` 设置为 `border-box` 时)，所以这里的 `width` 得到的是 `content-width` 还是在 `style` 标签中设置的 `width` ，我们设置 `div` 的样式为
>
> ```css
> div {
>     box-sizing: border-box;
>     padding: 10px;
>     width: 100px;
> }
> ```
>
> 这个时候在 `style` 里面设置的 `width` 为 `100` ，但是实际的 `content-width` 应该是 `80` ，我们来看一下 `width()` 方法得到的结果是多少
>
> ```javascript
> $("div").width(); // 80
> ```
>
> 上面得到的结果是 `80` ，所以 `width()` 方法得到的宽度是 `content-width` ，即是实际内容的宽度， `height()` 方法也是同理。

除了可以使用 `width()` 方法获得元素的宽度以外，还可以设置元素的宽度

- `width(value)`
  - `value` 的值可以是一个数字，单位默认为 `px`
  - 也可以是带单位的字符串(如果没有指定单位，则默认为 `px`)
  - 还可以是一个函数，函数的返回值作为要设置的值，该函数的上下文(`this`)是该元素

> 这里又有一个疑问，如果我设定宽度，那么这里的宽度我设定的是内容宽度 `conetnt-width` 还是元素的 `width` 样式，这里我们还是以上面的 `div` 为例
>
> ```css
> div {
>     box-sizing: border-box;
>     padding: 10px;
>     width: 100px;
> }
> ```
>
> 这里我们设定 `width` 为 `200` ，然后通过 `width()` 方法获得宽度，如果得到的值是 `200` ，那说明设定的是 `content-width` ，如果得到的是 `180` ，说明设定的是 `width` 样式的值
>
> ```javascript
> $("div").width(200);
> console.log($("div").width()); // 200
> ```
>
> 得到的结果是 `200` ，说明设定的是 `content-width` 。

> 在这里我突然又有一个想法，那就是 `css("width")` 与 `width()` 得到的结果是相同的吗，还是以上面的 `div` 为例
>
> ```css
> div {
>     box-sizing: border-box;
>     padding: 10px;
>     width: 100px;
> }
> ```
>
> ```javascript
> console.log($("div").width()); // 80
> console.log($("div").css("width")); // 100px
> ```
>
> 惊讶的发现二者得到的结果是在数值上是不一样的，可见使用 `css("width")` 获得的是 `width` 样式的值，那么使用 `css("width", 200)` 设定的是 `width` 样式的值吗，测试一下
>
> ```javascript
> $("div").css("width", 200);
> console.log($("div").width()); // 180
> console.log($("div").css("width")); // 200px
> ```
>
> 果然 `css()` 方法设置的值也是 `width` 样式的值，所以这里总结一下：
>
> - `width()`(`height()`)方法设定或者获取的值是指**内容的宽度(高度)**
> - `css()` 设定或获取的宽度(高度)是指样式里面的 `width` 值的大小
> - 在 `box-sizing` 为 `content-box` 下二者得到的数值结果是一样的，但是在 `box-sizing` 为 `border-box` 的情况下，由于 `padding` 和 `border` 的影响，二者的结果是不同的(在 `padding` 和 `border` 为 `0` 的情况下是相同的)。

> 经过一次实践，我发现上面的描述也不太准确，`css("width")`与`width()`方法获得的值符合上面情况的条件是，它的大小没有超过它父元素的大小，如果超过了，那么`css("width")`就是父元素的`width`样式的大小，而`width()`方法的大小我觉得很复杂，还不知道怎么计算，自己动手试试吧。

除了 `width()` 和 `height()` 方法，`jQuery` 还提供了特别的方法来获得特殊的宽度和高度

| 方法                  | 描述                                                         |
| --------------------- | ------------------------------------------------------------ |
| `innerwidth()`        | 内容宽度+内边距宽度                                          |
| `innerHeight()`       | 同上                                                         |
| `outerWidth(margin)`  | <ul><li>margin为false时，内容宽度 + 内边距宽度 + 边框宽度</li><li>margin为true时，再加上外边距的宽度</li></ul> |
| `outerHeight(margin)` | 同上                                                         |

### 获取元素位置

#### offset

`offset()`，获取元素离文档原点的距离，返回一个对象，里面有 `left` 和 `top` 两个属性，表示离文档原点经过测试，应该是元素的 `border` 的左上角离文档原点的距离。

#### position

另一个获取位置有关信息的是 `position()` ，它是获得与它元素左上角之间的距离，这个父元素是它所有父元素中使用了定位 `position` 且离他最近的父元素，虽然有点拗口，不过使用过绝对定位的话，就会很容易明白，因为它与绝对定位的规则是一样的。

> 经过我的实验，发现这个距离不是边框与边框的距离，而是子元素 margin 离父元素 padding 之间的距离。

## 操作 DOM 元素

### 设置 html 内容以及文本内容

- `html()`：获取jQuery集合中第一个元素的 `html` 内容
- `html(content)`：将传入的 `HTML` 片段，设置 `jQuery` 集合中元素的 `HTML` 内容
- `text()`：获得 `jQuery` 集合中所有元素的文本内容拼接的字符串
- `text(content)`：将 `jQuery` 集合元素的文本内容设置为传入的值

> `content` 可以为字符串，也可以是一个函数，函数的返回值作为要设置的内容，函数接受两个参数，第一个参数是元素在 `jQuery` 集合中的位置，第二个参数是当前`html`(`text`)的值。

### 追加元素

- `append(content)`：将 `content` 追加元素内容的最后面(子元素级别)
- `prepend(content)`：将 `content` 追加元素内容的最前面
- `before(content)`：将 `content` 追加到元素的前面(兄弟元素级别)
- `after(content)`：将 `content` 追加到元素的后面

> `content` 的值可以为字符串，元素，jQuery集合，函数，函数的返回值将被添加，函数将接受两个参数，元素的下标和原先的内容。
>
> 如果选择现有的DOM元素添加到某元素的最后，那么这是一个移动操作，而不是一个复制操作，比如有这么一个结构
>
> ```html
> <div></div>
> <p></p>
> ```
>
> 执行下面的语句
>
> ```javascript
> $("div").append($("p"));
> ```
>
> 得到的结果是
>
> ```html
> <div>
> 	<p></p>
> </div>
> ```
>
> 而不是
>
> ```html
> <div>
> 	<p></p>
> </div>
> <p></p>
> ```
>
> 如果`jQuery`集合中有多个元素，那么被添加的元素会复制多份，添加到这些元素内容的后面(如果添加的是`DOM`中已有的元素，也是移动操作)，其它的操作(`prepend`...)也是同理。

- `appendTo(target)`
- `prependTo(target)`
- `insertBefore(target)`
- `insertAfter(target)`

这些方法的作用同上面介绍的是个方法是一一对应的，不过是主动变为了被动，如

```javascript
$("div").append(content); // 向div中添加content
$("div").appendTo(target); // 将div添加到target中
```

> `target` 只能为元素或者字符串

### 删除元素

- `remove(selector)`：删除 `jQuery` 集合中的元素，`selector` 是一个可选的选择器，用来进一步筛选要删除的元素，被删除的元素会放入一个新的 `jQuery` 集合中返回，所以被删除的元素可以继续插入到 `DOM` 中，但是这个时候之前绑定的事件和数据全部被删除了。
- `empty()`：删除 `DOM` 元素中的内容，与 `remove` 不同， `remove` 是删除这个 `DOM` 元素，包括这个元素包括的内容，但是 `empty` 只是清除该 `DOM` 元素的内容。
- `detach(selector)`：该方法的作用与 `remove` 是一样的，但是保留绑定的事件和绑定的数据。

### 复制元素

- `clone(cloneHandler)`：复制 `jQuery` 集合中的元素，生成一个新的 `jQuery` 集合返回。 `cloneHandler` 如果为 `true`，那么就会复制绑定的事件，如果为 `false` 就不会复制绑定的事件

### 替换元素

- `replaceWith(content)`：使用 `content` 去替换 `jQuery` 集合中的元素，返回被替换的元素， `content` 可以是字符串，元素和函数，函数的返回值作为替换的内容，函数的上下文被设置为当前的元素，不传递任何的参数；如果 `content` 是已经在`DOM`中的元素，那么就相当于是移动到 `jQuery` 集合中元素的位置，如果 `jQuery` 集合中有多个元素，会复制多个 `content` 副本
- `replaceAll(selector)`：使用 `jQuery` 集合中的所有元素去替换匹配 `selector` 的元素，如果`jQuery`集合中的元素是已经在 `DOM` 中的元素，那么就相当于将符合 `selector` 的元素删除，然后将 `jQuery` 集合中的元素移动到这里，如果符合 `selector` 的元素有多个，那么会复制多个 `jQuery` 集合的副本移动

### 处理表单元素

- `val()`：获得集合中第一个元素的 `value` 值，如果该元素是一个可以多选的元素，那么返回所有选择项组成数组(比如`<select mupltiple="mupltiple"></select>`)

  > 如果第一个元素不是表单元素，则会返回空字符串

- `val(value)`：设置集合中表单元素的 `value` 值

  > value值也可以是函数，函数的返回值作为被设置的内容，函数接收两个参数，元素的下标和当前的value值

- `val(values)`：使得多选框，单选框，或者`select`元素中的可选项中，如果这些选项的值在`values`中，则它变为选中状态

## jQuery 事件

### 绑定事件

`jQuery` 使用 `on` 方法绑定事件，如下

- `on(eventType, childSelector, data, listener)`
  - `eventType`，必需，要绑定的事件的类型，如 `click`
  - `childSelector`，可选，规定把事件绑定到子元素上
  - `data`，可选，规定传递到函数的额外数据
  - `listener`，可选，规定事件发生时运行的函数

`on` 方法除了给已经存在于`DOM`中的元素绑定事件以外，还可以给现在还不存在于 `DOM` 中但是未来会添加到 `DOM` 中的元素绑定事件，如

```html
<div></div>
```

此时 `DOM` 中只有 `div` 标签，这时我想给未来会被添加到 `DOM` 中的 `p` 标签绑定点击事件

```javascript
$("p").appendTo($("div")); // 将p标签添加到div中

// 为div标签中的p标签绑定事件，即使p标签一开始并不在DOM中，也可以绑定成功
$("div").on("click", "p", function() {
    
})
```

使用 `one()` 绑定事件，该事件在执行完一次后会被删除

- `one(eventType, data, listener)`
  - 各参数的含义与 `on` 是一致的

除了可以使用上面的方法绑定事件以外，`jQuery` 还提供了十分方便的绑定特定事件方法，如

```javascript
// 为div标签绑定点击事件
$("div").click(function() {
    
})
```

这样直接以事件的名字绑定事件，既方便，语义又明确，支持这种方式的 `eventTypeName` 如下

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20201028160953.jpg" alt="2020-10-28_160902" style="zoom:50%;" />


### 解绑事件

- `off(event, selector, listener)`：移除通过 `on` 方法绑定的事件

### 触发事件

- `trigger(event, data)`
  - 这个方法是用来触发事件的，比如 `$("#foo").trigger("click")` 会触发 `id` 为 `foo` 的点击事件
  - `data` 是可选参数，是传递给事件处理的额外参数，它是一个数组或者对象

> 来解释一下 `data`，加入在绑定事件时需要额外的参数，如
>
> ```javascript
> // 需要额外的参数a, b
> $("#foo").click(function(event, a, b) {
>     
> })
> ```
>
> 这时我们在触发事件时就需要传入额外的参数
>
> ```javascript
> $("#foo").trigger("click", ["foo", "bar"])
> ```

- `triggerHandler(event, data)`

  - `triggerHandler` 的作用与 `trigger` 类似，不过有几点不同

> - `.triggerHandler()` 方法并不会触发事件的默认行为(例如，表单提交)。
>
> - `.trigger()` 会影响所有与 `jQuery` 对象相匹配的元素，而 `.triggerHandler()` 仅影响第一个匹配到的元素。
> - 使用 `.triggerHandler()` 创建的事件，并不会在 `DOM` 树中向上冒泡。
> - `.triggerHandler()` 返回最后一个处理的事件的返回值，而不是 `jQuery` 集合，即不能进行链式调用

除了上述的方法外，`jQuery` 还提供了可以直接通过名字直接触发事件，如

```javascript
$("#foo").click() // 触发点击事件
```

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20201028160953.jpg" alt="2020-10-28_160902" style="zoom:50%;" />


### 其他事件

- `toggle(listener1, listener2, ...)`
  - 当点击元素时会依次触发事件
  - 比如第一次点击触发 `listener1` ，第二次点击触发 `listener2` ，以此类推，如果 `toggle` 中的事件触发完了，那么会循环触发

- `hover(enterHandle, leaveHadnler)`
  - 当鼠标进入元素时触发 `enterHandle` ，离开元素时触发 `leaveHadnler`
- `hover(handler)`
  - 当鼠标进入元素以及离开元素时均会触发 `handler`

### Event实例

我们向元素绑定事件时，会向监听器(绑定的处理函数)传入一个 `event` 对象，那么 `event` 对象中有什么信息可以为我们所用呢?

| 属性            | 含义                                                         |
| --------------- | ------------------------------------------------------------ |
| altKey          | 当事件触发时，如果 Alt 键被按下，设置为 true，否则为 false   |
| ctrlKey         | 当事件触发时，如果 Ctrl 键被按下，设置为 true，否则为 false  |
| shiftKey        | 当事件触发时，如果 Shift 键已被按下，则设置为 true，否则为 false |
| target          | 触发事件的元素                                               |
| currentTarget   | 冒泡阶段的当前元素，它和事件处理器中 this 对象是同一个对象   |
| relatedTarget   | 对于鼠标事件，找出触发事件时光标离开或者进入的元素           |
| pageX/pageY     | 对于鼠标事件，指定触发事件时光标相对于页面原点的水平/垂直坐标 |
| screenX/screenY | 对于鼠标事件，指定触发事件时光标相对于屏幕原点的水平/垂直坐标 |
| timestamp       | jQuery.Event实例创建时的时间戳，以毫秒为单位                 |

| 方法                            | 含义                                                         |
| ------------------------------- | ------------------------------------------------------------ |
| preventDefault()                | 阻止默认动作(表单提交，链接跳转，复选框状态改变)             |
| stopPropagation()               | 停止冒泡                                                     |
| stopImmediatePropagation()      | 停止冒泡，包括当前元素上的事件                               |
| isDefaultPrevented()            | 如果已经在此实例上调用了 preventDefault() 方法，则返回 true  |
| isPropagationStopped()          | 如果已经在此实例上调用了 stopPropagation() 方法，则返回 true |
| isImmediatePropagationStopped() | 如果已经在此实例上调用了 stopImmediatePropagation() 方法，则返回 true |


## jQuery 显示与隐藏

### 显示和隐藏元素

- `hide()`：隐藏元素
- `show()`：显示元素

> 隐藏元素是将元素的 `display` 属性设置为 `none`，而显示属性会将 `display` 属性设置为与隐藏前的 `display` 相同，比如在隐藏前某元素的 `display` 为 `block`， `hide` 会将 `dispaly` 设置为 `none`，`show` 会将 `display` 设置为与之前相同为 `block`。

- `toggle()`：切换，如果为显示，则隐藏，如果隐藏，则显示

### 显示隐藏动画

上面的方法其实还可以接受两个参数

- `hide(speed, callback)`
  - `speed` 用来规定隐藏的速度，默认是没有动画效果的，元素瞬间消失，可以传入数字，单位是毫秒，也可以传入以下预定义的字符串，`slow, normal, fast`
  - `callback` 是动画执行完毕后会执行的回调函数

> 同理，`show()` 和`toggle()` 也由这两个参数，与`hide()` 的用法一模一样，所以不再赘述。

假设有一个 `div` 标签，当我们点击它时，它会先隐藏，会显示

```javascript
$("div").click(function() {
    $(this).hide("slow").show("fast")
})
```

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20201028171123.gif" alt="2" style="zoom:50%;" />


除了上面方法显示和隐藏元素以外，`jQuery` 还提供了很多的方法，如

> 淡入淡出(只改变元素的不透明度)：
>
> - `fadeIn(speed, callback)`
> - `fadeOut(speed, callback)`
> - `fadeTo(speed, opacity, callback)`：逐渐改变元素的透明度到设定的opacity
>
> 我们在使用 `hide` 或者 `show` 时，除了改变透明度以外，还会改变高度和宽度，而 `fade...` 方法只会改变不透明度，当 `opacity` 为 `0` 时，将 `display` 设置为 `none`
>
> ```javascript
> $("div").click(function() {
>  $(this).fadeOut("slow").fadeIn("fast")
> })
> ```
>
> <img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20201028171305.gif" alt="3" style="zoom:50%;" />
> 
> 
>滑入滑出：
> 
>- `slideDown(speed, callback)`：改变元素的垂直尺寸，使元素逐渐显示出来
> - `slideUp(speed, callback)`：改变元素的垂直尺寸，使元素逐渐消失
> - `slideToggle(speed, callback)`：隐藏则显示，显示则隐藏
> 
>```javascript
> $("div").click(function() {
>  $("div").slideUp("slow").slideDown("fast")
> })
> ```
> 
><center>
>  <img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/202004011106.gif"/>
> </center>

## jQuery 动画

- `animate(properties, duration, easing, callback)`
  - `properties`：元素最后达到的 `CSS` 状态
  - `duration`：持续时间
  - `easing`：动画曲线，`jQuery` 提供两个函数曲线 `linear` 和 `swing`
  - `callback`：动画执行完毕后的会执行的回调函数

我们通过给定希望元素最后能够达到的 `CSS` 状态，比如位置的变化，宽高的变化，而 `jQuery` 会自动根据持续的时间以及初始的状态计算出中间的过程，也就是帧，如

```javascript
$("div").click(function() {
    $("div").animate({
        width: "200px"
    }, 500, "swing")
})
```

<center>
    <img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/202004011118.gif"/>
</center>

### 自定义缩放动画

```javascript
$("div").hover(function() {
    // 鼠标放上去时，放大两倍
    $("div").animate({
        width: $(this).width() * 2,
        height: $(this).height() * 2
    })
}, function () { 
    // 离开时恢复原样
    $("div").animate({
        width: $(this).width() / 2,
        height: $(this).height() / 2
    })
})
```

<center>
    <img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/202004011122.gif"/>
</center>

### 自定义掉落动画

```javascript
$("div").click(function() {
    $(this).css({
        position: "relative"
    }).animate({
        opacity: 0,
        // 计算要下降的高度
        top: $(window).height() - $(this).height() - $(this).position().top
    }, "slow", function() {
        // 因为 opacity 为 0 只是不可见，但是仍然占据位置，所以最后要hide隐藏一下
        // 可以将 opacity 最后的状态设置为 hide，那么这个回调函数可以省略
        $(this).hide()
    })
})
```

<center>
    <img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/202004011129.gif"/>
</center>

> 将 `$.fx.off` 设置为 `true` 会禁用动画

## jQuery扩展及实用函数

### jQuery与其他库并存

- `$.noConflict()`：将 `$` 控制权给其他库，在执行该行代码后不能使用 `$`，只能使用 `jQuery`

### jQuery实用函数

- `$.trim(value)`：去除字符串前后的空白字符(不仅仅是空格，还有回车等等)
- `$.each(container, callback)`：会为 `container` (数组|对象)中的每个元素调用 `callback`，`callback` 的参数就是 `container` 中的每个元素
- `$.grep(array, callback, invert)`：为数组中的每个元素调用 `callback`，根据 `callback` 的返回值决定是否将元素添加到新的数组，如果 `invert` 为 `true`，那么 `callback` 返回 `false` 则将元素添加到新数组，反之若 `invert` 为 `false`，则 `callback` 返回 `true` 则将元素添加到新的数组，最后将新数组返回
- `$.map(array, callback)`：这个方法不想多解释
- `$.inArray(value, array)`：方法 `value` 在 `array` 中第一次出现的下标
- `$.makeArray(object)`：将一个伪数组转换为一个真正的数组
- `$.unique(array)`：返回 `array` 中不重复的元素组成的数组
- `$.merge(array1, array2)`：将第二个数组合并到第一个数组中，并返回第一个数组

> 上面的方法大多数 `JavaScript` 已经有原生的实现了

### 扩展jQuery

`$.fn.xxx`：该方法会被添加到 `jQuery` 对象的原型上，通过这种方法可以扩展 `jQuery`

## Ajax

### 启动一个Node来进行服务端测试

```javascript
const http = require('http');

const server = http.createServer();

server.on('request', (req, res) => {
    // 处理跨域 
    // 允许http://127.0.0.1:5500 跨域请求
    res.setHeader('Access-Control-Allow-Origin', 'http://127.0.0.1:5500');
    res.end("Hello World");
})

server.listen(4000, () => {
    console.log("服务启动在4000端口");
})
```

### 回顾Ajax

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <script src="https://cdn.bootcss.com/jquery/3.4.1/jquery.js"></script>
</head>
<body>
    <button>发送请求</button>
    <ul>

    </ul>

    <script>
        $("button").click(function () {
            let xhr = new XMLHttpRequest();
            // 设置请求方法和请求路径
            xhr.open("GET", "http://localhost:4000");
            
        	// 监听onreadystatechange，当readyState改变时会触发此函数
            xhr.onreadystatechange = function () {
                // readyState为4表示处理请求完成
                if (this.readyState == 4) {
                    if (this.status >= 200 && this.status < 300) {
                        // 成功
                        $("ul").append(`<li>${this.responseText}</li>`)
                    } else {
                        // 失败
                        alert("失败");
                    }
                }
            }
            // 发送的请求体数据 get方法不传参数，或者传入null
            xhr.send();
        })

    </script>
</body>
</html>
```

上面的代码是当点击`button`时发送一个`ajax`请求，并且获得的内容添加到`ul`中，效果如下

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20201028163216.gif" alt="1" style="zoom:50%;" />

`XMLHttpRequest` 对象拥有的属性和方法为

| 属性和方法                                   | 含义                                                         |
| -------------------------------------------- | ------------------------------------------------------------ |
| abort()                                      | 取消当前请求                                                 |
| getAllResponseHeaders()                      | 获得所以的响应头及相应的值组成的字符串                       |
| getResponseHeader()                          | 获得指定名称的响应头和值                                     |
| open(method, url, async, username, password) | 设置请求方法和 URL，可以设置同步请求和异步请求，也可以提高基于容器认证需要的用户名和密码 |
| send(content)                                | 发送带有指定内容的请求                                       |
| setRequestHeader(name, value)                | 设置请求头                                                   |
| onreadystatechange                           | 当状态改变时，触发此方法接收的回调函数                       |
| readyState                                   | 目前的请求状态：<br /><ul><li>0：已创建 XHR 对象，但是未调用 open 方法</li><li>1：已调用 open 方法，但是请求还未发出</li><li>2：已调用 send 方法</li><li>3：服务端正在处理，responseText 中已有部分数据</li><li>4：响应已经接收完成</li></ul> |
| reponseText                                  | 响应体中的数据                                               |
| responseXML                                  | 如果返回的内容指定为 XML，会复制一份 reponseText 的内容到这里 |
| status                                       | 服务端返回的状态码                                           |
| statusText                                   | 返回的文本状态信息，如 success ok                            |


### jQuery 发起 Ajax 请求

- `get(url, parameters, callback, type)`
  
  - `url`：请求的路径
  - `parameters`：请求参数，会被拼接到 `url` 后
  - `callback`：请求成功后调用的函数
  - `type`：以何种方式处理响应数据，如 `html`，`json`
- `getJSON(url, parameters, callback)`：相当于是 `get(url, parameters, callback, type)` 中 `type` 为 `json` 的快捷写法

- `post(url, parameters, callback, type)`：同 `get`，不过请求的方法是 `POST`，而且请求数据是放在请求体中的
- `ajax(options)`：
  
  - 通过传入的选项 options 可以控制发送 `ajax` 请求的各种细节
  
    | 参数        | 含义                                                         |
    | ----------- | ------------------------------------------------------------ |
    | url         | 请求的地址                                                   |
    | type        | 请求的方法，如 GET POST，默认为 GET                          |
    | data        | 请求参数，如果是 GET，则把参数放入请求地址中，如果是 POST，则把数据作为请求主题 |
    | processData | 将上面的 data 进行处理为 URL 编码格式，如果指定为 false，则不进行此操作 |
    | contentType | 指定请求内容的类型                                           |
    | cache       | 是否启动浏览器缓存，默认为 true                              |
    | timeout     | 超时时间(单位毫秒)，超过改时间没有得到应答，则调用处理错误的回调函数 |
    | async       | 如果指定 false，则将请求作为同步请求来提交，默认为异步       |
    | beforeSend  | 在发起请求之前调用的函数                                     |
    | success     | 返回成功状态码时调用的函数，第一个参数为响应主体，第二个参数为消息字符串，第三个参数为 XHR 实例的引用 |
    | error       | 返回错误状态码时调用的函数，第一个参数为 XHR 实例，第二个参数为消息字符串，第三个参数为异常对象 |
    | complete    | 请求结束时调用的方法，无论成功还是失败，接收两个参数，第一个参数为 XHR 实例，第二个参数为消息字符串 |
    | xhr         | 用来提供 XHR 实例自定义实现的回调函数                        |

`jQuery` 的 `ajax` 方法为我们提供了这么多可选项，如果没有设置一些选项的话，就会使用默认值，使用 `ajaxSetup` 方法来设置默认值

- `ajaxSetup(options)`

```javascript
$.ajaxSetup({
    type: 'POST',
    timeout: 5000
})
```

那么后面的每个 `ajax` 调用都会使用这些默认值(`$.get()` 的 `HTTP` 方法不会被改为 `POST`)。