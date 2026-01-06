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