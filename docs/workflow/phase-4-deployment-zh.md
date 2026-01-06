# 阶段5：部署 🚢

**工具**：CI/CD平台 + 托管提供商

**先决条件**：确保[阶段3：测试和调试](./phase-3-testing-debugging-zh.md)已完成，并且在部署之前满足所有[质量标准](../quality-standards/README-zh.md)。

## 推荐平台

### Cloudflare Pages/Workers ⭐ **最适合专业项目**

**Cloudflare Pages**（静态站点）
- **完全免费托管** - 无带宽限制，无访客上限
- **内置CI/CD** - 从Git存储库自动部署
- **全球CDN** - 即时缓存和全球快速加载
- **自定义域名**免费，带有SSL证书
- 用于动态内容的边缘函数，无需单独的后端

**Cloudflare Workers**（无服务器后端）
- **每天100,000次请求免费** - 足够大多数商业网站
- **之后按使用付费** - 每百万额外请求$0.15
- **全球边缘网络** - 全球低延迟
- 用于实时应用程序的持久对象
- 用于键值数据的KV存储

**为什么它非常适合客户项目：**
- **可预测的成本** - 您确切知道您将支付什么
- **企业级基础设施** - 被主要公司使用
- **无意外账单** - 使用是可预测的，计费是透明的
- **专业可靠性** - 99.9%+正常运行时间保证
- **可扩展性** - 处理流量激增而无需额外成本

### GitHub Pages 🆓 **最适合个人/作品集项目**

**完全免费用于：**
- 静态网站（HTML、CSS、JavaScript）
- 个人作品集
- 开源项目文档
- 实验性副项目

**限制：**
- **仅静态** - 无服务器端处理
- **GitHub品牌**在免费计划上
- **构建时间限制**（10分钟）
- **不适合客户工作** - 缺乏专业SLA和支持
- **自定义域名设置**需要手动DNS配置

**非常适合：**
- 构建第一个项目的学习开发人员
- 个人作品集和博客
- 开源文档
- 成本是主要关注的爱好项目

### Vercel/Netlify 🚀 **最适合（性能方面）交互式Next.js应用程序**

**Vercel**
- **针对Next.js优化** - 最佳性能和功能
- 支持服务器端渲染和API路由
- 用于计算密集型操作的边缘函数
- **免费层级限制：**
  - 每月100GB带宽
  - 有限的无服务器函数执行时间
- 基于使用的定价可能很快累积

**Netlify**
- **非常适合Jamstack站点**
- 表单处理和无服务器函数
- 构建插件生态系统
- 与Vercel类似的免费层级限制

**⚠️ 客户项目的成本警告：**
- **基于使用的计费**可能会随着流量变得昂贵
- **无服务器函数**按调用和执行时间收费
- **带宽超额**可能导致意外账单
- **不可预测** - 成本随用户参与度扩展

**何时使用：**
- 具有**定义预算**和**流量期望**的**客户项目**
- 需要服务器端功能的**Next.js应用程序**
- **性能证明成本合理**的项目
- 具有明确货币化策略的**应用程序**


## CI/CD实施

### 自动化部署管道

**标准工作流程：**
```bash
# 开发
git add .
git commit -m "feat: Add user authentication"
git push origin main

# 自动部署
# Cloudflare/GitHub Pages/GitLab Pages获取更改
# 自动构建和部署
```

### 平台特定部署

#### Cloudflare Pages（推荐）
**自动部署设置：**
1. 将GitHub存储库连接到Cloudflare Pages
2. 配置构建设置：
   - **构建命令**：`npm run build`
   - **构建输出目录**：`dist`
   - **Node版本**：18或20
3. 如需要设置环境变量
4. 每次推送到main分支时部署

**配置：**
```toml
# wrangler.toml（用于Workers/Functions）
name = "my-project"
compatibility_date = "2023-10-30"
pages_build_output_dir = "./dist"

[env.production]
vars = { ENVIRONMENT = "production" }
```

