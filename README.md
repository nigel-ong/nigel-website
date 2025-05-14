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

## ✅ Initial Setup Overview

### Tools Required

- **Hugo Extended** (installed inside WSL)
- **Go** (required for Hugo Modules)
- **Node.js + npm** (for theme scripts and Bootstrap)
- **Git + GitHub**
- **WSL** (used instead of Windows CMD/PowerShell)


# 🚀 Hugo + Adritian Theme + GitHub Pages Deployment (WSL Setup Guide)

This is a comprehensive guide to set up, customize, and deploy a Hugo site using the **Adritian theme**, hosted via **GitHub Pages**, all within **WSL (Windows Subsystem for Linux)**.

---

## 🧰 Requirements

- Windows with WSL (Ubuntu or Debian recommended)
- Hugo Extended (must be installed in WSL)
- Go (for Hugo modules)
- Node.js + npm (for Bootstrap and helper scripts)
- Git + GitHub
- Custom Domain (optional, via Porkbun)

---

## 📁 Project Initialization

```bash
# Step 1: Create Hugo site
hugo new site nigelongsite
cd nigelongsite

# Step 2: Initialize Hugo Modules
hugo mod init github.com/nigel-ong/nigelonghugo

# Step 3: Add Adritian Theme
hugo mod get -u github.com/zetxek/adritian-free-hugo-theme

# Step 4: Generate package.json
hugo mod npm pack

# Step 5: Install Bootstrap & helper
npm install

# Step 6: Download demo content (must be in WSL)
node ./node_modules/@zetxek/adritian-theme-helper/dist/scripts/download-content.js
```

---

## 🛠️ Install Hugo Extended in WSL

```bash
cd ~
wget https://github.com/gohugoio/hugo/releases/download/v0.147.3/hugo_extended_0.147.3_Linux-64bit.tar.gz
tar -xvzf hugo_extended_0.147.3_Linux-64bit.tar.gz
sudo mv hugo /usr/local/bin/
hugo version  # Should show extended
```

---

## 🧪 Run Local Server

```bash
hugo server
# Open browser to http://localhost:1313
```

---

## 🔐 SSH Key Setup for GitHub (inside WSL)

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
cat ~/.ssh/id_ed25519.pub
# Paste into GitHub > Settings > SSH and GPG Keys
```

```bash
# Switch remote to SSH
git remote set-url origin git@github.com:nigel-ong/nigel-website.git
```

---

## 🔁 GitHub Actions for Deployment

Create `.github/workflows/deploy.yml`:

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

Porkbun A Records (required):

```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

---

## 🔄 To Update Your Site (Always in WSL)

```bash
wsl
cd /mnt/c/Users/nigel/OneDrive\ -\ BCIT/Desktop/Hugo_nigelong_site/nigelongsite

# Preview changes
hugo server

# Make changes to content/config/static

# Stage and commit changes
git add .
git commit -m "Your commit message"
git push
```

---

## 🧼 Recommended .gitignore

```gitignore
node_modules/
public/
resources/
temp-clone/
.DS_Store
```

---

## ✅ Done!

You're now fully set up to maintain and deploy a modern Hugo site with a powerful theme and automatic GitHub Pages deployment — all through WSL!
