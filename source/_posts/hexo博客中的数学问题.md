---
title: Hexo中数学公式问题
categories: Hexo
tags: Hexo
date: 2020-02-29 12:23:42
cover: https://gitee.com/lastknightcoder/blogimage/raw/master/img/bg58.jpg
---

## 起因

之前我的 `Hexo` 主题使用的是 `Next` 主题，`Next` 主题因为对数学公式有进行处理，所以在用 `Next` 主题时写数学公式是十分的顺畅的，但是最近觉得Next主题颜色太淡了，所以就换了一个颜色比较鲜艳的 `Hexo` 主题，但是这一换，数学公式就有了麻烦。

首先我们要明白这个麻烦是怎么产生的，因为 `markdown` 的语法跟 `mathjax` 有重叠，比如 `_` 在 `mathjax` 中表示下标，但是在 `markdown` 中，被两个下划线 `_` 包裹起来的内容会被转为 `<em>` 标签，即其间的内容会被转为斜体，这就使得数学公式渲染不出来；另一个就是 `\\` 在 `mathjax` 表示换行，但是在解析 `markdown` 时却会被转义。

## 解决

这个时候我就去网上搜了解决办法，有好几种，比如第一种是修改 `marked.js(node_modules/marked/lib/marked.js)`，修改两个关键字为 `excape` 和 `em` 的正则表达式，这个方法我试了，但是没有用；第二个办法是卸载 `hexo` 默认的 `markdown` 解析 `hexo-renderer-marked`，然后安装 `hexo-renderer-pandoc`(好像 `Next` 就是这么干的)，前提是要安装 `Pandoc`，然后由于我的 `npm` 不知道出了什么原因，不能 `uninstall`，只能寻求它法。(不过你们可以尝试，解决办法链接[在这](https://segmentfault.com/a/1190000007261752))

不过功夫不负有心人，经过几个小时折腾，还是被我找到了解决办法，我在[hexo的issue](https://github.com/ppoffice/hexo-theme-icarus/issues/394)中看到，将有问题的数学公式如下处理：

<center>
    <img src="https://gitee.com/lastknightcoder/blogimage/raw/master/img/20200229131807.png"/>
</center>

这样数学公式就不会被 `marked` 解析，但是会被 `mathjax` 解析。

上面的做法可以解决大部分的问题，但是我们数学公式里面包含一对 `<>` 的话，又会出现问题，通过 `F12` 检查元素，发现被 `<>` 包裹的内容被误认为是标签了，导致渲染不出来，要解决这个办法，把公式中所有的 `<` 使用 `\lt` 替代，所有的 `>` 使用 `\gt` 替代，这样就可以了。

`Hexo` 的数学公式真的是折腾的我够呛，所以这次经历必须记录下来，为了下次遇到同样问题时能够迅速解决。


## 参考资料

- [关于数学公式的渲染问题](https://github.com/ppoffice/hexo-theme-icarus/issues/394)
- [Hexo下mathjax转义问题](https://segmentfault.com/a/1190000007261752)
