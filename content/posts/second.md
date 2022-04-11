---
title: "建站小记"
date: 2022-04-09T04:38:01+01:00
draft: false
---

因为无意间在毛毛象上看到友邻分享了建站心得，我也就试着照葫芦画瓢了一下，结果没想到好用到猫猫流泪啊！！全程一行码都不用写，我陆陆续续花了两天就搞完了！

铛铛铛，隆重推荐，小白搭博客三件套——

- Hugo
- Netlify
- Cusdis

Hugo 作为静态网页框架，简洁，高效，功能很多，我感觉搭个普通博客，免费版就够了。至于怎么用 Hugo 从零开始建站然后再用 Netlify 发布到网上，油管上有个老哥讲得非常通俗易懂实践性超强，在这里也贴一下：

{{< youtube hjD9jTi_DQ4 >}}
<br/>

全程无脑按照视频步骤来即可。

关于域名，我是从 github 的 student pack 里薅羊毛，在[这里](name.com)捞了个免费一年的 🤪

最后是关于如何在网站上添加评论区：

- 先在 Cusdis 上注册新账号，这样就可以得到 data-app-id；
- 修改官方给出的 html 文件如下：

```html
<h4>Comments:</h4>
<div
  id="cusdis_thread"
  data-host="https://cusdis.com"
  data-app-id="注册后获取id再修改"
  data-page-id="{{ .File.UniqueID }}"
  data-page-url="{{ .Permalink }}"
  data-page-title="{{ .Title }}"
></div>
<script async defer src="https://cusdis.com/js/cusdis.es.js"></script>
```

- 最后把这个文件改名放在正确路径下就完成啦。

需要注意的是，不同的 theme 有不同的路径，因为我用的 theme 是[paperMod](https://github.com/adityatelange/hugo-PaperMod/tree/c5d31c778b572c3952c35e918765901cb223c045)，所以放置文件的目录如下：

```
.(site root)
├── config.yml
├── content/
├── theme/hugo-PaperMod/
└── layouts
     └── partials
        └── comments.html  <---
```
