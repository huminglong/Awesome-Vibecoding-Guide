# Cloudflare KV（键值存储）

## 概述

Cloudflare Workers KV是一个全球、低延迟的键值数据存储。它专为高读取、低写入场景设计，您需要从世界任何地方快速访问数据。KV是最终一致的，并针对边缘计算进行了优化。

## 主要好处

### 全球低延迟读取
- **亚毫秒级读取**从边缘位置
- **全球复制** - 数据缓存在300+位置
- **最终一致** - 非常适合可缓存数据
- **针对读取优化** - 比数据库查询快1000倍

### 简单的键值模型
- **易于使用** - 只是键和值
- **无模式** - 存储任何数据结构
- **JSON支持** - 自动序列化
- **二进制支持** - 存储图像、文件等

### 高性价比
- **慷慨的免费层级** - 每天100,000次读取
- **负担得起的扩展** - 每百万次读取$0.50
- **免费层级无存储限制**，用于合理使用
- **包括全球分发**

### 完美用例
- **会话存储** - 用户会话和JWT令牌
- **配置数据** - 应用程序设置、功能标志
- **缓存层** - 数据库查询结果、API响应
- **速率限制** - 跟踪每个用户的请求计数
- **身份验证** - 存储哈希密码、API密钥
- **临时数据** - 上传令牌、电子邮件验证码

## 定价

### 免费层级
- **每天100,000次读取操作**
- **每天1,000次写入操作**
- **每天1,000次删除操作**
- **每天1,000次列出操作**
- **存储1 GB**（软限制）

### 付费计划（$5/月）
当您超过免费层级时：
- **包括1000万次读取**（之后每百万$0.50）
- **包括100万次写入**（之后每百万$5.00）
- **包括100万次删除**（之后每百万$5.00）
- **包括100万次列出**（之后每百万$5.00）
- **存储**：前1 GB免费，然后每GB $0.50

### 性能特征
- **读取延迟**：<1ms（在边缘缓存）
- **写入延迟**：~100-500ms（全球传播）
- **一致性**：最终一致（全球60秒）
- **键大小**：最多512字节
- **值大小**：最多25 MB

## 入门

### 先决条件
- Cloudflare账户
- 安装Wrangler CLI
- Worker或Pages项目

### 创建KV命名空间

```bash
# 创建生产KV命名空间
wrangler kv:namespace create "CACHE"

# 输出：
[[kv_namespaces]]
binding = "CACHE"
id = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"

# 创建预览/开发命名空间（可选）
wrangler kv:namespace create "CACHE" --preview

# 预览输出：
[[kv_namespaces]]
binding = "CACHE"
preview_id = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
```

### 在wrangler.toml中配置

```toml
name = "my-worker"
main = "src/index.ts"

# 生产KV命名空间
[[kv_namespaces]]
binding = "CACHE"
id = "your-namespace-id"
preview_id = "your-preview-namespace-id"

# 您可以有多个命名空间
[[kv_namespaces]]
binding = "SESSIONS"
id = "another-namespace-id"
```

## 基本操作

### 写入（Put）

```typescript
interface Env {
  CACHE: KVNamespace;
}

export default {
  async fetch(request: Request, env: Env): Promise<Response> {
    // 存储字符串
    await env.CACHE.put('key', 'value');

    // 存储带过期时间（1小时）
    await env.CACHE.put('session:123', 'user-data', {
      expirationTtl: 3600,
    });

    // 存储带绝对过期时间
    const expiresAt = Date.now() / 1000 + 3600; // 从现在起1小时
    await env.CACHE.put('token:abc', 'xyz', {
      expiration: expiresAt,
    });

    // 存储JSON对象
    const userData = { id: 123, name: 'John', email: 'john@example.com' };
    await env.CACHE.put('user:123', JSON.stringify(userData));

    return new Response('Stored successfully');
  },
};
```

### 读取（Get）

