---
title: "[CSS] Building a responsive website"
date: 2022-04-12T04:38:01+01:00
draft: false
tags: ["html", "CSS"]
categories: ["技术随感 tech"]
summary: Class notes of FrontendMaster's course-CSS part 1
---

# 准备工作

1. [调色盘](https://color.adobe.com/zh/explore)；
2. [字体](https://fonts.google.com/?query=ubuntu)与[字体搭配](https://www.fontpair.co);
3. [css gradient](https://cssgradient.io);

# html

- 使用`section`和`id`分层分区内容；
- 业界倾向于在 html 只使用 only one `h1` tag，一般用于 logo，所以第二重要的内容一般用`h2`；
- 文字内容中，完整句子要灵活使用`p`+`span`分层而非`h2`，不要将句子割裂成不同 heading；span 默认 in-line，若要转行，直接 display: block;
- navbar 固定模版：使用`nav`元素和无序 list `ul`，在`li`内部放`h1`+`a`作为 logo+链接，便于 css 装修；
- 在使用 fontawesome 时，make it accessible:
  1.  倾向于将 `<i class="fa-brands fa-linkedin"></i>`中的`<i>`修改为`<span>`;
  2.  大众熟悉的图标，倾向于将页面文字隐藏;"sr-only"是 screen reader only。
  ```html
  <span class="fa-brands fa-linkedin" aria-hidden="true"></span>
  <span class="sr-only"> LinkedIn </span>
  ```
- 使用空`div`+自带高度的`css gradient`来制作分割线可以避开浏览器兼容问题；

# css

- 使用 root 来定义调色盘和字体；
  ⚠️ 使用 root 而非 html，因为 1）html 中有 before 和 after，而 root 会影响整个 dom；2）pseudo-class 的 specificity 最高，若使用 universal selector 也容易被 overwritten；
- 使用 border box model；
- 使用 body 清除页面原始样式比如 padding；

```css
:root {
  --black: #0b0c10;
  --dkblue: #1f2833;
  --Plum: #45a29e;
  --whitblue: #a0dbf2;
  --midblue: #165f8c;
  --aqua: #66fcf1;
  --white: #c5c6c7;
  --font-size: 1.3rem;
  --epilogue: "Epilogue", Nunito Sans;
  --sans: "Source Serif Pro", sans-serif;
}

/* border box model: https://css-tricks.com/box-sizing/ */

html {
  box-sizing: border-box;
}
*,
*::before,
*::after {
  box-sizing: inherit;
}

/* generic styles for the page */
body {
  padding: 0;
  margin: 0;
  font-family: var(--sans);
  background-color: var(--dkblue);
  color: var(--white);
  font-size: var(--font-size);
}
/* 清除heading的原始格式 */
h1,
h2,
h3 {
  margin: 0;
}
a {
  color: var(--whitblue);
}

a:hover {
  color: var(--midblue);
  text-decoration: none;
}
```

## intro

- heading 自带 1m 的 margin，清除该样式为 0；
  ⚠️ 为何 heading 清除样式后段落文字没有重叠在一起？
  - 若两个元素在垂直方向的 margin 有重合的部分，它会自动取其中那个较大值，此处文字没有重叠是因为段落自带 margin 1m，若 heading 变成 0 了，则 margin 会自动取较大值，即 p 自带的 1m，所以文字上下行之间仍然有 1m 距离；
- 在正文中的链接需要有下划线，不要仅仅只靠颜色区分， for accessibility reason。
- 设置 line-height: 1.5;不带单位意味着 proportion，会随之变化，所以应该省略单位；

```css
/* intro styles */
#intro {
  padding: 4rem 1rem 10rem 1rem; /*顺序是trouble：top->right*/
  max-width: 1200px; /*与navbar一致*/
  margin: 0 auto; /*设置box居中*/
}

#intro p {
  font-size: 1rem;
  line-height: 1.5;
}

#intro .name {
  font-family: var(--sans);
  font-size: 1rem;
}

.name span {
  font-family: var(--epilogue);
  font-size: 4rem;
  color: var(--aqua);
  display: block;
  font-weight: 300;
}

#intro h2 {
  font-size: 4rem;
  font-weight: normal;
}
```

## contact

- 设置 contact 文字居中，修改 section 为 `width: 400px; margin: 0 auto; text-align: center;`即可自动将长度为 400px 的 box 居中，然后再修改 padding 值；

```css
/* contact styles */

#contact {
  width: 400px;
  margin: 0 auto;
  padding: 3rem 0;
  text-align: center;
  /* 直接添加background-color会导致出现一个400px的box，这是无法修改的，所以需要添加div */
}

#contact p:last-child {
  margin-top: 3rem;
}
```

## navbar

- 此处为基本款导航栏，也可以制作[汉堡包菜单](https://jen4web.substack.com/p/hamburgers?s=r)；
- font-size 设置为百分比，比如 80%就是 base size 的 80%，这样不用手动调整，更灵活；
- nav 模版：先清除默认格式；然后使用 flexbox，纵向与横向居中;
- 修改`a`链接：先清除字体样式；然后 display:block,因为 a 链接默认必须点击 inline 文字本身，修改后可以点击文字区域即可跳转；
- 设置 logo：单独一行，使用 flexbox 中的 flex 属性 flex-basis；它控制某元素有多宽，比如这个 li 元素；
  ⚠️ width 是固定的，flex-basis 是在 flexbox 内部的 flex 属性，跟着 box 变化；
- debugging border：随时使用 border:1px solid red 来看盒子是否符合要求；

```css
/*navbar*/
nav {
  font-family: var(--epilogue);
  font-size: 80%;
  padding: 1rem;
}

nav h1 a {
  font-family: var(--sans);
}

/*nav模板*/
nav ul {
  list-style-type: none;
  padding: 0;
  margin: 0;
  display: flex;
  flex-flow: row wrap;
  justify-content: center;
  align-items: center;
  gap: 2rem;
}

/*logo*/
nav li:first-child {
  flex-basis: 100%;
  text-align: center;
}

/*设置链接*/
nav a {
  color: var(--white);
  text-decoration: none;
  display: block;
}

nav a:hover {
  color: var(--midblue);
}
```

### icon:

- SVG 比起 font-awesome 更轻，一般正式使用 SVG；
- 修改 icon 的大小，所有 font-awesome 图标都用 fa 开头，所以使用 class 作为选择器；

```css
nav [class*="fa-"] {
  font-size: 150%;
  color: var(--aqua);
}

nav h1 [class*="fa-"] {
  font-size: 120%;
  color: var(--aqua);
}
```

### 假按钮：

- 装饰 li 元素，修改成按钮样式；

```css
.button {
  background-color: var(--dkblue);
  border: solid 0.5px var(--aqua);
  padding: 0.5rem;
  border-radius: 5px;
}

.button:hover {
  color: white;
  background-color: var(--dkblue);
}
```

### responsive 导航栏

```css
@media (min-width: 850px) {
  nav {
    max-width: 1200px;
    margin: 0 auto; /*make it center*/
  }
  nav li:first-child {
    flex-basis: auto; /*取消了100%，将logo变成跟其他的同一行*/
    text-align: left; /*取消居中*/
    /*这是默认的flexbox，空隙是even的*/
    margin-right: auto; /*将logo隔开，将navbar之外的所有空隙塞进第一个child和剩下的child之间*/
  }
}
```

## div 分割线

```
    <div class="gradient"></div>
```

```css
.gradient {
  height: 1.5px;

  background: linear-gradient(
    93deg,
    rgba(247, 248, 250, 1) 5%,
    rgba(134, 251, 251, 0.5620052770448549) 84%
  );
}
```

## 网页背景分区

- 使用 div 分区后填充背景色；

```css
<div class="section-dkblue">
<section id="whatever"></section>
</div>

/*backgound color divs*/
.section-dkblue {
  background-color: var(--dkblue);
}
```

## 添加图片边框

```css
#projects img {
  margin: 2rem 0 4rem 0;
  border-left: 1px solid var(--aqua);
  border-top: 1px solid var(--aqua);
  border-radius: 25px;
  padding: 1rem;
}
```

## grid 重叠图片

- 使用 grid 可以简单叠加图片，而使用 flexbox 不可以；

```css
/*桌面版*/
@media (min-width: 550px) {
  article {
    display: grid;
    grid-template-columns: repeat(10, 1fr); /*页面平均分成10fr*/
    gap: 1rem;
  }
  #projects img {
    grid-column: 1/6; /*起始于1终止于6*/
    grid-row: 1/2; /*从上到下起始于1终止于2，也就是第二行*/
  }

  .text {
    order: 2;
    text-align: right;
    grid-column: 5/11; /*从左往右起始于5终止于11，起始处5和6有重合部分*/
    grid-row: 1/2; /*从上到下起始于1终止于2，也就是第二行，与图片处于同一行*/
  }

  #projects ul {
    justify-content: flex-end;
  }
}
```
