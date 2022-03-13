---
title: 记一次 React 中 CSS 处理事件
categories: 
    - React
    - 踩坑
tags:
	- React 踩坑
date: 2020-09-22
---

事件的起因是对一个使用传统网页开发的 `Web` 项目(即 `html, css, javascript` 分开写的项目)重构为 `React` 项目。

这个事情大部分都是复制粘贴，假设有以下的文件目录

```
Main
    index.jsx
    index.module.css
```

`index.module.css` 的内容如下所示

```css
.box-left {
    /* 一些CSS样式 */
}
```

在 `Main` 组件的 `index.jsx` 中引入了 `index.module.css`

```jsx
import React from 'react'
import styles from './index.module.css'

function Main() {
    return (
        <div className={styles.boxLeft}></div>
    )
}

export default Main
```

当我绑定类名时，`WebStorm` 提示我写成 `boxLeft`，因为 `box-left` 中的 `-` 会被识别成减号，我以为 `React` 会根据 `boxLeft` 智能识别出 `box-left`，但是没想到样式失效了。

当时我还以为是 `React` 是不是不支持 `CSS Module`，为此我还上网搜了，并且使用 `npm run eject` 将配置文件弹出来改了，但是没有怀疑 `boxLeft` 并不能被对应为 `box-left`，这一切都是因为我很相信 `WebStorm`，以为这种写法是正确。

最后无计可施，就想着把 `index.module.css` 中的类名改为 `boxLeft`

```css
.boxLeft {
    /* 一些CSS样式 */
}
```

然后样式就出来了。

这件事告诉我，不要太相信 `IDE`，就是因为太相信 `WebStorm`，以至于我根本没有考虑过写法的问题，导致在这上面浪费了几个小时；也是因为浪费了几个小时，我觉得这是一个宝贵的经验，所以记录下来。