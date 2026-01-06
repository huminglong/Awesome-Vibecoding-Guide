# 托管工具指南

现代、高性价比托管解决方案的综合指南，专为Web开发人员设计，专注于Cloudflare的边缘平台生态系统。

## 概述

本部分涵盖了强大的托管工具，使您能够以最低成本和最高性能构建和部署生产就绪的应用程序。无论您是构建简单的静态网站、复杂的SaaS应用程序，还是介于两者之间的任何东西，这些工具都提供您所需的基础设施。

## 为什么选择Cloudflare的边缘平台？

Cloudflare的生态系统提供了独特的优势组合：

- **高性价比**：慷慨的免费层级和负担得起的扩展 [→ 商业模式](../business-model/README-zh.md)
- **全球性能**：全球300+边缘位置 [→ 性能标准](../quality-standards/performance-zh.md)
- **集成栈**：所有服务无缝协作
- **开发体验**：简单的部署、出色的工具 [→ 部署指南](../workflow/phase-4-deployment-zh.md)
- **生产就绪**：企业级可靠性和安全性
- **无基础设施管理**：完全无服务器

## 完整栈

### 静态网站托管
**[Cloudflare Pages](./cloudflare-pages-zh.md)** - 部署静态网站和JAMstack应用程序

- 静态网站的零成本托管
- 免费层级无带宽限制
- 完美适用于Astro、Next.js、React、Vue
- 通过GitHub与Keystatic CMS集成
- 理想用于商业网站和作品集

**最适合**：营销网站、文档、博客、作品集

---

### 无服务器计算
**[Cloudflare Workers](./cloudflare-workers-zh.md)** - 在全球边缘运行代码

- 托管具有后端逻辑的完整Web应用程序
- 每天100,000次请求免费
- 连接数据库和外部API
- 完美适用于SaaS应用程序
- 全球超低延迟

**最适合**：API、SaaS后端、全栈应用程序

---

### 文件存储
**[R2存储](./cloudflare-r2-zh.md)** - S3兼容的对象存储，零出口费用

- 存储图像、视频、文档、备份
- 10 GB免费存储
- 无带宽/出口费用（与AWS S3不同）
- S3兼容API
- 快速全球交付

**最适合**：用户上传、媒体库、静态资产、备份

---

### 关系数据库
**[D1数据库](./cloudflare-d1-zh.md)** - 边缘的无服务器SQLite数据库

- 具有SQLite的完整SQL支持
- 5 GB存储免费
- 与Workers/Pages原生集成
- 超便宜且可扩展
- 完美适用于微SaaS应用程序

**最适合**：应用程序数据、用户管理、分析

---

### 键值存储
**[KV（键值存储）](./cloudflare-kv-zh.md)** - 全球低延迟键值存储

- 亚毫秒级读取性能
- 每天100,000次读取免费
- 完美适用于缓存和会话
- 包括全球复制
- 简单的键值接口

**最适合**：会话、缓存、功能标志、速率限制

---

## 推荐组合

### 带CMS的静态网站
```
Pages + Keystatic CMS + GitHub
```
- **成本**：$0/月
- **用例**：商业网站、博客、文档
- **设置时间**：15分钟

### 微SaaS应用程序
```
Workers + D1 + KV + R2
```
- **成本**：$0-10/月（对于大多数应用程序）
- **用例**：URL缩短器、API服务、Web应用程序
- **设置时间**：1小时

### 全栈应用程序
```
Pages（前端）+ Workers（API）+ D1（数据库）+ R2（存储）+ KV（缓存）
```
- **成本**：$5-20/月（对于增长的应用程序）
- **用例**：电子商务、SaaS平台、社交应用程序
- **设置时间**：几小时到几天

### 高流量内容平台
```
Pages + R2 + KV（缓存）+ D1（元数据）
```
- **成本**：$10-30/月（即使有高流量）
- **用例**：媒体平台、内容交付
- **设置时间**：几小时

---

## 成本比较

### 传统托管 vs. Cloudflare堆栈
[→ 另见：商业模式和定价策略](../business-model/README-zh.md)

**示例**：具有10,000个用户的Web应用程序

