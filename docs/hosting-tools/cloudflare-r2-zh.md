# Cloudflare R2存储

## 概述

Cloudflare R2（Ridiculously Resilient）存储是一个S3兼容的对象存储服务，旨在存储大量非结构化数据，如图像、视频、音频文件、文档和备份。关键区别：**零出口费用**。

## 主要好处

### 高性价比存储
- **免费层级10 GB存储**
- **无出口费用**（与AWS S3不同，后者对下载收费）
- **免费层级无API请求费用**
- 仅按存储和操作付费，按规模

### S3兼容
- **AWS S3的直接替代品**
- 使用现有的S3库和工具
- 从S3无缝迁移
- 标准S3 API兼容性

### 快速和可靠
- **通过Cloudflare网络全球分发**
- **与Workers/Pages一起使用时自动缓存**
- **具有自动复制的高可用性**
- **全球低延迟访问**

### 完美用例
- **图像/视频托管**用于Web应用程序
- **用户生成内容**存储
- **静态资产托管**（CSS、JS、字体）
- **数据库和文件的备份存储**
- **内容平台的媒体库**
- **SaaS应用程序中的文件上传/下载**功能

## 定价

### 免费层级
- **包括10 GB存储**
- **每月100万次A类操作**（写入、列出）
- **每月1000万次B类操作**（读取）
- **永远无出口费用！**

### 付费定价（当您超过免费层级时）
- **存储**：每GB每月$0.015（~每TB $15）
- **A类操作**：每百万$4.50（写入）
- **B类操作**：每百万$0.36（读取）
- **出口**：$0（免费！）🎉

### 成本比较

**示例**：提供100 GB图像，每月1 TB下载

| 服务                 | 存储  | 出口    | 总计        |
| -------------------- | ----- | ------- | ----------- |
| AWS S3               | $2.30 | $92.00  | **$94.30**  |
| Google Cloud Storage | $2.00 | $120.00 | **$122.00** |
| Cloudflare R2        | $1.50 | $0.00   | **$1.50**   |

**R2在内容交付方面便宜60-80倍！**

## 入门

### 先决条件
- Cloudflare账户
- 安装Wrangler CLI（`npm install -g wrangler`）
- 对对象存储的基本了解

### 创建R2存储桶

#### 通过Wrangler CLI

```bash
# 创建新存储桶
wrangler r2 bucket create my-images

# 列出所有存储桶
wrangler r2 bucket list

# 删除存储桶（当为空时）
wrangler r2 bucket delete my-bucket-name
```

#### 通过Cloudflare Dashboard

