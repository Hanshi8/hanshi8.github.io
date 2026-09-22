---
title: Jekyll 写作入门：Markdown 与 front matter
description: 在 Jekyll 博客里写文章，只需要掌握两件事：Markdown 语法和 front matter 头部信息。
tags: [Jekyll, Markdown, 教程]
categories: [教程]
date: 2026-09-23 09:00:00 +0800
---

在 Jekyll 博客里写文章非常简单：每篇文章就是一个 Markdown 文件，放在 `_posts` 目录下，文件名格式为 `年-月-日-标题.md`，例如 `2026-09-23-my-first-post.md`。

## 什么是 front matter

文件开头用 `---` 包裹的一段 YAML 信息，就是 front matter（头部信息）。它告诉 Jekyll 这篇文章的标题、日期、分类等元数据：

```yaml
---
title: 我的第一篇文章
date: 2026-09-23 09:00:00 +0800
tags: [随笔]
categories: [生活]
---
```

常用的字段：

| 字段 | 作用 |
|---|---|
| `title` | 文章标题 |
| `description` | 摘要，显示在首页列表和搜索里 |
| `tags` | 标签，可多个 |
| `categories` | 分类，可多个 |
| `pin` | 设为 `true` 可在首页置顶 |
| `toc` | 设为 `false` 可关闭侧边目录 |

## 常用 Markdown 语法

**加粗**、*斜体*、`行内代码`：

```markdown
**加粗**、*斜体*、`行内代码`
```

列表：

```markdown
- 无序列表项
- 另一个列表项

1. 有序列表项
2. 第二个有序列表项
```

引用：

```markdown
> 这是引用内容。
```

图片：

```markdown
![图片说明](/assets/img/uploads/example.png)
```

## 结语

掌握 Markdown 和 front matter 之后，写文章就只剩「内容」这一件事了。配合 Decap CMS，连文件命名和格式都可以交给后台自动处理。