```typescript
export default {
  async fetch(request: Request, env: Env): Promise<Response> {
    // 获取字符串值
    const value = await env.CACHE.get('key');

    if (!value) {
      return new Response('Not found', { status: 404 });
    }

    // 作为JSON获取
    const userData = await env.CACHE.get('user:123', 'json');

    // 作为ArrayBuffer获取（用于二进制数据）
    const imageData = await env.CACHE.get('image:logo', 'arrayBuffer');

    // 作为流获取（用于大值）
    const stream = await env.CACHE.get('large-file', 'stream');

    // 带元数据获取
    const { value, metadata } = await env.CACHE.getWithMetadata('key');

    return Response.json({ value, userData });
  },
};
```

### 删除

```typescript
// 删除键
await env.CACHE.delete('key');

// 删除返回void，总是成功（即使键不存在）
await env.CACHE.delete('session:123');
```

### 列出键

```typescript
// 列出所有键
const list = await env.CACHE.list();

// 按前缀列出
const userKeys = await env.CACHE.list({ prefix: 'user:' });

// 分页列出
const firstPage = await env.CACHE.list({ limit: 100 });
const secondPage = await env.CACHE.list({
  limit: 100,
  cursor: firstPage.cursor,
});

// 列出键
for (const key of list.keys) {
  console.log(key.name, key.expiration, key.metadata);
}
```

## 高级特性

### 元数据

存储关于键的附加信息：

```typescript
// 带元数据存储
await env.CACHE.put('user:123', userData, {
  metadata: {
    createdAt: Date.now(),
    version: '1.0',
    tags: ['premium', 'verified'],
  },
});

// 带元数据检索
const { value, metadata } = await env.CACHE.getWithMetadata('user:123', 'json');

console.log(metadata.createdAt);
console.log(metadata.tags);
```

### 批量操作

```typescript
// 高效写入多个键
async function bulkWrite(data: Record<string, string>, env: Env) {
  await Promise.all(
    Object.entries(data).map(([key, value]) =>
      env.CACHE.put(key, value)
    )
  );
}

// 读取多个键
async function bulkRead(keys: string[], env: Env) {
  const values = await Promise.all(
    keys.map(key => env.CACHE.get(key))
  );

  return Object.fromEntries(
    keys.map((key, i) => [key, values[i]])
  );
}
```

## 常见用例

### 1. 会话管理

```typescript
interface Session {
  userId: string;
  email: string;
  createdAt: number;
}

// 创建会话
async function createSession(userId: string, email: string, env: Env): Promise<string> {
  const sessionId = crypto.randomUUID();
  const session: Session = {
    userId,
    email,
    createdAt: Date.now(),
  };

  // 存储会话24小时
  await env.SESSIONS.put(`session:${sessionId}`, JSON.stringify(session), {
    expirationTtl: 86400, // 24小时
  });

  return sessionId;
}

// 验证会话
async function validateSession(sessionId: string, env: Env): Promise<Session | null> {
  const session = await env.SESSIONS.get(`session:${sessionId}`, 'json') as Session | null;

  if (!session) return null;

  // 每次请求延长会话
  await env.SESSIONS.put(`session:${sessionId}`, JSON.stringify(session), {
    expirationTtl: 86400,
  });

  return session;
}

// 登出（删除会话）
async function logout(sessionId: string, env: Env) {
  await env.SESSIONS.delete(`session:${sessionId}`);
}
```

### 2. 速率限制

```typescript
async function checkRateLimit(ip: string, env: Env): Promise<boolean> {
  const key = `ratelimit:${ip}`;
  const current = await env.CACHE.get(key);

  if (!current) {
    // 第一次请求 - 允许并设置计数器
    await env.CACHE.put(key, '1', { expirationTtl: 60 }); // 1分钟窗口
    return true;
  }

  const count = parseInt(current);

  if (count >= 100) {
    // 超出速率限制（每分钟100次请求）
    return false;
  }

  // 递增计数器
  await env.CACHE.put(key, (count + 1).toString(), { expirationTtl: 60 });
  return true;
}

// 在Worker中使用
export default {
  async fetch(request: Request, env: Env): Promise<Response> {
    const ip = request.headers.get('cf-connecting-ip') || 'unknown';

    const allowed = await checkRateLimit(ip, env);

    if (!allowed) {
      return Response.json(
        { error: 'Rate limit exceeded' },
        { status: 429 }
      );
    }

    // 正常处理请求
    return Response.json({ success: true });
  },
};
```

