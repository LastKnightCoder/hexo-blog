---
title: Hexo渲染问题
date: 2021-03-30
category: Hexo
disableNunjucks: true
tag: 
- 踩坑
- Hexo
---

{% raw %}
这几天写了一篇有关于 <code>Vue.js</code> 的博客，在讲述插值语法时会用到 <code>}}</code>，但是 Hexo 博客使用的是 Nunjucks 来解析文章，内容中如果包含 <code>{{ }}</code> 或者 <code>{% %}</code> 就会发生错误

{% endraw %}

<img src="https://cdn.jsdelivr.net/gh/LastKnightCoder/ImgHosting2/20210330000446.png" alt="image-20210330000446314" style="zoom:50%;" />

官方给出了三种解决办法：

1. 使用 <code>raw</code> 标签

   ```
   {% raw %}
   将包含 {{ }} 或者 {{% %}} 的内容放在其中
   {% endraw %}
   ```

2. 使用 <code>`</code> 或者 <code>```</code>

   ```
   `{{ }}` `{% %}`
   ```

3. 在 <code>front-matter</code> 中禁止 Nunjucks

   ```
   disableNunjucks: true
   ```

{% raw %}

我首先选择的是第三个解决办法，设置 <code>front-matter</code>，但是没有用；而第二个办法直接排除，因为我的 <code>{{}}</code> 就是放在 single backtick 中，但是明显没有效果；那我只有选择第一种办法了，但是事情没有解决，因为 <code>}}</code> 会被解析为行内公式，这是因为我开启了 <code>mathjax</code>，导致页面显示错误。

但是山穷水复疑无路，我想了想为什么没有加 <code>raw</code> 标签之前不会将 <code>}}</code> 解析为数学公式，因为 <code>}}</code> 放在 single backtick 中，会被优先解析为 code 标签，加了 <code>raw</code> 标签之后就不会解析了，因此我将

{% endraw %}

```
`{{}}`
```

全部换成了

```
<code>{{}}</code>
```

修改之前：

<img src="https://cdn.jsdelivr.net/gh/LastKnightCoder/ImgHosting2/20210330000543.png" alt="image-20210330000543405" style="zoom:50%;" />

修改之后：

<img src="https://cdn.jsdelivr.net/gh/LastKnightCoder/ImgHosting2/20210330000651.png" alt="image-20210330000651668" style="zoom:50%;" />

至此果然能够正常的渲染了。
