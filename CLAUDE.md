# CLAUDE.md

本文件为 Claude Code (claude.ai/code) 提供在该仓库中工作的指引。

## 项目概述

这是一个基于 [Hugo](https://gohugo.io/) 的静态站点/博客，使用 [PaperMod](https://github.com/adityatelange/hugo-PaperMod) 主题。站点语言配置为中文（zh-CN），通过 GitHub Actions 自动部署到 `https://tindalos-cx.github.io/web`。

## 常用命令

- **启动开发服务器：** `hugo server -D`（包含草稿文章）
- **构建生产版本：** `hugo --minify`
- **创建新文章：** `hugo new content posts/<文件名>.md`
- **清理构建缓存：** `hugo --gc`

## 架构说明

### Hugo 配置
- **配置文件：** [`hugo.toml`](hugo.toml) — 站点元数据、主题、菜单和参数设置
- **主题：** PaperMod（以 git 子模块形式管理，位于 [`themes/PaperMod`](themes/PaperMod/)）
- **内容：** [`content/posts/`](content/posts/) — Markdown 格式的博客文章，使用 YAML 前置元数据
- **文章模板：** [`archetypes/default.md`](archetypes/default.md) — 新建内容时的默认模板
- **静态资源：** [`static/`](static/) — 直接复制到输出目录的文件
- **构建输出：** `public/` — 生产构建产物（已加入 gitignore，由 CI 部署）

### 主题与定制
- PaperMod 作为 **git 子模块** 引入。克隆或切换分支后，需执行 `git submodule update --init --recursive` 获取主题文件。
- 主题支持亮色/暗色模式、搜索、归档、面包屑导航、阅读时长等功能，均通过 [`hugo.toml`](hugo.toml) 中的 params 配置。
- 如需覆盖主题模板，可在根目录 [`layouts/`](layouts/) 下添加自定义布局或局部模板，避免直接修改子模块。

### 部署流程
- **部署平台：** GitHub Pages
- **工作流文件：** [`.github/workflows/deploy.yml`](.github/workflows/deploy.yml)
- **触发条件：** `main` 分支的 push 操作
- **构建方式：** Hugo Extended 版本执行 `hugo --minify`
- **发布目录：** `public/` 目录
- 工作流会自动拉取子模块，使用 `peaceiris/actions-hugo` 和 `peaceiris/actions-gh-pages` 完成构建与发布。

## 文章格式

文章使用 YAML 前置元数据，常用字段如下：

```yaml
---
title: "文章标题"
date: 2026-04-20T15:30:00+08:00
draft: false
tags: ["标签1", "标签2"]
---
```

站点导航菜单在 [`hugo.toml`](hugo.toml) 中定义，当前包含：首页、归档。
