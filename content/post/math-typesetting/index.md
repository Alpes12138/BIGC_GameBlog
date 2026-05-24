---
title: 数学公式排版
description: 使用 KaTeX 渲染数学公式
date: 2023-08-24 00:00:00+0000
math: true
categories:
    - 示例分类
tags:
    - 示例标签
---

Stack 内置了对 [KaTeX](https://katex.org/) 数学公式排版的支持。

**它默认不会在全站启用，** 你可以在单篇文章的 front matter 中加入 `math: true` 来开启；如果希望整个站点都支持，也可以在 `config.toml` 的 `params.article` 配置段中设置 `math = true`。

## 行内公式

这是一个行内数学表达式：$\varphi = \dfrac{1+\sqrt5}{2}= 1.6180339887…$

```markdown
$\varphi = \dfrac{1+\sqrt5}{2}= 1.6180339887…$
```

## 块级公式

$$
    \varphi = 1+\frac{1} {1+\frac{1} {1+\frac{1} {1+\cdots} } }
$$

```markdown
$$
    \varphi = 1+\frac{1} {1+\frac{1} {1+\frac{1} {1+\cdots} } }
$$
```

$$
    f(x) = \int_{-\infty}^\infty\hat f(\xi)\,e^{2 \pi i \xi x}\,d\xi
$$

```markdown
$$
    f(x) = \int_{-\infty}^\infty\hat f(\xi)\,e^{2 \pi i \xi x}\,d\xi
$$
```
