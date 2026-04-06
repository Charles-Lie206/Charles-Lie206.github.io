---
layout: single
title: "From Scratch: Building a Personal Blog with GitHub Pages + Jekyll"
date: 2026-04-05 10:00:00 +0800
categories: Technology
tags: [GitHub, Jekyll, Blog, Tutorial]
read_time: true
---

## Introduction

Recently, I wanted to set up a personal technical blog to record my learning insights and project experiences. After some research, I finally chose the **GitHub Pages + Jekyll** solution. This combination is completely free, easy to deploy, suitable for static content, and has official GitHub support with a well-developed community ecosystem. This article will document the entire setup process in detail, hoping to help friends who also want to build a blog.

## Why Choose GitHub Pages + Jekyll?

### Advantages of GitHub Pages

- **Completely Free**: Static site hosting service provided by GitHub, no need to purchase servers
- **Automatic Deployment**: Automatically trigger build and deployment by committing code via Git
- **HTTPS Support**: Automatically configure SSL certificates to ensure secure access
- **Custom Domain**: Support binding your own domain name
- **Version Control**: All content is in a Git repository, making it easy to manage history

### Features of Jekyll

- **Static Site Generator**: Converts Markdown files to static HTML
- **Ruby Ecosystem**: Rich themes and plugins
- **Zero Configuration**: Native support from GitHub Pages, no additional configuration needed
- **Focus on Writing**: Write articles in Markdown without worrying about frontend details

## Prerequisites

Before starting, you need to prepare:

