# 🚀 Hugo Site with Adritian Theme on GitHub Pages (WSL Setup)

This guide documents the full setup process and instructions for maintaining and updating your personal website, built with [Hugo](https://gohugo.io/) and the [Adritian Hugo Theme](https://github.com/zetxek/adritian-free-hugo-theme), hosted on GitHub Pages. All steps are performed **inside WSL (Windows Subsystem for Linux)**.


Please note we must run in WSL, for ease of use go to the folder 
```bash
/Hugo_nigelong_site/nigelongsite
```
right click open in "open in terminal or in VScode navigate to this folder and type `wsl` in the terminal.
Currently WSL does not have HUGO installed (too lazy lmao) so to run local server, just do it in Windows instead of WSL
When updating and doing git pulls and pushes run in WSL mode because of the line ending issues. 

---

## 📚 Table of Contents

* [💻 Setup Notes](#💻-setup-notes)
* [✅ Initial Setup Overview](#✅-initial-setup-overview)
* [🚰 Install Hugo Extended in WSL](#🚰-install-hugo-extended-in-wsl)
* [🧪 Run Local Server](#🧪-run-local-server)
* [🔐 SSH Key Setup for GitHub](#🔐-ssh-key-setup-for-github)
* [🔁 GitHub Actions for Deployment](#🔁-github-actions-for-deployment)
* [🌍 GitHub Pages & Custom Domain Setup](#🌍-github-pages--custom-domain-setup)
* [📄 Handling the CNAME File](#📄-handling-the-cname-file)
* [🔄 Updating Your Site](#🔄-updating-your-site)
* [🩼 Recommended .gitignore](#🩼-recommended-gitignore)
* [🎨 Customizing the Site](#🎨-customizing-the-site)
* [📺 Embedding YouTube Videos](#📺play-embedding-youtube-videos)
* [🔧 Useful Git Commands](#🔧-useful-git-commands)
* [✅ Done!](#✅-done)

---

## 💻 Setup Notes

You must run Git and Hugo build commands in **WSL**.
To open WSL easily in the right place:

```bash
# Navigate to this folder in File Explorer or VSCode:
Hugo_nigelong_site/nigelongsite

# Open terminal, then:
wsl
```

🚨 Hugo server doesn't work in WSL currently (missing install), so just preview using Windows. But all `git` commands **must be run in WSL** due to line-ending issues.

---

## ✅ Initial Setup Overview

### Tools Required

* Hugo Extended (WSL)
* Go
* Node.js + npm
* Git + GitHub
* WSL (Ubuntu)

### Project Initialization

```bash
# 1. Create Hugo site
hugo new site nigelongsite
cd nigelongsite

# 2. Initialize Hugo Modules
hugo mod init github.com/nigel-ong/nigelonghugo

# 3. Add Adritian Theme
hugo mod get -u github.com/zetxek/adritian-free-hugo-theme

# 4. Generate package.json
hugo mod npm pack

# 5. Install Bootstrap & helpers
npm install

# 6. Download demo content
node ./node_modules/@zetxek/adritian-theme-helper/dist/scripts/download-content.js
```

---

## 🚰 Install Hugo Extended in WSL

```bash
cd ~
wget https://github.com/gohugoio/hugo/releases/download/v0.147.3/hugo_extended_0.147.3_Linux-64bit.tar.gz
tar -xvzf hugo_extended_0.147.3_Linux-64bit.tar.gz
sudo mv hugo /usr/local/bin/
hugo version
```

---

## 🧪 Run Local Server

```bash
hugo server
# Visit http://localhost:1313
```

---

## 🔐 SSH Key Setup for GitHub

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
cat ~/.ssh/id_ed25519.pub
# Paste into GitHub > Settings > SSH and GPG Keys
```

```bash
# Switch to SSH remote
git remote set-url origin git@github.com:nigel-ong/nigel-website.git
```

---

## 🔁 GitHub Actions for Deployment

`.github/workflows/deploy.yml`:

```yaml
name: Deploy Hugo site to GitHub Pages

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v3
        with:
          submodules: true
          fetch-depth: 0

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: 'latest'
          extended: true

      - name: Build
        run: hugo --minify

      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./public
```

---

## 🌍 GitHub Pages & Custom Domain Setup

1. GitHub → **Settings > Pages**
2. Source: `gh-pages` branch → root
3. Custom Domain: `nigel.ong`
4. Enforce HTTPS ✅

---

## 📄 Handling the CNAME File

Without this, your custom domain will disappear after each deploy.

```bash
echo "nigel.ong" > static/CNAME
```

📅 Hugo will copy this into `public/` and GitHub will keep your domain setting.

---

## 🔄 Updating Your Site

```bash
wsl
cd /mnt/c/Users/nigel/OneDrive\ -\ BCIT/Desktop/Hugo_nigelong_site/nigelongsite

# Preview
hugo server

# Update content/config
git add .
git commit -m "Update section"
git push
```

---

## 🩼 Recommended .gitignore

```gitignore
node_modules/
public/
resources/
temp-clone/
.DS_Store
```

---

## 🎨 Customizing the Site

* **Header Logo Text**: Go to `i18n/en.toml` or `en.yaml` and update `logo_text1` and `logo_text2`
* **Images**: Place images in `assets/` folder and reference them in `content/home.md` or other sections
* **Project Sections**: Add `.md` files under `content/projects/` using:

```bash
hugo new projects/my-project.md
```

---

## 📺play Embedding YouTube Videos or Hugo embedded video

`{{< youtube -vS5hrA8dEI >}}`

Paste this below the front matter (`---`) in any `.md` file:

```html
<div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%;">
  <iframe
    src="https://www.youtube.com/embed/YOUTUBE_VIDEO_ID"
    frameborder="0"
    allow="autoplay; encrypted-media"
    allowfullscreen
    style="position: absolute; top:0; left: 0; width: 100%; height: 100%;"
  ></iframe>
</div>
```

If using the Adritian theme and want to replace an image with a video:

* Copy the section layout to `layouts/partials/sections/client-and-work.html`
* Add support for `params.video`

---

## 🔧 Useful Git Commands

### Branching

```bash
git checkout -b my-branch-name      # Create & switch to new branch
git push -u origin my-branch-name   # Push new branch to GitHub
git checkout main                   # Switch back to main
git merge my-branch-name            # Merge into main
git push                            # Push updated main
```

### Delete Branch

```bash
git branch -d my-branch-name             # Delete local branch
git push origin --delete my-branch-name  # Delete remote branch
```

### Stash Uncommitted Changes

```bash
git stash           # Temporarily save changes
git stash pop       # Reapply them later
```

---

## ✅ Done!

You're now fully set up to maintain and deploy a modern Hugo site with a powerful theme and automatic GitHub Pages deployment — all through WSL!
