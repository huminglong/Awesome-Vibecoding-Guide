# Cloudflare D1数据库

## 概述

Cloudflare D1是一个基于SQLite构建的无服务器SQL数据库，旨在Cloudflare的全球网络上运行。它专门针对Workers和Pages Functions进行了优化，提供快速、可扩展且极其高性价比的原生数据库解决方案。

## 主要好处

### 基于SQLite的强大功能
- **完整SQL支持** - 标准SQLite语法
- **ACID合规** - 可靠的事务
- **连接、索引、约束** - 所有标准SQL功能
- **JSON支持** - 存储和查询JSON数据
- **外键** - 维护数据完整性

### 边缘原生集成
- **零冷启动** - 始终就绪
- **原生Workers绑定** - 无连接字符串
- **全球复制** - 数据靠近用户（即将推出）
- **自动备份** - 时间点恢复
- **读取副本** - 全球扩展读取

### 极其便宜和可扩展
- **超级慷慨的免费层级**
- **线性定价** - 可预测的成本
- **无每数据库费用** - 按使用付费
- **自动扩展** - 无容量规划
- **卓越性能** - 边缘延迟

### 非常适合微SaaS
- **免费开始** - 零前期成本
- **随应用程序增长** - 平滑扩展
- **生产就绪** - 企业级可靠性
- **快速查询** - 亚10ms延迟
- **简单迁移** - 标准SQL

## 定价

### 免费层级（非常慷慨）
- **5 GB存储**每个账户
- **每天500万行读取**
- **每天10万行写入**
- **无限制数据库**
- 非常适合副业项目和微SaaS

### 付费计划（$5/月）
当您超过免费层级时：
- **每月250亿行读取**（之后每百万$0.001）
- **每月5000万行写入**（之后每百万$1.00）
- **包括5 GB存储**（之后每GB $0.75）

### 成本现实检验

**小型SaaS**（1,000个活跃用户）：
- 约500K读取/天 = 15M读取/月
- 约50K写入/天 = 1.5M写入/月
- 约1 GB存储
- **成本：$0**（在免费层级内！）

**增长的SaaS**（10,000个用户）：
- 约5M读取/天 = 150M读取/月
- 约500K写入/天 = 15M写入/月
- 约10 GB存储
- **成本：$5-10/月**

**比较**：
- **AWS RDS**：最低$50-200/月
- **PlanetScale**：$29+/月
- **Supabase**：$25+/月
- **D1**：相同工作负载$0-10/月

**D1比替代方案便宜5-20倍！**

## 入门

### 先决条件
- Cloudflare账户
- 安装Wrangler CLI（`npm install -g wrangler`）
- Worker或Pages项目

### 创建您的第一个数据库

```bash
# 创建新的D1数据库
wrangler d1 create my-database

# 输出显示：
[[d1_databases]]
binding = "DB"
database_name = "my-database"
database_id = "xxxx-xxxx-xxxx-xxxx"

# 将此复制到您的wrangler.toml
```

### 添加到wrangler.toml

```toml
name = "my-worker"
main = "src/index.ts"
compatibility_date = "2024-01-01"

[[d1_databases]]
binding = "DB"
database_name = "my-database"
database_id = "your-database-id-here"
```

### 创建架构

创建一个`schema.sql`文件：

```sql
-- schema.sql
CREATE TABLE IF NOT EXISTS users (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  email TEXT UNIQUE NOT NULL,
  name TEXT NOT NULL,
  password_hash TEXT NOT NULL,
  created_at INTEGER DEFAULT (strftime('%s', 'now')),
  updated_at INTEGER DEFAULT (strftime('%s', 'now'))
);

CREATE INDEX idx_users_email ON users(email);

CREATE TABLE IF NOT EXISTS subscriptions (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  user_id INTEGER NOT NULL,
  plan TEXT NOT NULL CHECK(plan IN ('free', 'pro', 'enterprise')),
  status TEXT NOT NULL CHECK(status IN ('active', 'cancelled', 'expired')),
  started_at INTEGER NOT NULL,
  expires_at INTEGER,
  FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);

CREATE INDEX idx_subscriptions_user_id ON subscriptions(user_id);
CREATE INDEX idx_subscriptions_status ON subscriptions(status);

CREATE TABLE IF NOT EXISTS api_keys (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  user_id INTEGER NOT NULL,
  key TEXT UNIQUE NOT NULL,
  name TEXT,
  last_used_at INTEGER,
  created_at INTEGER DEFAULT (strftime('%s', 'now')),
  FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);

CREATE INDEX idx_api_keys_key ON api_keys(key);
```

### 应用架构