| 服务     | 传统         | Cloudflare | 节省     |
| -------- | ------------ | ---------- | -------- |
| 托管     | $50-100      | $5         | 90%+     |
| 数据库   | $50-200      | $5-10      | 90%+     |
| 存储     | $20-50       | $1-2       | 95%+     |
| CDN      | $50-100      | $0         | 100%     |
| **总计** | **$170-450** | **$11-17** | **~95%** |

### 真实世界节省

**静态网站**：
- 传统：$10-50/月（Netlify、Vercel付费计划）
- Cloudflare Pages：$0/月
- **节省**：100%

**SaaS应用程序**：
- 传统：$200-500/月（AWS/GCP with RDS、S3、CloudFront）
- Cloudflare堆栈：$10-30/月
- **节省**：90-95%

**内容交付**：
- AWS S3 + CloudFront：$100+（具有出口费用）
- R2 + Pages：$1-5
- **节省**：95%+

---

## 入门

### 先决条件

1. **Cloudflare账户**（免费）
   - 在[cloudflare.com](https://cloudflare.com)注册

2. **Wrangler CLI**（Cloudflare的开发者工具）
   ```bash
   npm install -g wrangler
   wrangler login
   ```

3. **Git和GitHub**（用于Pages部署）
   - GitHub账户用于自动部署

### 快速入门：部署您的第一个项目

#### 静态网站（Pages）
```bash
# 创建Astro网站
npm create astro@latest my-site
cd my-site

# 推送到GitHub
git init && git add . && git commit -m "init"
git push

# 通过Cloudflare仪表板部署
# Pages > Create Project > Connect Git
```

#### Worker API
```bash
# 创建Worker
npm create cloudflare@latest my-api
cd my-api

# 部署
npm run deploy
```

#### 全栈应用程序
```bash
# 创建带有Functions的Pages项目
npm create cloudflare@latest my-app -- --framework=astro
cd my-app

# 添加D1数据库
wrangler d1 create my-db

# 添加到wrangler.toml，然后部署
git push
```

---

## 学习路径

### 初学者
1. 从**Pages**开始 - 部署静态网站
2. 添加**KV** - 实施简单缓存
3. 学习**D1** - 添加数据库

### 中级
4. 使用**Workers**构建 - 创建API
5. 使用**R2** - 处理文件上传
6. 组合服务 - 构建小型应用程序

### 高级
7. 通过缓存策略优化性能
8. 实施身份验证和授权
9. 通过监控扩展到生产
10. 构建完整的SaaS应用程序

---

## 何时使用每个服务

### 使用Pages当：
- 构建静态网站或JAMstack应用程序
- 需要具有无限带宽的免费托管
- 想要从Git自动部署
- 部署React、Vue、Astro、Next.js（静态）

### 使用Workers当：
- 构建API或后端服务
- 需要边缘的无服务器函数
- 想要全球超低延迟
- 构建全栈应用程序

### 使用R2当：
- 存储用户上传（图像、视频、文档）
- 需要S3兼容的存储
- 想要避免出口费用
- 构建媒体密集型应用程序

### 使用D1当：
- 需要关系数据库（SQL）
- 构建数据驱动的应用程序
- 想要便宜、可扩展的数据库
- 需要ACID事务

### 使用KV当：
- 实施缓存层
- 存储会话或身份验证令牌
- 需要功能标志或配置
- 构建速率限制器
- 想要亚毫秒级读取性能

---

## 最佳实践

### 架构
- 使用**Pages**作为前端，**Workers**作为API层
- 在**KV**中存储热数据，在**D1**中存储冷数据
- 使用**R2**满足所有文件存储需求
- 在每一层实施缓存

### 性能
- 使用**KV**积极缓存
- 使用**Workers**进行边缘计算
- 在上传到**R2**之前优化图像
- 在**D1**中正确索引数据库

### 成本优化
- 尽可能保持在免费层级内
- 使用**KV**减少**D1**查询
- 对大数据集实施分页
- 在Cloudflare仪表板中监控使用情况

### 安全
- 在环境变量中存储密钥
- 使用**KV**进行API密钥验证
- 使用**KV**实施速率限制
- 清理所有用户输入
- 适当使用CORS策略

### 开发工作流程
- 使用`wrangler dev --local`进行本地开发
- 使用预览部署进行测试
- 为开发/生产使用单独的数据库
- 对所有内容进行版本控制
- 使用Git自动化部署

---

## 常见模式

### 身份验证流程
```typescript
// 登录：在KV中创建会话
// 验证：从KV检查会话
// 存储用户数据：D1数据库
// 存储个人资料图片：R2存储
```

### 内容管理
```typescript
// 内容编辑：Keystatic CMS
// 存储：GitHub存储库
// 部署：通过Pages自动
// 媒体文件：R2存储
```

### 带缓存的API
```typescript
// 请求 → Worker
// 检查KV缓存 → 如果找到则返回
// 查询D1 → 存储在KV中 → 返回
```

### 文件上传流程
```typescript
// 前端：上传表单
// Worker：验证文件
// R2：存储文件
// D1：存储元数据
// 返回：文件URL
```

---

## 故障排除

### Pages不构建
- 检查设置中的构建命令
- 验证输出目录
- 检查Node.js版本兼容性
- 查看构建日志中的错误

### Worker不响应
- 检查`wrangler.toml`配置
- 验证绑定（D1、KV、R2）正确
- 使用`wrangler tail`查看日志
- 使用`wrangler dev`本地测试

### 数据库连接问题
- 确认`wrangler.toml`中的数据库ID
- 检查绑定名称与代码匹配
- 验证数据库存在：`wrangler d1 list`
- 运行迁移：`wrangler d1 execute`

### 存储/上传失败
- 检查R2存储桶名称和绑定
- 验证CORS配置
- 检查文件大小限制
- 查看上传权限

---

## 资源

### 官方文档
- [Cloudflare Pages文档](https://developers.cloudflare.com/pages/)
- [Workers文档](https://developers.cloudflare.com/workers/)
- [R2文档](https://developers.cloudflare.com/r2/)
- [D1文档](https://developers.cloudflare.com/d1/)
- [KV文档](https://developers.cloudflare.com/kv/)

### 社区
- [Cloudflare Discord](https://discord.cloudflare.com)
- [Workers示例](https://github.com/cloudflare/workers-sdk/tree/main/templates)
- [社区论坛](https://community.cloudflare.com)

### 工具
- [Wrangler CLI](https://developers.cloudflare.com/workers/wrangler/)
- [Cloudflare仪表板](https://dash.cloudflare.com)
- [状态页面](https://www.cloudflarestatus.com/)

---

## 下一步

1. **选择您的路径**：静态网站、API或全栈应用程序
2. **阅读相关指南**：点击上面的任何服务
3. **遵循设置**：每个指南都有分步说明
4. **构建一些东西**：从小开始，快速迭代
5. **扩展**：根据需要添加更多服务

---

## 为什么这个堆栈非常适合Vibecoding

Cloudflare边缘平台与vibecoding理念完美契合：

- **快速开始**：几分钟内部署，而不是几小时
- **保持便宜**：免费层级非常慷慨
- **自然扩展**：无需管理基础设施
- **专注于代码**：而不是DevOps
- **随处构建**：从第一天开始全球化
- **自信发布**：生产就绪的基础设施

无论您是在构建副业项目、客户网站，还是下一个大型SaaS，这些工具都能以传统托管成本的一小部分提供您所需的一切。

---

## 相关文档

**部署和工作流程：**
- [阶段4：部署](../workflow/phase-4-deployment-zh.md) - 分步部署指南
- [工作流程概述](../workflow/README-zh.md) - 完整的开发过程
- [核心技术](../core-technologies-zh.md) - Astro + Tailwind + Cloudflare堆栈

**质量和性能：**
- [性能标准](../quality-standards/performance-zh.md) - Core Web Vitals优化
- [SEO标准](../quality-standards/seo-zh.md) - 搜索引擎优化

**业务和定价：**
- [商业模式](../business-model/README-zh.md) - 定价策略和成本优化
- [客户管理](../business-model/client-management-zh.md) - 项目定价指导

**故障排除：**
- [故障排除指南](../troubleshooting/README-zh.md) - 常见问题和解决方案

---

**准备好构建了吗？选择一个服务并深入探索！**