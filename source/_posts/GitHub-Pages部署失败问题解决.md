---
title: GitHub Pages 部署失败问题解决：子模块配置与工作流优化
date: 2025-12-15 20:00:00
tags:
  - GitHub
  - Hexo
  - CI/CD
  - 笔记
---

## 问题描述

在使用 Hexo + Butterfly 主题搭建个人博客并部署到 GitHub Pages 时，遇到了以下问题：

1. **GitHub 提交显示 ❌（失败）**：每次推送代码后，提交旁边都显示红色的 ❌ 标记
2. **GitHub Actions 工作流失败**：错误信息显示 `fatal: No url found for submodule path 'themes/butterfly' in .gitmodules`
3. **博客网站未更新**：即使代码已推送，网站内容仍然是旧的主题

## 问题分析

### 1. 缺少 `.gitmodules` 文件

**错误信息**：
```
Error: fatal: No url found for submodule path 'themes/butterfly' in .gitmodules
Error: The process '/usr/bin/git' failed with exit code 128
```

**原因**：
- `themes/butterfly` 被 Git 识别为子模块（submodule）
- 但仓库中缺少 `.gitmodules` 文件
- GitHub Actions 在检出代码时，无法知道如何获取子模块的内容
- 导致构建过程中找不到主题文件，构建失败

### 2. 子模块修改无法提交

**现象**：
- 直接修改了 `themes/butterfly/_config.yml` 文件
- 使用 `git add .` 无法将子模块的修改添加到暂存区
- 即使本地提交了，GitHub Actions 也无法获取这些修改

**原因**：
- 子模块是独立的 Git 仓库
- 主仓库只记录子模块的提交哈希，不包含子模块内的文件
- 修改子模块需要先在子模块内提交，再在主仓库更新引用
- 由于 `butterfly` 是第三方主题，没有推送权限，无法将修改推送到远程

### 3. `.gitignore` 配置问题

**问题**：
- `.gitignore` 文件中包含了 `.github/`，导致工作流文件可能被忽略
- GitHub Actions 工作流文件必须提交到仓库才能运行

## 解决方案

### 步骤 1：创建 `.gitmodules` 文件

创建 `.gitmodules` 文件，配置子模块的 URL：

```ini
[submodule "themes/butterfly"]
	path = themes/butterfly
	url = https://github.com/jerryc127/hexo-theme-butterfly.git
```

这样 GitHub Actions 就知道如何检出子模块了。

### 步骤 2：修复 `.gitignore` 配置

修改 `.gitignore`，允许提交 `.github` 目录：

```gitignore
# Dependencies
node_modules/
# .github/  # Allow GitHub Actions workflows
# Hexo generated files
public/
```

### 步骤 3：使用配置文件覆盖主题配置

**最佳实践**：不要直接修改子模块内的文件，而是使用主仓库的配置文件覆盖。

创建 `_config.butterfly.yml` 文件（在 Hexo 根目录）：

```yaml
# Butterfly Theme Configuration Override
# This file will override the default theme configuration

# Social Links
social:
  fab fa-github: https://github.com/Joelle-Dev || Github || '#24292e'

# Image Settings
favicon: /images/favicon.png

avatar:
  img: /images/favicon.png
  effect: false
```

**优势**：
- 配置在主仓库中，可以正常提交和版本控制
- GitHub Actions 可以正常读取配置
- 更新主题时不会丢失自定义配置
- 符合 Hexo 的最佳实践

### 步骤 4：配置 GitHub Actions 工作流

创建 `.github/workflows/pages.yml`：

```yaml
name: Deploy GitHub Pages

on:
  push:
    branches:
      - master
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: "pages"
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v4
        with:
          submodules: true
          fetch-depth: 0

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '18'
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Build
        run: npm run build

      - name: Setup Pages
        uses: actions/configure-pages@v4

      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: ./public

  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
```

**关键配置**：
- `submodules: true`：启用子模块检出
- `npm ci`：使用锁定文件安装依赖，确保一致性
- `path: ./public`：上传 Hexo 生成的静态文件

