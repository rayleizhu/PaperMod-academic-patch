
PaperMod-academic-patch is a patch for [PaperMod theme](https://github.com/adityatelange/hugo-PaperMod/) for academic homepages.

## How to use

TBA

## How it works

- Purpose: this repository contains overrides (layouts, partials, shortcodes, assets and example config) that adapt the PaperMod theme for academic homepages.
- Mechanism: Hugo uses the first matching template or asset it finds. Applying this patch replaces or augments PaperMod files so Hugo renders the academic-oriented layouts and components instead of (or in addition to) the upstream defaults. Hugo override order is: site layouts/assets &gt; theme 1 layouts/assets &gt; theme 2 layouts/assets &gt; etc.

## TODO

- [x] site template using this theme patch
- [ ] refactor homepage layout to use sections/partials better
- [ ] usage instructions
- [x] adaptive layout for multiple devices
- [ ] professional service section
- [ ] merge header and About Section
- [x] Publication template (sort by year, sort by topic) 
- [ ] full News page
- [ ] other custom page templates (recruitment, teaching, etc.)
- [x] last updated timestamp on homepage
