# hanshi8.github.io

个人博客，基于 [Chirpy](https://github.com/cotes2020/jekyll-theme-chirpy) 主题 + [Decap CMS](https://decapcms.org)（网页后台）构建，托管在 GitHub Pages。

## 在线写文章（推荐，无需代码）

1. 打开 <https://hanshi8.github.io/admin>
2. 点击「登录 GitHub」并授权
3. 在「文章」中新建或编辑文章，保存并发布

发布后 1～2 分钟，站点会自动重新构建并上线新内容。

## 本地开发（可选）

需要 Ruby 3.x 环境：

```bash
bundle install
bundle exec jekyll s
```

## 目录结构

| 路径 | 说明 |
|---|---|
| `_posts/` | 博客文章（`年-月-日-标题.md`） |
| `_tabs/` | 页面（关于、归档、分类、标签） |
| `_data/` | 联系与分享配置 |
| `admin/` | Decap CMS 后台页面 |
| `assets/img/uploads/` | 后台上传的图片 |
| `.github/workflows/` | GitHub Actions 自动构建部署 |

## 技术要点

- 站点主题版本：`jekyll-theme-chirpy ~> 7.6`
- 部署方式：GitHub Actions 构建后发布到 Pages（`build_type: workflow`）
- 评论、统计等服务均为可选配置，见 `_config.yml`
