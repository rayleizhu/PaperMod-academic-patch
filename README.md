
## How to Use

1. install [Hugo](https://gohugo.io/getting-started/installing/) if you don't have it yet
2. fork this repo to your own github account with a name like `https://github.com/yourusername/yourusername.github.io`
3. clone your forked repo to your local machine `git clone --recursive https://github.com/yourusername/yourusername.github.io`
4. make changes to the `config.yml` file and the content files in the `content/` folder
5. run `hugo server` in the root folder of the repo to preview your homepage
6. enable github pages in the repo settings, set Code and automation -> Pages -> Source to `Github Actions`.
7. in GitHub repo: Settings -> Environments -> github-pages -> Edit: Deployment branches: set to “All branches” or add exampleSite.
8. back to local machine, commit and push your changes to your github repo
9. visit `https://yourusername.github.io` to see your homepage live

## TODO

- [x] github actions for auto-deploying to gh-pages
- [ ] update README with more instructions