1. 登录[Cloudflare Dashboard](https://dash.cloudflare.com)
2. 从左侧边栏选择**R2**
3. 单击**创建存储桶**
4. 输入存储桶名称（例如，`my-app-storage`）
5. 选择位置（可选）
6. 单击**创建存储桶**

### 存储桶配置

```bash
# 设置CORS策略（如果需要直接浏览器上传）
wrangler r2 bucket cors put my-images --config cors.json
```

示例CORS配置：
```json
{
  "cors_rules": [
    {
      "allowed_origins": ["https://yourdomain.com"],
      "allowed_methods": ["GET", "PUT", "POST", "DELETE"],
      "allowed_headers": ["*"],
      "max_age_seconds": 3600
    }
  ]
}
```

## 与Workers一起使用R2

### 将R2绑定到Worker

在`wrangler.toml`中：
```toml
name = "my-worker"
main = "src/index.ts"

[[r2_buckets]]
binding = "STORAGE"
bucket_name = "my-images"
```

### 基本上传示例

```typescript
interface Env {
  STORAGE: R2Bucket;
}

export default {
  async fetch(request: Request, env: Env): Promise<Response> {
    const url = new URL(request.url);

    // 上传文件
    if (url.pathname === '/upload' && request.method === 'POST') {
      const formData = await request.formData();
      const file = formData.get('file') as File;

      if (!file) {
        return Response.json({ error: 'No file provided' }, { status: 400 });
      }

      // 生成唯一文件名
      const filename = `${Date.now()}-${file.name}`;

      // 上传到R2
      await env.STORAGE.put(filename, file.stream(), {
        httpMetadata: {
          contentType: file.type,
        },
      });

      return Response.json({
        success: true,
        url: `/files/${filename}`,
        filename,
      });
    }

    // 下载文件
    if (url.pathname.startsWith('/files/')) {
      const filename = url.pathname.slice(7);

      const object = await env.STORAGE.get(filename);

      if (!object) {
        return new Response('File not found', { status: 404 });
      }

      const headers = new Headers();
      object.writeHttpMetadata(headers);
      headers.set('etag', object.httpEtag);

      return new Response(object.body, { headers });
    }

    return new Response('Not found', { status: 404 });
  },
};
```

### 带验证的图像上传

```typescript
const ALLOWED_TYPES = ['image/jpeg', 'image/png', 'image/gif', 'image/webp'];
const MAX_SIZE = 5 * 1024 * 1024; // 5MB

export default {
  async fetch(request: Request, env: Env): Promise<Response> {
    if (request.method !== 'POST') {
      return Response.json({ error: 'Method not allowed' }, { status: 405 });
    }

    const formData = await request.formData();
    const file = formData.get('image') as File;

    // 验证文件
    if (!file) {
      return Response.json({ error: 'No file provided' }, { status: 400 });
    }

    if (!ALLOWED_TYPES.includes(file.type)) {
      return Response.json({ error: 'Invalid file type' }, { status: 400 });
    }

    if (file.size > MAX_SIZE) {
      return Response.json({ error: 'File too large' }, { status: 400 });
    }

    // 使用UUID生成文件名
    const extension = file.name.split('.').pop();
    const filename = `${crypto.randomUUID()}.${extension}`;
    const key = `images/${filename}`;

    // 上传到R2
    await env.STORAGE.put(key, file.stream(), {
      httpMetadata: {
        contentType: file.type,
      },
      customMetadata: {
        originalName: file.name,
        uploadedAt: new Date().toISOString(),
      },
    });

    return Response.json({
      success: true,
      url: `https://your-worker.workers.dev/files/${key}`,
      filename,
    });
  },
};
```

### 列出文件

```typescript
export default {
  async fetch(request: Request, env: Env): Promise<Response> {
    const url = new URL(request.url);

    if (url.pathname === '/api/files') {
      // 列出所有对象
      const listed = await env.STORAGE.list({
        limit: 10,
        prefix: 'images/', // 可选：按前缀过滤
      });

      const files = listed.objects.map(obj => ({
        key: obj.key,
        size: obj.size,
        uploaded: obj.uploaded,
      }));

      return Response.json({
        files,
        truncated: listed.truncated,
        cursor: listed.cursor,
      });
    }

    return new Response('Not found', { status: 404 });
  },
};
```

### 删除文件

```typescript
export default {
  async fetch(request: Request, env: Env): Promise<Response> {
    const url = new URL(request.url);

    if (url.pathname === '/api/delete' && request.method === 'DELETE') {
      const { filename } = await request.json();

      // 从R2删除
      await env.STORAGE.delete(filename);

      return Response.json({ success: true });
    }

    return new Response('Not found', { status: 404 });
  },
};
```

## 高级特性

### 分段上传（大文件）

对于大于5MB的文件，使用分段上传：

```typescript
async function uploadLargeFile(file: File, env: Env) {
  const filename = `large-files/${crypto.randomUUID()}-${file.name}`;

  // 创建分段上传
  const multipart = await env.STORAGE.createMultipartUpload(filename);

  const CHUNK_SIZE = 5 * 1024 * 1024; // 5MB块
  const chunks: R2UploadedPart[] = [];

  let offset = 0;
  let partNumber = 1;

  // 分块上传
  while (offset < file.size) {
    const chunk = file.slice(offset, offset + CHUNK_SIZE);
    const buffer = await chunk.arrayBuffer();

    const part = await multipart.uploadPart(partNumber, buffer);
    chunks.push(part);

    offset += CHUNK_SIZE;
    partNumber++;
  }

  // 完成上传
  const object = await multipart.complete(chunks);

  return {
    success: true,
    key: filename,
    size: object.size,
  };
}
```

### 条件请求（缓存）

```typescript
export default {
  async fetch(request: Request, env: Env): Promise<Response> {
    const url = new URL(request.url);
    const filename = url.pathname.slice(1);

    const object = await env.STORAGE.get(filename);

    if (!object) {
      return new Response('Not found', { status: 404 });
    }

    // 检查客户端是否有缓存版本
    const ifNoneMatch = request.headers.get('If-None-Match');
    if (ifNoneMatch === object.httpEtag) {
      return new Response(null, { status: 304 }); // 未修改
    }

    const headers = new Headers();
    object.writeHttpMetadata(headers);
    headers.set('etag', object.httpEtag);
    headers.set('cache-control', 'public, max-age=31536000'); // 1年

    return new Response(object.body, { headers });
  },
};
```

### 自定义元数据

```typescript
// 与文件一起存储元数据
await env.STORAGE.put('user-123/profile.jpg', file.stream(), {
  httpMetadata: {
    contentType: 'image/jpeg',
  },
  customMetadata: {
    userId: '123',
    uploadedBy: 'john@example.com',
    category: 'profile-pictures',
    originalName: file.name,
  },
});