### 3. 功能标志

```typescript
interface FeatureFlags {
  newDashboard: boolean;
  darkMode: boolean;
  betaFeatures: boolean;
}

async function getFeatureFlags(userId: string, env: Env): Promise<FeatureFlags> {
  // 首先尝试用户特定标志
  const userFlags = await env.CACHE.get(`flags:user:${userId}`, 'json') as FeatureFlags | null;

  if (userFlags) return userFlags;

  // 回退到默认标志
  const defaultFlags = await env.CACHE.get('flags:default', 'json') as FeatureFlags | null;

  return defaultFlags || {
    newDashboard: false,
    darkMode: true,
    betaFeatures: false,
  };
}

async function setFeatureFlags(flags: FeatureFlags, env: Env) {
  await env.CACHE.put('flags:default', JSON.stringify(flags));
}

async function setUserFeatureFlags(userId: string, flags: Partial<FeatureFlags>, env: Env) {
  const currentFlags = await getFeatureFlags(userId, env);
  const newFlags = { ...currentFlags, ...flags };

  await env.CACHE.put(`flags:user:${userId}`, JSON.stringify(newFlags));
}
```

### 4. 缓存数据库查询

```typescript
interface Env {
  DB: D1Database;
  CACHE: KVNamespace;
}

async function getUserCached(userId: string, env: Env) {
  const cacheKey = `user:${userId}`;

  // 首先尝试缓存
  const cached = await env.CACHE.get(cacheKey, 'json');

  if (cached) {
    return cached;
  }

  // 缓存未命中 - 查询数据库
  const user = await env.DB.prepare(
    'SELECT id, email, name, created_at FROM users WHERE id = ?'
  ).bind(userId).first();

  if (!user) return null;

  // 存储到缓存5分钟
  await env.CACHE.put(cacheKey, JSON.stringify(user), {
    expirationTtl: 300,
  });

  return user;
}

async function updateUser(userId: string, data: any, env: Env) {
  // 更新数据库
  await env.DB.prepare(
    'UPDATE users SET name = ? WHERE id = ?'
  ).bind(data.name, userId).run();

  // 使缓存失效
  await env.CACHE.delete(`user:${userId}`);
}
```

### 5. API密钥存储

```typescript
interface APIKey {
  userId: string;
  permissions: string[];
  createdAt: number;
  lastUsed: number;
}

async function createAPIKey(userId: string, permissions: string[], env: Env): Promise<string> {
  const apiKey = `sk_${crypto.randomUUID().replace(/-/g, '')}`;
  const keyData: APIKey = {
    userId,
    permissions,
    createdAt: Date.now(),
    lastUsed: Date.now(),
  };

  await env.CACHE.put(`apikey:${apiKey}`, JSON.stringify(keyData));

  // 存储用户的API密钥列表
  const userKeys = await env.CACHE.get(`user:${userId}:apikeys`, 'json') as string[] || [];
  userKeys.push(apiKey);
  await env.CACHE.put(`user:${userId}:apikeys`, JSON.stringify(userKeys));

  return apiKey;
}

async function validateAPIKey(apiKey: string, env: Env): Promise<APIKey | null> {
  const keyData = await env.CACHE.get(`apikey:${apiKey}`, 'json') as APIKey | null;

  if (!keyData) return null;

  // 更新最后使用时间
  keyData.lastUsed = Date.now();
  await env.CACHE.put(`apikey:${apiKey}`, JSON.stringify(keyData));

  return keyData;
}

async function revokeAPIKey(apiKey: string, env: Env) {
  await env.CACHE.delete(`apikey:${apiKey}`);
}

// 在Worker中使用
export default {
  async fetch(request: Request, env: Env): Promise<Response> {
    const apiKey = request.headers.get('x-api-key');

    if (!apiKey) {
      return Response.json({ error: 'API key required' }, { status: 401 });
    }

    const keyData = await validateAPIKey(apiKey, env);

    if (!keyData) {
      return Response.json({ error: 'Invalid API key' }, { status: 401 });
    }

    // 检查权限
    if (!keyData.permissions.includes('read')) {
      return Response.json({ error: 'Insufficient permissions' }, { status: 403 });
    }

    // 用认证用户处理请求
    return Response.json({ userId: keyData.userId });
  },
};
```

