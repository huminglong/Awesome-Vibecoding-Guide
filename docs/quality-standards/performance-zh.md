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

<!-- ✅ 或异步（如果顺序不重要） -->
<script src="analytics.js" async></script>
```

**4. 优化服务器响应时间**
- **目标**：< 600ms TTFB（首字节时间）
- **使用CDN**：Cloudflare Pages从边缘位置提供服务
- **静态生成**：Astro预渲染HTML（无服务器处理）
- **缓存**：设置正确的缓存头

**5. 避免延迟加载LCP元素**
```html
<!-- ❌ 不要延迟加载英雄图像 -->
<img src="hero.jpg" loading="lazy" alt="Hero">

<!-- ✅ 英雄图像立即加载 -->
<img src="hero.jpg" alt="Hero" fetchpriority="high">

<!-- ✅ 首屏以下图像延迟加载 -->
<img src="below-fold.jpg" loading="lazy" alt="Content">
```

#### LCP常见问题和修复

**问题：图像文件大小过大**
- **修复**：压缩图像，使用WebP格式
- **工具**：Squoosh.app、ImageOptim、TinyPNG

**问题：服务器响应慢**
- **修复**：使用CDN（Cloudflare）、静态站点生成（Astro）
- **测试**：WebPageTest TTFB

**问题：渲染阻塞CSS/JS**
- **修复**：内联关键CSS，延迟JavaScript
- **工具**：Chrome DevTools覆盖率选项卡

**问题：客户端渲染**
- **修复**：使用SSG（静态站点生成）而不是CSR
- **Astro优势**：默认静态生成

### FID/INP（首次输入延迟/交互到下次绘制）

**FID（2024年3月已弃用，由INP取代）：**
- **衡量内容**：从首次用户交互到浏览器响应的时间
- **目标**：< 100ms

**INP（2024年3月起新指标）：**
- **衡量内容**：响应性 - 页面对所有用户交互的响应速度
- **具体**：整个页面生命周期中最坏情况下的交互延迟

**目标：**
- **良好**：< 200ms（绿色）
- **需要改进**：200 - 500ms（黄色）
- **糟糕**：> 500ms（红色）

**什么算作交互：**
- 点击
- 轻触
- 按键

**INP衡量内容：**
- 输入延迟（开始处理的时间）
- 处理时间（运行事件处理程序）
- 呈现延迟（渲染更新）

#### INP优化策略

**1. 最小化JavaScript执行**
```javascript
// ❌ 重量级同步处理
function handleClick() {
  const data = processLargeDataset(); // 阻塞主线程
  updateUI(data);
}

// ✅ 分解长任务
async function handleClick() {
  const data = await processInChunks();
  requestIdleCallback(() => updateUI(data));
}

// ✅ 使用Web Workers进行重量级计算
const worker = new Worker('process-worker.js');
worker.postMessage(largeDataset);
worker.onmessage = (e) => updateUI(e.data);
```

**2. 防抖昂贵操作**
```javascript
// ❌ 每次按键都运行
input.addEventListener('input', (e) => {
  expensiveSearchFunction(e.target.value);
});

// ✅ 防抖以减少调用
import { debounce } from 'lodash-es';

const debouncedSearch = debounce((value) => {
  expensiveSearchFunction(value);
}, 300);

input.addEventListener('input', (e) => {
  debouncedSearch(e.target.value);
});
```

**3. 优化事件处理程序**
```javascript
// ❌ 事件处理程序中的重量级工作
button.addEventListener('click', () => {
  // 500ms的同步工作
  performComplexCalculation();
  updateMultipleElements();
  triggerAnimations();
});

// ✅ 优化并分解工作
button.addEventListener('click', () => {
  // 立即反馈
  button.classList.add('loading');

  // 延迟重量级工作
  requestIdleCallback(() => {
    performComplexCalculation();
  });

  // 批量DOM更新
  requestAnimationFrame(() => {
    updateMultipleElements();
    triggerAnimations();
  });
});
```

**4. 代码分割**
```javascript
// ❌ 预先加载所有内容
import { heavyLibrary } from 'heavy-library';

// ✅ 需要时动态导入
button.addEventListener('click', async () => {
  const { heavyLibrary } = await import('heavy-library');
  heavyLibrary.doSomething();
});
```

**5. 减少第三方脚本影响**
```html
<!-- ❌ 同步第三方脚本 -->
<script src="https://example.com/widget.js"></script>

<!-- ✅ 异步加载 -->
<script src="https://example.com/widget.js" async></script>

<!-- ✅ 或延迟 -->
<script src="https://example.com/widget.js" defer></script>

<!-- ✅ 交互时加载（更好） -->
<script>
  button.addEventListener('click', () => {
    const script = document.createElement('script');
    script.src = 'https://example.com/widget.js';
    document.head.appendChild(script);
  }, { once: true });
</script>
```

#### INP常见问题和修复

**问题：大型JavaScript包**
- **修复**：代码分割、树摇、移除未使用代码
- **工具**：webpack-bundle-analyzer、Chrome DevTools覆盖率

**问题：长任务（> 50ms）**
- **修复**：分解为较小任务，使用requestIdleCallback
- **工具**：Chrome DevTools性能选项卡

**问题：第三方脚本阻塞主线程**
- **修复**：异步加载、延迟或用户交互时加载
- **工具**：Chrome DevTools性能选项卡

**问题：重量级事件处理程序**
- **修复**：防抖、节流或优化逻辑
- **工具**：Chrome DevTools性能分析

### CLS（累积布局偏移）

**它衡量什么**：视觉稳定性 - 加载过程中内容意外移动的程度。

**具体**：整个页面生命周期中所有意外布局偏移分数的总和。

**目标：**
- **良好**：< 0.1（绿色）
- **需要改进**：0.1 - 0.25（黄色）
- **糟糕**：> 0.25（红色）

**什么导致布局偏移：**
- 没有尺寸的图像
- 没有尺寸的广告/嵌入/iframe
- 动态注入的内容
- 导致FOIT/FOUT的Web字体
- 移动现有内容的动画

#### CLS优化策略

**1. 始终指定图像尺寸**
```html
<!-- ❌ 没有尺寸（图像加载时导致布局偏移） -->
<img src="photo.jpg" alt="Photo">