// 检索元数据
const object = await env.STORAGE.head('user-123/profile.jpg');
console.log(object.customMetadata);
```

## 与Pages函数一起使用R2

```typescript
// functions/upload.ts

interface Env {
  STORAGE: R2Bucket;
}

export const onRequestPost: PagesFunction<Env> = async (context) => {
  const formData = await context.request.formData();
  const file = formData.get('file') as File;

  if (!file) {
    return Response.json({ error: 'No file' }, { status: 400 });
  }

  const filename = `${Date.now()}-${file.name}`;

  await context.env.STORAGE.put(filename, file.stream(), {
    httpMetadata: { contentType: file.type },
  });

  return Response.json({ success: true, filename });
};
```

## 公共存储桶访问

### 公共文件的自定义域名

1. 创建存储桶：`my-public-assets`
2. 在R2仪表板中添加自定义域名
3. 配置DNS（如果域名在Cloudflare上则自动配置）
4. 文件可通过以下方式访问：`https://cdn.yourdomain.com/filename.jpg`

### Worker作为CDN

使用Worker通过自定义逻辑提供R2文件：

```typescript
export default {
  async fetch(request: Request, env: Env): Promise<Response> {
    const url = new URL(request.url);
    const key = url.pathname.slice(1);

    // 添加安全检查、分析等
    const object = await env.STORAGE.get(key);

    if (!object) {
      return new Response('Not found', { status: 404 });
    }

    const headers = new Headers();
    object.writeHttpMetadata(headers);

    // 添加自定义头
    headers.set('Cache-Control', 'public, max-age=86400');
    headers.set('CDN-Cache-Control', 'public, max-age=31536000');

    return new Response(object.body, { headers });
  },
};
```

## S3 API兼容性

R2支持S3 API，因此您可以使用AWS SDK：

### 设置S3客户端

```bash
npm install @aws-sdk/client-s3
```

```typescript
import { S3Client, PutObjectCommand } from '@aws-sdk/client-s3';

const S3 = new S3Client({
  region: 'auto',
  endpoint: `https://${accountId}.r2.cloudflarestorage.com`,
  credentials: {
    accessKeyId: 'your-access-key-id',
    secretAccessKey: 'your-secret-access-key',
  },
});

