# Cloudflare Workers

## 概述

Cloudflare Workers是一个无服务器执行环境，在边缘运行JavaScript、TypeScript、Python和WebAssembly。与传统的无服务器平台不同，Workers在Cloudflare的全球网络上的300+位置运行，为全球用户提供超低延迟。

## 主要好处

### 完整的Web应用程序托管
与Pages（仅静态）不同，Workers让您：
- **构建完整的Web应用程序**，具有后端逻辑
- **连接到数据库**（D1、PostgreSQL等）
- **处理API请求**和复杂的业务逻辑
- **处理付款**、发送电子邮件、验证用户
- **端到端托管SaaS应用程序**

### 边缘计算优势
- **全球亚50ms响应时间**
- **无冷启动**（即时执行）
- **在Cloudflare网络上靠近用户运行**
- **自动扩展**到数百万请求
- **内置DDoS保护**

### 慷慨的免费层级
- **每天100,000次请求**（免费）
- **每次请求10ms CPU时间**（免费层级）
- 非常适合微SaaS应用程序
- 许多应用程序永远不会超过免费层级
- 当您超过时，扩展很便宜

### 负担得起的扩展
**付费计划**（$5/月）：
- 包括1000万次请求
- 额外请求：每百万$0.50
- 每次请求50ms CPU时间

**示例成本计算**：
- 500k请求/天 = 约15M/月
- 成本：$5基础 + $2.50超额 = $7.50/月
- 对于生产SaaS来说这非常便宜！

## 完美用例

### 1. 微SaaS应用程序
- URL缩短器
- 图像优化服务
- API网关
- Webhook处理器
- 数据转换服务

### 2. 全栈应用程序
- 电子商务平台
- 预订系统
- CRM应用程序
- 项目管理工具
- 社交平台

### 3. API服务
- RESTful API
- GraphQL服务器
- 实时数据处理
- 第三方集成
- 身份验证服务

### 4. 中间件和代理
- A/B测试
- 功能标志
- 请求路由
- 标头操作
- 安全层

## 入门

### 先决条件
- 安装Node.js 18+
- Cloudflare账户（免费）
- 基本的JavaScript/TypeScript知识

### 安装

```bash
# 安装Wrangler（Cloudflare Workers CLI）
npm install -g wrangler

# 验证安装
wrangler --version
```

### 身份验证

```bash
# 登录Cloudflare
wrangler login

# 这会打开浏览器进行身份验证
# 身份验证后，您准备好部署
```

### 创建您的第一个Worker

```bash
# 创建新的Worker项目
npm create cloudflare@latest my-worker

# 按照提示操作：
# - 选择"Hello World" Worker模板
# - TypeScript或JavaScript
# - Git初始化（是）
```

### 项目结构

```
my-worker/
├── src/
│   └── index.ts          # 您的Worker代码
├── wrangler.toml         # 配置文件
├── package.json
└── tsconfig.json
```

### 基本Worker示例

```typescript
// src/index.ts

export default {
  async fetch(request: Request, env: Env): Promise<Response> {
    const url = new URL(request.url);

    // 处理不同的路由
    if (url.pathname === '/') {
      return new Response('Hello from Cloudflare Workers!', {
        headers: { 'Content-Type': 'text/plain' }
      });
    }

    if (url.pathname === '/api/data') {
      const data = { message: 'API working!', timestamp: Date.now() };
      return Response.json(data);
    }

    return new Response('Not found', { status: 404 });
  }
};
```

### 配置

```toml
# wrangler.toml

name = "my-worker"
main = "src/index.ts"
compatibility_date = "2024-01-01"

# 环境变量
[vars]
ENVIRONMENT = "production"

# 绑定D1数据库
[[d1_databases]]
binding = "DB"
database_name = "my-database"
database_id = "your-database-id"

# 绑定KV命名空间
[[kv_namespaces]]
binding = "KV"
id = "your-kv-id"

# 绑定R2存储桶
[[r2_buckets]]
binding = "STORAGE"
bucket_name = "my-bucket"
```

## 部署您的Worker

### 开发

```bash
# 使用热重载本地运行
npm run dev

# 或直接使用wrangler
wrangler dev

# 在http://localhost:8787访问
```

### 生产部署

```bash
# 部署到Cloudflare
npm run deploy

# 或
wrangler deploy

# 您的Worker现在上线了！
# URL: https://my-worker.your-subdomain.workers.dev
```

### 自定义域名

```bash
# 通过Wrangler添加自定义域名
wrangler domains add api.yourdomain.com

# 或在Cloudflare仪表板中：
# Workers & Pages > my-worker > Triggers > Custom Domains
```

## 连接到D1数据库

### 使用Workers设置D1

```bash
# 创建D1数据库
wrangler d1 create my-database

# 这会输出您的database_id
# 将其添加到wrangler.toml

# 创建架构
wrangler d1 execute my-database --file=schema.sql
```

