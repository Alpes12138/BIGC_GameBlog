---
title: 新页面创建规范-1.2
description: 如何创建新页面的指南
date: 2026-05-24T18:08:55+08:00
image: cover.jpg
categories:
  - 示例分类
tags:
  - 示例标签
build:
  list: always
weight: 1
---

# 新页面创建规范-1.2

本文介绍如何在BIGC游研会知识库新建页面。所有北京印刷学院校友均可参与知识分享。

## 一、环境配置

### 1. 注册 Gitee 并申请权限

本网站部署在 GitHub Pages，通过 Gitee 镜像方便国内协作。

- 注册 [Gitee](https://gitee.com/) 账号（参考[视频教程](https://www.bilibili.com/video/BV1hf4y1W7yT/?p=2&share_source=copy_web&vd_source=d315296042809e429bbf935da947523a)前两节）
- 向仓库管理员申请 [BIGC_GameBlog](https://gitee.com/alpes12138/BIGC_GameBlog) 的共创权限
- 联系方式：微信 wzc975728589

### 2. 安装 Git 和 VSCode

| 工具 | 下载/教程 |
|------|----------|
| Git | 参考[视频第三节](https://www.bilibili.com/video/BV1hf4y1W7yT/?p=2&share_source=copy_web&vd_source=d315296042809e429bbf935da947523a)（无需安装图形工具） |
| VSCode | [官方下载](https://vscode.js.cn/Download) |

### 3. 配置 VSCode 插件

推荐安装以下插件（[安装教程](https://vscode.js.cn/docs/getstarted/extensions)）：

1. **Chinese** — 中文语言包
2. **vscode-icons** — 文件图标美化
3. **KiloCode** — AI 辅助插件（也可使用 Codex、Claude Code 等替代）

> 如果你不使用 AI 辅助创建页面，可跳过 AI 配置，直接前往「手动创建新页面」章节。

### 4. 配置 KiloCode（以 DeepSeek 为例）

1. 在 [DeepSeek 开放平台](https://platform.deepseek.com/usage) 登录并充值 1 元
2. 进入 API Keys 页面，申请 API-Key 并复制
3. 打开 KiloCode → 设置（齿轮图标）→ 提供商 → 查看更多提供商 → 搜索 DeepSeek → 粘贴 API-Key
4. 模型选择 **DeepSeek V4 Pro**，模式选择 **Medium**

> 提示：可长按拖动 Kilo Code 图标到右侧栏，方便使用。

## 二、拉取仓库

1. 在本地创建一个空白文件夹
2. 在文件夹地址栏输入 `cmd` 打开命令行，执行：

```bash
git clone https://gitee.com/alpes12138/BIGC_GameBlog.git
```

## 三、通过 AI 创建新页面

### 1. 打开项目

在 VSCode 中：文件 → 打开文件夹 → 选择克隆的 `BIGC_GameBlog` 文件夹。

### 2. 使用 Skill 创建

在 KiloCode 中输入并发送：

```
/hugo-new-page
```

期间 AI 申请的所有权限请选择「同意」或「运行」。

AI 会提示："根据 hugo-new-page 技能，请提供要创建的文件夹名称（slug）："

### 3. 命名规则

格式：`昵称-文章类型-文章名称`

- **昵称**：用于区分作者，同一作者保持一致
- **文章类型**（可自定义）：

| 缩写 | 含义 |
|------|------|
| Tech | 技术 |
| Art | 美术 |
| Anim | 动画 |
| Other | 其他 |

- **文章名称**：英文简述文章内容

示例：`Alpes-Tech-MyAIExp`

输入名称并发送，AI 将自动创建新页面的文件夹和文件。

## 四、手动创建新页面 （已经用AI创建了就跳过这一步）

### 1. 打开项目并复制模板

在 VSCode 中打开项目，进入 `content/post` 文件夹，复制 `new-page-rules` 并粘贴一份。

### 2. 修改文件夹名称

按照命令规则修改文件夹名称（参考上一节的命名规则）。

### 3. 取消草稿模式

在 `index.md` 中删除以下这行：

```yaml
draft: true
```

## 五、修改页面属性

修改 `index.md` 顶部 `---` 之间的内容：

- **title**：文章标题
- **description**：文章简介
- **date**：AI 创建的会自动更新；手动创建需修改为当前时间（精确到天）
- **image**：封面图（需放在本文件夹内，大小 < 300KB）

### 分类与标签

```yaml
categories:
  - 奥尔普斯     # 作者昵称
  - 技术         # 大类
tags:
  - AI           # 小标签，自由发挥
  - 经验分享
```

- **分类**：先加作者昵称，再加大类（如 技术/美术/动画），尽量使用已有分类
- **标签**：可自由添加，表明文章亮点

其他属性不懂的不要修改。

## 六、撰写正文

本框架使用 Markdown 编写正文，语法参考 [MarkDown 教程](https://markdown.com.cn/basic-syntax/)。

- 可根据提示，直接将 Word 文件（或绝对路径）给 AI ，让 AI 转换为 Markdown 格式
- 图片引用、视频嵌入等可交给 AI 处理
- 手动操作参考：[图片画廊](https://alpes12138.github.io/BIGC_GameBlog/p/%E5%9B%BE%E7%89%87%E7%94%BB%E5%BB%8A/) 和 [ShortCode 示例](https://alpes12138.github.io/BIGC_GameBlog/p/shortcode-%E7%A4%BA%E4%BE%8B/)

> 想要将 Word 转换为 Markdown 格式，必须先在本地安装软件pandoc [官方网站](https://pandoc.org/installing.html), 跳转到GitHub页面后选择windows-x86_64.msi版本（或其他符合你操作系统的版本）进行下载和安装。


## 七、提交修改

### 1. 预览

按 `Ctrl + Shift + V` 在 VSCode 中预览正文效果（表格属性在网页中会正常显示）。

### 2. 提交并同步

1. 打开 Git 插件（侧边栏第三个图标）
2. 在「消息」栏填写说明，如："添加新页面：我的AI经验分享"
3. 点击「提交」→ 暂存所有更改 → 同步
4. 首次提交需登录 Gitee 账号
5. 「提交」按钮变暗即表示已提交
6. 等待 1~2 分钟，刷新知识库网站即可查看新页面