// 上传文件
await S3.send(new PutObjectCommand({
  Bucket: 'my-bucket',
  Key: 'test.jpg',
  Body: fileBuffer,
  ContentType: 'image/jpeg',
}));
```

### 生成访问密钥

1. 转到R2仪表板
2. 单击**管理R2 API令牌**
3. 创建API令牌
4. 保存访问密钥ID和秘密访问密钥

## 确保它工作

### 测试检查清单

1. **测试上传**:
   ```bash
   curl -X POST https://your-worker.workers.dev/upload \
     -F "file=@test-image.jpg"
   ```

2. **测试下载**:
   ```bash
   curl https://your-worker.workers.dev/files/test-image.jpg \
     --output downloaded.jpg
   ```

3. **在仪表板中验证**:
   - 转到Cloudflare仪表板中的R2存储桶
   - 检查文件是否列出
   - 验证大小和元数据

4. **测试删除**:
   ```bash
   curl -X DELETE https://your-worker.workers.dev/api/delete \
     -H "Content-Type: application/json" \
     -d '{"filename": "test-image.jpg"}'
   ```

### 常见问题和解决方案

**上传静默失败：**
- 检查`wrangler.toml`中的存储桶名称
- 验证绑定名称与代码匹配
- 检查文件大小限制

**浏览器中的CORS错误：**
- 在存储桶上配置CORS策略
- 添加适当的`Access-Control-Allow-*`头

**文件无法访问：**
- 检查存储桶是否公开或需要Worker
- 验证自定义域名配置
- 检查防火墙/安全设置

## 高效使用技巧

### 1. 使用文件夹/前缀

```typescript
// 逻辑组织文件
await env.STORAGE.put('users/123/avatar.jpg', file);
await env.STORAGE.put('products/456/image-1.jpg', file);
await env.STORAGE.put('documents/2024/report.pdf', file);

// 按类别轻松列出
const userFiles = await env.STORAGE.list({ prefix: 'users/123/' });
```

### 2. 实现缓存

```typescript
// 为静态资产添加缓存头
headers.set('Cache-Control', 'public, max-age=31536000, immutable');
headers.set('CDN-Cache-Control', 'public, max-age=31536000');
```

### 3. 上传时优化图像

```typescript
// 使用Workers调整大小/优化图像
import { Image } from '@cloudflare/workers-image';

const image = await Image.load(await file.arrayBuffer());
const optimized = await image
  .resize(1200) // 最大宽度
  .quality(85)
  .toBuffer('jpeg');

await env.STORAGE.put(filename, optimized);
```

### 4. 在D1中存储元数据

```typescript
// 在数据库中存储文件元数据以快速查询
await env.DB.prepare(`
  INSERT INTO files (key, size, type, uploaded_by, created_at)
  VALUES (?, ?, ?, ?)
`).bind(filename, file.size, file.type, userId, Date.now()).run();

// 无需列出R2即可查询文件
const files = await env.DB.prepare(`
  SELECT * FROM files WHERE uploaded_by = ? ORDER BY created_at DESC
`).bind(userId).all();
```

### 5. 实现签名URL

```typescript
// 生成临时访问URL
function generateSignedUrl(key: string, expiresIn: number): string {
  const expires = Date.now() + expiresIn;
  const signature = await crypto.subtle.sign(
    'HMAC',
    await getSigningKey(),
    new TextEncoder().encode(`${key}:${expires}`)
  );

  return `/files/${key}?expires=${expires}&sig=${btoa(String.fromCharCode(...new Uint8Array(signature)))}`;
}
```

## 真实世界用例

### 1. 用户头像存储

```typescript
async function uploadAvatar(file: File, userId: string, env: Env) {
  const key = `avatars/${userId}.jpg`;

  await env.STORAGE.put(key, file.stream(), {
    httpMetadata: { contentType: 'image/jpeg' },
    customMetadata: { userId, uploadedAt: new Date().toISOString() },
  });

  await env.DB.prepare(
    'UPDATE users SET avatar_url = ? WHERE id = ?'
  ).bind(`/avatars/${userId}.jpg`, userId).run();

  return { success: true, url: `/avatars/${userId}.jpg` };
}
```

### 2. 文档管理系统

```typescript
async function uploadDocument(file: File, folder: string, env: Env) {
  const filename = `${crypto.randomUUID()}.${file.name.split('.').pop()}`;
  const key = `documents/${folder}/${filename}`;

  await env.STORAGE.put(key, file.stream(), {
    httpMetadata: { contentType: file.type },
    customMetadata: {
      originalName: file.name,
      folder,
      uploadedAt: new Date().toISOString(),
    },
  });

  return { key, url: `/files/${key}` };
}
```

### 3. 视频平台存储

```typescript
async function uploadVideo(file: File, env: Env) {
  // 对大视频使用分段上传
  const key = `videos/${crypto.randomUUID()}.mp4`;

  const multipart = await env.STORAGE.createMultipartUpload(key);

  // 分块上传（上面的分段示例中的实现）
  // ...

  return { key, url: `/videos/${key}` };
}
```

## 与其他服务集成

### 与Workers
- 通过自定义CDN逻辑提供文件
- 添加身份验证/授权
- 实现使用情况跟踪

### 与Pages
- 存储前端的用户上传
- 提供静态资产
- 处理带文件上传的表单提交

### 与D1
- 存储用于查询的文件元数据
- 跟踪文件使用情况和分析
- 实现文件权限

### 与KV
- 缓存频繁访问的文件元数据
- 存储临时上传令牌
- 实现速率限制

## 官方资源

- **文档**: https://developers.cloudflare.com/r2/
- **API参考**: https://developers.cloudflare.com/r2/api/
- **定价**: https://developers.cloudflare.com/r2/pricing/
- **示例**: https://developers.cloudflare.com/r2/examples/
- **Discord社区**: https://discord.cloudflare.com

## 为什么R2适合现代应用

1. **无出口费用** - 节省90%的带宽成本
2. **S3兼容** - 从AWS轻松迁移
3. **全球快速** - Cloudflare的网络
4. **慷慨的免费层级** - 10GB免费
5. **Worker集成** - 边缘的自定义逻辑
6. **简单定价** - 无意外账单
7. **可靠存储** - 企业级基础设施

对于任何需要存储和提供文件的应用程序 - 图像、视频、文档、备份 - Cloudflare R2提供无与伦比的价值和性能。
{
  "cors_rules": [
    {
      "allowed_origins": ["https://yourdomain.com"],
      "allowed_methods": ["GET", "PUT", "POST", "DELETE"],
      "allowed_headers": ["*"],
      "max_age_seconds": 3600
    }
  ]
}
```

