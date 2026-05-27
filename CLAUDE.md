# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

这是一个 Hugo 个人博客，使用 [Archie](https://github.com/athul/archie) 主题，部署到 GitHub Pages（仓库名 `Insulindian-Phasmid`）。内容为中文。

## Commands

```bash
# 本地开发服务器（含草稿）
hugo server -D

# 构建生产站点（输出到 public/）
hugo --gc --minify

# 创建新文章
hugo new posts/文章名.md
```

没有测试、lint 或其他构建步骤。Hugo 版本要求 0.126.0（CI 中使用的版本）。

## Architecture

- **配置**：`hugo.toml` — `canonifyURLs = true` 会对所有 URL 使用绝对路径。`baseURL` 被注释掉了；本地开发时 `hugo server` 会自动处理，但取消注释时要注意与 GitHub Pages 仓库路径（`/Insulindian-Phasmid/`）匹配。
- **内容**：`content/posts/` 下的 Markdown 文章，front matter 使用 TOML 格式（`+++` 分隔符）。`content/about.md` 为关于页面。
- **自定义资源**：`assets/css/first.css` — 唯一的自定义 CSS，会与主题 CSS 合并。
- **原型**：`archetypes/default.md` — 新文章的模板。

## Theme (Archie)

主题以 git submodule 形式存在于 `themes/archie/`。关键模板：
- `layouts/_default/baseof.html` — 基础 HTML shell
- `layouts/index.html` — 首页，含分页
- `layouts/_default/single.html` — 单篇文章页，含标签和 Disqus
- `layouts/_default/list.html` — 文章列表页（/posts、/tags）
- `layouts/partials/head.html` — 站点标题和导航，标题链接使用 `absLangURL "/"`

主题支持深色/浅色模式切换（`params.mode`），KaTeX/MathJax，以及 feather icon 本地托管（`params.useCDN = false`）。

## URL / 导航陷阱

导航菜单中 Home 链接在 `head.html` 中使用 `absLangURL "/"` 生成。如果设置了 `baseURL` 且 `canonifyURLs = true`，所有链接会变成以该 baseURL 为前缀的绝对路径。确保 `baseURL` 与部署环境一致（GitHub Pages 用户站点为 `https://<user>.github.io/<repo>/`）。如果链接生成不正确，检查 `hugo.toml` 中 `baseURL` 和 `canonifyURLs` 的组合。

## Deployment

GitHub Actions（`.github/workflows/hugo.yaml`）会在推送到 `main` 分支时自动构建并部署到 GitHub Pages。