<!-- ✅ 明确的宽度/高度（保留空间） -->
<img
  src="photo.jpg"
  alt="Photo"
  width="800"
  height="600"
  loading="lazy"
>

<!-- ✅ 使用CSS的宽高比（响应式） -->
<img
  src="photo.jpg"
  alt="Photo"
  style="aspect-ratio: 16/9; width: 100%; height: auto;"
>
```

**2. 为广告/嵌入保留空间**
```html
<!-- ❌ 没有保留空间 -->
<div id="ad"></div>

<!-- ✅ 使用最小高度保留空间 -->
<div id="ad" style="min-height: 250px;">
  <!-- 广告在此加载 -->
</div>

<!-- ✅ 或明确尺寸 -->
<div id="ad" style="width: 300px; height: 250px;">
  <!-- 广告在此加载 -->
</div>
```

**3. 避免在现有内容上方插入内容**
```javascript
// ❌ 在上方插入会向下推内容
header.insertAdjacentHTML('afterend', '<div class="banner">...</div>');

// ✅ 在末尾插入或使用固定/粘性定位
document.body.insertAdjacentHTML('beforeend', '<div class="banner">...</div>');

// ✅ 或绝对/固定定位（不影响布局）
<div class="banner" style="position: fixed; top: 0; left: 0; right: 0;">
  ...
</div>
```

**4. 优化Web字体加载**
```css
/* ❌ FOUT（无样式文本闪烁）导致偏移 */
@font-face {
  font-family: 'Custom';
  src: url('custom.woff2');
}

/* ✅ 使用font-display: optional（防止偏移） */
@font-face {
  font-family: 'Custom';
  src: url('custom.woff2');
  font-display: optional; /* 无偏移，如果未快速加载则使用回退 */
}

/* ✅ 或font-display: swap与size-adjust */
@font-face {
  font-family: 'Custom';
  src: url('custom.woff2');
  font-display: swap;
  size-adjust: 100%; /* 匹配回退字体大小 */
}
```

**5. 预加载字体以更早加载**
```html
<link
  rel="preload"
  href="/fonts/custom-font.woff2"
  as="font"
  type="font/woff2"
  crossorigin
>
```

**6. 使用CSS变换进行动画**
```css
/* ❌ 动画位置属性导致布局偏移 */
.box {
  animation: moveDown 1s;
}

@keyframes moveDown {
  from { top: 0; }
  to { top: 100px; }
}

/* ✅ 使用transform（不影响布局） */
.box {
  animation: moveDown 1s;
}

@keyframes moveDown {
  from { transform: translateY(0); }
  to { transform: translateY(100px); }
}
```

**7. 为动态内容保留空间**
```html
<!-- ❌ 内容出现并移动布局 -->
<div id="dynamic-content"></div>

<!-- ✅ 骨架屏/占位符保留空间 -->
<div id="dynamic-content" class="skeleton" style="min-height: 200px;">
  <!-- 加载骨架屏显示直到内容加载 -->
</div>

