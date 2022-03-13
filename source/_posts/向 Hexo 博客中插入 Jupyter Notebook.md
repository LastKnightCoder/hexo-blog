---
title: 向Hexo博客中插入Jupyter Notebook
categories: Hexo
tags: Hexo
date: 2020-10-28
---

因为 Python 的笔记使用 Jupyter Notebook 进行记录十分的方便，并且 Jupyter Notebook 本身也支持 Markdown 的语法，所以在学习 Python 有关的内容时我喜欢使用 Jupyter Notebook 进行记录，但是一般我的笔记都分布在 Hexo 博客中，但是 Hexo 博客只能解析 Markdown，所以如何将 Jupyter Notebook 整合到 Hexo 博客中成了我的一大难题，经过几次的折腾，算是比较完美的解决了我的需求。

下面介绍我依次使用的方法

- 将 Jupyter Notebook 转 Markdown 文件
- 将 Jupyter Notebook 上传到 Github 上，然后通过 nbviewer.jupyter.org 查看，然后使用 iframe 插入到页面中
- 将 Jupyter Notebook 转为 HTML，上传到 Github(Gitee)，启动 Github(Gitee) Page 服务，通过 iframe 引入 HTML 页面进行插入

## 转为Markdown

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20201027231654.png" alt="image-20201027231653854" style="zoom:50%;" />



通过上图方法将 jupyter notebook 转为 Markdown，然后放在 `_post` 目录下即可。转换出来的 Markdown 文件肯定跟 notebook 中的样式有所不同的，所以还要花时间调一下，不过相比 Jupyter Notebook 还是很丑。

## 上传到Github

将 `.ipynb` 文件上传到 Github 中，然后复制文件在 Github 中的链接，例如

```http
https://github.com/LastKnightCoder/numpy-note/blob/main/numpy%E5%85%A5%E9%97%A8.ipynb
```

然后打开 https://nbviewer.jupyter.org/

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20201027234152.png" alt="image-20201027234152241" style="zoom:50%;" />

然后回车，进入查看页面，复制该页面的链接

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20201027234011.png" alt="image-20201027234011702" style="zoom:50%;" />

然后在 Markdown 文件中使用 iframe 标签引用该链接，如

```html
<iframe src="https://nbviewer.jupyter.org/github/LastKnightCoder/numpy-note/blob/main/numpy%E5%85%A5%E9%97%A8.ipynb" width="100%" height="600"></iframe>
```

效果如下

<iframe src="https://nbviewer.jupyter.org/github/LastKnightCoder/numpy-note/blob/main/numpy%E5%85%A5%E9%97%A8.ipynb" width="100%" height="600"></iframe>

效果还不错，除了文档的开头和结尾有 `nbviewer` 的标志。

## 转为HTML插入

上面的第二种方法我已经比较满意了，但是 Jupyter Notebook 的样式改变不了，例如字体，我对字体的要求还是比较高的。

目前我又找到一种更好的办法，可以更好的满足我的需求，那就是将 Jupyter Notebook 转为 HTML 文件，因为在 HTML 文件中可以引入自定义的样式，这样就可以对文档的样式进行更改，从而达到我的要求，首先将文件下载为 HTML 文件

<img src="https://gitee.com/lastknightcoder/blogimage/raw/master/20201027235033.png" alt="image-20201027235033702" style="zoom:50%;" />

因为下载的 HTML 文件会默认的引用当前目录下的 `custom.css` 的样式，所以我们只要在 HTML 文件所在的目录下新建 `custom.css` 文件即可修改样式了。

接下来就是如何将这个 HTML 文件插入到 Markdown 文件中了，当然还是使用 iframe 标签进行插入。首先我们将 HTML 文件及 `custom.css` 文件上传到 Gitee 的一个仓库中，然后启动 Gitee Page 服务，接着我们就可以访问该 HTML 文件了，例如可以通过 https://lastknightcoder.gitee.io/jupyter-for-iframe/numpy%E5%85%A5%E9%97%A8.html 访问我上传的 HTML 文件，接着我们就在 iframe 中引用即可

```html
<iframe src="https://lastknightcoder.gitee.io/jupyter-for-iframe/numpy%E5%85%A5%E9%97%A8.html" width="100%" height="600"></iframe>
```

效果如下

<iframe src="https://lastknightcoder.gitee.io/jupyter-for-iframe/numpy%E5%85%A5%E9%97%A8.html" width="100%" height="600"></iframe>

可见我对样式进行了修改，不过还有一个地方令我不满意的就是嵌入的 iframe 有滚动条，对于一个有强迫症的人来说真的很难受，我想到使用 jQuery 来获得页面的高度，然后将 iframe 的 height 值设为该高度 +50 作用即可，通过在生成的 HTML 文件的最后加入下面的脚本就可以获得文档的高度

```html
<script>
    let script = document.createElement("script");
    script.src = "https://cdn.bootcdn.net/ajax/libs/jquery/2.1.0/jquery.min.js";
    script.text = "text/javascript";
    document.body.append(script);
    console.log($("#notebook").height())
</script>
```

例如我获得的高度为 7191，我将高度设置 7200 就没有滚动条了，如下

```html
<iframe src="https://lastknightcoder.gitee.io/jupyter-for-iframe/numpy%E5%85%A5%E9%97%A8.html" width="100%" height="7200"></iframe>
```

<iframe src="https://lastknightcoder.gitee.io/jupyter-for-iframe/numpy%E5%85%A5%E9%97%A8.html" width="100%" height="7200"></iframe>

从此就可以安心的使用 Jupyter Notebook 写程序了。