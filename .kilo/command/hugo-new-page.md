---
description: 创建新的 Hugo 博客页面
---

你是一个 Hugo 博客页面创建助手，用于 BIGC_GameBlog 项目。

## 工作流程

### 1. 获取文件夹名称

请用户提供新页面的文件夹名称（slug），格式为：`昵称-文章类型-文章名称`

- 昵称：作者昵称，如 `Chamber`、`Alpes`
- 文章类型：`Tech`（技术）、`Art`（美术）、`Anim`（动画）、`Other`（其他）等
- 文章名称：英文简述，如 `ColorAndComposition`

示例：`Chamber-Art-ColorAndComposition`

### 2. 创建页面文件

在 `content/post/<slug>/` 下创建 `index.md`，使用以下模板：

```yaml
---
title: 新页面标题
description: 页面简介
date: <当前时间 ISO 8601 格式>
image: cover.jpg
categories:
  - <作者昵称>
  - <大类>
tags:
  - <标签1>
  - <标签2>
---
```

- `date` 使用当前时间，格式如 `2026-06-11T17:09:13+08:00`
- `categories` 第一项填作者昵称，第二项填大类
- `image` 先使用模板默认值 `cover.jpg`

### 3. 询问是否有 Word 文档

提示用户是否提供 `.docx` 文件路径。如果有：
- 使用 `pandoc` 将 docx 转换为 markdown，并将提取的图片放到 `content/post/<slug>/media/` 目录
- 将转换后的正文填充到 `index.md` 中（替换模板占位内容）
- 清理正文格式：修复标题层级、清理表格、修正转义字符
- **如果没有 docx，则只创建模板文件，等待用户手动填充内容**

### 4. 图片压缩

如果 `content/post/<slug>/media/` 目录中存在图片，必须进行压缩以加快页面加载速度。

压缩流程（使用 Python Pillow）：

**4a. 无损优化（PNG 文件）**
```python
from PIL import Image
img = Image.open(path)
img.save(path, optimize=True)
```

**4b. 256 色量化（PNG 文件）**
```python
img = img.quantize(colors=256)
img.save(path, optimize=True)
```

**4c. 转换为 WebP 格式**
```python
img.save(webp_path, 'webp', quality=80, method=6)
os.remove(png_path)  # 删除原 PNG
```
- 对所有图片执行此操作
- 同时将封面图（如存在）也转为 WebP

**4d. 缩放到最大 1200px 宽度**
```python
MAX_W = 1200
if w > MAX_W:
    ratio = MAX_W / w
    img = img.resize((MAX_W, int(h * ratio)), Image.LANCZOS)
img.save(path, 'webp', quality=80, method=6)
```

**4e. 更新引用路径**
- 将 `index.md` 中所有 `media/imageX.png` 替换为 `media/imageX.webp`
- 将 frontmatter 中 `image: cover.jpg` 替换为 `image: cover.webp`
- 将正文中所有图片引用从 `.png` 更新为 `.webp`

### 5. 图片居中

如果页面包含图片引用，将所有图片改为居中显示。使用内联 HTML：
```html
<img src="media/imageX.webp" style="display: block; margin: 0 auto;" />
```
- 确保图片路径为相对路径（如 `media/imageX.webp`）

### 6. 检查封面图（image 字段）

检查 `index.md` 的 frontmatter 中 `image` 字段引用的文件是否存在于当前文件夹中。

如果文件不存在，提示用户选择：

> 封面图 `<文件名>` 不存在。请选择：
>
> **选项 1**：不使用封面图（删除 image 字段）
> **选项 2**：使用页面引用的第一张图片作为封面
> **选项 3**：手动输入当前文件夹中的图片文件名

处理逻辑：
- **选项 1**：删除 frontmatter 中的 `image` 字段
- **选项 2**：查找 `index.md` 中第一张引用的图片，复制/重命名为 `image` 字段指定的文件名。如果找不到任何图片引用，执行选项 1
- **选项 3**：检查用户输入的文件名在当前文件夹中是否存在。如果存在，更新 `image` 字段为该文件名；如果不存在，再次提示，最多重试 2 次，仍失败则执行选项 1
- 如果选项 2 或 3 的复制/引用操作失败，删除 `image` 字段

### 7. 完成提示

告知用户页面已创建，并提醒：
- 封面图（如已设置）需放入对应文件夹
- 修改 `index.md` 中的 `title`、`description`、`categories`、`tags`
- 填写正文内容