<style>
  .skeleton {
    background: linear-gradient(90deg, #f0f0f0 25%, #e0e0e0 50%, #f0f0f0 75%);
    background-size: 200% 100%;
    animation: loading 1.5s infinite;
  }

  @keyframes loading {
    0% { background-position: 200% 0; }
    100% { background-position: -200% 0; }
  }
</style>
```

#### CLS常见问题和修复

**问题：没有尺寸的图像**
- **修复**：添加宽度/高度属性或宽高比CSS
- **工具**：Lighthouse、Chrome DevTools布局偏移区域

**问题：Web字体导致FOUT/FOIT**
- **修复**：使用font-display: optional或swap，预加载字体
- **工具**：Chrome DevTools网络选项卡

**问题：广告/第三方嵌入**
- **修复**：使用最小高度保留空间
- **工具**：Lighthouse

**问题：动态内容注入**
- **修复**：使用骨架屏占位符保留空间
- **工具**：Chrome DevTools布局偏移区域

## 图像优化

图像通常占总页面权重的50-70%。优化至关重要。

### 格式选择

**现代格式（最佳）：**
- **WebP** - 比JPG小25-35%，支持透明度
- **AVIF** - 比JPG小50%，但编码较慢且浏览器支持有限

**传统格式：**
- **JPG** - 照片、复杂图像
- **PNG** - 需要透明度、简单图形
- **SVG** - 徽标、图标、简单插图

**格式决策树：**
1. 徽标/图标/插图？ → **SVG**
2. 照片/复杂图像？ → **WebP**与JPG回退
3. 需要透明度？ → **PNG**或**WebP**
4. 动画？ → **GIF**（或视频以获得更好压缩）

**使用现代格式与回退：**
```html
<picture>
  <source srcset="image.avif" type="image/avif">
  <source srcset="image.webp" type="image/webp">
  <img src="image.jpg" alt="Description" width="800" height="600">
</picture>
```

### 压缩

**工具：**
- **Squoosh** - https://squoosh.app（在线，视觉比较）
- **ImageOptim** - Mac应用（无损/有损压缩）
- **TinyPNG** - https://tinypng.com（在线，有损PNG/JPG）
- **Sharp** - Node.js库（自动压缩）

**压缩目标：**
- **英雄图像**：100-200 KB
- **内容图像**：50-100 KB
- **缩略图**：10-30 KB
- **图标（PNG）**：5-10 KB（或使用SVG）

**质量设置：**
- **JPG**：75-85质量（最佳点）
- **WebP**：75-80质量
- **PNG**：使用工具优化（无损压缩）

### 响应式图像

**使用srcset适应不同屏幕尺寸：**
```html
<img
  src="image-800w.jpg"
  srcset="
    image-400w.jpg 400w,
    image-800w.jpg 800w,
    image-1200w.jpg 1200w,
    image-1600w.jpg 1600w
  "
  sizes="(max-width: 640px) 100vw, (max-width: 1024px) 50vw, 800px"
  alt="Description"
  width="800"
  height="600"
  loading="lazy"
>
```

**工作原理：**
- **srcset** - 可用图像尺寸（宽度描述符）
- **sizes** - 图像在不同视口中的宽度
- 浏览器根据设备DPR和视口选择最佳图像

**艺术指导（不同屏幕的不同裁剪）：**
```html
<picture>
  <source
    media="(max-width: 640px)"
    srcset="hero-mobile.jpg"
  >
  <source
    media="(max-width: 1024px)"
    srcset="hero-tablet.jpg"
  >
  <img
    src="hero-desktop.jpg"
    alt="Hero image"
    width="1200"
    height="600"
  >
</picture>
```

### 延迟加载

**原生延迟加载（简单，有效）：**
```html
<!-- ✅ 延迟加载首屏以下图像 -->
<img src="image.jpg" alt="Description" loading="lazy" width="800" height="600">

<!-- ✅ 首屏图像立即加载 -->
<img src="hero.jpg" alt="Hero" fetchpriority="high" width="1200" height="600">
```

**何时延迟加载：**
- ✅ 首屏以下图像
- ✅ 图像库
- ✅ 有许多图像的长页面
- ❌ 英雄图像（首屏）
- ❌ LCP图像

**浏览器支持**：97%（原生`loading="lazy"`）

### Cloudflare图像优化

**Cloudflare Polish（自动图像优化）：**
1. 登录Cloudflare仪表板
2. 选择域名
3. 速度 → 优化
4. 启用"Polish"（有损或无损）
5. 启用"WebP"

**它做什么：**
- 自动转换为支持浏览器的WebP
- 压缩图像
- 从CDN边缘提供服务

**成本**：包含在Cloudflare Pro计划中（20美元/月），或免费使用Cloudflare Images（5美元/10万次请求）

**Cloudflare Images（图像调整大小API）：**
```html
<!-- 即时调整大小 -->
<img src="https://imagedelivery.net/[ACCOUNT_HASH]/[IMAGE_ID]/w=800,h=600,fit=cover">

<!-- 多个变体 -->
<img
  srcset="
    https://imagedelivery.net/.../w=400 400w,
    https://imagedelivery.net/.../w=800 800w,
    https://imagedelivery.net/.../w=1200 1200w
  "
  sizes="(max-width: 640px) 100vw, 800px"
>
```

## JavaScript优化

JavaScript是大多数网站的第一性能瓶颈。

### 包大小减少

**分析包大小：**
```bash
# Astro构建分析
npm run build
# 检查dist/文件夹大小

# 或使用包分析器
npx vite-bundle-visualizer
```

**减少技术：**
1. **移除未使用的依赖项**
   ```bash
   npm uninstall unused-package
   ```

2. **使用更轻的替代品**
   ```javascript
   // ❌ 重量级库（70 KB）
   import moment from 'moment';

   // ✅ 更轻的替代品（2 KB）
   import { format } from 'date-fns';
   ```

3. **只导入您需要的内容**
   ```javascript
   // ❌ 导入整个库
   import _ from 'lodash';

   // ✅ 导入特定函数
   import debounce from 'lodash-es/debounce';
   ```

**目标**：总JavaScript < 200 KB（压缩后）

### 代码分割

**动态导入（按需加载）：**
```javascript
// ❌ 预先加载重量级库
import Chart from 'chart.js';

// ✅ 只在需要时加载
button.addEventListener('click', async () => {
  const { default: Chart } = await import('chart.js');
  new Chart(ctx, config);
});
```

**Astro组件级代码分割：**
```astro
---
// 只有在条件为真时才加载重量级组件
const showChart = Astro.url.searchParams.has('chart');
---

{showChart && (
  <Chart client:visible />
)}
```

### 树摇

从包中移除未使用的代码。

**如何启用（Astro/Vite自动执行）：**
- 使用ES模块（`import`/`export`）
- 避免CommonJS（`require`）
- 避免模块中的副作用

**检查包含的内容：**
```bash
# 构建并检查大小
npm run build

# 分析包
npx vite-bundle-visualizer
```

### 最小化

**Astro在生产环境中自动最小化：**
```bash
npm run build  # 最小化JS、CSS、HTML
```

**最小化做什么：**
- 移除空白
- 缩短变量名
- 移除注释
- 优化语法

**之前（10 KB）：**
```javascript
function calculateTotalPrice(items) {
  let totalPrice = 0;
  for (let i = 0; i < items.length; i++) {
    totalPrice += items[i].price;
  }
  return totalPrice;
}
```

**之后（0.5 KB）：**
```javascript
function c(i){let t=0;for(let e=0;e<i.length;e++)t+=i[e].price;return t}
```

### 延迟/异步加载策略

**脚本加载选项：**

**1. 默认（阻塞）：**
```html
<script src="app.js"></script>
<!-- 阻塞HTML解析，直到脚本下载并执行 -->
```

**2. 异步（非阻塞下载）：**
```html
<script src="analytics.js" async></script>
<!-- 并行下载，准备就绪后立即执行 -->
<!-- 用于：分析、广告、独立脚本 -->
```

**3. 延迟（非阻塞，有序执行）：**
```html
<script src="app.js" defer></script>
<!-- 并行下载，HTML解析后执行 -->
<!-- 用于：依赖DOM的主应用脚本 -->
```

**视觉比较：**
```
阻塞：  |--- HTML ---|--- 下载 + 执行 ---|--- HTML ---|
异步：  |--- HTML + 下载 ---|X 执行 X|--- HTML ---|
延迟：  |--- HTML + 下载 ---|--- HTML ---|X 执行 X|
```

**最佳实践：**
- **异步** - 分析、广告、独立小部件
- **延迟** - 主应用脚本，任何需要DOM的内容
- **阻塞** - 仅关键内联脚本

## CSS优化

### Tailwind PurgeCSS配置

**Astro + Tailwind自动清除未使用的CSS：**

```javascript
// tailwind.config.cjs
module.exports = {
  content: ['./src/**/*.{astro,html,js,jsx,md,mdx,svelte,ts,tsx,vue}'],
  theme: {
    extend: {},
  },
  plugins: [],
};
```

**它做什么：**
- 扫描内容文件中的类名
- 移除未使用的Tailwind类
- 将CSS从约3 MB减少到约10-50 KB

**手动清除配置：**
```javascript
// vite.config.js（如果不使用Tailwind）
import { defineConfig } from 'vite';
import purgecss from '@fullhuman/postcss-purgecss';

export default defineConfig({
  css: {
    postcss: {
      plugins: [
        purgecss({
          content: ['./src/**/*.{astro,html,js}'],
          safelist: ['class-to-keep', /^dynamic-/],
        }),
      ],
    },
  },
});
```

### 关键CSS提取

**内联关键CSS，延迟非关键CSS：**

```astro
---
// src/layouts/Layout.astro
---

<!DOCTYPE html>
<html>
<head>
  <!-- 关键CSS内联 -->
  <style>
    /* 首屏样式 */
    .hero { ... }
    .nav { ... }
    .button { ... }
  </style>

  <!-- 非关键CSS延迟 -->
  <link rel="preload" href="/styles/non-critical.css" as="style" onload="this.rel='stylesheet'">
  <noscript><link rel="stylesheet" href="/styles/non-critical.css"></noscript>
</head>
<body>
  <slot />
</body>
</html>
```

**工具：**
- **Critical** - https://github.com/addyosmani/critical（提取关键CSS）
- **Critters** - https://github.com/GoogleChromeLabs/critters（Astro插件）

### 最小化

**Astro在生产环境中自动最小化CSS：**
```bash
npm run build  # 最小化CSS
```

**手动最小化（如果需要）：**
```javascript
// vite.config.js
import { defineConfig } from 'vite';

export default defineConfig({
  build: {
    cssMinify: 'lightningcss', // 比默认更快
  },
});
```

### 移除未使用的样式

**使用Chrome DevTools覆盖率工具：**
1. 打开DevTools（F12）
2. Cmd+Shift+P（Mac）或Ctrl+Shift+P（Windows）
3. 输入"Coverage" → 显示覆盖率
4. 单击重新加载图标
5. 查看CSS覆盖率报告

**解释结果：**
- 红条 = 未使用的CSS
- 绿条 = 已使用的CSS
- 目标：80%+覆盖率（绿色）

**未使用CSS的常见来源：**
- 未使用的Bootstrap/Foundation组件
- 旧的遗留样式
- 已注释但未删除的样式
- 未使用功能的框架CSS

## Astro特定性能

### 静态生成优势

**Astro在构建时预渲染HTML（SSG）：**

**性能优势：**
- ✅ 无服务器处理时间
- ✅ 从CDN即时TTFB
- ✅ 完美适合缓存
- ✅ 极快的LCP
- ✅ 请求时无数据库查询
- ✅ 无服务器故障

**与服务器端渲染（SSR）对比：**
```
SSG（Astro默认）：
请求 → CDN（即时） → HTML
TTFB：10-50ms ✅

SSR（传统）：
请求 → 服务器 → 数据库 → 渲染 → HTML
TTFB：200-1000ms ❌
```

**何时使用SSG（Astro默认）：**
- 营销站点
- 商业网站
- 博客
- 文档
- 着陆页
- 大多数内容站点

**何时需要SSR：**
- 用户特定内容（仪表板）
- 实时数据
- 基于请求头动态
- 服务器端身份验证

### 岛屿架构（部分水合）

**Astro的岛屿架构 = 默认零JS + 选择性水合**

**工作原理：**
```astro
---
// src/pages/index.astro
import StaticHeader from '@components/StaticHeader.astro';
import InteractiveCounter from '@components/InteractiveCounter.jsx';
import StaticFooter from '@components/StaticFooter.astro';
---

<!-- ✅ 静态HTML（无JS） -->
<StaticHeader />

<!-- ✅ 交互"岛屿"（仅此部分有JS） -->
<InteractiveCounter client:visible />

<!-- ✅ 静态HTML（无JS） -->
<StaticFooter />
```

**客户端指令：**
- `client:load` - 立即加载JS
- `client:idle` - 浏览器空闲时加载
- `client:visible` - 元素进入视口时加载（最适合首屏以下）
- `client:media` - 媒体查询匹配时加载
- `client:only` - 跳过SSR，仅客户端

**性能优势：**
```
传统React站点：
- HTML：50 KB
- JavaScript：500 KB
- 总计：550 KB

Astro与岛屿：
- HTML：50 KB
- JavaScript：20 KB（仅交互部分）
- 总计：70 KB ✅
```

### 组件级客户端JS

**仅向需要的组件添加JS：**

```astro
---
// src/components/ContactForm.astro
// 没有客户端指令 = 不发送JS
---

<form method="POST" action="/api/contact">
  <!-- 纯HTML表单，无需JS即可工作 -->
  <input type="text" name="name" required>
  <input type="email" name="email" required>
  <button type="submit">Send</button>
</form>
```

```jsx
// src/components/SearchWidget.jsx
// 有客户端指令 = 发送JS
export default function SearchWidget() {
  const [query, setQuery] = useState('');
  // 使用React的交互式搜索
  return <input value={query} onChange={e => setQuery(e.target.value)} />;
}
```

```astro
---
// src/pages/index.astro
import ContactForm from '@components/ContactForm.astro';
import SearchWidget from '@components/SearchWidget.jsx';
---

<!-- 无JS -->
<ContactForm />

<!-- 发送React + 组件JS（仅在可见时） -->
<SearchWidget client:visible />
```

### 默认零JS

**Astro默认发送零JavaScript：**

```astro
---
// 此页面发送零JavaScript
---

<html>
<head>
  <title>My Page</title>
</head>
<body>
  <header>
    <nav>
      <a href="/">Home</a>
      <a href="/about">About</a>
    </nav>
  </header>

  <main>
    <h1>Welcome</h1>
    <p>Pure HTML, no JS needed.</p>
  </main>
</body>
</html>
```

**结果：**
- LCP：< 1秒 ✅
- INP：不适用（无交互）✅
- CLS：0 ✅
- 包大小：0 KB ✅
- Lighthouse分数：100 ✅

**仅在需要时添加JS：**
```astro
<!-- 如果需要，添加小型内联脚本 -->
<script>
  // 页面加载时运行一次
  document.querySelector('.mobile-menu-toggle').addEventListener('click', () => {
    document.querySelector('.mobile-menu').classList.toggle('open');
  });
</script>

<!-- 或导入外部脚本 -->
<script src="/scripts/analytics.js" defer></script>
```

## 缓存策略

### 浏览器缓存（Cache-Control头）

**在Cloudflare Pages中配置（`_headers`文件）：**

```
# public/_headers

/*
  Cache-Control: public, max-age=0, must-revalidate

/*.css
  Cache-Control: public, max-age=31536000, immutable

/*.js
  Cache-Control: public, max-age=31536000, immutable

/*.woff2
  Cache-Control: public, max-age=31536000, immutable

/*.jpg
  Cache-Control: public, max-age=31536000, immutable

/*.png
  Cache-Control: public, max-age=31536000, immutable

/*.webp
  Cache-Control: public, max-age=31536000, immutable

/*.svg
  Cache-Control: public, max-age=31536000, immutable
```

**这些头的含义：**
- `public` - 可以被CDN和浏览器缓存
- `max-age=31536000` - 缓存1年（以秒为单位）
- `immutable` - 即使在重新加载时也不重新验证
- `max-age=0, must-revalidate` - 始终重新验证（用于HTML）

**Astro的内置资产哈希：**
```html
<!-- 构建生成唯一文件名 -->
<link rel="stylesheet" href="/assets/styles.a3f2b8.css">
<script src="/assets/app.7c8d4e.js"></script>

<!-- 可以永久缓存，因为内容更改时文件名会更改 -->
```

### Cloudflare边缘缓存

**Cloudflare在全球边缘位置缓存：**

**默认缓存（自动）：**
- 静态文件（CSS、JS、图像）自动缓存
- HTML基于头缓存

**自定义缓存（页面规则）：**
1. 登录Cloudflare
2. 选择域名
3. 规则 → 页面规则 → 创建页面规则
4. 模式：`www.smithplumbing.com/*`
5. 设置：
   - 缓存级别：标准
   - 浏览器缓存TTL：4小时
   - 边缘缓存TTL：2小时
6. 保存

**缓存所有内容（包括HTML）：**
1. 创建页面规则
2. 模式：`www.smithplumbing.com/*`
3. 设置：缓存级别 → 缓存所有内容
4. 边缘缓存TTL：2小时

**部署时清除缓存：**
```bash
# Cloudflare Pages在部署时自动清除缓存
# 或通过API/仪表板手动清除
```

### Service Workers

**高级：离线支持和自定义缓存**

```javascript
// public/sw.js
const CACHE_NAME = 'v1';
const urlsToCache = [
  '/',
  '/styles/main.css',
  '/scripts/app.js',
  '/images/logo.svg',
];

// 安装service worker并缓存文件
self.addEventListener('install', (event) => {
  event.waitUntil(
    caches.open(CACHE_NAME).then((cache) => {
      return cache.addAll(urlsToCache);
    })
  );
});

// 从缓存提供服务，回退到网络
self.addEventListener('fetch', (event) => {
  event.respondWith(
    caches.match(event.request).then((response) => {
      return response || fetch(event.request);
    })
  );
});
```

**注册service worker：**
```html
<script>
  if ('serviceWorker' in navigator) {
    navigator.serviceWorker.register('/sw.js');
  }
</script>
```

**优势：**
- 离线支持
- 更快的重复访问
- 自定义缓存策略

**用例：**
- PWA（渐进式Web应用）
- 离线文档
- 新闻站点（缓存文章）

### 缓存TTL配置

**TTL（生存时间）建议：**

| 资源类型 | 浏览器缓存 | CDN缓存 |
|----------|------------|---------|
| HTML | 0（重新验证） | 1-2小时 |
| CSS/JS（哈希） | 1年 | 1年 |
| 图像（哈希） | 1年 | 1年 |
| 图像（未哈希） | 1天 | 1周 |
| 字体 | 1年 | 1年 |
| API响应 | 0（无缓存） | 5-60分钟 |

**过期时重新验证模式：**
```
Cache-Control: max-age=3600, stale-while-revalidate=86400
```
- 提供1小时缓存版本
- 1小时后，在后台获取新版本时提供过期缓存
- 为下次请求更新缓存

## 字体优化

### 字体子集化

**移除未使用的字符以减少文件大小：**

```bash
# 安装pyftsubset
pip install fonttools

# 仅子集化拉丁字符
pyftsubset font.ttf \
  --output-file=font-subset.woff2 \
  --flavor=woff2 \
  --unicodes=U+0000-00FF,U+0131,U+0152-0153,U+02BB-02BC,U+02C6,U+02DA,U+02DC,U+2000-206F,U+2074,U+20AC,U+2122,U+2191,U+2193,U+2212,U+2215,U+FEFF,U+FFFD
```

**或使用在线工具：**
- **Font Squirrel Webfont Generator** - https://www.fontsquirrel.com/tools/webfont-generator
- **Fontie** - https://fontie.pixelsvsbytes.com/webfont-generator

**大小减少：**
- 完整字体：200-400 KB
- 拉丁子集：20-40 KB ✅

### font-display策略

**控制字体加载方式：**

```css
@font-face {
  font-family: 'Custom Font';
  src: url('/fonts/custom.woff2') format('woff2');
  font-display: swap; /* 或 optional, fallback, block */
  font-weight: 400;
  font-style: normal;
}
```

**font-display选项：**

**1. `swap`（大多数情况推荐）**
- 立即显示回退字体
- 加载后切换到自定义字体
- 如果字体大小不同，可能导致布局偏移（CLS）
```css
font-display: swap;
```

**2. `optional`（最佳性能）**
- 立即显示回退字体
- 仅在约100ms内加载字体时才切换
- 如果字体未快速加载，则无布局偏移
- 最适合避免CLS
```css
font-display: optional;
```

**3. `fallback`**
- 短暂不可见期（约100ms）
- 如果自定义字体未加载则显示回退
- 如果在约3秒内加载则切换
```css
font-display: fallback;
```

**4. `block`**
- 字体加载时文本不可见（最多3秒）
- 超时后显示自定义字体或回退
- 不推荐（用户体验差）
```css
font-display: block;
```

**建议：**
- **正文文本**：`font-display: optional`（避免CLS）
- **标题/品牌**：`font-display: swap`（显示自定义字体很重要）

### WOFF2格式

**使用WOFF2（最佳压缩）：**

```css
@font-face {
  font-family: 'Custom Font';
  src: url('/fonts/custom.woff2') format('woff2'),
       url('/fonts/custom.woff') format('woff'); /* 旧浏览器回退 */
  font-display: optional;
}
```

**格式比较：**
- **TTF**：200 KB（未压缩）
- **WOFF**：100 KB（压缩）
- **WOFF2**：50 KB（更好压缩）✅

**浏览器支持**：97%（WOFF2）

### 预加载字体

**预加载关键字体：**

```html
<link
  rel="preload"
  href="/fonts/custom-regular.woff2"
  as="font"
  type="font/woff2"
  crossorigin