### 6. 缓存外部API响应

```typescript
async function fetchWithCache(url: string, env: Env, cacheTtl: number = 300) {
  const cacheKey = `api:${url}`;

  // 首先尝试缓存
  const cached = await env.CACHE.get(cacheKey);

  if (cached) {
    return JSON.parse(cached);
  }

  // 缓存未命中 - 从API获取
  const response = await fetch(url);
  const data = await response.json();

  // 存储到缓存
  await env.CACHE.put(cacheKey, JSON.stringify(data), {
    expirationTtl: cacheTtl,
  });

  return data;
}

// 使用
export default {
  async fetch(request: Request, env: Env): Promise<Response> {
    // 昂贵的API调用缓存10分钟
    const data = await fetchWithCache(
      'https://api.example.com/data',
      env,
      600
    );

    return Response.json(data);
  },
};
```

### 7. 邮箱验证码

```typescript
async function generateVerificationCode(email: string, env: Env): Promise<string> {
  const code = Math.random().toString(36).substring(2, 8).toUpperCase();

  await env.CACHE.put(`verify:${email}`, code, {
    expirationTtl: 900, // 15分钟
  });

  // 发送带代码的邮件（实现未显示）
  await sendEmail(email, `Your verification code is: ${code}`);

  return code;
}

async function verifyCode(email: string, code: string, env: Env): Promise<boolean> {
  const storedCode = await env.CACHE.get(`verify:${email}`);

  if (!storedCode || storedCode !== code) {
    return false;
  }

  // 成功验证后删除代码
  await env.CACHE.delete(`verify:${email}`);

  return true;
}
```

### 8. 购物车存储

```typescript
interface CartItem {
  productId: string;
  quantity: number;
  price: number;
}

interface Cart {
  items: CartItem[];
  total: number;
  updatedAt: number;
}

async function addToCart(sessionId: string, item: CartItem, env: Env) {
  const cartKey = `cart:${sessionId}`;
  const cart = await env.CACHE.get(cartKey, 'json') as Cart | null;

  const items = cart?.items || [];
  const existingItem = items.find(i => i.productId === item.productId);

  if (existingItem) {
    existingItem.quantity += item.quantity;
  } else {
    items.push(item);
  }

  const total = items.reduce((sum, i) => sum + (i.price * i.quantity), 0);

  const newCart: Cart = {
    items,
    total,
    updatedAt: Date.now(),
  };

  // 存储购物车7天
  await env.CACHE.put(cartKey, JSON.stringify(newCart), {
    expirationTtl: 604800,
  });

  return newCart;
}

async function getCart(sessionId: string, env: Env): Promise<Cart | null> {
  return await env.CACHE.get(`cart:${sessionId}`, 'json') as Cart | null;
}

async function clearCart(sessionId: string, env: Env) {
  await env.CACHE.delete(`cart:${sessionId}`);
}
```

## CLI管理

### 从命令行使用KV