#### GitHub Pages
**静态站点设置：**
1. 在存储库设置中启用Pages
2. 选择源分支（通常是`main`）
3. 如果使用Jekyll/Hugo，配置构建
4. 自定义域名设置（可选）

**工作流程：**
```yaml
# .github/workflows/deploy.yml
name: Deploy to GitHub Pages
on:
  push:
    branches: [ main ]
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    - name: Setup Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '20'
    - name: Install dependencies
      run: npm install
    - name: Build
      run: npm run build
    - name: Deploy
      uses: peaceiris/actions-gh-pages@v3
      with:
        github_token: ${{ secrets.GITHUB_TOKEN }}
        publish_dir: ./dist
```

## 生产部署清单

**部署前：**
- [ ] [测试完成](./phase-3-testing-debugging-zh.md) - 所有测试通过
- [ ] [代码审查](./phase-3-testing-debugging-zh.md#code-review-process) - 满足质量标准
- [ ] [环境变量配置](../core-technologies-zh.md#environment-variables)
- [ ] [本地测试构建过程](../core-technologies-zh.md#build-for-production)
- [ ] [域名注册和配置](#domain-setup)

**部署配置：**
- [ ] [托管平台配置](./phase-4-deployment-zh.md#recommended-platforms)
- [ ] [生产环境变量设置](#environment-variables)
- [ ] [SSL证书配置](#ssl-setup)
- [ ] [自定义域名正确指向](#domain-setup)
- [ ] [分析和监控设置](#monitoring-setup)

**部署后验证：**
- [ ] [网站正确加载](#post-deployment-testing)
- [ ] [所有功能在生产环境中工作](#functionality-testing)
- [ ] [性能指标可接受](#performance-testing)
- [ ] [SEO元素正确配置](#seo-verification)
- [ ] [错误监控激活](#error-tracking)

## 环境变量和配置

### 生产环境设置

**常见环境变量：**
```bash
# API配置
API_BASE_URL=https://api.yourdomain.com
PUBLIC_API_KEY=your_public_key

# 数据库
DATABASE_URL=your_production_db_url
DB_ENCRYPTION_KEY=your_encryption_key

# 外部服务
STRIPE_SECRET_KEY=sk_live_...
SENDGRID_API_KEY=SG....
CLOUDFLARE_ZONE_ID=your_zone_id

# 分析
GOOGLE_ANALYTICS_ID=G-XXXXXXXXXX
MIXPANEL_TOKEN=your_mixpanel_token
```

**按平台设置变量：**

**Cloudflare Pages：**
1. 转到Pages仪表板
2. 选择您的项目
3. 导航到设置 > 环境变量
4. 添加每个变量及其值
5. 重新部署以应用更改

**Vercel：**
1. 转到项目仪表板
2. 导航到设置 > 环境变量
3. 为生产、预览和开发环境添加变量
4. 部署自动更新

**GitHub Actions：**
```yaml
env:
  PRODUCTION_URL: ${{ secrets.PRODUCTION_URL }}
  DATABASE_URL: ${{ secrets.DATABASE_URL }}
```

## 域名设置和SSL

### 自定义域名配置

**DNS设置：**
```dns
# 根域名的A记录
A @ 192.0.2.1
A www 192.0.2.1

# 子域名的CNAME
CNAME app your-app.pages.dev

# 对于Cloudflare（推荐）
CNAME @ your-site.pages.dev
CNAME www your-site.pages.dev
```

**Cloudflare特定：**
1. 将域名添加到Cloudflare DNS
2. 在域名注册商处更新名称服务器
3. 启用"始终使用HTTPS"
4. 配置缓存规则

**SSL证书管理：**
- **自动**：大多数平台（Cloudflare、Vercel）提供免费SSL
- **Let's Encrypt**：自定义设置的免费SSL证书
- **手动**：为企业需求购买SSL证书

## 监控和错误跟踪

### 生产监控设置

**基本监控工具：**

**1. 正常运行时间监控**
- [Cloudflare Analytics](https://dash.cloudflare.com) - 内置分析
- [Pingdom](https://www.pingdom.com) - 外部正常运行时间监控
- [UptimeRobot](https://uptimerobot.com) - 提供免费层级

**2. 性能监控**
- [Google Analytics](https://analytics.google.com) - 用户行为跟踪
- [Google PageSpeed Insights](https://pagespeed.web.dev) - 性能指标
- [GTmetrix](https://gtmetrix.com) - 详细性能分析

**3. 错误跟踪**
- [Sentry](https://sentry.io) - 错误监控和性能跟踪
- [LogRocket](https://logrocket.com) - 会话回放和错误跟踪
- [Bugsnag](https://www.bugsnag.com) - 错误监控和警报

### 健康检查端点

**API健康检查：**
```typescript
// /api/health.ts
export async function GET() {
  try {
    // 检查数据库连接
    await db.ping();
    
    // 检查外部服务
    await fetch(process.env.API_HEALTH_CHECK);
    
    return new Response(
      JSON.stringify({ status: 'healthy', timestamp: new Date().toISOString() }),
      { headers: { 'Content-Type': 'application/json' } }
    );
  } catch (error) {
    return new Response(
      JSON.stringify({ status: 'unhealthy', error: error.message }),
      { status: 500, headers: { 'Content-Type': 'application/json' } }
    );
  }
}
```

## 备份和回滚策略

### 数据库备份

**自动备份：**
```sql
-- 每日备份脚本
pg_dump production_db > backup_$(date +%Y%m%d).sql

-- 上传到云存储
aws s3 cp backup_$(date +%Y%m%d).sql s3://backups-bucket/
```

**手动备份过程：**
1. 创建数据库转储
2. 上传到云存储（S3、R2等）
3. 测试恢复过程
4. 记录备份保留策略

### 回滚程序

**应用程序回滚：**
```bash
# 基于Git的回滚
git revert HEAD~2..HEAD
git push origin main

# 或回滚到特定部署
cf pages deployment list --project-name=my-project
cf pages deployment rollback --project-name=my-project --deployment=<id>
```

**数据库回滚：**
```bash
# 从备份恢复
psql production_db < backup_20231201.sql

# 将应用程序指向恢复的数据库
# 如需要，更新环境变量
```

## 客户项目考虑因素

### 移交文档

**创建全面的移交包：**
1. **技术文档** - 架构和技术栈
2. **管理员访问** - 托管、域名、分析
3. **内容管理** - 如何更新内容
4. **维护计划** - 定期更新和监控
5. **联系信息** - 用于持续支持

**客户培训材料：**
- 如何进行基本内容更新
- 如何访问分析仪表板
- 如何请求功能添加
- 维护时间表和成本

### 维护和更新

**定期维护任务：**
- 安全更新（每月）
- 内容更新（根据需要）
- 性能监控（每周）
- 备份验证（每周）
- SSL证书续订（每年）

**更新过程：**
```bash
# 开发
git checkout -b update/feature-addition
# 进行更改
git add .
git commit -m "feat: Add client request feature"
git push origin update/feature-addition

# 审查和部署
# 在暂存环境中测试
# 部署到生产环境
# 监控问题
```

## 部署后性能优化

### 核心Web指标监控

**目标指标：**
- **LCP（最大内容绘制）**：< 2.5秒
- **FID（首次输入延迟）**：< 100毫秒
- **CLS（累积布局偏移）**：< 0.1

**优化清单：**
- [ ] 图像优化和压缩
- [ ] CSS和JavaScript最小化
- [ ] 缓存头配置
- [ ] CDN正确配置
- [ ] 数据库查询优化

### CDN配置

**Cloudflare CDN：**
```javascript
// 缓存静态资产1年
addEventListener('fetch', event => {
  event.respondWith(handleRequest(event.request))
})

async function handleRequest(request) {
  const url = new URL(request.url)
  
  // 缓存静态资产
  if (url.pathname.match(/\.(js|css|png|jpg|gif|ico|svg)$/)) {
    const cacheKey = new Request(url.toString(), request)
    const cachedResponse = await caches.default.match(cacheKey)
    
    if (cachedResponse) {
      return cachedResponse
    }
    
    const response = await fetch(request)
    const cacheResponse = new Response(response.body, response)
    cacheResponse.headers.set('Cache-Control', 'public, max-age=31536000')
    
    return cacheResponse
  }
  
  return fetch(request)
}
```

## 部署故障排除

**部署过程中的常见问题及其解决方案**

### 构建失败

**症状：** 构建过程失败，出现开发中不存在的错误。

**常见原因：**
- 环境变量缺失
- 依赖版本不匹配
- 构建优化问题
- 内存/资源限制

**快速修复：**
```
"在生产中构建失败但在开发中工作：

构建错误：
[构建日志中的错误消息]

环境：
- 开发：Node 18.16.0，工作正常
- 生产：[平台，Node版本]

最近更改：
[失败前更改的内容]

检查：
1. 环境变量（是否都在生产中设置？）
2. 依赖版本（是否提交了package-lock.json？）
3. 构建配置（vite.config.ts、next.config.js等）
4. 内存限制（如需要则增加）
5. 缺失文件（.gitignore隐藏了必需文件？）"
```

**预防：**
- 本地测试构建：`npm run build`
- 提交package-lock.json/yarn.lock
- 记录所有必需的环境变量
- 首先在暂存环境中测试

---

### 环境特定问题

**症状：** 代码在本地工作但在生产中失败。

**快速诊断：**
```bash
# 检查环境变量
"列出代码中使用的所有环境变量"

# 检查配置差异
"比较开发与生产配置"

# 检查依赖项
"检查所有依赖项是否在生产中可用"
```

**快速修复：**
```
"在开发中工作，在生产中失败：

生产中的错误：
[错误消息]

环境差异：
开发：
- 数据库：localhost:5432
- API URL：http://localhost:3000
- Node ENV：development

生产：
- 数据库：[生产数据库URL]
- API URL：[生产URL]
- Node ENV：production

可疑问题：
[可能不同的地方]

调试并修复环境特定问题。"
```

**常见环境问题：**
- 数据库连接字符串
- API端点（localhost与生产）
- CORS配置
- 文件路径（绝对与相对）
- SSL/HTTPS要求

---

### DNS/域名问题

**症状：** 域名未指向部署，或SSL证书问题。

**快速诊断：**
```bash
# 检查DNS传播
dig yourdomain.com
nslookup yourdomain.com

# 检查DNS记录
# 应显示：
# A记录 → 您的服务器IP
# 或CNAME → 您的托管平台
```

**快速修复（Cloudflare Pages）：**
```
1. 在Cloudflare Pages仪表板中添加自定义域名
2. 添加DNS记录：
   - yourdomain.com → CNAME → your-project.pages.dev
   - www.yourdomain.com → CNAME → your-project.pages.dev
3. 启用"自动HTTPS重写"
4. 等待SSL证书（5-30分钟）
5. 测试：https://yourdomain.com
```

**预防：**
- 在宣布站点之前设置DNS
- 等待24-48小时完全DNS传播
- 测试www和非www版本
- 从开始启用SSL/HTTPS

---

### 部署管道故障

**症状：** CI/CD管道在部署期间失败。

**快速诊断：**
```bash
# 检查管道日志
# 查找：
- 失败的测试
- 构建错误
- 部署错误
- 权限问题
```

**快速修复：**
```
"CI/CD管道失败：

管道日志：
[CI/CD中的相关错误]

失败的阶段：
- [ ] 测试
- [ ] 构建
- [ ] 部署
- [ ] 部署后检查

可疑原因：
[假设]

修复管道配置。"
```

**常见管道问题：**
- 测试在CI中失败但在本地通过
- CI中缺少环境变量
- 不同的Node/依赖版本
- 权限不足
- 超出超时限制

---

### 部署后性能下降

**症状：** 尽管在本地很快，但在生产中站点很慢。

**快速诊断：**
```
使用Chrome DevTools（在生产站点上）：
1. 打开网络选项卡
2. 硬刷新（Cmd+Shift+R / Ctrl+Shift+R）
3. 检查：
   - 总加载时间
   - 最大请求
   - 失败的请求
   - 慢API调用

使用Lighthouse：
1. 运行Lighthouse审计
2. 检查核心Web指标：
   - LCP（应<2.5秒）
   - FID（应<100毫秒）
   - CLS（应<0.1）
```

**快速修复：**
```
生产中的常见性能问题：

1. 图像未优化
   - 解决方案：使用图像优化服务
   - 解决方案：使用现代格式（WebP、AVIF）
   - 解决方案：实现延迟加载

2. 无缓存
   - 解决方案：配置缓存头
   - 解决方案：启用CDN缓存
   - 解决方案：使用服务工作者

3. 慢API调用
   - 解决方案：添加数据库索引
   - 解决方案：实现缓存（Redis）
   - 解决方案：对静态数据使用CDN

4. 大包大小
   - 解决方案：代码分割
   - 解决方案：树摇
   - 解决方案：移除未使用的依赖项

5. 无压缩
   - 解决方案：启用gzip/brotli压缩
   - 解决方案：在托管平台中配置
```

---

### 数据库连接问题

**症状：** 无法从生产环境连接到数据库。

**快速修复：**
```
"生产中数据库连接失败：

错误：
[连接错误消息]

配置：
- 数据库主机：[主机]
- 端口：[端口]
- 需要SSL：[是/否]
- 防火墙规则：[已配置？]

连接字符串：
[连接字符串，密码已编辑]

检查：
1. 数据库服务器运行且可访问
2. 防火墙允许来自生产服务器的连接
3. 环境变量中的数据库凭据正确
4. 如需要，配置SSL/TLS
5. 连接池设置适当
6. 数据库未达到连接限制"
```

**预防：**
- 在部署前从生产环境测试数据库连接
- 使用连接池
- 设置合理的连接限制
- 监控连接数
- 启用数据库日志进行调试

---

### 回滚程序

**何时回滚：**
- 生产中发现关键错误
- 性能严重下降
- 数据损坏风险
- 安全漏洞

**快速回滚（Cloudflare Pages）：**
```bash
# 通过仪表板：
1. 转到Cloudflare Pages仪表板
2. 选择您的项目
3. 查看部署
4. 在上一个稳定部署上单击"回滚"

# 通过CLI：
wrangler pages deployment list
wrangler pages deployment rollback [DEPLOYMENT_ID]
```

**快速回滚（基于Git）：**
```bash
# 查找最后一个好的提交
git log --oneline

# 回滚到最后一个好的提交
git revert [bad-commit-hash]
git push origin main

# 或硬重置（如果安全）
git reset --hard [last-good-commit]
git push --force origin main
```

**回滚后：**
1. 如需要，通知用户
2. 记录出了什么问题
3. 在开发中修复问题
4. 彻底测试
5. 部署修复

---

### 部署清单

部署到生产环境之前：

**部署前：**
- [ ] 所有测试在本地通过
- [ ] 本地构建成功（`npm run build`）
- [ ] 在暂存环境中测试
- [ ] 环境变量已记录
- [ ] 数据库迁移准备就绪（如果有）
- [ ] 回滚计划准备就绪

**配置：**
- [ ] 生产环境变量已设置
- [ ] 数据库连接已配置
- [ ] API端点指向生产环境
- [ ] CORS正确配置
- [ ] SSL/HTTPS已启用
- [ ] 域名DNS已配置

**部署后：**
- [ ] 部署成功（检查日志）
- [ ] 站点可在域名访问
- [ ] 核心功能工作
- [ ] API调用工作
- [ ] 数据库操作工作
- [ ] 无控制台错误
- [ ] 性能可接受
- [ ] SSL证书有效

**监控：**
- [ ] 错误跟踪已启用（Sentry等）
- [ ] 性能监控激活
- [ ] 正常运行时间监控已配置
- [ ] 备份系统运行
- [ ] 日志可访问

---

## 常见问题和故障排除

### 部署因构建错误而失败
**问题：** 构建在本地成功但在生产环境中失败
- **解决方案：** 检查本地和生产之间的Node版本兼容性
- **常见原因：** 生产设置中缺少环境变量
- **参见：** [环境变量和配置](#environment-variables--configuration)部分

### 部署后站点显示404或空白页
**问题：** 部署成功但站点未正确加载
- **解决方案：** 验证构建输出目录与平台配置匹配
- **检查：** 资产路径是相对的，不是绝对的
- **另请参见：** [生产部署清单](#production-deployment-checklist)

### 环境变量不工作
**问题：** 应用程序无法在生产环境中访问环境变量
- **解决方案：** 确保变量在托管平台仪表板中设置
- **提示：** 使用平台特定前缀（例如，Astro使用`PUBLIC_`）
- **参见：** [生产环境设置](#production-environment-setup)

### SSL/HTTPS不工作
**问题：** 站点无法通过HTTPS加载或显示证书错误
- **解决方案：** 检查DNS正确配置且SSL证书已颁发
- **等待时间：** SSL证书可能需要10-60分钟来配置
- **参见：** [域名设置](#domain-setup)部分

### 生产中的性能问题
**问题：** 站点在生产中比在开发中慢
- **解决方案：** 检查[部署后性能优化](#performance-optimization-post-deployment)
- **验证：** CDN正确配置且资产已缓存
- **另请参见：** [质量标准：性能](../quality-standards/performance-zh.md)

### 数据库连接失败
**问题：** 应用程序无法连接到生产数据库
- **解决方案：** 验证DATABASE_URL在环境变量中正确设置
- **检查：** 数据库防火墙规则允许来自托管平台的连接
- **测试：** 连接字符串用于生产数据库，不是开发数据库

**如需额外帮助：** 请查阅[故障排除指南](../troubleshooting/README-zh.md)获取全面的部署故障排除和[托管工具指南](../hosting-tools/README-zh.md)获取平台特定问题。

---

## 先决条件和下一步

**需要完成：**
- [阶段3：测试和调试](./phase-3-testing-debugging-zh.md) — 全面QA完成
- [核心技术设置](../core-technologies-zh.md) — 生产就绪代码库
- [托管工具配置](../hosting-tools/README-zh.md) — 基础设施决策已做出
- [业务需求最终确定](../introduction/README-zh.md) — 客户交付物已定义

**已完成工作流程：**
- ✅ [阶段0：准备](./phase-0-vibecoder-preparation-zh.md) — 工具和心态就绪
- ✅ [阶段1：规划](./phase-1-planning-zh.md) — 规范完成
- ✅ [阶段2：开发](./phase-2-development-zh.md) — 功能已实现
- ✅ [阶段3：测试](./phase-3-testing-debugging-zh.md) — 质量保证通过
- ✅ [阶段4：部署](./phase-4-deployment-zh.md) — 生产就绪

**业务实施：**
- [客户移交文档](#client-project-considerations) — 项目交付
- [维护计划建立](#maintenance--updates) — 持续支持
- [性能监控](#performance-optimization-post-deployment) — 成功指标

**相关阅读：**
- [托管工具](../hosting-tools/README-zh.md) — 完整托管平台指南
- [业务策略](../introduction/README-zh.md) — 自由职业和客户管理
- [上下文管理](../context-management/README-zh.md) — 项目移交工作流程

返回：[阶段3 → 测试、调试和代码审查](./phase-3-testing-debugging-zh.md)

下一步：[介绍 → 业务模型](../introduction/README-zh.md)