### 架构示例

```sql
-- schema.sql
CREATE TABLE users (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  email TEXT UNIQUE NOT NULL,
  name TEXT NOT NULL,
  created_at INTEGER DEFAULT (strftime('%s', 'now'))
);

CREATE TABLE subscriptions (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  user_id INTEGER NOT NULL,
  plan TEXT NOT NULL,
  status TEXT NOT NULL,
  FOREIGN KEY (user_id) REFERENCES users(id)
);
```

### 在Worker中使用D1

```typescript
export default {
  async fetch(request: Request, env: Env): Promise<Response> {
    const { pathname } = new URL(request.url);

    if (pathname === '/api/users') {
      // 查询数据库
      const { results } = await env.DB.prepare(
        'SELECT * FROM users LIMIT 10'
      ).all();

      return Response.json(results);
    }

    if (pathname === '/api/user/create' && request.method === 'POST') {
      const { email, name } = await request.json();

      // 插入数据库
      const result = await env.DB.prepare(
        'INSERT INTO users (email, name) VALUES (?, ?)'
      ).bind(email, name).run();

      return Response.json({
        success: true,
        userId: result.meta.last_row_id
      });
    }

    return new Response('Not found', { status: 404 });
  }
};
```

## 构建微SaaS示例

### 带有D1的URL缩短器

```typescript
interface Env {
  DB: D1Database;
  KV: KVNamespace;
}

export default {
  async fetch(request: Request, env: Env): Promise<Response> {
    const url = new URL(request.url);

    // 重定向缩短的URL
    if (url.pathname.startsWith('/s/')) {
      const code = url.pathname.slice(3);

      // 首先检查缓存
      let targetUrl = await env.KV.get(`short:${code}`);

      if (!targetUrl) {
        // 回退到数据库
        const result = await env.DB.prepare(
          'SELECT url FROM links WHERE code = ?'
        ).bind(code).first();

        if (result) {
          targetUrl = result.url as string;
          // 为下次缓存
          await env.KV.put(`short:${code}`, targetUrl, {
            expirationTtl: 3600
          });
        }
      }

      if (targetUrl) {
        // 跟踪点击
        await env.DB.prepare(
          'UPDATE links SET clicks = clicks + 1 WHERE code = ?'
        ).bind(code).run();

        return Response.redirect(targetUrl, 301);
      }

      return new Response('Short link not found', { status: 404 });
    }

    // 创建缩短的URL
    if (url.pathname === '/api/shorten' && request.method === 'POST') {
      const { url: targetUrl } = await request.json();

      // 生成随机代码
      const code = generateCode(6);

      // 存储在数据库中
      await env.DB.prepare(
        'INSERT INTO links (code, url, clicks) VALUES (?, ?, 0)'
      ).bind(code, targetUrl).run();

      return Response.json({
        shortUrl: `${url.origin}/s/${code}`,
        code
      });
    }

    return new Response('Not found', { status: 404 });
  }
};

function generateCode(length: number): string {
  const chars = 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789';
  let result = '';
  for (let i = 0; i < length; i++) {
    result += chars.charAt(Math.floor(Math.random() * chars.length));
  }
  return result;
}
```

## 环境变量和密钥

### 添加密钥

```bash
# 添加密钥（在wrangler.toml中不可见）
wrangler secret put API_KEY
# 提示时输入值

# 添加另一个密钥
wrangler secret put STRIPE_SECRET_KEY

# 列出密钥
wrangler secret list
```

### 在代码中使用密钥

```typescript
export default {
  async fetch(request: Request, env: Env): Promise<Response> {
    // 通过env访问密钥
    const apiKey = env.API_KEY;
    const stripeKey = env.STRIPE_SECRET_KEY;

    // 用于API调用
    const response = await fetch('https://api.example.com/data', {
      headers: {
        'Authorization': `Bearer ${apiKey}`
      }
    });

    return response;
  }
};
```

## 确保它工作

### 测试清单

1. **本地测试**：
   ```bash
   wrangler dev
   # 本地测试所有端点
   curl http://localhost:8787/api/test
   ```

2. **检查日志**：
   ```bash
   # 实时日志
   wrangler tail

   # 或在仪表板中：
   # Workers & Pages > my-worker > Logs
   ```

3. **测试生产**：
   ```bash
   # 首先部署
   wrangler deploy

   # 测试生产URL
   curl https://my-worker.your-subdomain.workers.dev/api/test
   ```

4. **监控性能**：
   - 检查Cloudflare仪表板
   - 查看请求数
   - 检查错误率
   - 监控CPU时间使用

### 常见问题和解决方案

**Worker不更新：**
```bash
# 清除缓存并重新部署
wrangler deploy --force
```