```bash
# 对于生产数据库
wrangler d1 execute my-database --file=schema.sql

# 对于本地开发（使用--local标志）
wrangler d1 execute my-database --local --file=schema.sql
```

## 在Workers中的基本用法

### 简单查询示例

```typescript
interface Env {
  DB: D1Database;
}

export default {
  async fetch(request: Request, env: Env): Promise<Response> {
    const url = new URL(request.url);

    // 获取所有用户
    if (url.pathname === '/api/users') {
      const { results } = await env.DB.prepare(
        'SELECT id, email, name, created_at FROM users ORDER BY created_at DESC'
      ).all();

      return Response.json(results);
    }

    // 获取单个用户
    if (url.pathname.startsWith('/api/users/')) {
      const userId = url.pathname.split('/')[3];

      const user = await env.DB.prepare(
        'SELECT id, email, name, created_at FROM users WHERE id = ?'
      ).bind(userId).first();

      if (!user) {
        return Response.json({ error: 'User not found' }, { status: 404 });
      }

      return Response.json(user);
    }

    return new Response('Not found', { status: 404 });
  },
};
```

### 插入数据

```typescript
// 创建新用户
if (url.pathname === '/api/users' && request.method === 'POST') {
  const { email, name, password } = await request.json();

  // 哈希密码（在生产中使用bcrypt或类似工具）
  const passwordHash = await hashPassword(password);

  const result = await env.DB.prepare(
    'INSERT INTO users (email, name, password_hash) VALUES (?, ?, ?)'
  ).bind(email, name, passwordHash).run();

  if (!result.success) {
    return Response.json(
      { error: 'Failed to create user' },
      { status: 500 }
    );
  }

  return Response.json({
    success: true,
    userId: result.meta.last_row_id,
  });
}
```

### 更新数据

```typescript
// 更新用户
if (url.pathname.startsWith('/api/users/') && request.method === 'PUT') {
  const userId = url.pathname.split('/')[3];
  const { name } = await request.json();

  const result = await env.DB.prepare(
    'UPDATE users SET name = ?, updated_at = strftime(\'%s\', \'now\') WHERE id = ?'
  ).bind(name, userId).run();

  if (result.meta.changes === 0) {
    return Response.json({ error: 'User not found' }, { status: 404 });
  }

  return Response.json({ success: true });
}
```

### 删除数据

```typescript
// 删除用户
if (url.pathname.startsWith('/api/users/') && request.method === 'DELETE') {
  const userId = url.pathname.split('/')[3];

  const result = await env.DB.prepare(
    'DELETE FROM users WHERE id = ?'
  ).bind(userId).run();

  if (result.meta.changes === 0) {
    return Response.json({ error: 'User not found' }, { status: 404 });
  }

  return Response.json({ success: true });
}
```

## 高级查询

### 连接

```typescript
// 获取用户及其订阅
async function getUserWithSubscription(userId: string, db: D1Database) {
  const result = await db.prepare(`
    SELECT
      u.id,
      u.email,
      u.name,
      s.plan,
      s.status,
      s.expires_at
    FROM users u
    LEFT JOIN subscriptions s ON u.id = s.user_id
    WHERE u.id = ? AND (s.status = 'active' OR s.status IS NULL)
  `).bind(userId).first();

  return result;
}
```

### 聚合

```typescript
// 按订阅计划获取用户计数
async function getSubscriptionStats(db: D1Database) {
  const { results } = await db.prepare(`
    SELECT
      plan,
      COUNT(*) as user_count,
      SUM(CASE WHEN status = 'active' THEN 1 ELSE 0 END) as active_count
    FROM subscriptions
    GROUP BY plan
    ORDER BY user_count DESC
  `).all();

  return results;
}
```

### 分页

```typescript
async function getUsers(page: number, limit: number, db: D1Database) {
  const offset = (page - 1) * limit;

  const { results } = await db.prepare(`
    SELECT id, email, name, created_at
    FROM users
    ORDER BY created_at DESC
    LIMIT ? OFFSET ?
  `).bind(limit, offset).all();

  // 获取总数
  const { total } = await db.prepare(
    'SELECT COUNT(*) as total FROM users'
  ).first() as { total: number };

  return {
    users: results,
    pagination: {
      page,
      limit,
      total,
      totalPages: Math.ceil(total / limit),
    },
  };
}
```

### 全文搜索

```typescript
// 创建FTS表
await db.prepare(`
  CREATE VIRTUAL TABLE IF NOT EXISTS posts_fts USING fts5(
    title,
    content,
    content='posts',
    content_rowid='id'
  )
