# PaperMod-Academic-patch 演示站点

[English](README.md) | 简体中文

[PaperMod-academic-patch](https://github.com/rayleizhu/PaperMod-academic-patch) 学术主页主题的示例站点：既用来展示主题效果，也可以直接 fork 后当成个人主页的起点。

> Demo站点：<https://rayleizhu.github.io/PaperMod-Academic-patch/>
>
> 开发者个人主页（使用本模板）：<https://rayleizhu.github.io/>
>
> 本仓库 `main` 分支是**主题**，`exampleSite` 分支是**站点**（也就是你当前看到的分支）；站点通过 git submodule 引用主题。

## How to Use

思路是**先把模板原样部署上线，再回到本地慢慢改成自己的内容**，这样你从一开始就有了一个能访问的站点。共 9 步：

1. **Fork 本仓库**
   把仓库 fork 到你自己的 GitHub 账号，名字取 `<username>.github.io`（`<username>` 换成你的用户名），之后站点地址就是 `https://<username>.github.io`。
   本仓库默认分支就是 `exampleSite`，fork 后保持不变。

2. **开启 GitHub Pages**
   仓库 Settings → Code and automation → Pages → Source 选 **GitHub Actions**。

3. **允许部署分支**
   Settings → Environments → github-pages → Edit → Deployment branches 设为 **All branches**（或单独添加 `exampleSite`）。

   到这里托管就配置好了。想先看看模板长什么样，可以在 Actions 页面选中 `Deploy Hugo PaperMod Academic Demo to Pages` → **Run workflow** 手动跑一次，跑完访问 `https://<username>.github.io`，就能看到模板的默认主页（也可以跳过，等第 9 步推送时自动构建）。

4. **克隆到本地**
   把 fork 后的仓库克隆下来，主题是 submodule，`--recursive` 不能少：

   ```bash
   git clone --recursive https://github.com/<username>/<username>.github.io.git
   cd <username>.github.io
   ```

   已经克隆过、或 `themes/` 是空目录时，补拉 submodule：

   ```bash
   git submodule update --init --recursive
   ```

5. **安装 Hugo（extended 版本）**
   主题依赖 SCSS 编译与图片缩放，必须是 extended 版且不低于 `0.146.0`，安装方法见下方[环境要求](#环境要求)。装好后用 `hugo version` 确认输出中带有 `+extended`。

6. **改成你自己的信息**
   - `config.yml`：姓名、职位、单位、邮箱、头像、经历、获奖、社交图标、顶部导航（见[改个人信息](#改个人信息)）。
   - `content/`：`about.md`（自我介绍）、`pubs/`（论文）、`news/`（动态）、`blog/`（博客）。

7. **同步 `baseURL`**
   把 `config.yml` 顶部的 `baseURL` 改成 `https://<username>.github.io/`（结尾的 `/` 不要漏）。

8. **本地预览**
   在仓库根目录运行 `hugo server`，打开 <http://localhost:1313/>，改动会自动热重载，可以边改边看效果。

9. **提交并推送**

   ```bash
   git add -A && git commit -m "update my homepage" && git push
   ```

   push 会自动触发构建部署，完成后刷新 `https://<username>.github.io`，你的主页就上线了。以后每次改动都是「本地预览 → 提交推送」这两步。

## 技术栈

| 组成 | 说明 |
| --- | --- |
| [Hugo](https://gohugo.io/) | 静态站点生成器，**需要 extended 版本**（编译 SCSS、缩放图片） |
| [hugo-PaperMod](https://github.com/adityatelange/hugo-PaperMod) | 基础主题，以 git submodule 引入 |
| [PaperMod-academic-patch](https://github.com/rayleizhu/PaperMod-academic-patch) | 自研补丁主题（本仓库 `main` 分支），以 git submodule 引入，覆盖首页、论文列表、News 等模板与样式 |

两个主题是**嵌套关系**：`config.yml` 里 `theme: [PaperMod-academic-patch, hugo-PaperMod]`，前者优先级更高，只覆盖需要改动的部分，其余沿用上游。

## 环境要求

- Hugo **extended** ≥ `0.146.0`（`hugo-PaperMod` 的 `min_version`，部署工作流默认锁定 `0.146.0`）

安装方式任选其一：

```bash
# 1) 官方二进制（推荐，版本可控；无 root 权限时放 ~/.local/bin）
wget https://github.com/gohugoio/hugo/releases/download/v0.146.0/hugo_extended_0.146.0_linux-amd64.tar.gz
mkdir -p ~/.local/bin && tar -xzf hugo_extended_0.146.0_linux-amd64.tar.gz -C ~/.local/bin hugo
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc && source ~/.bashrc

# 2) 包管理器
brew install hugo          # macOS
sudo apt install hugo      # Debian/Ubuntu（需确认是 extended 版）
```

验证：

```bash
hugo version   # 输出中必须包含 +extended
```

## 本地开发

```bash
hugo server                            # 本地预览
hugo server -D                         # 连草稿一起预览
hugo server --bind 0.0.0.0 --port 1313 # 允许局域网/远程访问
hugo                                   # 仅构建，产物输出到 public/
```

> 在远程开发机上预览需要端口转发：`ssh -L 1313:localhost:1313 <host>`。
> 建议本地 Hugo 版本与工作流锁定的 `0.146.0` 保持一致，避免版本差异带来的构建行为不同（例如新版 Hugo 会对 front matter 中的重复键直接报错）。

## 目录结构

```
.(site root)
├── assets/                     # 经 Hugo Pipes 处理的资源（会被加 hash、按需缩放）
│   └── images/
│       ├── prof_pic.webp       # 首页头像
│       ├── papermod-cover.png  # 社交分享封面图
│       └── msg.png             # 示例文章封面
├── content/
│   ├── about.md                # About 正文（render: never，被拼进首页，不是独立页面）
│   ├── blog/                   # 博客
│   │   ├── posts/*.md
│   │   ├── archives.md         # → /blog/archives/
│   │   └── search.md           # → /blog/search/
│   ├── news/                   # News
│   │   ├── _index.md           # → /news/ 完整列表页（按年份分组）
│   │   └── 2023-03-01-*.md     # 单条 news（render: never，作为数据源）
│   └── pubs/                   # 论文
│       ├── _index.md           # → /pubs/ 完整列表页（可切换按 topic / 按年份）
│       ├── 1905-*.md           # 单篇论文（render: never，作为数据源）
│       └── preview/            # 论文配图（headless bundle）
├── static/
│   └── assets/favicon_ae/      # 站点图标（原样拷贝，URL 不带 hash）
├── themes/
│   ├── hugo-PaperMod/          # submodule：上游主题
│   └── PaperMod-academic-patch/  # submodule：本仓库 main 分支的补丁主题
├── config.yml                  # 站点配置（个人信息、导航、社交图标等）
└── .github/workflows/gh-pages.yml
```

## 日常维护

### 改个人信息

几乎都在 `config.yml` 的 `params.profileMode` 下：`title` / `subtitle` / `affiliation` / `email` / `imageUrl`、`experience`（经历）、`awards`（获奖）。几个开关值得留意：

- `academic: true`：启用学术首页（头像 + About + 论文 + News + 经历 + 获奖）。
- `sideBySideAbout`：为 `true` 时把 `about.md` 放到头像右侧，为 `false` 时作为独立区块排在首页。
- `showToc`：是否在右侧显示目录。
- `nameInPub`：在论文作者列表中需要加粗的名字。

社交图标在 `params.socialIcons`（支持 `github`、`googlescholar`、`linkedin`、`orcid`、`openreview`、`dblp`、`alphaxiv` 等），顶部导航在 `languages.en.menu.main`。

### 加一篇论文

在 `content/pubs/` 新建 `<年份>-<简称>.md`：

```yaml
---
title: "On the Electrodynamics of Moving Bodies"
date: 1905-06-30
authors: ["Your Name", "Coauthor1", "Coauthor2"]
equal_contributors: ["Your Name"]        # 共同一作，名字后加 *
corresponding_authors: ["Coauthor2"]    # 通讯作者，名字后加 ✉
venue: "Annals of Physics"
abbr: "Ann. Phys."
topics: ["Relativity"]                   # /pubs/ 页面的按主题分组
preview: "special_relativity.png"        # 配图，放在 content/pubs/preview/
full_text: "https://en.wikisource.org/wiki/On_the_Electrodynamics_of_Moving_Bodies"
code: "https://github.com/<username>/<repo>"
selected: true                           # 是否在首页「Selected Publications」展示
---

一句话简介（可选，首页会显示在标题下方）。
```

配图放进 `content/pubs/preview/`（该目录是 headless bundle，本身不会被渲染成页面）。首页会显示 `${nameInPub}` 加粗、`*` 与 `✉` 的图例说明。

### 加一条 News

在 `content/news/` 新建 `<日期>-<简称>.md`：

```yaml
---
title: "Example News"
date: 2023-03-01
---

这里写新闻内容，支持 Markdown 与链接。
```

首页按日期倒序显示最近 5 条，完整列表在 `/news/`。

### 写一篇博客

在 `content/blog/posts/` 新建 `.md`。草稿加 `draft: true`（默认不会被构建，本地用 `hugo server -D` 预览）。

## 部署

推送到 `exampleSite` 分支即触发 [`.github/workflows/gh-pages.yml`](.github/workflows/gh-pages.yml)：安装 Hugo extended 0.146.0 → 拉取 submodule → `hugo --buildDrafts --gc --baseURL <Pages URL>` → 上传 `public/` 并发布到 GitHub Pages。也可以在 Actions 页面手动触发（可指定 Hugo 版本）。

注意构建命令带 `--buildDrafts`，草稿内容同样会出现在线上站点。

## 注意事项

- **必须初始化 submodule**，否则会报找不到主题/模板。
- **`assets/` 与 `static/` 的区别**：需要 Hugo 处理（缩放、加 hash）的图片放 `assets/`；要固定 URL 的文件（favicon、`robots.txt`）放 `static/`。
- `content/news/**` 和 `content/pubs/**` 在 `config.yml` 的 `cascade` 中被设为 `render: never, list: local`——单条 news / 单篇论文只作为数据源被首页和列表页引用，**不是独立页面**，直接访问对应 URL 会 404。列表页本身（`_index.md`）是真实页面，即 `/news/` 与 `/pubs/`。
- `content/about.md` 同样设了 `build.render: never`，内容拼在首页，所以 `/about/` 是 404（导航里的 About 指向 `/`）。
- `params.env: production` 会启用 Google Analytics、OpenGraph 以及图片缩放；本地调试时若想跳过这些，可临时改成 `development`。
- `public/` 和 `resources/_gen/` 是构建产物，已在 `.gitignore` 中，不要提交。
- 主题更新后需要在站点里同步指针：进入 `themes/PaperMod-academic-patch` 拉取 `main` 最新提交，然后在仓库根目录提交新的 submodule 指针（CI 按指针拉取，不会自动跟随最新）。

## TODO

- [x] github actions for auto-deploying to gh-pages
- [x] update README with more instructions