>
```

**何时预加载：**
- ✅ 首屏使用的字体
- ✅ 品牌/标题字体
- ❌ 正文文本字体（如果使用font-display: optional）
- ❌ 所有字体粗细（仅预加载需要的内容）

**最大预加载数**：1-2个字体（预加载太多会损害性能）

### 系统字体回退

**使用系统字体作为回退（无需下载）：**

```css
body {
  font-family:
    'Custom Font',
    -apple-system,
    BlinkMacSystemFont,
    'Segoe UI',
    Roboto,
    'Helvetica Neue',
    Arial,
    sans-serif;
}
```

**或仅使用系统字体（零字体加载时间）：**
```css
body {
  font-family:
    -apple-system,
    BlinkMacSystemFont,
    'Segoe UI',
    Roboto,
    'Helvetica Neue',
    Arial,
    sans-serif;
}
```

**系统字体堆栈优势：**
- ✅ 零加载时间
- ✅ 零CLS
- ✅ 用户熟悉（原生于操作系统）
- ✅ 优秀性能

**考虑系统字体用于：**
- 正文文本（性能优先）
- 仪表板/应用（功能优于品牌）
- MVP（更快交付）

## 第三方脚本

### 延迟加载策略

**用户交互时加载：**
```html
<!-- 仅在用户点击时加载聊天小部件 -->
<button id="open-chat">Chat with us</button>