## 在Workers中使用R2

### 将R2绑定到Worker

在`wrangler.toml`中：
```toml
name = "my-worker"
main = "src/index.ts"

[[r2_buckets]]
binding = "STORAGE"
bucket_name = "my-images"
```

### 基本上传示例

```typescript
interface Env {
  STORAGE: R2Bucket;
}

export default {
  async fetch(request: Request, env: Env): Promise<Response> {
    const url = new URL(request.url);

    // 上传文件
    if (url.pathname === '/upload' && request.method === 'POST') {
      const formData = await request.formData();
      const file = formData.get('file') as File;

      if (!file) {
        return Response.json({ error: 'No file provided' }, { status: 400 });
      }

      // 生成唯一文件名
      const filename = `${Date.now()}-${file.name}`;

      // 上传到R2
      await env.STORAGE.put(filename, file.stream(), {
        httpMetadata: {
          contentType: file.type,
        },
      });

      return Response.json({
        success: true,
        url: `/files/${filename}`,
        filename,
      });
    }

    // 下载文件
    if (url.pathname.startsWith('/files/')) {
      const filename = url.pathname.slice(7);

      const object = await env.STORAGE.get(filename);

      if (!object) {
        return new Response('File not found', { status: 404 });
      }

      const headers = new Headers();
      object.writeHttpMetadata(headers);
      headers.set('etag', object.httpEtag);

      return new Response(object.body, { headers });
    }

    return new Response('Not found', { status: 404 });
  },
};
```

### 带验证的图像上传

