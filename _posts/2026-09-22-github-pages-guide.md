---
title: 用 GitHub Pages 免费搭建个人博客
description: GitHub Pages 是免费的静态站点托管服务，配合 Jekyll 主题，几分钟就能拥有自己的博客。
tags: [GitHub, 博客, 教程]
categories: [教程]
date: 2026-09-22 20:00:00 +0800
---

GitHub Pages 是 GitHub 提供的免费静态网站托管服务，每个账号可以拥有一个 `用户名.github.io` 的个人站点，还可以为其他仓库单独开启 Pages。

## 为什么选择 GitHub Pages

- **免费**：托管、HTTPS、自定义域名都不收费
- **无需服务器**：静态文件由 GitHub 全球 CDN 分发，访问速度快
- **版本管理**：站点内容天然纳入 Git 管理，不怕丢失
- **可编程**：支持 Jekyll 等静态站生成器，也可以完全手写 HTML

## 架构：Actions 自动构建

本站使用 GitHub Actions 自动构建：每次往仓库推送内容，工作流就会自动安装 Jekyll、生成静态页面并部署到 Pages，全程无需手动操作。

```yaml
# .github/workflows/pages-deploy.yml 的核心逻辑
name: "Build and Deploy"
on:
  push:
    branches: [main]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v7
      - uses: ruby/setup-ruby@v1
        with: { ruby-version: "3.4", bundler-cache: true }
      - run: bundle exec jekyll b -d _site
      - uses: actions/upload-pages-artifact@v5
  deploy:
    needs: build
    steps:
      - uses: actions/deploy-pages@v5
```

## 局限与对策

GitHub Pages 只能托管静态文件，没有数据库和服务器端代码。但常见的「动态」需求都有免费方案：

| 需求 | 方案 |
|---|---|
| 在线写文章 | Decap CMS（本站的方案） |
| 评论区 | giscus / utterances |
| 访问统计 | umami / Google Analytics |
| 站内搜索 | Pagefind / Algolia |

所以，静态托管并不等于功能简陋，关键看怎么组合工具。