1. A [GitHub](https://github.com) account
2. [Git](https://git-scm.com/) installed locally
3. Basic command-line operation skills

## Step 1: Configure Git

First, configure Git user information:

```bash
git config --global user.name "your-username"
git config --global user.email "your-email@example.com"
```

**Note**: It's recommended to use your GitHub account's email here for easy identification of commit records.

## Step 2: Generate SSH Keys

To communicate with GitHub via SSH, you need to generate an SSH key pair.

### Generate Keys

```bash
ssh-keygen -t ed25519 -C "your-email@example.com" -f ~/.ssh/id_ed25519 -N ""
```

**Parameter Explanation**:
- `-t ed25519`: Use ED25519 algorithm (more secure and faster than RSA)
- `-C "email"`: Add a comment (usually email)
- `-f ~/.ssh/id_ed25519`: Specify the key file path
- `-N ""`: Set empty password (passwordless login)

### Add Key to SSH Agent

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

### Add Public Key to GitHub

1. Copy the public key content:

```bash
cat ~/.ssh/id_ed25519.pub
```

You'll see something like this:

```
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIIzKJ44IjgfNwkZ/p8z/2+Q1G2GWTLJst1oYPpJW6FQc your-email@example.com
```

2. Add the public key on GitHub:
   - Open GitHub → Settings → SSH and GPG keys
   - Click "New SSH key"
   - Title: Enter device name (e.g., "My Laptop")
   - Key: Paste the public key content above
   - Click "Add SSH key"

### Test Connection

```bash
ssh -T git@github.com
```

If you see a prompt like `Hi username! You've successfully authenticated`, the configuration is successful!

## Step 3: Create GitHub Repository

1. Create a new repository on GitHub
2. Repository name format: `username.github.io` (username is your GitHub username)
3. Choose Public for the repository
4. Recommended to check "Add a README file"

## Step 4: Clone Repository

```bash
git clone git@github.com:username/username.github.io.git
cd username.github.io
```

## Step 5: Install Jekyll

Jekyll is based on Ruby, so you need to install Ruby-related tools first.

### Install Ruby and Build Tools

**Ubuntu/Debian**:
```bash
sudo apt update
sudo apt install -y ruby ruby-dev build-essential zlib1g-dev
```

**macOS** (using Homebrew):
```bash
brew install ruby
```

**Windows**: Recommended to use [RubyInstaller](https://rubyinstaller.org/)

### Install Jekyll and Bundler

```bash
gem install jekyll bundler
```

If you encounter permission issues, you can use the `--user-install` parameter:

```bash
gem install --user-install jekyll bundler
```

Then add the gem's bin directory to PATH (may vary by system):

```bash
export PATH="$HOME/.local/share/gem/ruby/3.0.0/bin:$PATH"
```

Verify installation:

```bash
jekyll --version
```

If a version number is displayed (e.g., `jekyll 4.4.1`), the installation is successful.

## Step 6: Initialize Jekyll Blog

Run in the repository directory:

```bash
jekyll new . --force
```

**Parameter Explanation**:
- `.`: Initialize in the current directory (rather than creating a subdirectory)
- `--force`: Force overwrite existing files (if directory is not empty)

Jekyll will automatically generate the following file structure:

```
.
├── _config.yml       # Site configuration file
├── _posts/           # Posts directory
│   └── 2026-04-05-welcome-to-jekyll.markdown
├── about.markdown    # About page
├── index.markdown    # Homepage
├── 404.html          # 404 page
├── Gemfile           # Dependency management
└── Gemfile.lock      # Dependency version lock
```

## Step 7: Configure Blog Information

Edit the `_config.yml` file and modify basic information:

```yaml
title: Your Blog Title
email: your-email@example.com
description: >-
  Your blog description, which will appear in search engine results
baseurl: ""  # Subpath of your site (if any)
url: "https://username.github.io"  # Your blog address
twitter_username:  # Twitter username (optional)
github_username: username  # GitHub username
```

**Important**: The `url` field should be filled with the correct GitHub Pages address, in the format `https://username.github.io`.

## Step 8: Commit and Push to GitHub

```bash
git add -A
git commit -m "Initial commit: Jekyll blog setup"
git push -u origin main
```

**Note**: If your default branch is `master`, replace `main` with `master`.

## Step 9: Enable GitHub Pages

1. Open the GitHub repository page
2. Click the **Settings** tab
3. Find **Pages** in the left menu
4. In the **Build and deployment** section:

   **Option 1: Use GitHub Actions (Recommended)**
   - Source select **GitHub Actions**
   - GitHub will automatically create workflow files

   **Option 2: Deploy from Branch**
   - Source select **Deploy from a branch**
   - Branch select **main** (or **master**)
   - Folder select **/ (root)**
   - Click Save

5. Wait 1-2 minutes, GitHub will automatically build your blog

After the build is complete, the GitHub Pages page will display your blog address:

```
https://username.github.io
```

## Step 10: Write Your First Article

### Article Format

Create a new file in the `_posts/` directory with the filename format:

```
YYYY-MM-DD-title.markdown
```

For example: `2026-04-05-my-first-post.markdown`

### Article Structure

```markdown
---
layout: post
title: "Article Title"
date: 2026-04-05 12:00:00 +0800
categories: Category
tags: [tag1, tag2, tag3]
---

Article content, written in Markdown syntax...

## Level 2 Heading

### Level 3 Heading

- List item 1
- List item 2

**Bold** and *Italic*

[Link Text](https://example.com)
```

### Categories and Tags

- **categories**: Can have hierarchical structure, e.g., `categories: [Technology, Tutorial]`
- **tags**: Flat structure, e.g., `tags: [jekyll, blog, markdown]`

## Local Preview (Optional)

Before pushing, you can preview the blog locally:

```bash
cd username.github.io
bundle exec jekyll serve
```

Then visit `http://localhost:4000` in your browser.

**Note**: If Jekyll was installed with `--user-install`, ensure the gem's bin directory is in your PATH.

## Common Issues

### 1. Blog Not Updating After Push

GitHub Pages build takes 1-2 minutes, be patient. You can also check the build status in the repository's **Actions** tab.

### 2. Style Display Abnormalities

Check if the `url` field in `_config.yml` is correct, it should be `https://username.github.io` (don't add a trailing slash).

### 3. Article Not Displaying

Ensure the filename format is correct: `YYYY-MM-DD-title.markdown`, and the date must be valid.

### 4. Local Preview Failed

Try running:

```bash
bundle install
bundle exec jekyll serve
```

## Advanced Topics

### Change Theme

Jekyll has many ready-made themes. You can check [Jekyll Themes](https://jekyllthemes.io/) or [Awesome Jekyll Themes](https://github.com/planetjekyll/awesome-jekyll-themes) on GitHub.

### Custom Domain

1. Set `url` and `baseurl` in `_config.yml`
2. Add custom domain in repository Settings → Pages
3. Configure DNS resolution (CNAME record pointing to `username.github.io`)

### Add Comment Functionality

You can use [Giscus](https://giscus.app/) (based on GitHub Discussions) or [Disqus](https://disqus.com/) and other comment systems.

### Add Analytics

Use Google Analytics or other website analytics tools, add tracking code in `_includes/head.html`.

## Summary

Using GitHub Pages + Jekyll to build a blog is a great choice, especially for technical blogs. The entire process doesn't require purchasing servers or configuring complex backends, just focus on content creation.

If you also want to build your own blog, I hope this article helps you. If you have any questions, feel free to discuss in the comments!

## References

- [GitHub Pages Official Documentation](https://docs.github.com/en/pages)
- [Jekyll Official Documentation](https://jekyllrb.com/docs/)
- [Creating a GitHub Pages Site with Jekyll](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/creating-a-github-pages-site-with-jekyll)

---

**Updated**: 2026-04-05
