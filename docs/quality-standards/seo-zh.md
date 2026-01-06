# SEO标准和最佳实践

## 概述

本指南根据Google的最新标准和最佳实践涵盖全面的SEO（搜索引擎优化）。SEO对于依赖有机搜索流量寻找客户的小企业客户至关重要。

**为什么SEO对您的客户很重要：**
- **免费流量** - 有机搜索是大多数小企业的第一大流量来源
- **高意向潜在客户** - 搜索的人有购买意向
- **长期ROI** - SEO随时间复合，不像付费广告
- **本地发现** - "附近"搜索为实体企业带来客流
- **竞争优势** - 许多本地企业的SEO很差

**SEO基础：**
- **页面SEO** - 内容、HTML、meta标签（本指南）
- **技术SEO** - 网站速度、移动友好性、结构
- **页面外SEO** - 反向链接、引用、评论（客户的责任）
- **本地SEO** - Google Business Profile、本地关键词、NAP一致性

## HTML Meta标签

Meta标签向搜索引擎和社交媒体平台提供有关您页面的信息。

### 基本Meta标签

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <!-- 字符编码（必需） -->
  <meta charset="UTF-8">

  <!-- 视口（移动设备必需） -->
  <meta name="viewport" content="width=device-width, initial-scale=1.0">

  <!-- 标题（最重要的meta标签） -->
  <title>Professional Plumbing Services in Austin, TX | Smith Plumbing</title>

  <!-- Meta描述（出现在搜索结果中） -->
  <meta name="description" content="Smith Plumbing provides 24/7 emergency plumbing services in Austin, TX. Licensed, insured, and highly rated. Call (512) 555-0100 for fast, reliable service.">

  <!-- 关键词（现在不太重要，但无害） -->
  <meta name="keywords" content="plumber austin, emergency plumbing, water heater repair, drain cleaning, austin plumber">

  <!-- 规范URL（防止重复内容） -->
  <link rel="canonical" href="https://www.smithplumbing.com/">

  <!-- Robots（控制索引） -->
  <meta name="robots" content="index, follow">

  <!-- 语言 -->
  <meta http-equiv="content-language" content="en-US">

  <!-- 作者 -->
  <meta name="author" content="Smith Plumbing">

  <!-- 主题颜色（移动浏览器） -->
  <meta name="theme-color" content="#0066cc">
</head>
<body>
  ...
</body>
</html>
```

### 标题标签最佳实践

**格式**：`主要关键词 - 次要关键词 | 品牌名称`

**规则**：
- **长度**：50-60个字符（更长会被截断）
- **关键词**：在开头附近包含主要关键词
- **唯一**：每个页面应该有唯一的标题
- **品牌**：在末尾包含品牌名称（或主页在开头）
- **引人注目**：为用户编写，而不仅仅是搜索引擎

**示例**：
```html
<!-- 主页 -->
<title>Smith Plumbing - 24/7 Emergency Plumber in Austin, TX</title>

<!-- 服务页面 -->
<title>Water Heater Repair & Installation Austin | Smith Plumbing</title>

<!-- 位置页面 -->
<title>Plumber in South Austin, TX - Fast & Reliable | Smith Plumbing</title>

<!-- 博客文章 -->
<title>How to Fix a Leaky Faucet: Step-by-Step Guide | Smith Plumbing</title>

<!-- 联系页面 -->
<title>Contact Smith Plumbing - Get a Free Quote Today</title>
```

**糟糕的示例**：
```html
<!-- ❌ 太长（90+个字符） -->
<title>Professional Plumbing Services Including Water Heater Repair, Drain Cleaning, and Emergency Plumbing in Austin, TX | Smith Plumbing</title>

<!-- ❌ 关键词堆砌