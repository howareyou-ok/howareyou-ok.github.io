# Beyond Geek Guide (B-GG.com)

这是 Beyond Geek Guide 网站的源代码，基于 Hugo 静态网站生成器构建。

## 特点

- 🚀 **简单易用**：只需要上传 Markdown 文件即可发布内容
- 🎨 **简洁设计**：专注内容的简洁主题
- 📱 **响应式**：完美适配各种设备
- ⚡ **自动部署**：GitHub Actions 自动构建和部署
- 🔍 **SEO 友好**：优化的 HTML 结构和元数据

## 快速开始

### 1. 克隆仓库

```bash
git clone https://github.com/your-username/b-gg.com.git
cd b-gg.com
```

### 2. 本地预览

确保已安装 Hugo：

```bash
# macOS
brew install hugo

# Windows
choco install hugo-extended

# Linux
sudo snap install hugo
```

启动本地服务器：

```bash
hugo server -D
```

访问 http://localhost:1313 预览网站。

### 3. 添加内容

#### 添加新文章

在 `content/posts/` 目录下创建新的 Markdown 文件：

```markdown
---
title: "你的文章标题"
date: 2024-01-16T14:30:00+08:00
tags: ["标签1", "标签2"]
---

这里是文章内容...
```

#### 添加新页面

在 `content/` 目录下创建新的 Markdown 文件：

```markdown
---
title: "页面标题"
menu: "main"  # 可选：添加到主菜单
---

页面内容...
```

### 4. 发布内容

将更改提交到 GitHub：

```bash
git add .
git commit -m "添加新内容"
git push origin main
```

GitHub Actions 会自动构建并部署到 GitHub Pages。

## 项目结构

```
b-gg.com/
├── content/              # 内容目录
│   ├── posts/           # 文章目录
│   ├── _index.md        # 首页内容
│   └── about.md         # 关于页面
├── themes/simple/       # 主题目录
│   ├── layouts/         # 模板文件
│   └── static/          # 静态资源
├── .github/workflows/   # GitHub Actions
├── hugo.toml           # Hugo 配置文件
└── README.md           # 说明文档
```

## 配置说明

### hugo.toml

主要配置项：

- `baseURL`: 网站域名
- `title`: 网站标题
- `theme`: 使用的主题
- `menu`: 导航菜单配置

### GitHub Actions

`.github/workflows/hugo.yml` 配置了自动部署流程：

1. 检测到 `main` 分支有新提交
2. 安装 Hugo 和依赖
3. 构建静态网站
4. 部署到 GitHub Pages

## 使用技巧

### 1. 文章排序

文章按照 `date` 字段倒序排列，最新的文章会显示在前面。

### 2. 标签系统

使用 `tags` 字段为文章添加标签，方便分类和检索。

### 3. 菜单配置

在 Front Matter 中添加 `menu: "main"` 可以将页面添加到主导航菜单。

### 4. 草稿功能

在 Front Matter 中添加 `draft: true` 可以将文章标记为草稿，不会在生产环境显示。

## 自定义

### 修改样式

编辑 `themes/simple/static/css/style.css` 文件来自定义网站样式。

### 修改布局

编辑 `themes/simple/layouts/` 目录下的模板文件来自定义页面布局。

### 添加功能

可以通过修改模板文件添加新功能，如评论系统、搜索功能等。

## 部署

### GitHub Pages

1. 在 GitHub 仓库设置中启用 Pages
2. 选择 "GitHub Actions" 作为部署源
3. 推送代码到 `main` 分支即可自动部署

### 其他平台

也可以部署到其他静态网站托管平台：

- Netlify
- Vercel
- Cloudflare Pages

## 贡献

欢迎提交 Issue 和 Pull Request 来改进这个项目！

## 许可证

MIT License