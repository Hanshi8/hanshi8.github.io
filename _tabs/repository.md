---
# the default layout is 'page'
icon: fas fa-folder-open
order: 2
---

这里存放一些文件与文字，相当于我的小网盘。

<div id="file-list">
  <table class="file-table">
    <thead>
      <tr>
        <th>名称</th>
        <th>大小</th>
        <th style="text-align: right;">操作</th>
      </tr>
    </thead>
    <tbody>
      {%- assign repo_files = site.static_files
            | where_exp: "f", "f.path contains '/files/'"
            | where_exp: "f", "f.name != 'README.md'"
            | sort: "name" -%}
      {%- for f in repo_files -%}
      {%- assign ext = f.extname | downcase | remove: "." -%}
      {%- assign icons = "fa-regular fa-file" -%}
      {%- if ext == "png" or ext == "jpg" or ext == "jpeg" or ext == "gif" or ext == "webp" or ext == "svg" -%}
        {%- assign icons = "fa-regular fa-image" -%}
      {%- elsif ext == "md" or ext == "markdown" or ext == "txt" -%}
        {%- assign icons = "fa-regular fa-file-lines" -%}
      {%- elsif ext == "pdf" -%}
        {%- assign icons = "fa-regular fa-file-pdf" -%}
      {%- elsif ext == "zip" or ext == "rar" or ext == "7z" or ext == "tar" or ext == "gz" -%}
        {%- assign icons = "fa-regular fa-file-zipper" -%}
      {%- endif -%}
      <tr class="file-row" data-path="{{ f.path | relative_url }}" data-ext="{{ ext }}">
        <td class="file-name">
          <i class="{{ icons }}"></i>
          <a href="{{ f.path | relative_url }}" target="_blank" rel="noopener">{{ f.name }}</a>
        </td>
        <td class="file-size">
          {%- if f.size >= 1048576 -%}
            {{ f.size | divided_by: 1048576.0 | round: 1 }} MB
          {%- elsif f.size >= 1024 -%}
            {{ f.size | divided_by: 1024.0 | round: 1 }} KB
          {%- else -%}
            {{ f.size }} B
          {%- endif -%}
        </td>
        <td class="file-actions" style="text-align: right; white-space: nowrap;">
          <button type="button" class="btn btn-sm btn-outline-primary file-preview">预览</button>
          <a class="btn btn-sm btn-outline-secondary file-dl" href="{{ f.path | relative_url }}" download>下载</a>
        </td>
      </tr>
      {%- else -%}
      <tr><td colspan="3"><i>（files 目录目前为空）</i></td></tr>
      {%- endfor -%}
    </tbody>
  </table>
</div>

<!-- Preview modal -->
<div id="file-modal" class="file-modal" hidden>
  <div class="file-modal-box">
    <div class="file-modal-head">
      <span id="file-modal-title"></span>
      <button type="button" id="file-modal-close" aria-label="关闭">&times;</button>
    </div>
    <div class="file-modal-body" id="file-modal-body"></div>
    <div class="file-modal-foot">
      <a id="file-modal-dl" class="btn btn-primary btn-sm" href="#" download>下载文件</a>
    </div>
  </div>
</div>

<style>
.file-table { width: 100%; border-collapse: collapse; }
.file-table th { text-align: left; font-size: 0.85rem; opacity: 0.7; padding: 0.5rem 0.6rem; border-bottom: 1px solid var(--border-color, #dee2e6); }
.file-table td { padding: 0.6rem; border-bottom: 1px solid var(--border-color, #eee); vertical-align: middle; }
.file-table .file-row:hover { background: rgba(128, 128, 128, 0.06); }
.file-table .file-name i { margin-right: 0.5rem; }
.file-table .file-size { color: var(--text-muted-color, #6c757d); font-size: 0.85rem; }
.file-modal { position: fixed; inset: 0; background: rgba(15,23,42,.6); z-index: 2000; display: flex; align-items: center; justify-content: center; padding: 24px; }
.file-modal-box { background: #fff; color: #111; width: 100%; max-width: 820px; max-height: 86vh; border-radius: 10px; overflow: hidden; display: flex; flex-direction: column; box-shadow: 0 20px 60px rgba(0,0,0,.35); }
.file-modal-head { display: flex; justify-content: space-between; align-items: center; padding: 10px 16px; border-bottom: 1px solid #e5e7eb; font-weight: 600; word-break: break-all; }
#file-modal-close { border: none; background: #f3f4f6; color: #374151; width: 30px; height: 30px; border-radius: 6px; font-size: 18px; cursor: pointer; line-height: 1; }
#file-modal-close:hover { background: #e5e7eb; }
.file-modal-body { flex: 1; overflow: auto; padding: 14px 16px; min-height: 120px; }
.file-modal-body img { max-width: 100%; display: block; margin: 0 auto; }
.file-modal-body iframe { width: 100%; height: 62vh; border: none; }
.file-modal-body pre { white-space: pre-wrap; word-break: break-word; font-size: 0.85rem; background: #f8f9fa; padding: 12px; border-radius: 6px; max-height: 60vh; overflow: auto; }
.file-modal-foot { padding: 10px 16px; border-top: 1px solid #e5e7eb; text-align: right; }
</style>

<script>
(function () {
  var modal = document.getElementById('file-modal');
  var title = document.getElementById('file-modal-title');
  var body = document.getElementById('file-modal-body');
  var closeBtn = document.getElementById('file-modal-close');
  var dlBtn = document.getElementById('file-modal-dl');

  function openPreview(row) {
    var path = row.getAttribute('data-path');
    var ext = (row.getAttribute('data-ext') || '').toLowerCase();
    var name = row.querySelector('.file-name a').textContent;
    title.textContent = name;
    dlBtn.href = path;
    body.innerHTML = '';
    if (['png', 'jpg', 'jpeg', 'gif', 'webp', 'svg'].indexOf(ext) >= 0) {
      var img = document.createElement('img');
      img.src = path;
      img.alt = name;
      body.appendChild(img);
    } else if (ext === 'pdf') {
      var iframe = document.createElement('iframe');
      iframe.src = path;
      body.appendChild(iframe);
    } else if (['md', 'markdown', 'txt'].indexOf(ext) >= 0) {
      body.innerHTML = '<p style="opacity:.6;">加载中…</p>';
      fetch(path)
        .then(function (r) { if (!r.ok) throw new Error(); return r.text(); })
        .then(function (text) {
          var pre = document.createElement('pre');
          pre.textContent = text;
          body.innerHTML = '';
          body.appendChild(pre);
        })
        .catch(function () { body.innerHTML = '<p>预览失败，可直接下载查看。</p>'; });
    } else {
      body.innerHTML = '<p style="opacity:.6;">该类型暂不支持在线预览，请直接下载。</p>';
    }
    modal.hidden = false;
  }

  document.querySelectorAll('.file-preview').forEach(function (btn) {
    btn.addEventListener('click', function () {
      openPreview(btn.closest('.file-row'));
    });
  });
  closeBtn.addEventListener('click', function () { modal.hidden = true; });
  modal.addEventListener('click', function (e) { if (e.target === modal) modal.hidden = true; });
  document.addEventListener('keydown', function (e) { if (e.key === 'Escape') modal.hidden = true; });
})();
</script>