<script>
  document.getElementById('open-chat').addEventListener('click', () => {
    const script = document.createElement('script');
    script.src = 'https://example.com/chat-widget.js';
    document.head.appendChild(script);
  }, { once: true });
</script>
```

**滚动到元素时加载：**
```javascript
// Intersection Observer
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      const script = document.createElement('script');
      script.src = 'https://example.com/widget.js';
      document.head.appendChild(script);
      observer.unobserve(entry.target);
    }
  });
});

observer.observe(document.getElementById('widget-container'));
```

**Astro client:visible方法：**
```astro
---
import ChatWidget from '@components/ChatWidget.jsx';
---

<!-- 仅在滚动到视图时加载JS -->
<ChatWidget client:visible />
```

### 尽可能自托管

**而不是第三方CDN：**
```html
<!-- ❌ 第三方CDN（DNS查找、SSL握手开销） -->
<script src="https://cdn.example.com/library.js"></script>

<!-- ✅ 自托管（同源，已连接） -->
<script src="/scripts/library.js"></script>
```

**优势：**
- 无额外DNS查找
- 无额外SSL握手
- 更好的缓存控制
- 无单点故障（SPOF）
- 隐私（无第三方跟踪）

**自托管这些：**
- Google Fonts（使用Fontsource）
- Google Analytics（使用Plausible或自托管）
- jQuery、Bootstrap等

**Fontsource示例：**
```bash
npm install @fontsource/inter

