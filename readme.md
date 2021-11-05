
hexo 文档信息: [Server]( https://hexo.io/zh-cn/docs/tag-plugins)

```
 相关命令:
    - 启动运行： hexo serve 或 hexo s 
    - 创建新页面：hexo new [layout] <title>
        Hexo 有三种默认布局：post(source/_posts)、page(source) 和 draft(source/_drafts)
```

# 文件最上方的---分隔区域内的相关信息
---
layout: 布局 // config.default_layout
title: 标题  // 文章的文件名
date: 建立日期 // 文件建立日期
updated: 更新日期 // 文件更新日期
comments:开启文章的评论功能 //	true
tags:标签（不适用于分页）
categories:分类（不适用于分页）
permalink:覆盖文章网址
excerpt: Page excerpt in plain text. Use this plugin to format the text
disableNunjucks: Disable rendering of Nunjucks tag {{ }}/{% %} and tag plugins when enabled
lang:	Set the language to override auto-detection	Inherited from _config.yml
---

---
标签:
- Diary
   
单个分类：
categories:
- Diary
- Life
  (PS: 注意Life会成为Diary的分类，并不是并列类)
 
多个分类：
categories:
- [Diary, PlayStation]
- [Diary, Games]
- [Life]
---