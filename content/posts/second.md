---
title: "Hugo建站小记"
date: 2022-04-09T04:38:01+01:00
draft: false
tags: ["blog"]
categories: ["技术随感 tech"]
summary: 小白搭博客三件套纪实
---

因为无意间在毛毛象上看到友邻([酸橘汁腌鱼](https://toot.seviche.cc/@nonsense))分享了建站心得，我也就试着照葫芦画瓢了一下，结果没想到好用到猫猫流泪啊！！全程一行码都不用写，我陆陆续续花了两天就搞完了！

铛铛铛，隆重推荐，小白搭博客三件套——

- Hugo
- Netlify
- Cusdis

Hugo 作为静态网页框架，简洁，高效，功能很多，我感觉搭个普通博客，免费版就够了。至于怎么用 Hugo 从零开始建站然后再用 Netlify 发布到网上，油管上有个老哥讲得非常通俗易懂实践性超强，在这里也贴一下：

{{< youtube hjD9jTi_DQ4 >}}
<br/>

全程无脑按照视频步骤来即可。

关于域名，我是从 github 的 student pack 里薅羊毛，在[这里](https://www.name.com/)捞了个免费一年的 🤪

然后是关于如何在网站上添加评论区：

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

最后，关于如何在 hugo 上添加友情链接的问题，我直接从 github 上找了个模版，简单粗暴，复制粘贴。

大力感谢[kkkgo](https://github.com/kkkgo/hugo-friendlinks)，步骤如下：

1. 添加 layouts/\_default/links.html.

```html
<style type="text/css">
  .myfriends {
    text-align: center;
    background-color: #fff;
    opacity: 0.9;
  }

  .myfriends a {
    color: black;
  }

  .myfriends p {
    display: none;
  }

  .friendurl {
    text-decoration: none !important;
    color: black;
  }

  .myfriend {
    width: 56px !important;
    height: 56px !important;
    border-radius: 50%;
    border: 1px solid #ddd;
    padding: 2px;
    box-shadow: 1px 1px 1px rgba(0, 0, 0, 0.15);
    margin-top: 14px !important;
    margin-left: 14px !important;
    background-color: #fff;
  }

  .frienddiv {
    height: 92px;
    margin-top: 10px;
    width: 48%;
    display: inline-block !important;
  }

  .frienddiv:hover {
    background: rgba(87, 142, 224, 0.15);
  }

  .frienddiv:hover .frienddivleft img {
    transition: 0.9s !important;
    -webkit-transition: 0.9s !important;
    -moz-transition: 0.9s !important;
    -o-transition: 0.9s !important;
    -ms-transition: 0.9s !important;
    transform: rotate(360deg) !important;
    -webkit-transform: rotate(360deg) !important;
    -moz-transform: rotate(360deg) !important;
    -o-transform: rotate(360deg) !important;
    -ms-transform: rotate(360deg) !important;
  }

  .frienddivleft {
    width: 92px;
    float: left;
  }

  .frienddivleft {
    margin-right: 2px;
  }

  .frienddivright {
    margin-top: 18px;
    margin-right: 18px;
    text-overflow: ellipsis;
    overflow: hidden;
    white-space: nowrap;
  }
</style>
<div class="myfriends">{{ .Content }}</div>
```

2. 添加 layouts/shortcodes/friend.html.

```js
{{ if .IsNamedParams }}
<p><a target="_blank" href={{ .Get "url" }} title={{ .Get "name" }} class="friendurl"></p>
	<div class="frienddiv">
      <div class="frienddivleft">
        <img class="myfriend" src={{ .Get "logo" }} />
      </div>
      <div class="frienddivright">
        {{ .Get "name" }}<br />{{ .Get "word" }}
      </div>
    </div></a>
{{ end }}
```

3. 新建 md 文件，在 menu 添加友链，通过 shortcode 来添加不同的链接。

```
---
title: "Myfriends"
date: 2018-11-02
layout: "links"
menu: "main"
weight: 50
---

{ {< friend name="名字" url="链接" logo="图片路径" word="描述" >} }

```