# 在Astro组件中
import '@fontsource/inter';
import '@fontsource/inter/600.css';
```

### 嵌入的立面模式

**用轻量级预览替换重量级嵌入：**

**YouTube嵌入（传统）：**
```html
<!-- ❌ 加载1 MB+的YouTube脚本 -->
<iframe
  src="https://www.youtube.com/embed/VIDEO_ID"
  width="560"
  height="315"
></iframe>
```

**YouTube立面（优化）：**
```html
<!-- ✅ 轻量级预览，点击时加载iframe -->
<div class="youtube-facade" data-video-id="VIDEO_ID">
  <img
    src="https://img.youtube.com/vi/VIDEO_ID/maxresdefault.jpg"
    alt="Video thumbnail"
  >
  <button class="play-button">Play</button>
</div>

<style>
  .youtube-facade {
    position: relative;
    cursor: pointer;
  }
  .play-button {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    /* 样式为YouTube播放按钮 */
  }
</style>

<script>
  document.querySelectorAll('.youtube-facade').forEach(facade => {
    facade.addEventListener('click', () => {
      const videoId = facade.dataset.videoId;
      const iframe = document.createElement('iframe');
      iframe.src = `https://www.youtube.com/embed/${videoId}?autoplay=1`;
      iframe.width = '560';
      iframe.height = '315';
      iframe.allow = 'autoplay';
      facade.replaceWith(iframe);
    });
  });
