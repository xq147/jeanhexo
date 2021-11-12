---
title: html(一)
date: 2021-11-10 16:44:58
tags: [html]
---

html、css相关积累
<!--more-->

![原文来源](https://mp.weixin.qq.com/s/iL_asPp-R0QGgwZ0av4PvQ) 该文仅供作者本人学习记录所用。

# head
head标签用于文档头部信息，是所有头部元素的容器，head中的元素可以引用脚本、指示浏览器在哪里找到样式表、提供元信息等等。
head中的标签： base、link、meta、script、style以及title。

## title 标签
&#8195;定义文档的标题，是head中唯一必须存在的元素。浏览器会以特殊的方式使用标题，标题不会显示在文档中，会显示在浏览器窗口的标题栏或者是状态栏上， 如果标题为空则会显示页面的地址信息。

- dir属性
规定元素中内容的所属方向。
  
- lang属性
规定元素中语言的代码。

## meta 标签
- meta 元素往往不会引起用户的注意，但是meta对整个网页有影响，会对网页能否被搜索引擎检索，和在搜索中的排名起着关键性的作用。
- meta有个必须的属性content用于表示需要设置的项的值。
- meta存在两个非必须的属性http-equiv和name, 用于表示要设置的项。
- 比如<meta http-equiv="Content-Security-Policy" content="upgrade-insecure-requests">,设置的项是Content-Security-Policy设置的值是upgrade-insecure-requests


# 可编辑的元素节点
- 给元素设置属性contenteditable="true" 即可成为可编辑的区域
- contenteditable 属性是 HTML5 中的新属性。

```html
<p contenteditable="true" style="border: 1px solid #00c851;border-radius: 10px; padding: 10px">
    这是一个可编辑的段落。
</p>
<div contenteditable="true"> 这是一个可编辑的div</div>
```

<p contenteditable="true" style="border: 1px solid #00c851;border-radius: 10px; padding: 10px">这是一个可编辑的段落。</p>