`).run();

// 搜索帖子
async function searchPosts(query: string, db: D1Database) {
  const { results } = await db.prepare(`
    SELECT p.*, rank
    FROM posts_fts
    JOIN posts p ON posts_fts.rowid = p.id
    WHERE posts_fts MATCH ?
    ORDER BY rank
    LIMIT 20
  `).bind(query).all();

  return results;
}
```

## 事务

```typescript
async function transferCredits(
  fromUserId: string,
  toUserId: string,
  amount: number,
  db: D1Database
) {
  try {
    // 开始事务
    await db.prepare('BEGIN TRANSACTION').run();

    // 从发送者扣除
    await db.prepare(
      'UPDATE users SET credits = credits - ? WHERE id = ?'
    ).bind(amount, fromUserId).run();

    // 添加到接收者
    await db.prepare(
      'UPDATE users SET credits = credits + ? WHERE id = ?'
    ).bind(amount, toUserId).run();

    // 记录事务
    await db.prepare(
      'INSERT INTO transactions (from_user_id, to_user_id, amount) VALUES (?, ?, ?)'
    ).bind(fromUserId, toUserId, amount).run();

    // 提交
    await db.prepare('COMMIT').run();

    return { success: true };

  } catch (error) {
    // 错误时回滚
    await db.prepare('ROLLBACK').run();
    throw error;
  }
}
```

## 批量操作

```typescript
// 高效插入多条记录
async function batchInsertUsers(users: Array<{ email: string, name: string }>, db: D1Database) {
  const statements = users.map(user =>
    db.prepare(
      'INSERT INTO users (email, name, password_hash) VALUES (?, ?, ?)'
    ).bind(user.email, user.name, 'temporary-hash')
  );

  // 在一个批次中执行所有操作
  const results = await db.batch(statements);

  return {
    success: true,
    count: results.length,
  };
}
```

## JSON支持

```typescript
// 存储JSON数据
await db.prepare(`
  CREATE TABLE IF NOT EXISTS settings (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    user_id INTEGER NOT NULL,
    preferences TEXT NOT NULL, -- JSON存储为TEXT
    FOREIGN KEY (user_id) REFERENCES users(id)
  )
`).run();

// 插入JSON
async function saveUserPreferences(userId: string, preferences: object, db: D1Database) {
  await db.prepare(
    'INSERT OR REPLACE INTO settings (user_id, preferences) VALUES (?, ?)'
  ).bind(userId, JSON.stringify(preferences)).run();
}

// 查询JSON
async function getUserPreferences(userId: string, db: D1Database) {
  const result = await db.prepare(
    'SELECT preferences FROM settings WHERE user_id = ?'
  ).bind(userId).first();

  if (!result) return null;

  return JSON.parse(result.preferences as string);
}

// 查询JSON字段（SQLite JSON函数）
async function getUsersByTheme(theme: string, db: D1Database) {
  const { results } = await db.prepare(`
    SELECT u.id, u.name
    FROM users u
    JOIN settings s ON u.id = s.user_id
    WHERE json_extract(s.preferences, '$.theme') = ?
  `).bind(theme).all();

  return results;
}
```

## 在Pages Functions中使用

```typescript
// functions/api/users/[id].ts

interface Env {
  DB: D1Database;
}

export const onRequestGet: PagesFunction<Env> = async (context) => {
  const userId = context.params.id as string;

  const user = await context.env.DB.prepare(
    'SELECT id, email, name FROM users WHERE id = ?'
  ).bind(userId).first();

  if (!user) {
    return Response.json({ error: 'User not found' }, { status: 404 });
  }

  return Response.json(user);
};

export const onRequestPut: PagesFunction<Env> = async (context) => {
  const userId = context.params.id as string;
  const { name } = await context.request.json();

  await context.env.DB.prepare(
    'UPDATE users SET name = ? WHERE id = ?'
  ).bind(name, userId).run();

  return Response.json({ success: true });
};
```

## 本地开发

```bash
# 使用本地D1运行worker
wrangler dev --local

# 本地执行SQL
wrangler d1 execute my-database --local --command="SELECT * FROM users"

# 本地导入数据
wrangler d1 execute my-database --local --file=seed.sql
```

### 开发种子数据

```sql
-- seed.sql
INSERT INTO users (email, name, password_hash) VALUES
  ('alice@example.com', 'Alice', 'hash1'),
  ('bob@example.com', 'Bob', 'hash2'),
  ('charlie@example.com', 'Charlie', 'hash3');

INSERT INTO subscriptions (user_id, plan, status, started_at) VALUES
  (1, 'pro', 'active', strftime('%s', 'now')),
  (2, 'free', 'active', strftime('%s', 'now')),
  (3, 'enterprise', 'active', strftime('%s', 'now'));
```