</script>
```

**库：**
- **lite-youtube-embed** - https://github.com/paulirish/lite-youtube-embed
- **React Lite YouTube Embed** - https://github.com/ibrahimcesar/react-lite-youtube-embed

**节省：**
- 传统嵌入：1.2 MB
- 立面：200 KB（图像）+ 点击前0 KB JS
- **节省：1 MB+** ✅

## 测试和监控

### Google PageSpeed Insights

**URL**：https://pagespeed.web.dev

**如何使用：**
1. 输入您的URL
2. 单击"分析"
3. 查看移动和桌面分数
4. 查看Core Web Vitals
5. 检查机会和诊断

**目标分数：**
- **性能**：90+（桌面），80+（移动）
- **可访问性**：95+
- **最佳实践**：95+
- **SEO**：95+

**专注于：**
- Core Web Vitals（LCP、INP、CLS）
- 机会（最大收益）
- 诊断（问题）

### Lighthouse CI集成

**在CI/CD管道中运行Lighthouse：**

```bash
# 安装
npm install -g @lhci/cli

# 配置
# lighthouserc.js
module.exports = {
  ci: {
    collect: {
      url: ['http://localhost:3000'],
      numberOfRuns: 3,
    },
    assert: {
      preset: 'lighthouse:recommended',
      assertions: {
        'categories:performance': ['error', { minScore: 0.9 }],
        'categories:accessibility': ['error', { minScore: 0.95 }],
        'first-contentful-paint': ['error', { maxNumericValue: 2000 }],
        'largest-contentful-paint': ['error', { maxNumericValue: 2500 }],
        'cumulative-layout-shift': ['error', { maxNumericValue: 0.1 }],
      },
    },
    upload: {
      target: 'temporary-public-storage',
    },
  },
};

# 运行
lhci autorun
```

**GitHub Actions示例：**
```yaml
name: Lighthouse CI
on: [push]
jobs:
  lighthouse:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
      - run: npm ci
      - run: npm run build
      - run: npm install -g @lhci/cli
      - run: lhci autorun
```

### WebPageTest

**URL**：https://www.webpagetest.org

**功能：**
- 从多个位置测试
- 在真实设备上测试
- 胶片条视图（视觉加载）
- 瀑布图（网络请求）
- 重复视图（缓存性能）

**如何使用：**
1. 输入URL
2. 选择位置（最接近目标受众）
3. 选择浏览器/设备
4. 单击"开始测试"
5. 等待结果（2-5分钟）

**关键指标：**
- TTFB（首字节时间）
- 开始渲染
- LCP
- 总加载时间
- 完全加载时间

**高级：**
- 使用慢速连接测试（3G、4G）
- 比较优化前后
- 测试经过身份验证的页面
- 自定义脚本

### Chrome DevTools性能选项卡

**如何分析：**
1. 打开DevTools（F12）
2. 单击"性能"选项卡
3. 单击记录按钮（●）
4. 与页面交互
5. 单击停止
6. 分析时间线

**要查找的内容：**
- **长任务**（> 50ms） - 任务上的红角
- **布局偏移** - 带有"布局偏移"标签的蓝条
- **主线程活动** - JavaScript执行时间
- **网络请求** - 阻塞请求
- **渲染** - 绘制和合成时间

**识别INP问题：**
1. 在摘要中单击"交互"
2. 查找慢交互（> 200ms）
3. 单击交互查看细分
4. 优化识别的瓶颈

### 真实用户监控（RUM）

**监控实际用户体验：**

**工具：**
- **Google Analytics 4** - 基本Core Web Vitals
- **Cloudflare Web Analytics** - 免费，注重隐私
- **SpeedCurve** - 详细RUM和综合监控
- **web-vitals库** - 发送到您自己的分析

**web-vitals示例：**
```bash
npm install web-vitals
```

```javascript
// public/vitals.js
import { onCLS, onFID, onLCP, onINP } from 'web-vitals';