```typescript
const ALLOWED_TYPES = ['image/jpeg', 'image/png', 'image/gif', 'image/webp'];
const MAX_SIZE = 5 * 1024 * 1024; // 5MB

export default {
  async fetch(request: Request, env: Env): Promise<Response> {
    if (request.method !== 'POST') {
      return Response.json({ error: 'Method not allowed' }, { status: 405 });
    }

    const formData = await request.formData();
    const file = formData.get('image') as File;

    // 验证文件
    if (!file) {
      return Response.json({ error: 'No file provided' }, { status: 400 });
    }

    if (!ALLOWED_TYPES.includes(file.type)) {
      return Response.json({ error: 'Invalid file type' }, { status: 400 });
    }

    if (file.size > MAX_SIZE) {
      return Response.json({ error: 'File too large' }, { status: 400 });
    }

    // 使用UUID生成文件名
    const extension = file.name.split('.').pop();
    const filename = `${crypto.randomUUID()}.${extension}`;
    const key = `images/${filename}`;

    // 上传到R2
    await env.STORAGE.put(key, file.stream(), {
      httpMetadata: {
        contentType: file.type,
      },
      customMetadata: {
        originalName: file.name,
        uploadedAt: new Date().toISOString(),
      },
    });

    return Response.json({
      success: true,
      url: `https://your-worker.workers.dev/files/${key}`,
      filename,
    });
  },
};
```

### 列出文件

```typescript
export default {
  async fetch(request: Request, env: Env): Promise<Response> {
    const url = new URL(request.url);

    if (url.pathname === '/api/files') {
      // 列出所有对象
      const listed = await env.STORAGE.list({
        limit: 100,
        prefix: 'images/', // 可选：按前缀过滤
      });

      const files = listed.objects.map(obj => ({
        key: obj.key,
        size: obj.size,
        uploaded: obj.uploaded,
      }));

      return Response.json({
        files,
        truncated: listed.truncated,
        cursor: listed.cursor,
      });
    }

    return new Response('Not found', { status: 404 });
  },
};
```

### 删除文件

```typescript
export default {
  async fetch(request: Request, env: Env): Promise<Response> {
    const url = new URL(request.url);

    if (url.pathname === '/api/delete' && request.method === 'DELETE') {
      const { filename } = await request.json();

      // 从R2删除
      await env.STORAGE.delete(filename);

      return Response.json({ success: true });
    }

    return new Response('Not found', { status: 404 });
  },
};
```

## 高级功能

### 多部分上传（大文件）

对于大于5MB的文件，使用多部分上传：

```typescript
async function uploadLargeFile(file: File, env: Env) {
  const filename = `large-files/${crypto.randomUUID()}-${file.name}`;

  // 创建多部分上传
  const multipart = await env.STORAGE.createMultipartUpload(filename);

  const CHUNK_SIZE = 5 * 1024 * 1024; // 5MB块
  const chunks: R2UploadedPart[] = [];

  let offset = 0;
  let partNumber = 1;

  // 分块上传
  while (offset < file.size) {
    const chunk = file.slice(offset, offset + CHUNK_SIZE);
    const buffer = await chunk.arrayBuffer();

    const part = await multipart.uploadPart(partNumber, buffer);
    chunks.push(part);

    offset += CHUNK_SIZE;
    partNumber++;
  }

  // 完成上传
  const object = await multipart.complete(chunks);

  return {
    success: true,
    key: filename,
    size: object.size,
  };
}
```

### 条件请求（缓存）

```typescript
export default {
  async fetch(request: Request, env: Env): Promise<Response> {
    const url = new URL(request.url);
    const filename = url.pathname.slice(1);

    const object = await env.STORAGE.get(filename);

    if (!object) {
      return new Response('Not found', { status: 404 });
    }

    // 检查客户端是否有缓存版本
    const ifNoneMatch = request.headers.get('If-None-Match');
    if (ifNoneMatch === object.httpEtag) {
      return new Response(null, { status: 304 }); // Not Modified
    }

    const headers = new Headers();
    object.writeHttpMetadata(headers);
    headers.set('etag', object.httpEtag);
    headers.set('cache-control', 'public, max-age=31536000'); // 1年

    return new Response(object.body, { headers });
  },
};
```

### 自定义元数据

```typescript
// 使用文件存储元数据
await env.STORAGE.put('user-123/profile.jpg', file.stream(), {
  httpMetadata: {
    contentType: 'image/jpeg',
  },
  customMetadata: {
    userId: '123',
    uploadedBy: 'john@example.com',
    category: 'profile-pictures',
    originalName: file.name,
  },
});

// 检索元数据
const object = await env.STORAGE.head('user-123/profile.jpg');
console.log(object.customMetadata);
```

## 在Pages Functions中使用R2

```typescript
// functions/upload.ts

interface Env {
  STORAGE: R2Bucket;
}