```bash
# 创建命名空间
wrangler kv:namespace create "MY_KV"

# 列出命名空间
wrangler kv:namespace list

# 删除命名空间
wrangler kv:namespace delete --namespace-id="xxx"

# 设置键
wrangler kv:key put "key" "value" --namespace-id="xxx"

# 获取键
wrangler kv:key get "key" --namespace-id="xxx"

# 删除键
wrangler kv:key delete "key" --namespace-id="xxx"

# 列出键
wrangler kv:key list --namespace-id="xxx"

# 从JSON文件批量上传
wrangler kv:bulk put data.json --namespace-id="xxx"
```

### 批量上传示例

```json
// data.json
[
  { "key": "user:1", "value": "{\"name\":\"John\",\"email\":\"john@example.com\"}" },
  { "key": "user:2", "value": "{\"name\":\"Jane\",\"email\":\"jane@example.com\"}" },
  { "key": "config:theme", "value": "dark" }
]
```

```bash
wrangler kv:bulk put data.json --namespace-id="xxx"
```

## 与Pages函数一起使用

```typescript
// functions/api/session.ts

interface Env {
  SESSIONS: KVNamespace;
}

export const onRequestPost: PagesFunction<Env> = async (context) => {
  const { email, password } = await context.request.json();

  // 验证凭据（实现未显示）
  const userId = await validateCredentials(email, password);

  if (!userId) {
    return Response.json({ error: 'Invalid credentials' }, { status: 401 });
  }

  // 创建会话
  const sessionId = crypto.randomUUID();
  await context.env.SESSIONS.put(
    `session:${sessionId}`,
    JSON.stringify({ userId, email }),
    { expirationTtl: 86400 }
  );

  return Response.json({ sessionId });
};

export const onRequestGet: PagesFunction<Env> = async (context) => {
  const sessionId = context.request.headers.get('x-session-id');

  if (!sessionId) {
    return Response.json({ error: 'No session' }, { status: 401 });
  }

  const session = await context.env.SESSIONS.get(`session:${sessionId}`, 'json');

  if (!session) {
    return Response.json({ error: 'Invalid session' }, { status: 401 });
  }

  return Response.json(session);
};
```

## 确保它工作

### 测试检查清单

1. **本地测试**:
   ```bash
   # 启动开发服务器
   wrangler dev --local

   # 测试KV操作
   curl http://localhost:8787/api/set
   curl http://localhost:8787/api/get
   ```

2. **在仪表板中验证**:
   - 转到 Workers & Pages > KV
   - 选择您的命名空间
   - 查看键和值
   - 手动添加/编辑/删除键进行测试

3. **CLI测试**:
   ```bash
   # 写入测试键
   wrangler kv:key put "test" "hello" --namespace-id="xxx"

   # 读取回来
   wrangler kv:key get "test" --namespace-id="xxx"

   # 列出键
   wrangler kv:key list --namespace-id="xxx"
   ```

4. **生产测试**:
   ```bash
   wrangler deploy
   curl https://your-worker.workers.dev/api/test
   ```

### 常见问题和解决方案

**KV命名空间未找到：**
- 验证`wrangler.toml`中的`id`与实际命名空间匹配
- 检查绑定名称与代码匹配
- 运行`wrangler kv:namespace list`验证

**值未更新：**
- 记住KV是最终一致的
- 变更在60秒内在全球传播
- 使用缓存头（如果为浏览器提供服务）

**超出限制：**
- 在仪表板中检查每日操作限制
- 考虑升级到付费计划
- 实现缓存以减少操作

## 最佳实践

### 1. 使用适当的TTL

```typescript
// 频繁变化数据的短TTL
await env.KV.put('user:online', 'true', { expirationTtl: 60 }); // 1分钟

// 半静态数据的中等TL
await env.KV.put('api:response', data, { expirationTtl: 300 }); // 5分钟

// 很少变化数据的长TTL
await env.KV.put('config:settings', data, { expirationTtl: 86400 }); // 24小时
```

### 2. 使用前缀进行组织

```typescript
// 按类型组织键
await env.KV.put('user:123', userData);
await env.KV.put('session:abc', sessionData);
await env.KV.put('cache:query:xyz', queryResult);
await env.KV.put('config:feature-flags', flags);

// 易于按类型列出
const sessions = await env.KV.list({ prefix: 'session:' });
```

