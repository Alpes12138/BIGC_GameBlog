---
#标题
title: 新页面创建规范-1.0
#介绍
description: 如何创建新页面的指南
#日期
date: 2026-05-24T18:08:55+08:00
#封面图（例如 cover.jpg，本文件夹必须有 cover.jpg 这个文件）
image: cover.jpg
#分类，一行代表有一个（- example）
categories:
    - 示例分类
#标签，一行代表有一个（- example）
tags:
    - 示例标签
build:
    list: always
weight: 1
---

# 新页面创建规范-1.0

本文将讲述如何在BIGC游研会知识库中新建一个页面。

所有北京印刷学院校友，只要拥有想分享的知识，都可以新建自己的知识分享页。

## 一、环境配置

### 1、注册gitee账号

本网页部署在GitHub Pages上，并且通过gitee镜像GitHub项目的方式，让协作不受网络限制。

所以首先需要gitee账号。

这里给出一个视频(只需要看前两节就够) [gitee(码云)的注册和代码提交【手把手】](https://www.bilibili.com/video/BV1hf4y1W7yT/?p=2&share_source=copy_web&vd_source=d315296042809e429bbf935da947523a)

### 2、申请gitee仓库共创权限

本项目仓库地址为：https://gitee.com/alpes12138/BIGC_GameBlog

合作权限需要向我申请，有了权限才能提交你的修改内容。

微信：wzc975728589

### 3、下载git和VSCode

git是在本地实现git所有功能的基础软件。
VSCode是在本地实现开发的编程工具。

git具体安装参考视频（只看第三节。不需要其中的另一个图形软件！！！）[gitee(码云)的注册和代码提交【手把手】](https://www.bilibili.com/video/BV1hf4y1W7yT/?p=2&share_source=copy_web&vd_source=d315296042809e429bbf935da947523a)

VSCode安装官方地址：https://vscode.js.cn/Download

### 4、配置VSCode，添加插件

为了开发更方便，我们需要给VScode添加一些插件。

其中包含一个AI插件。为了创建页面更方便，我创建了一个skill，通过AI能很快完成页面的创建。
如果你完全不想通过AI来更快捷地新建页面，那么你现在可以跳转到第四章。

安装插件教程：https://vscode.js.cn/docs/getstarted/extensions

插件列表：
1. Chinese      |中文插件
2. vscode-icons |文件图标插件
3. KiloCode     |AI插件（如果你了解其他插件如Codex、Claude Code等，可以替代）

### 5、配置KiloCode（以DeepSeek-v4-pro模型为例）

这里拿性价比最高，最好接入的DeepSeek为例。

首先需要取得一个API-key：在 [DeepSeek开放平台] (https://platform.deepseek.com/usage) 登录，并充值1元余额（够用了），然后在API Keys页面申请一个API-Key。复制后先不要关闭窗口，等待配置完成再关闭。

打开KiloCode -> 点击设置（齿轮图标） -> 提供商 -> 查看更多提供商 -> 搜索DeepSeek -> 粘贴 API-Key 并确定。

（可以点击 Kilo Code 的图标，把它拖在右边栏，更方便使用）
点击下方第二个下拉框，选择最下面的 "DeepSeek V4 Pro"，第三个下拉框选择"Medium"。

接下来可以使用AI了。

## 二、下拉知识库的仓库

### 1、找一个空白文件夹

在本地电脑找一个空白文件夹，用来存放项目仓库

### 2、下载/克隆项目

在选择的文件夹中，点击资源浏览器上方的地址栏，删除地址，输入
```html
cmd
```
打开命令台，再输入
```html
git clone https://gitee.com/alpes12138/BIGC_GameBlog.git
```
来克隆项目

## 三、通过AI加载Skill，来创建新页面

### 1、打开项目文件夹

在VSCode中，点击文件 -> 打开文件夹 -> 打开克隆的文件夹BIGC_GameBlog

### 2、通过Kilo Code新建页面
打开Kilo Code，输入
```html
/hugo-new-page
```
后发送


#### 期间AI申请的全部权限请“同意”或“运行”


等待AI回复：“根据 hugo-new-page 技能，请提供要创建的文件夹名称（slug）：”

输入一个文件夹英文名称，规则如下：

你的昵称-文章类型-文章名称（这里仅作文件上的区分，可简写）

昵称用来区分创建者，自己的文章的昵称前缀要保持一致

文章类型（仅做例子，可以自己根据实际情况自己修改）：
1. Tech |(技术)
2. Art  |(美术)
3. Anim |(动画)
4. Other|(其他)

文章名称：用英文表示文章内容（仅作文件名区分，可以随便写）

例如：我要写一篇AI使用经验的文章，我可以命名为
```html
Alpes-Tech-MyAIExp
```

输入名称并发送，之后会自动创建新页面的文件夹和文件。

## 四、手动创建新页面（用AI创建的可以跳过）

### 1、打开项目文件夹

在VSCode中，点击文件 -> 打开文件夹 -> 打开克隆的文件夹BIGC_GameBlog

### 2、复制示例文件夹

在VSCode中，打开content/post文件夹，复制new-page-rules并粘贴一份在本文件夹。

### 3、修改名称

修改其文件夹名称（参考上一节最后的规则）

### 4、取消草稿模式
删除新增的index.md中
```html
draft: true
```
这一行内容

## 五、修改新页面各属性

根据绿色注释，修改标题、介绍等内容。

AI创建的页面时间会自动修改。
手动新建页面需要修改时间为创建时间（精确到天即可，时分秒可不管）。

封面图片必须在新建的文件夹中才能引用成功。
要求大小要在300KB以下。

### 分类和标签

分类首先要添加昵称（可中文），方便以作者分类文章。
然后添加大的分类，例如“技术”、“美术”、“动画”等，尽量不要添加没有的大分类。

标签添加小的分类，可以自由发挥，表明技术亮点。

比如我的AI经验分享文章可以这么写分类和标签：
```html
#分类,一行代表有一个(- example)
categories:
    - 奥尔普斯
    - 技术
#标签,一行代表有一个(- example)
tags:
    - 技术
    - AI
```

其他内容不懂的不要添加或修改。

## 六、制作正文

本框架通过MarkDown语言来制作正文内容。

MD本身不算复杂，可以参考文章 [MarkDown 语法教程](https://markdown.com.cn/basic-syntax/)。

当然你也完全可以通过Word等格式写一遍，然后让AI转换为MarkDown格式，添加在新建页面的正文处。

添加图片，引用网络视频等功能也完全可以交给AI。
手动添加的话参考文章 [图片画廊](https://alpes12138.github.io/BIGC_GameBlog/p/%E5%9B%BE%E7%89%87%E7%94%BB%E5%BB%8A/) 和 [ShortCode 示例](https://alpes12138.github.io/BIGC_GameBlog/p/shortcode-%E7%A4%BA%E4%BE%8B/)。

## 七、提交修改

### 1、预览

首先你的正文修改完毕后，可以在VSCode中预览一下。
点击快捷键 Ctrl + Shift + V进行预览。
无视表格属性，网页中只会显示下方的正文内容。

### 2、提交修改并同步

1. 打开Git插件（默认侧边栏第三个），在“消息”栏中介绍本次修改的内容。
2. 例如“添加了新的页面：我的AI经验分享”。
3. 点击“提交”。
4. 暂存所有更改。
5. 同步所有更改。
6. 提示登录gitee账号（第一次提交需要）。
7. “提交”按钮变暗，代表已经提交修改。
8. 等待1~2分钟，查看新建页面是否已经出现在了知识库网站。
