# Hexo 简介与常用命令指南

## 什么是 Hexo？

Hexo 是一个快速、简洁且强大的博客框架，使用 Node.js 编写。它可以将 Markdown 文件渲染成静态 HTML 文件，非常适合用来搭建个人博客。

### 主要特点
- 📝 支持 Markdown 语法
- 🎨 丰富的主题系统
- ⚡ 快速生成静态页面
- 🔌 丰富的插件生态
- 📦 一键部署到 GitHub Pages、Netlify 等平台

---

## Hexo 工作流程

```
Markdown 源文件 (source/_posts/)
    ↓
Hexo 生成 (hexo generate)
    ↓
静态 HTML 文件 (public/)
    ↓
部署到服务器 (hexo deploy)
```

---

## 安装 Hexo

### 1. 安装 Node.js
确保已安装 Node.js（建议 v12.0 或更高版本）

### 2. 全局安装 Hexo CLI
```bash
npm install -g hexo-cli
```

### 3. 验证安装
```bash
hexo version
```

---

## 项目初始化

### 创建新项目
```bash
hexo init <folder>
cd <folder>
npm install
```

### 项目结构
```
.
├── _config.yml          # 站点配置文件
├── package.json         # 项目依赖
├── scaffolds/           # 模板文件夹
│   ├── draft.md
│   ├── page.md
│   └── post.md
├── source/              # 源文件目录
│   ├── _drafts/        # 草稿文件夹
│   ├── _posts/         # 文章文件夹
│   └── _pages/         # 页面文件夹
├── themes/              # 主题文件夹
│   └── landscape/      # 默认主题
└── public/              # 生成的静态文件（部署用）
```

---

## 常用命令

### 基础命令

#### 1. 创建新文章
```bash
hexo new "文章标题"
# 或简写
hexo n "文章标题"
```
会在 `source/_posts/` 目录下创建新的 Markdown 文件。

#### 2. 创建新页面
```bash
hexo new page "页面名称"
# 或简写
hexo n page "页面名称"
```
会在 `source/` 目录下创建新的文件夹和 `index.md` 文件。

#### 3. 创建草稿
```bash
hexo new draft "草稿标题"
# 或简写
hexo n draft "草稿标题"
```
会在 `source/_drafts/` 目录下创建草稿文件。

#### 4. 发布草稿
```bash
hexo publish "草稿标题"
# 或简写
hexo p "草稿标题"
```
将草稿移动到 `source/_posts/` 目录。

---

### 生成和预览

#### 5. 生成静态文件
```bash
hexo generate
# 或简写
hexo g
```
根据源文件生成静态 HTML 文件到 `public/` 目录。

#### 6. 启动本地服务器
```bash
hexo server
# 或简写
hexo s
```
启动本地服务器，默认地址：`http://localhost:4000`

#### 7. 指定端口启动
```bash
hexo server -p 5000
# 或简写
hexo s -p 5000
```

#### 8. 生成并启动服务器
```bash
hexo generate --watch
hexo server --watch
# 或组合命令
hexo g -w && hexo s
```
`--watch` 参数会监听文件变化，自动重新生成。

---

### 部署命令

#### 9. 部署到远程服务器
```bash
hexo deploy
# 或简写
hexo d
```
需要先在 `_config.yml` 中配置部署设置。

#### 10. 生成并部署（常用组合）
```bash
hexo generate --deploy
# 或简写
hexo g -d
```
先生成静态文件，然后部署。

---

### 清理命令

#### 11. 清理缓存和生成的文件
```bash
hexo clean
```
删除 `public/` 目录和缓存文件，通常在遇到问题时使用。

#### 12. 完整流程（推荐）
```bash
hexo clean && hexo generate && hexo deploy
# 或简写
hexo clean && hexo g -d
```

---

## 常用命令组合

### 开发流程
```bash
# 1. 创建新文章
hexo new "我的新文章"

# 2. 编辑文章（使用你喜欢的编辑器）
# 编辑 source/_posts/我的新文章.md

# 3. 本地预览
hexo server

# 4. 生成并部署
hexo clean && hexo g -d
```

### 快速预览
```bash
# 生成并启动服务器（带监听）
hexo g -w && hexo s
```

---

## 配置文件

### 站点配置 (`_config.yml`)
主要配置项：
```yaml
# 网站信息
title: 网站标题
subtitle: 网站副标题
description: 网站描述
author: 作者名
language: zh-CN

# URL 设置
url: https://yourname.github.io
root: /
permalink: :year/:month/:day/:title/

# 部署设置
deploy:
  type: git
  repo: https://github.com/yourname/yourname.github.io.git
  branch: master
```

### 主题配置
每个主题通常有自己的 `_config.yml` 文件，位于 `themes/主题名/` 目录下。

---

## 常用插件

### 安装插件
```bash
npm install hexo-plugin-name --save
```

### 推荐插件
- `hexo-deployer-git` - Git 部署插件
- `hexo-generator-feed` - RSS 订阅
- `hexo-generator-sitemap` - 生成站点地图
- `hexo-renderer-marked` - Markdown 渲染器
- `hexo-server` - 本地服务器

---

## 主题管理

### 安装主题
```bash
cd themes
git clone https://github.com/theme-author/theme-name.git
```

### 启用主题
在 `_config.yml` 中设置：
```yaml
theme: theme-name
```

### 更新主题
```bash
cd themes/theme-name
git pull
```

---

## 实用技巧

### 1. 文章 Front Matter
每篇文章开头可以设置元数据：
```markdown
---
title: 文章标题
date: 2024-01-01 12:00:00
tags:
  - 标签1
  - 标签2
categories:
  - 分类1
---
```

### 2. 文章摘要
在文章中插入 `<!-- more -->` 来设置摘要分隔线。

### 3. 数学公式
安装 `hexo-renderer-markdown-it-plus` 或 `hexo-math` 插件支持 LaTeX 公式。

### 4. 图片引用
将图片放在 `source/images/` 目录，使用相对路径引用：
```markdown
![图片描述](/images/photo.jpg)
```

---

## 常见问题

### Q: 修改后网站没有更新？
A: 运行 `hexo clean && hexo g` 清理并重新生成。

### Q: 如何备份 Hexo 源文件？
A: 将整个项目目录（除了 `node_modules/` 和 `public/`）提交到 Git。

### Q: 如何迁移到新电脑？
A: 
1. 克隆仓库
2. 运行 `npm install` 安装依赖
3. 运行 `hexo g` 生成静态文件

---

## 快速参考表

| 命令 | 简写 | 说明 |
|------|------|------|
| `hexo new "标题"` | `hexo n` | 创建新文章 |
| `hexo new page "名称"` | `hexo n page` | 创建新页面 |
| `hexo generate` | `hexo g` | 生成静态文件 |
| `hexo server` | `hexo s` | 启动本地服务器 |
| `hexo deploy` | `hexo d` | 部署到远程 |
| `hexo clean` | - | 清理缓存 |
| `hexo generate --deploy` | `hexo g -d` | 生成并部署 |
| `hexo server --watch` | `hexo s -w` | 监听模式启动 |

---

## 更多资源

- 官方文档：https://hexo.io/zh-cn/docs/
- 主题市场：https://hexo.io/themes/
- 插件列表：https://hexo.io/plugins/

