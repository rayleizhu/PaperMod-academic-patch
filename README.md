# PaperMod-Academic-patch Demo Site

English | [简体中文](README-zh.md)

Demo site for the [PaperMod-academic-patch](https://github.com/rayleizhu/PaperMod-academic-patch) academic homepage theme: it shows what the theme looks like, and it can also be forked as the starting point of your own homepage.

> Live site: <https://rayleizhu.github.io/PaperMod-Academic-patch/>
>
> Developer's personal homepage (built with this template): <https://rayleizhu.github.io/>
>
> The `main` branch of this repository is the **theme**, the `exampleSite` branch is the **site** (the branch you are looking at); the site pulls the theme in as a git submodule.

## How to Use

The idea is to **deploy the template as-is first, then customize it locally**, so you have a live site right from the start. 9 steps:

1. **Fork this repository**
   Fork it into your own GitHub account and name it `<username>.github.io` (replace `<username>` with your GitHub username) — the site will then be served at `https://<username>.github.io`.
   The default branch of this repository is `exampleSite`, and it stays that way after forking.

2. **Enable GitHub Pages**
   Settings → Code and automation → Pages → Source: **GitHub Actions**.

3. **Allow the deployment branch**
   Settings → Environments → github-pages → Edit → Deployment branches: **All branches** (or add `exampleSite` only).

   Hosting is ready at this point. To see what the template looks like, open the Actions tab, select `Deploy Hugo PaperMod Academic Demo to Pages` → **Run workflow**; once it finishes, visit `https://<username>.github.io` to browse the default site. You can also skip this — the build runs automatically when you push in step 9.

4. **Clone it locally**
   Clone your fork; the theme is a submodule, so `--recursive` is required:

   ```bash
   git clone --recursive https://github.com/<username>/<username>.github.io.git
   cd <username>.github.io
   ```

   If you already cloned it, or `themes/` is empty, fetch the submodules:

   ```bash
   git submodule update --init --recursive
   ```

5. **Install Hugo (extended build)**
   The theme relies on SCSS compilation and image resizing, so you need the **extended** build, `0.146.0` or newer; see [Requirements](#requirements) below. Afterwards run `hugo version` and make sure the output contains `+extended`.

6. **Fill in your own information**
   - `config.yml`: name, position, affiliation, email, profile picture, experience, awards, social icons, top navigation (see [Update your personal information](#update-your-personal-information)).
   - `content/`: `about.md` (bio), `pubs/` (publications), `news/` (news items), `blog/` (blog posts).

7. **Update `baseURL`**
   Set the `baseURL` at the top of `config.yml` to `https://<username>.github.io/` (keep the trailing `/`).

8. **Preview locally**
   Run `hugo server` in the repository root and open <http://localhost:1313/>; changes are hot-reloaded, so you can edit and check the result at the same time.

9. **Commit and push**

   ```bash
   git add -A && git commit -m "update my homepage" && git push
   ```

   Pushing triggers the build and deployment; when it finishes, refresh `https://<username>.github.io` and your homepage is live. From then on every change is just "preview locally → commit and push".

## Tech Stack

| Component | Description |
| --- | --- |
| [Hugo](https://gohugo.io/) | Static site generator — the **extended** build is required (SCSS compilation, image resizing) |
| [hugo-PaperMod](https://github.com/adityatelange/hugo-PaperMod) | Base theme, included as a git submodule |
| [PaperMod-academic-patch](https://github.com/rayleizhu/PaperMod-academic-patch) | Custom patch theme (the `main` branch of this repository), included as a git submodule; overrides the templates and styles of the homepage, publication list, news, etc. |

The two themes are **nested**: `config.yml` sets `theme: [PaperMod-academic-patch, hugo-PaperMod]`, where the first one takes precedence and only overrides what it needs to change; everything else comes from upstream.

## Requirements

- Hugo **extended** ≥ `0.146.0` (`min_version` of `hugo-PaperMod`; the deployment workflow pins `0.146.0`)

Pick either install method:

```bash
# 1) Official binary (recommended: you control the version; use ~/.local/bin when you have no root)
wget https://github.com/gohugoio/hugo/releases/download/v0.146.0/hugo_extended_0.146.0_linux-amd64.tar.gz
mkdir -p ~/.local/bin && tar -xzf hugo_extended_0.146.0_linux-amd64.tar.gz -C ~/.local/bin hugo
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc && source ~/.bashrc

# 2) Package manager
brew install hugo          # macOS
sudo apt install hugo      # Debian/Ubuntu (make sure it is the extended build)
```

Verify:

```bash
hugo version   # the output must contain +extended
```

## Local Development

```bash
hugo server                            # local preview
hugo server -D                         # include drafts
hugo server --bind 0.0.0.0 --port 1313 # allow LAN / remote access
hugo                                   # build only, output goes to public/
```

> To preview on a remote machine you need port forwarding: `ssh -L 1313:localhost:1313 <host>`.
> Keep your local Hugo version in sync with the `0.146.0` pinned by the workflow to avoid differences in build behaviour (for example, newer Hugo releases fail on duplicate front matter keys).

## Directory Structure

```
.(site root)
├── assets/                     # processed by Hugo Pipes (hashed, resized on demand)
│   └── images/
│       ├── prof_pic.webp       # profile picture on the homepage
│       ├── papermod-cover.png  # social sharing cover image
│       └── msg.png             # sample post cover
├── content/
│   ├── about.md                # bio (render: never — spliced into the homepage, not a standalone page)
│   ├── blog/                   # blog
│   │   ├── posts/*.md
│   │   ├── archives.md         # → /blog/archives/
│   │   └── search.md           # → /blog/search/
│   ├── news/                   # news
│   │   ├── _index.md           # → /news/ full list (grouped by year)
│   │   └── 2023-03-01-*.md     # single news item (render: never, used as data)
│   └── pubs/                   # publications
│       ├── _index.md           # → /pubs/ full list (switchable by topic / by year)
│       ├── 1905-*.md           # single publication (render: never, used as data)
│       └── preview/            # publication images (headless bundle)
├── static/
│   └── assets/favicon_ae/      # favicons (copied as-is, no hash in the URL)
├── themes/
│   ├── hugo-PaperMod/          # submodule: upstream theme
│   └── PaperMod-academic-patch/  # submodule: the patch theme from the main branch
├── config.yml                  # site config (personal info, menus, social icons, ...)
└── .github/workflows/gh-pages.yml
```

## Day-to-day Maintenance

### Update your personal information

Almost everything lives under `params.profileMode` in `config.yml`: `title` / `subtitle` / `affiliation` / `email` / `imageUrl`, `experience`, `awards`. A few switches worth knowing:

- `academic: true`: enables the academic homepage (profile + bio + publications + news + experience + awards).
- `sideBySideAbout`: when `true`, `about.md` is placed to the right of the profile picture; when `false`, it becomes a separate section further down.
- `showToc`: whether to show the table of contents on the right.
- `nameInPub`: the name that gets bolded in publication author lists.

Social icons are configured in `params.socialIcons` (`github`, `googlescholar`, `linkedin`, `orcid`, `openreview`, `dblp`, `alphaxiv`, ...), the top menu in `languages.en.menu.main`.

### Add a publication

Create `<year>-<short-name>.md` in `content/pubs/`:

```yaml
---
title: "On the Electrodynamics of Moving Bodies"
date: 1905-06-30
authors: ["Your Name", "Coauthor1", "Coauthor2"]
equal_contributors: ["Your Name"]        # equal contribution, marked with *
corresponding_authors: ["Coauthor2"]    # corresponding author, marked with ✉
venue: "Annals of Physics"
abbr: "Ann. Phys."
topics: ["Relativity"]                   # grouping on the /pubs/ page
preview: "special_relativity.png"        # image, put it in content/pubs/preview/
full_text: "https://en.wikisource.org/wiki/On_the_Electrodynamics_of_Moving_Bodies"
code: "https://github.com/<username>/<repo>"
selected: true                           # feature it under "Selected Publications" on the homepage
---

One-sentence summary (optional, shown below the title).
```

Put the image in `content/pubs/preview/` (a headless bundle that is never rendered as a page itself). The homepage also shows a legend explaining the bold name, `*` and `✉`.

### Add a news item

Create `<date>-<short-name>.md` in `content/news/`:

```yaml
---
title: "Example News"
date: 2023-03-01
---

Write the news here; Markdown and links are supported.
```

The homepage shows the 5 most recent items in reverse chronological order; the full list lives at `/news/`.

### Write a blog post

Create a `.md` file in `content/blog/posts/`. Add `draft: true` for drafts (excluded from the build by default; preview them locally with `hugo server -D`).

## Deployment

Pushing to the `exampleSite` branch triggers [`.github/workflows/gh-pages.yml`](.github/workflows/gh-pages.yml): install Hugo extended 0.146.0 → fetch submodules → `hugo --buildDrafts --gc --baseURL <Pages URL>` → upload `public/` and publish it to GitHub Pages. You can also trigger it manually from the Actions tab (with an optional Hugo version).

Note that the build command passes `--buildDrafts`, so drafts are published to the live site as well.

## Notes

- **Submodules must be initialized**, otherwise Hugo reports missing themes/templates.
- **`assets/` vs `static/`**: images that need Hugo processing (resizing, hashing) go to `assets/`; files that need a fixed URL (favicons, `robots.txt`) go to `static/`.
- `content/news/**` and `content/pubs/**` are set to `render: never, list: local` by the `cascade` in `config.yml` — a single news item or publication is only a data source referenced by the homepage and the list pages, **not a standalone page**, so visiting its URL gives a 404. The list pages themselves (`_index.md`) are real pages, i.e. `/news/` and `/pubs/`.
- `content/about.md` is likewise `build.render: never`; its content is spliced into the homepage, so `/about/` is a 404 (the About menu entry points to `/`).
- `params.env: production` enables Google Analytics, OpenGraph and image resizing; switch it to `development` temporarily if you want to skip these while debugging locally.
- `public/` and `resources/_gen/` are build artifacts, already in `.gitignore`; do not commit them.
- After the theme is updated you need to move the pointer in the site: `git pull` the latest `main` inside `themes/PaperMod-academic-patch`, then commit the new submodule pointer at the repository root (CI checks out the pointer, it does not follow the latest commit automatically).

## TODO

- [x] GitHub Actions for auto-deploying to GitHub Pages
- [x] More instructions in the README
