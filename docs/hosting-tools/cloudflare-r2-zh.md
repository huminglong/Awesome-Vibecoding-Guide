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