# 性能标准

## 概述

Web性能直接影响用户体验、SEO排名和转化率。本指南专注于Google的Core Web Vitals，Google用作排名因素的官方指标。

**为什么性能很重要：**
- **用户体验** - 快速的网站感觉专业和值得信赖
- **SEO排名** - Core Web Vitals是官方Google排名因素（自2021年6月起）
- **转化** - 亚马逊发现每100ms延迟损失1%收入
- **跳出率** - 53%的移动用户放弃加载超过3秒的网站
- **移动用户** - 性能在较慢连接上甚至更关键

**性能目标：**
- **Core Web Vitals**：所有三个指标都是绿色
- **PageSpeed Insights分数**：90+（桌面），80+（移动）
- **Lighthouse性能分数**：90+
- **可交互时间**：< 3.8秒
- **总页面大小**：< 2 MB

## Core Web Vitals

Google的Core Web Vitals是三个特定指标，衡量加载、交互性和视觉稳定性。这些是官方排名因素。

### LCP（最大内容绘制）

**它衡量什么**：加载性能 - 主内容可见需要多长时间。

**具体**：视口中最大图像或文本块渲染的时间。

**目标**：
- **良好**：< 2.5秒（绿色）
- **需要改进**：2.5 - 4.0秒（黄色）
- **糟糕**：> 4.0秒（红色）

**什么算作LCP元素：**
- `<img>`元素
- `<svg>`内的`<image>`元素
- `<video>`元素（海报图像）
- 通过`url()`加载的背景图像
- 包含文本节点的块级元素

**常见LCP元素：**
- 英雄图像
- 横幅图像
- 大文本块（H1）
- 视频缩略图
- 全宽内容图像

#### LCP优化策略

**1. 优化图像（最大影响）**
```html
<!-- ✅ 优化的英雄图像 -->
<img
  src="hero-image.webp"
  alt="专业管道服务"
  width="1200"
  height="600"
  fetchpriority="high"
  decoding="async"
>
```

**关键技术：**
- **格式**：使用WebP（比JPG小20-30%）
- **压缩**：压缩图像（英雄图像目标为100-200 KB）
- **尺寸**：提供正确大小的图像（不要缩小）
- **获取优先级**：向LCP图像添加`fetchpriority="high"`
- **预加载**：如果LCP图像不在初始HTML中，则预加载

```html
<!-- 预加载LCP图像（如果通过CSS加载或延迟加载） -->
<link rel="preload" as="image" href="hero-image.webp" fetchpriority="high">
```

**2. 移除渲染阻塞资源**
```html
<!-- ❌ 渲染阻塞CSS -->
<link rel="stylesheet" href="styles.css">

<!-- ✅ 内联关键CSS，延迟非关键CSS -->
<style>
  /* 关键首屏CSS内联在此 */
  .hero { ... }
  .nav { ... }
</style>

<!-- 异步加载非关键CSS -->
<link rel="preload" href="non-critical.css" as="style" onload="this.onload=null;this.rel='stylesheet'">
<noscript><link rel="stylesheet" href="non-critical.css"></noscript>
```

**3. 最小化JavaScript执行**
```html
<!-- ❌ 渲染阻塞JavaScript -->
<script src="app.js"></script>

<!-- ✅ 延迟JavaScript -->
<script src="app.js" defer></script>