export const onRequestPost: PagesFunction<Env> = async (context) => {
  const formData = await context.request.formData();
  const file = formData.get('file') as File;

  if (!file) {
    return Response.json({ error: 'No file' }, { status: 400 });
  }

  const filename = `${Date.now()}-${file.name}`;

  await context.env.STORAGE.put(filename, file.stream(), {
    httpMetadata: { contentType: file.type },
  });

  return Response.json({ success: true, filename });
};
```

## 公共存储桶访问

### 公共文件的自定义域名

1. 创建存储桶：`my-public-assets`
2. 在R2仪表板中添加自定义域名
3. 配置DNS（如果域名在Cloudflare上则自动）
4. 文件可访问于：`https://cdn.yourdomain.com/filename.jpg`

### Worker作为CDN

使用Worker通过自定义逻辑提供R2文件：

```typescript
export default {
  async fetch(request: Request, env: Env): Promise<Response> {
    const url = new URL(request.url);
    const key = url.pathname.slice(1);

    // 添加安全检查、分析等
    const object = await env.STORAGE.get(key);

    if (!object) {
      return new Response('Not found', { status: 404 });
    }

    const headers = new Headers();
    object.writeHttpMetadata(headers);

    // 添加自定义头
    headers.set('Cache-Control', 'public, max-age=86400');
    headers.set('CDN-Cache-Control', 'public, max-age=31536000');

    return new Response(object.body, { headers });
  },
};
```

## S3 API兼容性

R2支持S3 API，因此您可以使用AWS SDK：

### 设置S3客户端

```bash
npm install @aws-sdk/client-s3
```

```typescript
import { S3Client, PutObjectCommand } from '@aws-sdk/client-s3';

const S3 = new S3Client({
  region: 'auto',
  endpoint: `https://${accountId}.r2.cloudflarestorage.com`,
  credentials: {
    accessKeyId: 'your-access-key-id',
    secretAccessKey: 'your-secret-access-key',
  },
});

// 上传文件
await S3.send(new PutObjectCommand({
  Bucket: 'my-bucket',
  Key: 'test.jpg',
  Body: fileBuffer,
  ContentType: 'image/jpeg',
}));
```

### 生成访问密钥

1. 转到R2仪表板
2. 单击**Manage R2 API Tokens**
3. 创建API令牌
4. 保存访问密钥ID和秘密访问密钥

## 确保它工作

### 测试清单

1. **测试上传**：
   ```bash
   curl -X POST https://your-worker.workers.dev/upload \
     -F "file=@test-image.jpg"
   ```

2. **测试下载**：
   ```bash
   curl https://your-worker.workers.dev/files/test-image.jpg \
     --output downloaded.jpg
   ```

3. **在仪表板中验证**：
   - 转到Cloudflare仪表板中的R2存储桶
   - 检查文件已列出
   - 验证大小和元数据

4. **测试删除**：
   ```bash
   curl -X DELETE https://your-worker.workers.dev/api/delete \
     -H "Content-Type: application/json" \
     -d '{"filename": "test-image.jpg"}'
   ```

### 常见问题和解决方案

**上传静默失败：**
- 检查`wrangler.toml`中的存储桶名称
- 验证绑定名称与代码匹配
- 检查文件大小限制

**浏览器中的CORS错误：**
- 在存储桶上配置CORS策略
- 添加适当的`Access-Control-Allow-*`头

**文件无法访问：**
- 检查存储桶是公共的还是需要Worker
- 验证自定义域名配置
- 检查防火墙/安全设置

## 高效使用提示

### 1. 使用文件夹/前缀

```typescript
// 逻辑组织文件
await env.STORAGE.put('users/123/avatar.jpg', file);
await env.STORAGE.put('products/456/image-1.jpg', file);
await env.STORAGE.put('documents/2024/report.pdf', file);

// 按类别轻松列出
const userFiles = await env.STORAGE.list({ prefix: 'users/123/' });
```

### 2. 实施缓存

```typescript
// 为静态资产添加缓存头
headers.set('Cache-Control', 'public, max-age=31536000, immutable');
headers.set('CDN-Cache-Control', 'public, max-age=31536000');
```

### 3. 上传时优化图像

```typescript
// 使用Workers调整大小/优化图像
import { Image } from '@cloudflare/workers-image';

