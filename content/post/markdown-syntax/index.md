---
title: Markdown 语法指南
date: 2023-09-07
description: 一篇展示基础 Markdown 语法与常见 HTML 元素格式效果的示例文章。
tags:
    - 示例标签
categories:
    - 示例分类
---

这篇文章展示了 Hugo 内容文件中常见的 Markdown 基础语法，也可以顺便观察主题是否为基础 HTML 元素提供了合适的样式。

<!--more-->

## 标题

下面的 HTML `<h1>` 到 `<h6>` 元素分别表示六个层级的标题，其中 `<h1>` 级别最高，`<h6>` 级别最低。

# H1
## H2
### H3
#### H4
##### H5
###### H6

## 段落

这是一个普通段落示例，用来展示正文在当前主题中的排版效果。你可以观察行高、段间距、链接样式以及不同长度文本在页面中的阅读体验。

这是第二段示例文字。通常在正式写作中，这里可以放文章导语、背景说明，或者更完整的内容段落，用来测试主题对连续文本的呈现效果。

## 引用块

blockquote 元素用于表示引用内容，也可以搭配出处信息使用；如果需要，还可以在引用内部继续使用强调、链接等行内 Markdown 语法。

### 不带出处的引用

> 这是一段示例引用文字。
> **注意**：你也可以在引用块中继续使用 *Markdown 语法*。

### 带出处的引用

> 不要通过共享内存来通信，而要通过通信来共享内存。<br>
> — <cite>Rob Pike[^1]</cite>

[^1]: 这句话摘自 Rob Pike 于 2015 年 11 月 18 日在 Gopherfest 上的 [演讲](https://www.youtube.com/watch?v=PAAkCSZUG1c)。

## 表格

表格并不是最核心的 Markdown 语法之一，但 Hugo 默认就支持它。

   姓名 | 年龄
--------|------
    小王 | 27
  小李 | 23

### 表格中的行内 Markdown

| 斜体      | 加粗     | 代码   |
| --------- | -------- | ------ |
| *斜体*    | **加粗** | `code` |

| A           | B             | C             | D             | E             | F             |
|-------------|---------------|---------------|---------------|---------------|---------------|
| 这里是一段较长的示例文本。 | 这里用于演示多列表格。 | 你可以观察单元格换行表现。 | 也可以测试宽表格的横向布局。 | 适合检查主题在文章内的可读性。 | 这是最后一列示例内容。 |

## 代码块

### 使用反引号的代码块

```html
<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Example HTML5 Document</title>
</head>
<body>
  <p>Test</p>
</body>
</html>
```

### 使用四个空格缩进的代码块

    <!doctype html>
    <html lang="en">
    <head>
      <meta charset="utf-8">
      <title>Example HTML5 Document</title>
    </head>
    <body>
      <p>Test</p>
    </body>
    </html>

### Diff 代码块

```diff
[dependencies.bevy]
git = "https://github.com/bevyengine/bevy"
rev = "11f52b8c72fc3a568e8bb4a4cd1f3eb025ac2e13"
- features = ["dynamic"]
+ features = ["jpeg", "dynamic"]
```

### 单行代码块

```html
<p>A paragraph</p>
```

## 列表类型

### 有序列表

1. 第一项
2. 第二项
3. 第三项

### 无序列表

* 列表项
* 另一项
* 再来一项

### 嵌套列表

* 水果
  * 苹果
  * 橙子
  * 香蕉
* 乳制品
  * 牛奶
  * 奶酪

## 其他元素：abbr、sub、sup、kbd、mark

<abbr title="Graphics Interchange Format">GIF</abbr> 是一种位图图像格式。

H<sub>2</sub>O

X<sup>n</sup> + Y<sup>n</sup> = Z<sup>n</sup>

按下 <kbd>CTRL</kbd> + <kbd>ALT</kbd> + <kbd>Delete</kbd> 可以结束当前会话。

大多数 <mark>蝾螈</mark> 都是夜行性动物，会捕食昆虫、蠕虫以及其他小型生物。
