# Moonless

个人作品与笔记站点，基于 [Astro](https://astro.build) 构建，通过 GitHub Pages 发布。

- 线上地址：<https://10deg.github.io/moonless/>
- 仓库：<https://github.com/10deg/moonless>

## 技术栈

Astro 7 · TypeScript · MDX · Content Collections · Pagefind 静态搜索 · GitHub Actions

## 本地开发

需要 Node.js 22.12 或更高版本（仓库里的 `.nvmrc` 已固定大版本）。

```sh
npm install
npm run dev
```

开发服务器地址：<http://localhost:4321/moonless/>。注意路径里的 `/moonless/`，它来自
`astro.config.mjs` 中的 `base`。

常用命令：

```sh
npm run dev          # 开发服务器
npm run build        # 构建到 dist/，并生成 Pagefind 搜索索引
npm run preview      # 预览构建结果
npm run check        # Astro / TypeScript 类型检查
npm run format       # Prettier 格式化
npm run format:check # 检查格式（CI 会跑）
```

## 目录结构

```text
src/
  consts.ts              # 站点名称、描述、作者、导航、社交链接、Giscus 配置
  content/
    blog/                # 博客文章（.md / .mdx）
    works/               # 作品 / 项目（.md / .mdx）
  pages/                 # 路由：首页、关于、博客、作品、搜索、RSS、OG 图片
  layouts/BaseLayout.astro
  styles/global.css      # 设计变量与全局样式
  i18n/                  # 界面文案字典
public/                  # favicon、默认分享图等静态资源
```

## 开始编辑

### 1. 站点信息

编辑 `src/consts.ts` 中的 `SITE`：

```ts
title: 'Moonless',
description: 'Projects, notes, and experiments from 10deg.',
author: '10deg',
footerText: '© 2026 Moonless',
```

同一文件里还可以调整：

- `NAV_ITEMS` — 顶部导航
- `SOCIAL_LINKS` — 页脚社交图标
- `GISCUS` — 博客评论（默认关闭，需要 GitHub Discussions + giscus App）

### 2. 发布文章

在 `src/content/blog/` 下新建 `.md` 或 `.mdx` 文件，参考
[`_template.md`](src/content/blog/_template.md)。以下划线开头的文件不会被构建，
所以模板文件可以安全保留。

```md
---
title: '文章标题'
publishDate: 2026-01-01
tags: ['astro']
description: '列表、meta 标签和 RSS 中使用的一句话摘要。'
draft: false
---

正文……
```

- `draft: true` 的文章不会出现在列表、RSS 或详情页。
- 可选 `heroImage`，图片放在 `src/content/blog/` 内，用相对路径引用。
- 文章 ID 就是去掉扩展名的文件名，会直接作为 URL：`/blog/<文件名>/`。

### 3. 发布作品

在 `src/content/works/` 下新建文件，参考
[`_template.md`](src/content/works/_template.md)。支持
`title`、`description`、`tech`、`link`、`repo`、`thumbnail`、`order`、`publishDate`。

### 4. 站点地址

`astro.config.mjs` 顶部的两项必须与 GitHub Pages 地址一致：

```js
site: 'https://10deg.github.io',
base: '/moonless',
```

如果以后改了仓库名，或绑定自定义域名，这两项也要同步修改（自定义域名时把 `base` 改为
`/`）。

### 5. 外观

- 强调色：`src/styles/global.css` 中的 `--color-accent`（浅色/深色各一处）。
- 字体、间距、版心宽度：同文件的 `:root` 变量。
- 页面文案：`src/pages/` 下对应页面；界面通用文案在 `src/i18n/en.ts`。

## 部署到 GitHub Pages

仓库已经包含 `.github/workflows/deploy.yml`，推送到 `main` 后会构建并部署。

首次使用需要在 GitHub 上启用一次 Pages：

1. 打开 <https://github.com/10deg/moonless/settings/pages>。
2. 在 **Build and deployment → Source** 选择 **GitHub Actions**。
3. 回到 **Actions**，运行（或等待）`Deploy to GitHub Pages` 工作流。

之后每次 `git push` 到 `main` 都会自动发布。`CI` 工作流会检查格式、类型和构建。

## 维护说明

- 内容文件夹里的 `_template.md` 不会被构建，可以复制后改名使用。
- 搜索索引在生产构建时生成，开发服务器上搜索不可用；用 `npm run build && npm run preview` 测试。
- 自动分享图使用英文字体（`src/pages/og/[collection]/[slug].png.ts`）。如果文章标题使用中文，需要把这里的字体换成包含中文的字体，否则分享图会缺字。
- 基于 [Astro Keel](https://github.com/kpab/astro-keel) 主题（MIT），已按个人站点需求清理和调整。

## License

[MIT](LICENSE)。主题原始版权归 [kpab](https://github.com/kpab) 所有。
