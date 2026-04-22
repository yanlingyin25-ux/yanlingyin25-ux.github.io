---
layout: post
title: "Hello World：从零部署一个静态个人主页"
date: 2026-04-22 14:00:00 +0800
categories: [技术, 部署]
tags: [jekyll, github-pages, 静态网站]
---

这是这个网站的第一篇文章，也是整个部署过程的完整记录。

## 为什么要做这个站

一人公司的三个矢量——期权交易、AI SaaS 创业、内容创作——都需要一个中心化的输出窗口。之前的内容散在各个平台，没有属于自己的地盘。一个静态个人主页是最低成本、最可控的解决方案。

## 技术选型的思路

在选抎框架时用了第一性原理：

1. **目标是什么**：一个可公开访问的、支持博客功能的、能自己控制的个人页面
2. **约束条件是什么**：零服务器成本、零运维成本、推代码即部署
3. **现有工具能做什么**：GitHub Pages 免费托管静态网站，Jekyll 是其原生支持的静态站点生成器

最终选择的组合：**Jekyll + GitHub Pages**。逆向思考去掉的选项：

- 不用 WordPress/有数据库的 CMS —— 增加攻击面和运维负担
- 不用自己购买服务器 —— 非必要支出
- 不用复杂的 CI/CD 流水线 —— GitHub Pages 已内置 Jekyll 构建

## 部署步骤

### 1. 生成 SSH 密钥对

```bash
ssh-keygen -t ed25519 -C "yanlingyin25@gmail.com" -f ~/.ssh/github_pages
```

使用 SSH 而非 HTTPS Token，因为：
- 私钥永存本地，一劳永逸
- 不存在 Token 过期问题
- 所有 GitHub 项目通用

将公钥添加到 GitHub Settings → SSH and GPG keys。

### 2. 创建本地项目

```bash
mkdir -p ~/github-pages-test
cd ~/github-pages-test
git init
```

### 3. 创建 GitHub 仓库

仓库名必须严格匹配用户名：`yanlingyin25-ux.github.io`。这样才能通过根域名 `https://yanlingyin25-ux.github.io/` 直接访问。

```bash
git remote add origin git@github.com:yanlingyin25-ux/yanlingyin25-ux.github.io.git
```

### 4. 配置 Jekyll

创建 `_config.yml`：

```yaml
title: "小天"
description: "系统思维 · 财富自由 · AI 原生"
url: "https://yanlingyin25-ux.github.io"
plugins:
  - jekyll-feed
  - jekyll-sitemap
  - jekyll-seo-tag
```

创建 `Gemfile`：

```ruby
source "https://rubygems.org"
gem "github-pages", group: :jekyll_plugins
```

### 5. 创建页面结构

Jekyll 的核心概念：

- **`_layouts/`** —— 页面布局模板（HTML + Liquid 语法）
- **`_posts/`** —— 博客文章（Markdown 格式，文件名必须含日期）
- **`assets/css/`** —— 静态资源
- **`index.html`** —— 首页
- **`about.md`** / **`blog.md`** —— 其他页面

### 6. 自定义主题

没有使用现成主题，而是从零编写 CSS。设计意图：

- 暗色主题过时了，切换到 Claude 风格的温暖米白配色
- 配色方案：`#FAFAF8` 背景 + `#D97757` 珊瑚橡炬色强调
- 使用 CSS 变量（`:root` 自定义属性）方便统一调整

### 7. 推送并部署

```bash
git add -A
git commit -m "Initial Jekyll setup"
git branch -M main
git push -u origin main
```

推送后 GitHub 自动触发 Jekyll 构建，约 30 秒后通过 `https://yanlingyin25-ux.github.io/` 访问。

## 文章工作流

文章的本地存储位置是项目内的 `_posts/` 目录。文件命名规范：

```
_posts/YYYY-MM-DD-标题.md
```

每篇文章必须包含 front matter：

```markdown
---
layout: post
title: "文章标题"
date: 2026-04-22 12:00:00 +0800
categories: [分类1, 分类2]
---

正文内容...
```

写完后 `git push`，GitHub Pages 自动更新。

## 未来优化

- [ ] 整合 RSS 订阅（`jekyll-feed` 已安装，需要验证）
- [ ] 添加访客统计（Plausible 或 GoatCounter，无 Cookie 方案）
- [ ] SEO 优化（结构化数据、Open Graph）
- [ ] 暗色/亮色模式切换
- [ ] 自动化部署：本地写文章 → cron 任务自动推送 → 自动构建

---

**结论**：从零到一个可访问的个人站点，整个流程用时约 20 分钟。真正的成本不是时间，而是决策——选择 Jekyll 意味着接受了 Markdown + Liquid 的技术栈，这是一个可逆的决定。如果未来需要更复杂的交互功能，可以平滑迁移到 Astro 或 Next.js，但目前的静态站点足够了。
