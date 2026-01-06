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
   - 实时观看构建过程
   - 您的网站将在约1-2分钟内上线

## 自定义域名

### 添加自定义域名

1. 转到您的Pages项目
2. 单击**自定义域名**标签
3. 单击**设置自定义域名**
4. 输入您的域名（例如，`example.com`）
5. 按照DNS配置说明操作

### DNS配置
如果您的域名在Cloudflare上：
- 自动CNAME设置
- 即时SSL证书

如果您的域名在其他地方：
- 添加指向您的Pages URL的CNAME记录
- SSL证书在几分钟内颁发

## 环境变量

### 添加机密/配置

1. 转到项目**设置**
2. 导航到**环境变量**
3. 为不同环境添加变量：
   - 生产
   - 预览（用于PR部署）

示例变量：
```
API_KEY=your-api-key
SITE_URL=https://example.com
```

在代码中访问：
```javascript
// Astro
const apiKey = import.meta.env.API_KEY;

// Next.js
const apiKey = process.env.NEXT_PUBLIC_API_KEY;
```

## 预览部署

每个拉取请求都有自己的预览部署：
- 用于测试的唯一URL
- 与生产相同的环境
- 非常适合客户预览
- 在PR上自动评论预览链接

## 确保它工作

### 构建验证清单

1. **本地构建测试**：
   ```bash
   npm run build
   npm run preview  # 测试构建的输出
   ```

2. **检查部署日志**：
   - 在Cloudflare仪表板中查看构建日志
   - 查找任何警告或错误
   - 验证构建成功完成

3. **测试实时网站**：
   - 打开提供的`.pages.dev` URL
   - 检查所有页面正确加载
   - 验证图像和资产加载
   - 测试导航和链接

4. **性能检查**：
   - 使用[PageSpeed Insights](https://pagespeed.web.dev/)
   - 应该在所有指标上得分90+
   - 检查移动和桌面版本

### 常见问题和解决方案

**构建失败：**
- 检查构建设置中的Node.js版本
- 验证`package.json`中的所有依赖项
- 确保构建命令正确

**404错误：**
- 检查输出目录是否匹配构建输出
- 验证SPA的路由配置

**资产未加载：**
- 使用相对路径或正确的基本URL
- 检查构建输出中的资产路径

## 高效使用提示

### 1. 优化构建时间
```javascript
// 使用依赖项缓存
// Cloudflare自动缓存node_modules
```

### 2. 利用Astro获得性能
```bash
npm create astro@latest
# 选择"Empty"模板
# 只添加您需要的内容
```

### 3. 使用Tailwind进行样式设计
```bash
npm install -D tailwindcss
npx tailwindcss init
```

### 4. 实施Keystatic CMS
```bash
npm install @keystatic/core @keystatic/astro
```

配置示例：
```javascript
// keystatic.config.js
import { config, collection, fields } from '@keystatic/core';

export default config({
  storage: {
    kind: 'github',
    repo: 'your-username/your-repo',
  },
  collections: {
    posts: collection({
      label: 'Posts',
      path: 'src/content/posts/*',
      schema: {
        title: fields.text({ label: 'Title' }),
        content: fields.document({ label: 'Content' }),
      },
    }),
  },
});
```

### 5. 监控和分析
- 在Cloudflare中启用**Web Analytics**（免费、隐私优先）
- 在没有cookie的情况下跟踪页面浏览
- 对性能无影响

## 定价

### 免费层级（非常慷慨）
- ✅ 无限网站
- ✅ 无限请求
- ✅ 无限带宽
- ✅ 每月500次构建
- ✅ 1个并发构建
- ✅ 每次部署20,000个文件

### 付费计划（$20/月 - 如需要）
- 每月5,000次构建
- 5个并发构建
- 高级构建配置
- 更大的文件限制

**现实**：大多数项目永远不需要静态网站的付费计划。

## 与其他Cloudflare服务的集成

- **Pages Functions**：添加无服务器函数（类似于Workers）
- **D1数据库**：连接数据库以获得动态功能
- **R2存储**：存储和提供媒体文件
- **KV**：存储配置和缓存数据

## 官方资源

- **文档**：https://developers.cloudflare.com/pages/
- **框架指南**：https://developers.cloudflare.com/pages/framework-guides/
- **Discord社区**：https://discord.cloudflare.com
- **状态页面**：https://www.cloudflarestatus.com/

## 真实世界示例

部署带有Keystatic CMS的Astro + Tailwind商业网站：

```bash
# 1. 创建项目
npm create astro@latest client-business-site
cd client-business-site

# 2. 添加Tailwind
npx astro add tailwind

# 3. 添加Keystatic
npm install @keystatic/core @keystatic/astro
npx @keystatic/cli init

# 4. 构建和测试
npm run build
npm run preview

# 5. 推送到GitHub
git init
git add .
git commit -m "Initial business site"
git push

# 6. 通过仪表板部署到Cloudflare Pages
# 总时间：~10分钟
# 总成本：$0
# 结果：专业、快速、安全的网站
```

## 为什么它非常适合客户工作

1. **零托管成本** - 为您提供更好的利润率
2. **设置后忘记** - 无服务器维护
3. **不可能被黑客攻击** - 只有静态文件
4. **全球快速** - Cloudflare的CDN
5. **客户可以编辑内容** - 通过Keystatic
6. **专业结果** - 企业级基础设施
7. **可扩展** - 自动处理流量激增

这使得Cloudflare Pages非常适合为客户构建网站，您可以在没有持续托管成本或安全担忧的情况下提供专业结果。