### 步骤 5：配置 GitHub Pages 设置

在 GitHub 仓库设置中：
1. 进入 **Settings** → **Pages**
2. 将 **Source** 选择为 **GitHub Actions**（而不是 "Deploy from a branch"）

## 核心概念理解

### Git 子模块（Submodule）

**什么是子模块**：
- 子模块是嵌套在另一个 Git 仓库中的独立 Git 仓库
- 主仓库只记录子模块的提交哈希，不包含子模块的实际文件
- 子模块有自己的版本历史和远程仓库

**子模块的工作方式**：
```bash
# 查看子模块状态
git submodule status

# 初始化并更新子模块
git submodule update --init --recursive

# 更新子模块到最新版本
git submodule update --remote
```

**子模块的文件模式**：
- 模式 `160000` 表示这是一个子模块引用
- 主仓库中只存储子模块的提交哈希，不存储文件内容

### 配置文件覆盖机制

Hexo 支持通过配置文件覆盖主题默认配置：

1. **主题默认配置**：`themes/butterfly/_config.yml`
2. **覆盖配置**：`_config.butterfly.yml`（在 Hexo 根目录）
3. **优先级**：覆盖配置会合并到默认配置中，同名配置项会被覆盖

这种方式的好处：
- ✅ 配置可以版本控制
- ✅ 更新主题时不会丢失配置
- ✅ 可以清晰地看到自定义配置

## 最佳实践总结

### 1. 子模块管理

- ✅ **使用 `.gitmodules` 文件**：明确配置子模块的 URL 和路径
- ✅ **不要直接修改子模块**：使用配置文件覆盖或 fork 主题仓库
- ✅ **定期更新子模块**：获取主题的最新功能和修复

### 2. GitHub Actions 配置

- ✅ **启用子模块检出**：`submodules: true`
- ✅ **使用锁定文件**：`npm ci` 而不是 `npm install`
- ✅ **缓存依赖**：使用 `cache: 'npm'` 加速构建

### 3. 主题配置

- ✅ **使用配置文件覆盖**：创建 `_config.butterfly.yml`
- ✅ **不要修改主题源码**：保持主题可更新性
- ✅ **文档化配置**：在配置文件中添加注释说明

### 4. 版本控制

- ✅ **提交工作流文件**：`.github/workflows/` 必须提交
- ✅ **提交配置文件**：`_config.butterfly.yml` 必须提交
- ✅ **提交子模块引用**：`themes/butterfly` 作为子模块引用提交

## 常见问题

### Q: 为什么 `git add .` 无法添加子模块的修改？

A: 子模块是独立的 Git 仓库，修改子模块需要：
1. 在子模块目录内提交：`cd themes/butterfly && git commit`
2. 在主仓库更新引用：`git add themes/butterfly && git commit`

### Q: 如何更新主题到最新版本？

A: 
```bash
cd themes/butterfly
git pull origin master
cd ../..
git add themes/butterfly
git commit -m "chore: update butterfly theme"
```

### Q: 子模块和直接克隆有什么区别？

A:
- **子模块**：主仓库只记录引用，子模块内容从远程获取，适合第三方依赖
- **直接克隆**：所有文件都在主仓库中，适合自己维护的主题

## 总结

通过这次问题解决，我们学到了：

1. **子模块的正确使用**：需要 `.gitmodules` 文件配置
2. **配置文件覆盖**：比直接修改主题更优雅
3. **GitHub Actions 配置**：需要正确处理子模块
4. **最佳实践**：遵循项目结构规范，便于维护和更新

现在博客可以正常部署了！🎉

## 参考资源

- [Git Submodules 文档](https://git-scm.com/book/en/v2/Git-Tools-Submodules)
- [Hexo 配置文件](https://hexo.io/docs/configuration.html)
- [GitHub Actions 文档](https://docs.github.com/en/actions)
- [Butterfly 主题文档](https://butterfly.js.org/)

