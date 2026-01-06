# Cloudflare Pages

## 概述

Cloudflare Pages是一个用于部署静态网站和前端应用程序的JAMstack平台。它特别适合现代框架，如Astro、Next.js、React、Vue等。

## 主要好处

### 静态网站完全免费
- **零成本**托管静态页面
- **免费层级无带宽限制**
- **无限制请求** - 非常适合生产网站
- 无隐性费用或意外账单

### 安全和性能
- **内置DDoS保护**
- **防黑客托管** - 静态网站具有最小的攻击面
- **全球CDN** - 从全球300+位置提供内容
- **自动HTTPS**，具有SSL证书
- **始终在线**，具有100%正常运行时间SLA

### 完美用例
- 客户的商业网站
- 作品集网站
- 文档网站
- 营销着陆页
- Astro + Tailwind CSS栈（推荐基线）
- 具有CMS集成的静态博客

## CMS集成

### 通过GitHub的Keystatic CMS
Cloudflare Pages通过GitHub与Keystatic CMS无缝集成：

1. **内容管理**：非技术用户可以编辑内容
2. **基于Git的工作流程**：所有更改在版本控制中跟踪
3. **自动部署**：推送到GitHub = 即时部署
4. **免费托管**：无CMS托管费用
5. **协作编辑**：多个团队成员可以管理内容

## 入门

### 先决条件
- GitHub账户（或GitLab/Bitbucket）
- Cloudflare账户（免费）
- 您的静态网站或框架项目

### 设置过程

1. **准备您的项目**
   ```bash
   # Astro示例
   npm create astro@latest my-site
   cd my-site
   npm install
   npm run build
   ```

2. **连接到Git**
   ```bash
   git init
   git add .
   git commit -m "Initial commit"
   git remote add origin <your-repo-url>
   git push -u origin main
   ```

3. **部署到Cloudflare Pages**
   - 登录[Cloudflare Dashboard](https://dash.cloudflare.com)
   - 导航到侧边栏中的**Pages**
   - 单击**创建项目**
   - 单击**连接到Git**
   - 授权Cloudflare访问您的仓库
   - 选择您的仓库

4. **配置构建设置**

   对于**Astro**项目：
   ```
   框架预设：Astro
   构建命令：npm run build
   构建输出目录：dist
   ```

   对于**Next.js**（静态导出）：
   ```
   框架预设：Next.js（静态HTML导出）
   构建命令：npm run build
   构建输出目录：out
   ```

   对于**React/Vite**：
   ```
   框架预设：Create React App / Vite
   构建命令：npm run build
   构建输出目录：dist
   ```

5. **部署**
   - 单击**保存并部署**