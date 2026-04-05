---
layout: post
title: "从零开始：使用 GitHub Pages + Jekyll 搭建个人博客"
date: 2026-04-05 12:00:00 +0800
categories: 技术
tags: [GitHub, Jekyll, 博客, 教程]
---

## 前言

最近想搭建一个个人技术博客，记录学习心得和项目经验。经过一番调研，最终选择了 **GitHub Pages + Jekyll** 方案。这个组合完全免费、部署简单、适合静态内容，而且 GitHub 官方支持，社区生态完善。本文将详细记录整个搭建过程，希望能帮助到同样想搭建博客的朋友。

## 为什么选择 GitHub Pages + Jekyll？

### GitHub Pages 的优势

- **完全免费**：GitHub 提供的静态网站托管服务，无需购买服务器
- **自动部署**：通过 Git 提交代码，自动触发构建和部署
- **HTTPS 支持**：自动配置 SSL 证书，保证访问安全
- **自定义域名**：支持绑定自己的域名
- **版本控制**：所有内容都在 Git 仓库中，方便管理历史

### Jekyll 的特点

- **静态网站生成器**：将 Markdown 文件转换为静态 HTML
- **Ruby 生态**：丰富的主题和插件
- **零配置**：GitHub Pages 原生支持，无需额外配置
- **专注写作**：用 Markdown 写文章，不需要关心前端细节

## 准备工作

在开始之前，你需要准备：