const image = await Image.load(await file.arrayBuffer());
const optimized = await image
  .resize(1200) // 最大宽度
  .quality(85)
  .toBuffer('jpeg');

await env.STORAGE.put(filename, optimized);
```

### 4. 在D1中存储元数据

```typescript
// 在数据库中存储文件元数据以进行快速查询
await env.DB.prepare(`
  INSERT INTO files (key, size, type, uploaded_by, created_at)
  VALUES (?, ?, ?, ?, ?)
`).bind(filename, file.size, file.type, userId, Date.now()).run();

// 无需列出R2即可查询文件
const files = await env.DB.prepare(`
  SELECT * FROM files WHERE uploaded_by = ? ORDER BY created_at DESC
`).bind(userId).all();
```

### 5. 实施签名URL

```typescript
// 生成临时访问URL
function generateSignedUrl(key: string, expiresIn: number): string {
  const expires = Date.now() + expiresIn;
  const signature = await crypto.subtle.sign(
    'HMAC',
    await getSigningKey(),
    new TextEncoder().encode(`${key}:${expires}`)
  );

  return `/files/${key}?expires=${expires}&sig=${btoa(String.fromCharCode(...new Uint8Array(signature)))}`;
}
```

## 真实世界用例

### 1. 用户头像存储

```typescript
async function uploadAvatar(file: File, userId: string, env: Env) {
  const key = `avatars/${userId}.jpg`;

  await env.STORAGE.put(key, file.stream(), {
    httpMetadata: { contentType: 'image/jpeg' },
    customMetadata: { userId, uploadedAt: new Date().toISOString() },
  });

  await env.DB.prepare(
    'UPDATE users SET avatar_url = ? WHERE id = ?'
  ).bind(`/avatars/${userId}.jpg`, userId).run();

  return { success: true, url: `/avatars/${userId}.jpg` };
}
```

### 2. 文档管理系统

```typescript
async function uploadDocument(file: File, folder: string, env: Env) {
  const filename = `${crypto.randomUUID()}.${file.name.split('.').pop()}`;
  const key = `documents/${folder}/${filename}`;

  await env.STORAGE.put(key, file.stream(), {
    httpMetadata: { contentType: file.type },
    customMetadata: {
      originalName: file.name,
      folder,
      uploadedAt: new Date().toISOString(),
    },
  });

  return { key, url: `/files/${key}` };
}
```

### 3. 视频平台存储

```typescript
async function uploadVideo(file: File, env: Env) {
  // 对大视频使用多部分上传
  const key = `videos/${crypto.randomUUID()}.mp4`;

  const multipart = await env.STORAGE.createMultipartUpload(key);

  // 分块上传（来自上面的多部分示例的实现）
  // ...

  return { key, url: `/videos/${key}` };
}
```

## 与其他服务的集成

### 与Workers
- 通过自定义CDN逻辑提供文件
- 添加身份验证/授权
- 实施使用跟踪

### 与Pages
- 从前端存储用户上传
- 提供静态资产
- 处理带有文件上传的表单提交

### 与D1
- 存储文件元数据以进行查询
- 跟踪文件使用和分析
- 实施文件权限

### 与KV
- 缓存频繁访问的文件元数据
- 存储临时上传令牌
- 实施速率限制

## 官方资源

- **文档**：https://developers.cloudflare.com/r2/
- **API参考**：https://developers.cloudflare.com/r2/api/
- **定价**：https://developers.cloudflare.com/r2/pricing/
- **示例**：https://developers.cloudflare.com/r2/examples/
- **Discord社区**：https://discord.cloudflare.com

## 为什么R2非常适合现代应用程序

1. **无出口费用** - 在带宽成本上节省90%
2. **S3兼容** - 从AWS轻松迁移
3. **全球快速** - Cloudflare的网络
4. **慷慨的免费层级** - 10GB免费
5. **Worker集成** - 边缘的自定义逻辑
6. **简单定价** - 无意外账单
7. **可靠存储** - 企业级基础设施

对于任何需要存储和提供文件的应用程序——图像、视频、文档、备份——Cloudflare R2提供无与伦比的价值和性能。