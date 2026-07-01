# 网站发布说明

本目录已生成静态网站，入口为：

- `website/index.html`
- `website/reports/`：由 Markdown 转换出的网页报告
- `website/md/`：原始 Markdown 文件副本

## 内网访问

如需本机预览，或配合 Cloudflare Tunnel 对外分享，在本目录运行：

```powershell
.\start_website_server.ps1
```

脚本会输出本机访问地址。默认只绑定 `127.0.0.1`，不会直接暴露到局域网。

停止服务：

```powershell
.\stop_website_server.ps1
```

## 知道链接即可访问

如果不需要指定邮箱验证，只想“知道链接的人能看”，可以用 GitHub Pages 或 Cloudflare Pages。

### GitHub Pages

1. 在 GitHub 新建仓库，建议仓库名使用随机一些的名字，例如 `coal-report-7x4q29`。
2. 仓库可设为 `Public`。如果要用私有仓库发布 Pages，需要 GitHub 付费/组织计划支持。
3. 打开仓库，点 `Add file` -> `Upload files`。
4. 把本目录 `github-pages` 文件夹里的所有内容拖进去，不要只上传 zip。
5. Commit 后进入 `Settings` -> `Pages`。
6. Source 选择 `Deploy from a branch`，Branch 选 `main`，Folder 选 `/root`，保存。
7. 等 1-10 分钟，访问 `https://你的用户名.github.io/仓库名/`。

当前站点已包含 `.nojekyll`、`robots.txt` 和 `noindex`，用于减少 GitHub Pages 处理干扰并降低搜索引擎收录概率。但这不是权限控制，链接被转发后，拿到链接的人仍然可以访问。

### Cloudflare Pages

如果不需要指定邮箱验证，只想“知道链接的人能看”，也可以用 Cloudflare Pages：

1. 打开 Cloudflare Dashboard。
2. 进入 `Workers & Pages`。
3. 选择 `Create application`。
4. 选择 `Pages` / `Upload assets` / `Drag and drop`。
5. 上传本目录里的 `website-cloudflare-upload.zip`，或直接拖入 `website` 文件夹。
6. 部署完成后，把 Cloudflare 给出的 `https://项目名.pages.dev` 链接发给对方。

当前站点已包含 `robots.txt`、`noindex` 和 `X-Robots-Tag`，用于降低搜索引擎收录概率。但这不是权限控制，链接被转发后，拿到链接的人仍然可以访问。

如需更不容易被猜到，可使用随机项目名，例如 `coal-report-7x4q29`。

## Cloudflare Access 私密分享

如果之后需要改成“只有指定邮箱能看”，再使用 Cloudflare Tunnel + Access：

1. 在 Cloudflare 托管一个域名，例如 `example.com`。
2. 在 Zero Trust 里创建 Access application，域名使用 `reports.example.com`。
3. Access policy 选择 `Allow`，Include 填指定人员邮箱。
4. 在本机运行 `.\start_website_server.ps1`。
5. 创建 Cloudflare Tunnel，把 `reports.example.com` 指向本机脚本输出的 `http://127.0.0.1:端口`。

## 普通公网静态站

把整个 `website` 文件夹上传到任意静态网站托管服务即可，例如公司 Web 服务器、对象存储静态网站、GitHub Pages、Cloudflare Pages、Netlify 或 Vercel。入口文件是 `index.html`。

## 更新内容

修改或新增当前目录下的 `.md` 文件后，运行：

```powershell
python build_site.py
```

即可重新生成网站。