1. 一个 [GitHub](https://github.com) 账号
2. 本地安装 [Git](https://git-scm.com/)
3. 基本的命令行操作能力

## 第一步：配置 Git

首先配置 Git 的用户信息：

```bash
git config --global user.name "你的用户名"
git config --global user.email "your-email@example.com"
```

**注意**：这里的邮箱建议使用 GitHub 账号的邮箱，方便提交记录的识别。

## 第二步：生成 SSH 密钥

为了能够通过 SSH 方式与 GitHub 通信，需要生成 SSH 密钥对。

### 生成密钥

```bash
ssh-keygen -t ed25519 -C "your-email@example.com" -f ~/.ssh/id_ed25519 -N ""
```

**参数说明**：
- `-t ed25519`：使用 ED25519 算法（比 RSA 更安全、更快）
- `-C "email"`：添加注释（通常是邮箱）
- `-f ~/.ssh/id_ed25519`：指定密钥文件路径
- `-N ""`：设置空密码（免密登录）

### 添加密钥到 SSH 代理

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

### 添加公钥到 GitHub

1. 复制公钥内容：

```bash
cat ~/.ssh/id_ed25519.pub
```

你会看到类似这样的内容：

```
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIIzKJ44IjgfNwkZ/p8z/2+Q1G2GWTLJst1oYPpJW6FQc your-email@example.com
```

2. 在 GitHub 上添加公钥：
   - 打开 GitHub → Settings → SSH and GPG keys
   - 点击 "New SSH key"
   - Title 填写设备名称（如 "My Laptop"）
   - Key 粘贴上面的公钥内容
   - 点击 "Add SSH key"

### 测试连接

```bash
ssh -T git@github.com
```

如果看到类似 `Hi username! You've successfully authenticated` 的提示，说明配置成功！

## 第三步：创建 GitHub 仓库

1. 在 GitHub 上创建新仓库
2. 仓库名格式：`username.github.io`（username 是你的 GitHub 用户名）
3. 仓库选择 Public（公开）
4. 建议勾选 "Add a README file"

## 第四步：克隆仓库

```bash
git clone git@github.com:username/username.github.io.git
cd username.github.io
```

## 第五步：安装 Jekyll

Jekyll 是基于 Ruby 的，需要先安装 Ruby 相关工具。

### 安装 Ruby 和构建工具

**Ubuntu/Debian**：
```bash
sudo apt update
sudo apt install -y ruby ruby-dev build-essential zlib1g-dev
```

**macOS**（使用 Homebrew）：
```bash
brew install ruby
```

**Windows**：推荐使用 [RubyInstaller](https://rubyinstaller.org/)

### 安装 Jekyll 和 Bundler

```bash
gem install jekyll bundler
```

如果遇到权限问题，可以使用 `--user-install` 参数：

```bash
gem install --user-install jekyll bundler
```

然后将 gem 的 bin 目录添加到 PATH（根据系统可能不同）：

```bash
export PATH="$HOME/.local/share/gem/ruby/3.0.0/bin:$PATH"
```

验证安装：

```bash
jekyll --version
```

如果显示版本号（如 `jekyll 4.4.1`），说明安装成功。

## 第六步：初始化 Jekyll 博客

在仓库目录下运行：

```bash
jekyll new . --force
```

**参数说明**：
- `.`：在当前目录初始化（而不是创建子目录）
- `--force`：强制覆盖现有文件（如果目录不为空）

Jekyll 会自动生成以下文件结构：

```
.
├── _config.yml       # 站点配置文件
├── _posts/           # 文章目录
│   └── 2026-04-05-welcome-to-jekyll.markdown
├── about.markdown    # 关于页面
├── index.markdown    # 首页
├── 404.html          # 404 页面
├── Gemfile           # 依赖管理
└── Gemfile.lock      # 依赖版本锁定
```

## 第七步：配置博客信息

编辑 `_config.yml` 文件，修改基本信息：

```yaml
title: 你的博客标题
email: your-email@example.com
description: >-
  你的博客描述，会显示在搜索引擎结果中
baseurl: ""  # 站点的子路径（如果有）
url: "https://username.github.io"  # 你的博客地址
twitter_username:  # Twitter 用户名（可选）
github_username: username  # GitHub 用户名
```

**重要**：`url` 字段要填写正确的 GitHub Pages 地址，格式为 `https://username.github.io`。

## 第八步：提交并推送到 GitHub

```bash
git add -A
git commit -m "Initial commit: Jekyll blog setup"
git push -u origin main
```

**注意**：如果你的默认分支是 `master`，将 `main` 替换为 `master`。

## 第九步：启用 GitHub Pages

1. 打开 GitHub 仓库页面
2. 点击 **Settings** 标签
3. 左侧菜单找到 **Pages**
4. 在 **Build and deployment** 部分：

   **方案一：使用 GitHub Actions（推荐）**
   - Source 选择 **GitHub Actions**
   - GitHub 会自动创建工作流文件

   **方案二：从分支部署**
   - Source 选择 **Deploy from a branch**
   - Branch 选择 **main**（或 **master**）
   - Folder 选择 **/ (root)**
   - 点击 Save

5. 等待 1-2 分钟，GitHub 会自动构建你的博客

构建完成后，GitHub Pages 页面会显示你的博客地址：

```
https://username.github.io
```

## 第十步：写你的第一篇文章

### 文章格式

在 `_posts/` 目录下创建新文件，文件名格式：

```
YYYY-MM-DD-标题.markdown
```

例如：`2026-04-05-my-first-post.markdown`

### 文章结构

```markdown
---
layout: post
title: "文章标题"
date: 2026-04-05 12:00:00 +0800
categories: 分类
tags: [标签1, 标签2, 标签3]
---

文章内容，使用 Markdown 语法编写...

## 二级标题

### 三级标题

- 列表项 1
- 列表项 2

**粗体** 和 *斜体*

[链接文字](https://example.com)
```

### 分类和标签

- **categories（分类）**：可以有层级结构，如 `categories: [技术, 教程]`
- **tags（标签）**：扁平结构，如 `tags: [jekyll, 博客, markdown]`

## 本地预览（可选）

在推送之前，你可以本地预览博客效果：

```bash
cd username.github.io
bundle exec jekyll serve
```

然后在浏览器中访问 `http://localhost:4000`

**注意**：如果使用 `--user-install` 安装的 Jekyll，需要确保 gem 的 bin 目录在 PATH 中。

## 常见问题

### 1. 推送后博客没有更新

GitHub Pages 构建需要 1-2 分钟时间，耐心等待。也可以在仓库的 **Actions** 标签查看构建状态。

### 2. 样式显示异常

检查 `_config.yml` 中的 `url` 字段是否正确，应该是 `https://username.github.io`（不要加末尾的斜杠）。

### 3. 文章没有显示

确保文件名格式正确：`YYYY-MM-DD-标题.markdown`，日期必须是有效的。

### 4. 本地预览失败

尝试运行：

```bash
bundle install
bundle exec jekyll serve
```

## 进阶主题

### 更换主题

Jekyll 有很多现成的主题，可以查看 [Jekyll Themes](https://jekyllthemes.io/) 或者 GitHub 上的 [Awesome Jekyll Themes](https://github.com/planetjekyll/awesome-jekyll-themes)。

### 自定义域名

1. 在 `_config.yml` 中设置 `url` 和 `baseurl`
2. 在仓库 Settings → Pages 中添加自定义域名
3. 配置 DNS 解析（CNAME 记录指向 `username.github.io`）

### 添加评论功能

可以使用 [Giscus](https://giscus.app/)（基于 GitHub Discussions）或 [Disqus](https://disqus.com/) 等评论系统。

### 添加统计功能

使用 Google Analytics 或其他网站统计工具，在 `_includes/head.html` 中添加统计代码。

## 总结

使用 GitHub Pages + Jekyll 搭建博客是一个不错的选择，尤其是对于技术博客来说。整个过程不需要购买服务器，不需要配置复杂的后端，专注于内容创作即可。

如果你也想搭建自己的博客，希望这篇文章能帮到你。有任何问题，欢迎在评论区交流！

## 参考资料

- [GitHub Pages 官方文档](https://docs.github.com/zh/pages)
- [Jekyll 官方文档](https://jekyllrb.com/docs/)
- [GitHub Pages 使用 Jekyll 创建网站](https://docs.github.com/zh/pages/setting-up-a-github-pages-site-with-jekyll/creating-a-github-pages-site-with-jekyll)

---

**更新时间**：2026-04-05