```bash
# 加载种子数据
wrangler d1 execute my-database --local --file=seed.sql
```

## 迁移

### 管理架构更改

创建迁移文件：

```sql
-- migrations/001_initial.sql
CREATE TABLE users (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  email TEXT UNIQUE NOT NULL,
  name TEXT NOT NULL,
  created_at INTEGER DEFAULT (strftime('%s', 'now'))
);

-- migrations/002_add_subscriptions.sql
CREATE TABLE subscriptions (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  user_id INTEGER NOT NULL,
  plan TEXT NOT NULL,
  status TEXT NOT NULL,
  FOREIGN KEY (user_id) REFERENCES users(id)
);

-- migrations/003_add_api_keys.sql
CREATE TABLE api_keys (
  id INTEGER PRIMARY KEY AUTOINCREMENT,
  user_id INTEGER NOT NULL,
  key TEXT UNIQUE NOT NULL,
  created_at INTEGER DEFAULT (strftime('%s', 'now')),
  FOREIGN KEY (user_id) REFERENCES users(id)
);
```

应用迁移：

```bash
# 按顺序应用每个迁移
wrangler d1 execute my-database --file=migrations/001_initial.sql
wrangler d1 execute my-database --file=migrations/002_add_subscriptions.sql
wrangler d1 execute my-database --file=migrations/003_add_api_keys.sql
```

## 确保它工作

### 测试清单

1. **本地测试**：
   ```bash
   # 启动开发服务器
   wrangler dev --local

   # 测试端点
   curl http://localhost:8787/api/users
   ```

2. **验证架构**：
   ```bash
   # 检查表
   wrangler d1 execute my-database --command="SELECT name FROM sqlite_master WHERE type='table'"

   # 检查索引
   wrangler d1 execute my-database --command="SELECT name FROM sqlite_master WHERE type='index'"
   ```

3. **测试查询**：
   ```bash
   # 测试数据插入
   wrangler d1 execute my-database --command="INSERT INTO users (email, name, password_hash) VALUES ('test@example.com', 'Test User', 'hash')"

   # 验证数据
   wrangler d1 execute my-database --command="SELECT * FROM users"
   ```

4. **生产部署**：
   ```bash
   # 部署worker
   wrangler deploy

   # 测试生产端点
   curl https://my-worker.workers.dev/api/users
   ```

### 监控

```bash
# 查看实时日志
wrangler tail

# 在仪表板中检查指标：
# - 查询计数
# - 存储使用
# - 错误率
```

### 常见问题和解决方案

**数据库未找到：**
- 验证`wrangler.toml`中的`database_id`
- 检查绑定名称是否与代码匹配
- 确认数据库存在：`wrangler d1 list`

**架构未应用：**
- 重新运行架构文件：`wrangler d1 execute db-name --file=schema.sql`
- 检查SQL语法错误

**慢查询：**
- 在频繁查询的列上添加索引
- 使用`EXPLAIN QUERY PLAN`分析查询
- 考虑对复杂连接进行反规范化

## 高效使用提示

### 1. 智慧使用索引

```sql
-- 为频繁查询的列创建索引
CREATE INDEX idx_users_email ON users(email);
CREATE INDEX idx_subscriptions_user_status ON subscriptions(user_id, status);

-- 为复杂查询创建复合索引
CREATE INDEX idx_api_keys_user_last_used ON api_keys(user_id, last_used_at);
```

### 2. 优化查询

```typescript
// 不好 - N+1查询问题
for (const user of users) {
  const subscription = await db.prepare(
    'SELECT * FROM subscriptions WHERE user_id = ?'
  ).bind(user.id).first();
}

// 好 - 单个查询与连接
const { results } = await db.prepare(`
  SELECT u.*, s.plan, s.status
  FROM users u
  LEFT JOIN subscriptions s ON u.id = s.user_id
  WHERE u.id IN (?, ?, ?)
`).bind(...userIds).all();
```

### 3. 在KV中缓存频繁查询

```typescript
async function getUserCached(userId: string, env: Env) {
  // 首先尝试缓存
  const cached = await env.KV.get(`user:${userId}`, 'json');
  if (cached) return cached;

  // 查询数据库
  const user = await env.DB.prepare(
    'SELECT * FROM users WHERE id = ?'
  ).bind(userId).first();

  // 缓存5分钟
  await env.KV.put(`user:${userId}`, JSON.stringify(user), {
    expirationTtl: 300,
  });

  return user;
}
```

### 4. 使用准备语句