function sendToAnalytics({ name, value, id }) {
  // 发送到您的分析端点
  fetch('/api/vitals', {
    method: 'POST',
    body: JSON.stringify({ name, value, id }),
    headers: { 'Content-Type': 'application/json' },
  });
}

onCLS(sendToAnalytics);
onFID(sendToAnalytics);
onLCP(sendToAnalytics);
onINP(sendToAnalytics);
```

**为什么RUM很重要：**
- 实验室测试不反映真实世界条件
- RUM捕获实际用户体验
- 不同的设备、连接、位置
- 识别仅影响某些用户的问题

## 性能预算

**设置目标并强制执行：**

### 设置目标

**示例性能预算：**

| 指标 | 目标 | 最大 |
|------|------|-----|
| LCP | < 2.0s | 2.5s |
| INP | < 150ms | 200ms |
| CLS | < 0.05 | 0.1 |
| 总JS | < 150 KB | 200 KB |
| 总CSS | < 50 KB | 75 KB |
| 总图像 | < 500 KB | 1 MB |
| 总页面大小 | < 1 MB | 1.5 MB |
| 请求 | < 30 | 50 |

### 在CI/CD中自动强制执行

**Lighthouse CI断言：**
```javascript
// lighthouserc.js
module.exports = {
  ci: {
    assert: {
      assertions: {
        'largest-contentful-paint': ['error', { maxNumericValue: 2500 }],
        'cumulative-layout-shift': ['error', { maxNumericValue: 0.1 }],
        'total-byte-weight': ['error', { maxNumericValue: 1500000 }], // 1.5 MB
        'resource-summary:script:size': ['error', { maxNumericValue: 200000 }], // 200 KB JS
      },
    },
  },
};
```

**包大小限制（webpack/vite）：**
```javascript
// vite.config.js
export default {
  build: {
    rollupOptions: {
      output: {
        manualChunks(id) {
          if (id.includes('node_modules')) {
            return 'vendor';
          }
        },
      },
    },
    chunkSizeWarningLimit: 500, // 如果块 > 500 KB则警告
  },
};
```

### 随时间跟踪

**监控趋势：**
1. 设置RUM（真实用户监控）
2. 跟踪每周/每月平均值
3. 回归时警报
4. 主要发布前审查

**工具：**
- **Cloudflare Web Analytics** - 免费仪表板
- **Google Search Console** - Core Web Vitals报告
- **SpeedCurve** - 历史性能跟踪

## Cloudflare特定功能

Cloudflare提供自动性能优化。

### 自动最小化

**自动最小化CSS、JS、HTML：**

1. 登录Cloudflare仪表板
2. 选择域名
3. 速度 → 优化
4. 启用自动最小化：
   - ✅ JavaScript
   - ✅ CSS
   - ✅ HTML

**它做什么：**
- 移除空白
- 移除注释
- 缩短变量名（仅当安全时，JS）

**节省**：文件大小减少20-40%

### Rocket Loader

**异步加载JavaScript：**

1. 速度 → 优化
2. 启用"Rocket Loader"

**它做什么：**
- 延迟JavaScript加载
- 异步加载脚本
- 改善页面加载时间

**警告**：可能破坏某些脚本（彻底测试）

**何时使用：**
- 站点有许多阻塞脚本
- 无法修改HTML添加defer/async
- 在暂存上测试后

**何时避免：**
- 已有async/defer的现代站点
- 需要立即JS的交互式应用

### Mirage

**自动延迟加载图像：**

1. 速度 → 优化
2. 启用"Mirage"

**它做什么：**
- 自动延迟加载图像
- 显示低分辨率占位符
- 滚动时加载完整图像

**成本**：需要Cloudflare Pro计划（20美元/月）

**替代方案**：使用原生`loading="lazy"`（免费，随处可用）

### Polish

**自动图像优化：**

1. 速度 → 优化
2. 启用"Polish"
3. 选择"有损"或"无损"
4. 启用"WebP"

**它做什么：**
- 压缩图像
- 为支持浏览器转换为WebP
- 从CDN提供优化版本

**成本**：需要Cloudflare Pro计划（20美元/月）

**节省：**
- 有损：大小减少30-50%
- 无损：大小减少10-20%
- WebP：额外减少25-35%

## 交叉引用

**相互依赖的质量标准：**
- **[SEO标准](./seo-zh.md)** - Core Web Vitals是直接的Google排名因素
- **[可访问性标准](./accessibility-zh.md)** - 清洁、可访问的标记改善性能
- **[这些标准如何关联](./README-zh.md#how-these-standards-relate)** - 理解质量三角形

**相关文档：**
- [测试和调试](../workflow/phase-3-testing-debugging-zh.md) - 工作流程中的性能测试
- [Cloudflare Pages](../hosting-tools/cloudflare-pages-zh.md) - 性能的托管配置
- [Cloudflare CDN](../hosting-tools/cloudflare-cdn-zh.md) - CDN配置
- [质量标准概述](./README-zh.md) - 返回质量标准主页

---

**记住**：性能不是一次性任务。持续监控，在真实设备上测试，并迭代优化。优先考虑Core Web Vitals（LCP、INP、CLS），因为它们直接影响SEO排名和用户体验。