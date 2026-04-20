# 博客操作指南

## 新增日志

### 1. 创建文章

```bash
hugo new content posts/文章文件名.md
```

执行后会按 `archetypes/default.md` 模板生成文章，自动填充当前时间。

### 2. 编辑内容

打开生成的文件，修改前置元数据：

```yaml
---
title: "文章标题"
date: 2026-04-20T15:30:00+08:00
draft: false        # true 为草稿，false 发布
tags: ["标签1", "标签2"]
---
```

正文使用 Markdown 语法编写。

### 3. 本地预览

```bash
hugo server -D
```

访问 `http://localhost:1313` 查看效果。`-D` 参数包含草稿文章。

### 4. 提交发布

```bash
git add content/posts/文章文件名.md
git commit -m "add: 文章标题"
git push origin main
```

push 后 GitHub Actions 自动构建并部署到 Pages。

---

## 常用命令

| 命令 | 作用 |
|------|------|
| `hugo server -D` | 启动开发服务器（含草稿） |
| `hugo server` | 启动开发服务器（不含草稿） |
| `hugo --minify` | 构建生产版本 |
| `hugo --gc` | 清理缓存后构建 |

## 项目结构

```
.
├── content/posts/     # 文章目录
├── static/            # 静态资源（图片等）
├── themes/PaperMod/   # 主题（git 子模块）
├── hugo.toml          # 站点配置
└── .github/workflows/ # 自动部署配置
```

## 更换主题

### 1. 查找主题

访问 [Hugo 官方主题库](https://themes.gohugo.io/)，选择喜欢的主题，记录其 GitHub 仓库地址。

### 2. 移除旧主题

```bash
# 删除子模块
git submodule deinit -f themes/PaperMod
git rm -f themes/PaperMod
rm -rf .git/modules/themes/PaperMod
```

### 3. 添加新主题

```bash
git submodule add https://github.com/作者/主题名.git themes/主题名
```

### 4. 修改配置

编辑 `hugo.toml`，将 `theme = 'PaperMod'` 改为 `theme = '主题名'`。

**注意：** 不同主题的 `params` 配置参数各不相同，需参考新主题的文档调整 `[params]` 部分。部分主题可能还需要额外的菜单配置或自定义参数。

### 5. 本地验证

```bash
hugo server -D
```

确认页面渲染正常后再提交。

### 6. 提交更改

```bash
git add .
git commit -m "update: 更换主题为 xxx"
git push origin main
```

---

## 注意事项

- 主题通过 git 子模块管理，克隆仓库后执行 `git submodule update --init --recursive` 获取主题文件
- 站点配置见 `hugo.toml`，可修改标题、菜单、主题参数等
- 更换主题后，原有文章的 `tags`、`date` 等基础字段通常通用，但部分主题特有的短代码或参数会失效
