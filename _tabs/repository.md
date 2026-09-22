---
# the default layout is 'page'
icon: fas fa-folder-open
order: 2
---

这里存放一些文件与文字，相当于一个小型网盘，所有内容都保存在本仓库的 `files/` 目录中。

**如何添加内容：**

- **文字文件**：打开 [后台](https://hanshi8.github.io/admin)，在左侧「仓库」分类中新建，保存后即可发布到这里。
- **其他文件**（PDF、压缩包、文档等）：使用 [GitHub 网页上传](https://github.com/Hanshi8/hanshi8.github.io/upload/main/files)，上传完成后本页会自动列出。

<div id="file-list" style="margin-top: 1.5rem;">
  <p>正在加载文件列表…</p>
</div>

<script>
(function () {
  var list = document.getElementById('file-list');
  fetch('https://api.github.com/repos/Hanshi8/hanshi8.github.io/contents/files')
    .then(function (r) { return r.json(); })
    .then(function (files) {
      if (!Array.isArray(files)) {
        list.innerHTML = '<p><i>（files 目录目前为空）</i></p>';
        return;
      }
      var items = files
        .filter(function (f) { return f.type === 'file' && f.name !== 'README.md'; })
        .map(function (f) {
          return '<li style="margin: 0.4rem 0;">' +
            '<a href="' + f.download_url + '" target="_blank" rel="noopener">' + f.name + '</a>' +
            ' <span style="color: var(--text-muted-color); font-size: 0.85em;">' +
            (f.size / 1024).toFixed(1) + ' KB · ' +
            new Date(f.updated_at).toLocaleDateString('zh-CN') +
            '</span></li>';
        })
        .join('');
      list.innerHTML = items
        ? '<ul style="list-style: none; padding-left: 0;">' + items + '</ul>'
        : '<p><i>（files 目录目前为空）</i></p>';
    })
    .catch(function () {
      list.innerHTML = '<p>文件列表加载失败，请稍后刷新。</p>';
    });
})();
</script>