**数据库连接失败：**
- 验证`wrangler.toml`中的`database_id`
- 检查D1绑定名称与代码匹配
- 运行`wrangler d1 list`确认数据库存在

**超过CPU时间：**
- 优化数据库查询
- 使用KV进行缓存
- 减少外部API调用
- 考虑对大数据集进行分页

## 高效使用提示

### 1. 使用KV积极缓存

```typescript
// 缓存昂贵的操作
async function getCachedData(key: string, env: Env) {
  // 首先尝试缓存
  let data = await env.KV.get(key, 'json');

  if (!data) {
    // 获取并缓存
    data = await fetchExpensiveData();
    await env.KV.put(key, JSON.stringify(data), {
      expirationTtl: 3600 // 1小时
    });
  }

  return data;
}
```

### 2. 使用Durable Objects进行状态管理

对于实时功能（聊天、协作）：
```bash
# 启用Durable Objects
wrangler d1 create my-objects
```

### 3. 优化数据库查询

```typescript
// 不好 - 多个查询
for (const userId of userIds) {
  await env.DB.prepare('SELECT * FROM users WHERE id = ?')
    .bind(userId).first();
}

// 好 - 单个查询
const placeholders = userIds.map(() => '?').join(',');
await env.DB.prepare(`SELECT * FROM users WHERE id IN (${placeholders})`)
  .bind(...userIds).all();
```

### 4. 优雅地处理错误

```typescript
export default {
  async fetch(request: Request, env: Env): Promise<Response> {
    try {
      // 您的代码在这里
      const result = await processRequest(request, env);
      return Response.json(result);

    } catch (error) {
      console.error('Error:', error);

      return Response.json({
        error: 'Internal server error',
        message: error.message
      }, { status: 500 });
    }
  }
};
```

### 5. 使用TypeScript

```typescript
// 定义您的环境接口
interface Env {
  DB: D1Database;
  KV: KVNamespace;
  STORAGE: R2Bucket;
  API_KEY: string;
  STRIPE_SECRET_KEY: string;
}

// 获得完整的类型安全
export default {
  async fetch(request: Request, env: Env): Promise<Response> {
    // env.DB是正确类型的！
    const result = await env.DB.prepare('SELECT * FROM users').all();
    return Response.json(result);
  }
};
```

## 定价现实检验

### 微SaaS场景
- **100个活跃用户**
- **每个用户每天50个请求** = 5,000个请求/天
- **150,000个请求/月**
- **成本**：$0（在免费层级内）

### 小型SaaS场景
- **1,000个活跃用户**
- **每个用户每天50个请求** = 50,000个请求/天
- **1.5M个请求/月**
- **成本**：$5/月（付费计划）

### 增长的SaaS场景
- **10,000个活跃用户**
- **每个用户每天50个请求** = 500,000个请求/天
- **15M个请求/月**
- **成本**：$5 + $2.50超额 = $7.50/月

### 与替代方案比较
- **AWS Lambda**：相同规模约$25-50/月
- **Heroku**：最低$25-50/月
- **DigitalOcean**：VPS $12+/月
- **Vercel**：无服务器$20+/月

**Cloudflare Workers比替代方案便宜3-10倍！**

## 与Cloudflare服务的集成

### 与Pages Functions一起使用
在Pages上部署前端，在Workers中部署后端逻辑：
```typescript
// pages/functions/api/[[path]].ts
export async function onRequest(context) {
  // 访问与Workers相同的绑定
  const result = await context.env.DB.prepare(
    'SELECT * FROM users'
  ).all();

  return Response.json(result);
}
```

### 组合堆栈
- **Pages**：前端（React/Vue/Astro）
- **Workers**：API层
- **D1**：数据库
- **R2**：文件存储
- **KV**：缓存和会话

全部集成，全部便宜，全部快速！

## 官方资源

- **文档**：https://developers.cloudflare.com/workers/
- **教程**：https://developers.cloudflare.com/workers/tutorials/
- **示例**：https://github.com/cloudflare/workers-sdk/tree/main/templates
- **Discord社区**：https://discord.cloudflare.com
- **Workers Playground**：https://workers.cloudflare.com/playground

## 真实世界成功故事

Workers为以下提供支持：
- **Discord** - 处理数十亿次请求
- **Shopify** - 结账流程
- **Zerodha** - 印度最大的股票经纪商
- 数千个微SaaS应用程序

## 为什么它非常适合SaaS

1. **超低的启动成本** - 免费层级慷慨
2. **从第一天开始全球化** - 边缘部署
3. **自动扩展** - 无基础设施管理
4. **快速开发** - 几秒钟内部署
5. **集成生态系统** - D1、KV、R2无缝协作
6. **生产就绪** - 企业级可靠性
7. **成本效益扩展** - 定价随您增长

对于现代Web应用程序和SaaS产品，Cloudflare Workers在性能、成本和开发者体验方面提供了无与伦比的价值。