### 3. 优雅处理缺失键

```typescript
// 始终检查null
const value = await env.KV.get('key');

if (!value) {
  // 处理缺失键
  return Response.json({ error: 'Not found' }, { status: 404 });
}
```

### 4. 不要在高写入场景中使用KV

KV针对读取进行了优化，而不是写入。对于高写入用例：
- 改用D1数据库
- 使用Durable Objects进行协调
- 尽可能批量写入

### 5. 考虑一致性模型

```typescript
// KV是最终一致的
await env.KV.put('key', 'value1');

// 立即读取可能返回旧值
const value = await env.KV.get('key'); // 可能是null或旧值

// 对于强一致性，改用D1
```

## 高效使用技巧

### 1. 与D1结合以获得最佳性能

```typescript
// 使用KV作为热数据，D1作为冷数据
async function getUser(userId: string, env: Env) {
  // 首先尝试KV缓存（热数据）
  let user = await env.CACHE.get(`user:${userId}`, 'json');

  if (!user) {
    // 未命中 - 从D1获取（冷数据）
    user = await env.DB.prepare(
      'SELECT * FROM users WHERE id = ?'
    ).bind(userId).first();

    // 预热缓存
    if (user) {
      await env.CACHE.put(`user:${userId}`, JSON.stringify(user), {
        expirationTtl: 300,
      });
    }
  }

  return user;
}
```

### 2. 使用元数据进行过滤

```typescript
// 带元数据存储
await env.KV.put('item:1', data, {
  metadata: {
    category: 'electronics',
    price: 999,
    featured: true,
  },
});

// 列出并按元数据过滤
const list = await env.KV.list({ prefix: 'item:' });
const featured = list.keys.filter(k => k.metadata?.featured === true);
```

### 3. 实现Stale-While-Revalidate

```typescript
async function getWithSWR(key: string, fetcher: () => Promise<any>, env: Env) {
  const cached = await env.KV.getWithMetadata(key, 'json');

  if (cached.value) {
    const age = Date.now() - (cached.metadata?.cachedAt || 0);

    // 如果新鲜（< 5分钟）则返回缓存
    if (age < 300000) {
      return cached.value;
    }

    // 如果陈旧（5-10分钟）则在后台重新验证
    if (age < 600000) {
      // 立即返回陈旧数据
      env.waitUntil(
        fetcher().then(fresh =>
          env.KV.put(key, JSON.stringify(fresh), {
            metadata: { cachedAt: Date.now() },
          })
        )
      );
      return cached.value;
    }
  }

  // 如果非常陈旧或缺失则获取新鲜数据
  const fresh = await fetcher();
  await env.KV.put(key, JSON.stringify(fresh), {
    metadata: { cachedAt: Date.now() },
  });
  return fresh;
}
```

## 官方资源

- **文档**: https://developers.cloudflare.com/kv/
- **API参考**: https://developers.cloudflare.com/kv/api/
- **定价**: https://developers.cloudflare.com/kv/platform/pricing/
- **最佳实践**: https://developers.cloudflare.com/kv/learning/kv-best-practices/
- **限制**: https://developers.cloudflare.com/kv/platform/limits/
- **Discord社区**: https://discord.cloudflare.com

## 为什么KV对边缘应用至关重要

1. **闪电般快速的读取** - 亚毫秒级访问
2. **全球分发** - 数据在用户所在的所有地方
3. **简单API** - 易于使用的键值接口
4. **慷慨的免费层级** - 每天100k次读取免费
5. **完美用于缓存** - 大大减少数据库负载
6. **会话存储** - 理想用于身份验证
7. **功能标志** - 即时配置更新
8. **速率限制** - 保护您的API

KV是D1和Workers的完美补充，提供高性能缓存层，显著减少延迟和数据库负载。将KV用于热数据和缓存，D1用于关系数据，R2用于文件，您就有了一个完整的边缘原生堆栈！