```typescript
// 重用准备语句以获得更好的性能
const getUserStmt = env.DB.prepare(
  'SELECT * FROM users WHERE id = ?'
);

// 多次执行
const user1 = await getUserStmt.bind('1').first();
const user2 = await getUserStmt.bind('2').first();
```

### 5. 批量操作

```typescript
// 不好 - 多个单独操作
for (const user of users) {
  await db.prepare(
    'INSERT INTO users (email, name) VALUES (?, ?)'
  ).bind(user.email, user.name).run();
}

// 好 - 批量操作
const statements = users.map(user =>
  db.prepare('INSERT INTO users (email, name) VALUES (?, ?)').bind(user.email, user.name)
);
await db.batch(statements);
```

## 真实世界示例：微SaaS API

带有分析功能的URL缩短器的完整示例：

```typescript
interface Env {
  DB: D1Database;
  KV: KVNamespace;
}

export default {
  async fetch(request: Request, env: Env): Promise<Response> {
    const url = new URL(request.url);

    // 重定向短URL
    if (url.pathname.startsWith('/s/')) {
      const code = url.pathname.slice(3);

      // 检查缓存
      let targetUrl = await env.KV.get(`link:${code}`);

      if (!targetUrl) {
        // 从数据库获取
        const link = await env.DB.prepare(
          'SELECT url FROM links WHERE code = ? AND active = 1'
        ).bind(code).first() as { url: string } | null;

        if (!link) {
          return new Response('Link not found', { status: 404 });
        }

        targetUrl = link.url;
        await env.KV.put(`link:${code}`, targetUrl, { expirationTtl: 3600 });
      }

      // 跟踪点击
      await env.DB.prepare(
        'INSERT INTO clicks (link_code, clicked_at, user_agent, country) VALUES (?, ?, ?, ?)'
      ).bind(code, Date.now(), request.headers.get('user-agent'), request.cf?.country).run();

      return Response.redirect(targetUrl, 301);
    }

    // 创建短链接
    if (url.pathname === '/api/shorten' && request.method === 'POST') {
      const { url: targetUrl, customCode } = await request.json();

      const code = customCode || generateCode(6);

      // 检查代码是否存在
      const existing = await env.DB.prepare(
        'SELECT 1 FROM links WHERE code = ?'
      ).bind(code).first();

      if (existing) {
        return Response.json({ error: 'Code already exists' }, { status: 400 });
      }

      // 创建链接
      await env.DB.prepare(
        'INSERT INTO links (code, url, created_at, active) VALUES (?, ?, ?, 1)'
      ).bind(code, targetUrl, Date.now()).run();

      return Response.json({
        shortUrl: `${url.origin}/s/${code}`,
        code,
      });
    }

    // 获取链接分析
    if (url.pathname.startsWith('/api/analytics/')) {
      const code = url.pathname.split('/')[3];

      const stats = await env.DB.prepare(`
        SELECT
          COUNT(*) as total_clicks,
          COUNT(DISTINCT country) as unique_countries,
          DATE(clicked_at / 1000, 'unixepoch') as date,
          COUNT(*) as clicks_per_day
        FROM clicks
        WHERE link_code = ?
        GROUP BY date
        ORDER BY date DESC
        LIMIT 30
      `).bind(code).all();

      return Response.json(stats.results);
    }

    return new Response('Not found', { status: 404 });
  },
};

function generateCode(length: number): string {
  const chars = 'abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789';
  return Array.from({ length }, () => chars[Math.floor(Math.random() * chars.length)]).join('');
}
```

## 官方资源

- **文档**：https://developers.cloudflare.com/d1/
- **API参考**：https://developers.cloudflare.com/d1/platform/client-api/
- **定价**：https://developers.cloudflare.com/d1/platform/pricing/
- **示例**：https://developers.cloudflare.com/d1/examples/
- **SQLite参考**：https://www.sqlite.org/docs.html
- **Discord社区**：https://discord.cloudflare.com

## 为什么D1非常适合微SaaS

1. **快速从零到生产** - 在几分钟内创建数据库
2. **SQLite简单性** - 无复杂设置
3. **边缘性能** - 亚10ms查询
4. **极其便宜** - 大多数应用$0-10/月
5. **原生Workers集成** - 无连接字符串
6. **自动扩展** - 无容量规划
7. **完整SQL功能** - 不像NoSQL那样受限
8. **生产就绪** - 企业级可靠性

对于微SaaS应用程序和现代Web应用程序，D1提供了简单性、性能和成本效益的完美平衡。与Workers、Pages、KV和R2结合，您拥有一个完整的边缘原生堆栈，可以从零扩展到数百万用户，同时保持成本极低。