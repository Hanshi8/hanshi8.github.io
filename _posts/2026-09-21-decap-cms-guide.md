---
title: Decap CMS：在网页上直接写文章
description: Decap CMS 是开源的 Git-based CMS，让静态博客也拥有「可视化后台」——登录网页就能写文章、传图片、一键发布。
tags: [DecapCMS, 博客, 教程]
categories: [教程]
date: 2026-09-21 21:00:00 +0800
---

Decap CMS（原 Netlify CMS）是一个开源的「Git-based」内容管理系统：你写的内容直接以文件形式提交到 Git 仓库，没有独立的数据库。

## 它是怎么工作的

Decap CMS 提供一个网页后台（本站就是 `https://hanshi8.github.io/admin`），通过 GitHub 授权后，后台可以直接读写仓库里的 Markdown 文件：

1. 在后台写文章 → 保存为 `_posts/2026-09-23-xxx.md`
2. 点击发布 → 自动提交到 GitHub 仓库
3. GitHub Actions 检测到推送 → 自动重新构建站点 → 文章上线

## 为什么适合静态博客

- **所见即所得**：不用记 Markdown 语法，支持富文本编辑
- **图片上传**：直接在编辑器里传图，自动存到仓库
- **草稿与发布分离**：可以保存草稿，准备好再发布
- **全部开源**：后台本身就是站点里的一页静态页面（`admin/` 目录），不依赖任何第三方服务器

## 配置长什么样

后台的核心配置就一个文件 `admin/config.yml`：

```yaml
backend:
  name: github
  repo: Hanshi8/hanshi8.github.io
  branch: main

collections:
  - name: "posts"
    label: "文章"
    folder: "_posts"
    create: true
    slug: "{{year}}-{{month}}-{{day}}-{{slug}}"
    fields:
      - { label: "标题", name: "title", widget: "string" }
      - { label: "正文", name: "body", widget: "markdown" }
```

## 使用体验

对写作者来说，Decap CMS 的体验和主流博客平台非常接近：登录、新建文章、写内容、发布。区别在于内容的所有权完全属于你的 Git 仓库，随时可以迁移，永远不会被平台锁死。
