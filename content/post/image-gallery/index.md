---
title: 图片画廊
description: 使用 Markdown 创建美观的交互式图片画廊
date: 2023-08-26 00:00:00+0000
image: 2.jpg
categories:
    - 示例分类
tags:
    - 示例标签
---

Hugo 主题 Stack 支持使用 Markdown 创建交互式图片画廊。它基于 [PhotoSwipe](https://photoswipe.com/) 实现，语法灵感参考了 [Typlog](https://typlog.com/)。

使用这个功能时，图片必须与 Markdown 文件位于同一目录中，因为它依赖 Hugo 的 page bundle 功能来读取图片尺寸。**暂不支持外部图片。**

## 语法

```markdown
![图片 1](1.jpg) ![图片 2](2.jpg)
```

## 效果

![图片 1](1.jpg) ![图片 2](2.jpg)

> 图片来自 [mymind](https://unsplash.com/@mymind) 和 [Luke Chesser](https://unsplash.com/@lukechesser) / [Unsplash](https://unsplash.com/)
