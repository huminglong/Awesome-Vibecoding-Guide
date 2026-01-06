# 设计一致性：反Vibe-Coded指南

阻止您的网站看起来匆忙。本指南涵盖了将专业工作与业余"vibe coded"网站区分开来的视觉和结构模式。

## 为什么设计一致性很重要

**对于您的业务：**
- 视觉精致证明您的[$300-700定价](../business-model/pricing-economics-zh.md)合理
- 将您与廉价的Fiverr/WordPress网站区分开来
- 减少客户修订请求
- 建立专业作品集
- 创造值得推荐的工作

**对于您的客户：**
- 专业外观建立信任
- 一致的UX减少困惑
- 更好的转化率
- 在他们市场中的竞争优势

**现实**：技术质量（[SEO](./seo-zh.md)、[性能](./performance-zh.md)、[可访问性](./accessibility-zh.md)）让您达到"功能"。设计一致性让您达到"专业"。

---

## 关键视觉信号

这些是立即尖叫"匆忙构建"的危险信号。修复这些或不要发布。

### 1. 紫色渐变综合症

**问题：**
随机紫色渐变、紫色发光、紫色阴影、紫色强调，当品牌不是紫色时。

**为什么发生：**
默认AI/模板建议。紫色是vibe coded设计的非官方颜色。

**修复：**
```css
/* ❌ 糟糕 - 随机紫色强调 */
.hero {
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
}

/* ✅ 良好 - 品牌对齐颜色 */
.hero {
  background: linear-gradient(135deg, #1e40af 0%, #3730a3 100%);
}
/* 仅当蓝色实际上是品牌颜色时 */
```

**规则**：仅在以下情况下使用紫色：
- 它明确是品牌标识的一部分
- 客户要求它
- 您有特定的设计原因

否则，根据客户的实际品牌或行业惯例选择颜色。

### 2. 表情符号过载

**问题：**
✨ 到处都是闪光 ✨
表情符号作为UI元素、在标题中、在按钮上、作为项目符号。

**为什么发生：**
无需设计努力的快速视觉兴趣。

**修复：**

**可接受：**
- 页面标题中的一个表情符号以显示个性
- 非正式博客内容中的偶尔表情符号

**不可接受：**
- 导航中表情符号代替图标
- 按钮上的表情符号
- 表情符号作为功能列表项目符号
- 单个标题中的多个表情符号

```html
<!-- ❌ 糟糕 -->
<h1>欢迎来到我们的网站 ✨🚀💫</h1>
<button>开始 ✨</button>
<ul>
  <li>✅ 快速交付</li>
  <li>✅ 优质服务</li>
</ul>

<!-- ✅ 良好 -->
<h1>专业管道服务</h1>
<button>请求报价</button>
<ul class="feature-list">
  <li>快速交付</li>
  <li>优质服务</li>
</ul>
```

**客户网站规则**：每页最多1-2个表情符号，永远不在关键UI元素中。

### 3. 假的或通用的推荐信

**问题：**
- AI生成的头像
- 像"Sarah P."这样的名字
- 通用引用："优质服务！"
- 没有职位或链接
- 同一张脸使用两次

**为什么发生：**
试图在没有实际推荐信的情况下添加社会证明。

**修复：**

**选项A - 真实推荐信：**
```html
<div class="testimonial">
  <img src="real-customer-photo.jpg" alt="John Martinez">
  <blockquote>
    "在周日的2小时内修复了我的热水器。
    专业，公平定价，并且自己清理干净。"
  </blockquote>
  <cite>
    John Martinez, 房主<br>
    奥斯汀，德克萨斯州
  </cite>
</div>
```

**选项B - 无推荐信：**
如果客户还没有真实推荐信，完全省略该部分。没有推荐信比假推荐信好。

**选项C - 替代社会证明：**
- "自2015年以来服务奥斯汀"
- "500+满意客户"
- "持牌和保险（#12345）"
- Google商业评分（如果好）

**规则：** 一个真实推荐信 > 三个假推荐信 > 无推荐信。

### 4. 不一致的边框半径

**问题：**
这里4px，那里12px，32px按钮，圆形头像，方形图像。

**为什么发生：**
在没有系统的情况下复制粘贴组件。

**修复：**

创建边框半径比例并坚持使用：

```css
/* 设计令牌 - 定义一次 */
:root {
  --radius-sm: 4px;   /* 小元素，徽章 */
  --radius-md: 8px;   /* 按钮，卡片 */
  --radius-lg: 12px;  /* 大卡片，模态框 */
  --radius-full: 9999px; /* 胶囊，头像 */
}

/* ✅ 良好 - 一致使用 */
.button { border-radius: var(--radius-md); }
.card { border-radius: var(--radius-lg); }
.avatar { border-radius: var(--radius-full); }
.badge { border-radius: var(--radius-sm); }

/* ❌ 糟糕 - 随机值 */
.button { border-radius: 7px; }
.card { border-radius: 15px; }
.avatar { border-radius: 50%; }
.badge { border-radius: 3px; }
```

**规则：** 最多选择3-4个值。在任何地方使用它们。没有例外。

### 5. 过度的悬停动画

**问题：**
- 卡片激进地抬起
- 卡片旋转
- 元素弹跳
- 看起来像手电筒的阴影
- 悬停时破坏对齐

**为什么发生：**
为了移动而添加移动。

**修复：**

**微妙是专业的：**
```css
/* ❌ 糟糕 - 激进移动 */
.card:hover {
  transform: translateY(-20px) rotate(2deg);
  box-shadow: 0 30px 60px rgba(0,0,0,0.5);
  transition: all 0.2s ease;
}

/* ✅ 良好 - 微妙反馈 */
.card {
  transition: box-shadow 0.3s ease;
}

.card:hover {
  box-shadow: 0 4px 12px rgba(0,0,0,0.1);
}
```

**何时使用悬停效果：**
- 按钮（微妙颜色/阴影变化）
- 链接（下划线或颜色变化）
- 交互式卡片（温和阴影增加）

**何时不使用悬停效果：**
- 非交互式元素
- 文本块
- 图像（除非可点击）
- 已经交互式元素（表单）

**规则：** 如果悬停破坏布局或对齐，移除它。

---

## 间距和节奏系统

一致的间距是有意设计的#1信号。

### 8点网格系统

**为什么是8pt？**
- 可被2整除（响应式缩放）
- 与常见屏幕尺寸配合良好
- 防止随机间距值

**比例：**
```css
:root {
  --space-1: 8px;   /* 0.5rem */
  --space-2: 16px;  /* 1rem */
  --space-3: 24px;  /* 1.5rem */
  --space-4: 32px;  /* 2rem */
  --space-6: 48px;  /* 3rem */
  --space-8: 64px;  /* 4rem */
  --space-12: 96px; /* 6rem */
  --space-16: 128px; /* 8rem */
}
```

**用法：**
```css
/* ✅ 良好 - 使用比例 */
.section {
  padding-block: var(--space-12); /* 顶部/底部96px */
  padding-inline: var(--space-4); /* 左/右32px */
}

.card {
  padding: var(--space-4);
  margin-bottom: var(--space-6);
}

/* ❌ 糟糕 - 随机值 */
.section {
  padding: 73px 25px; /* 为什么是这些数字？ */
}
```

**Tailwind用户：**
Tailwind已经使用基于8pt的系统。坚持使用它：
```html
<!-- ✅ 良好 - 一致间距 -->
<section class="py-24 px-8">
  <div class="space-y-12">
    <div class="p-8">...</div>
  </div>
</section>

<!-- ❌ 糟糕 - 随机值 -->
<section class="py-[73px] px-[25px]">
  <div class="space-y-[47px]">
    <div class="p-[13px]">...</div>
  </div>
</section>
```

**规则：** 永远不要使用定义比例之外的间距值。

### 字体层次结构

不一致的字体排印破坏视觉精致度。

**系统：**
```css
/* 字体比例 */
:root {
  --text-xs: 0.75rem;   /* 12px - 标签，说明 */
  --text-sm: 0.875rem;  /* 14px - 小文本 */
  --text-base: 1rem;    /* 16px - 正文 */
  --text-lg: 1.125rem;  /* 18px - 引导段落 */
  --text-xl: 1.25rem;   /* 20px - h4 */
  --text-2xl: 1.5rem;   /* 24px - h3 */
  --text-3xl: 1.875rem; /* 30px - h2 */
  --text-4xl: 2.25rem;  /* 36px - h1 */
  --text-5xl: 3rem;     /* 48px - 展示 */
}

/* 字体粗细 */
:root {
  --font-normal: 400;
  --font-medium: 500;
  --font-semibold: 600;
  --font-bold: 700;
}

/* 行高 */
:root {
  --leading-tight: 1.25;   /* 标题 */
  --leading-normal: 1.5;   /* 正文 */
  --leading-relaxed: 1.75; /* 长篇 */
}
```

**应用：**
```css
/* ✅ 良好 - 系统化字体排印 */
h1 {
  font-size: var(--text-4xl);
  font-weight: var(--font-bold);
  line-height: var(--leading-tight);
}

body {
  font-size: var(--text-base);
  font-weight: var(--font-normal);
  line-height: var(--leading-normal);
}

/* ❌ 糟糕 - 随机值 */
h1 {
  font-size: 37px;
  font-weight: 650;
  line-height: 1.3;
}
```

**常见错误：**
- 标题太粗（900粗细）
- 正文太轻（300粗细）
- 不一致的行高
- 太多字体大小

**规则：** 定义6-8个大小，3-4个粗细。只使用那些。

---

## 组件一致性

组件应该看起来属于一起。

### 高度和阴影

选择一个阴影系统并在任何地方使用它。

```css
/* 定义高度级别 */
:root {
  --shadow-sm: 0 1px 2px rgba(0,0,0,0.05);
  --shadow-md: 0 4px 6px rgba(0,0,0,0.1);
  --shadow-lg: 0 10px 15px rgba(0,0,0,0.1);
  --shadow-xl: 0 20px 25px rgba(0,0,0,0.1);
}

/* ✅ 良好 - 一致高度 */
.card { box-shadow: var(--shadow-md); }
.card:hover { box-shadow: var(--shadow-lg); }
.modal { box-shadow: var(--shadow-xl); }

/* ❌ 糟糕 - 随机阴影 */
.card { box-shadow: 0 3px 7px rgba(0,0,0,0.15); }
.button { box-shadow: 0 2px 8px #00000033; }
```

**规则：** 在相同高度级别的所有组件上使用相同的阴影值。

### 按钮一致性

所有按钮应该共享核心DNA。

```css
/* 基础按钮 */
.btn {
  padding: var(--space-2) var(--space-4);
  border-radius: var(--radius-md);
  font-weight: var(--font-semibold);
  transition: all 0.2s ease;
}

/* 变体 */
.btn-primary {
  background: var(--color-primary);
  color: white;
}

.btn-secondary {
  background: transparent;
  border: 2px solid var(--color-primary);
  color: var(--color-primary);
}

/* 状态 */
.btn:hover {
  transform: translateY(-1px);
  box-shadow: var(--shadow-md);
}

.btn:active {
  transform: translateY(0);
}
```

**什么应该一致：**
- 内边距（所有按钮相同）
- 边框半径（所有按钮相同）
- 字体粗细（所有按钮相同）
- 过渡时间（所有按钮相同）
- 悬停行为（所有按钮相同）

**什么可以变化：**
- 颜色（主要 vs 次要）
- 大小（小 vs 大）
- 图标存在

**规则：** 如果您复制粘贴按钮并更改内边距，您就做错了。

---

## 交互质量

事物的行为与它们的外观同样重要。

### 加载状态

**问题：**
点击按钮3秒没有任何反应。业余时间。

**修复：**

每个异步操作都需要视觉反馈：

```html
<!-- ✅ 良好 - 加载状态 -->
<button
  class="btn"
  onclick="handleSubmit(this)"
>
  <span class="button-text">提交</span>
  <span class="button-loader hidden">
    <svg class="spinner">...</svg>
  </span>
</button>

<script>
async function handleSubmit(button) {
  // 显示加载状态
  button.disabled = true;
  button.querySelector('.button-text').classList.add('hidden');
  button.querySelector('.button-loader').classList.remove('hidden');

  // 执行操作
  await submitForm();

  // 重置状态
  button.disabled = false;
  button.querySelector('.button-text').classList.remove('hidden');
  button.querySelector('.button-loader').classList.add('hidden');
}
</script>
```

**需要加载状态的常见地方：**
- 表单提交
- "加载更多"按钮
- 搜索栏
- 登录/注册
- 用户触发的任何API调用

**规则：** 如果需要>500ms，显示加载状态。

### 功能性交互元素

**问题：**
- 不滑动的轮播
- 不切换的标签
- 不打开的手风琴
- 不关闭的模态框
- 不起作用的暗模式切换

**修复：**

在发布前测试每个交互元素：

**发布前交互检查清单：**
- [ ] 所有按钮触发操作
- [ ] 所有表单成功提交
- [ ] 所有链接转到正确目的地
- [ ] 所有导航项工作
- [ ] 模态框打开和关闭
- [ ] 下拉菜单展开和折叠
- [ ] 手风琴正确切换
- [ ] 标签切换内容
- [ ] 搜索实际搜索
- [ ] 过滤器实际过滤

**规则：** 如果它看起来可点击，它必须工作。如果它不工作，移除它。

### 响应式行为

**常见响应式失败：**
- 文本溢出容器
- 卡片奇怪堆叠
- 移动设备上按钮太宽
- 布局折叠
- 水平滚动

**修复：**

在这些断点测试：
- **375px** - iPhone SE（最小的现代手机）
- **768px** - iPad竖屏（平板）
- **1024px** - iPad横屏/小型笔记本电脑
- **1440px** - 标准桌面

```css
/* 移动优先方法 */
.container {
  padding: var(--space-4); /* 移动设备上32px */
}

.grid {
  display: grid;
  grid-template-columns: 1fr; /* 移动设备上堆叠 */
  gap: var(--space-4);
}

/* 平板及以上 */
@media (min-width: 768px) {
  .container {
    padding: var(--space-8); /* 平板+上64px */
  }

  .grid {
    grid-template-columns: repeat(2, 1fr); /* 2列 */
  }
}

/* 桌面 */
@media (min-width: 1024px) {
  .grid {
    grid-template-columns: repeat(3, 1fr); /* 3列 */
  }
}
```

**规则：** 移动优先设计，为桌面增强。在真实设备上测试。

---

## 副本和内容质量

设计不仅仅是视觉。文字很重要。

### 避免通用标语

**问题：**
无意义的英雄副本，可以适用于任何业务。

**坏例子：**
- "构建您的梦想"
- "更快启动"
- "无限制创造"
- "想法成为现实的地方"
- "[行业]的未来"

**好例子（具体和清晰）：**
- "奥斯汀紧急管道。24/7服务，30分钟响应"
- "小企业税务准备。299美元固定费率"
- "律师定制WordPress网站。2周交付"

**公式：**
```
[服务]为[受众]。[关键利益/差异化因素]
```

**规则：** 如果您的标语可以适用于任何业务，那就是糟糕的副本。

### 版权和页脚文本

**问题：**
草率的页脚文本标志着整体草率。

```html
<!-- ❌ 糟糕 -->
<footer>
  <p>Copyright 2024 YourSiteName</p>
  <p>All right reversed</p>
  <p>Made by Me</p>
</footer>

<!-- ✅ 良好 -->
<footer>
  <p>&copy; 2024 Johnson Plumbing LLC. 保留所有权利。</p>
  <p>持牌和保险 | 许可证#12345</p>
</footer>
```

**规则：** 把小细节做对。它们标志着专业性。

### 占位符文本

**问题：**
发布时带有"Lorem ipsum"或测试内容。

**发布前内容检查清单：**
- [ ] 任何地方都没有"Lorem ipsum"
- [ ] 没有"测试标题"或"示例文本"
- [ ] 没有"在此插入内容"
- [ ] 没有"[您的业务名称]"占位符
- [ ] 没有"TODO：写此部分"
- [ ] 所有电话号码都是真实的
- [ ] 所有电子邮件地址都有效
- [ ] 所有地址都正确

**规则：** 在发布前搜索"lorem"、"test"、"sample"、"TODO"、"["。

---

## 技术基础

设计质量需要技术卓越。

### 元标签和SEO基础

视觉设计让用户点击。元标签让他们找到您。

**必需的元标签：**
```html
<head>
  <!-- 标题（50-60个字符） -->
  <title>Johnson Plumbing - 奥斯汀，德克萨斯州24/7紧急服务</title>

  <!-- 描述（150-160个字符） -->
  <meta name="description" content="奥斯汀持牌管道工。紧急维修，安装，排水清洁。30分钟响应时间。立即致电！">

  <!-- 社交分享的Open Graph -->
  <meta property="og:title" content="Johnson Plumbing - 24/7紧急服务">
  <meta property="og:description" content="奥斯汀持牌管道工。紧急维修，安装，排水清洁。">
  <meta property="og:image" content="https://example.com/og-image.jpg">
  <meta property="og:url" content="https://example.com">

  <!-- 网站图标 -->
  <link rel="icon" type="image/png" href="/favicon.png">

  <!-- 视口 -->
  <meta name="viewport" content="width=device-width, initial-scale=1">
</head>
```

**另请参阅：** [完整SEO指南](./seo-zh.md)

### 功能要求

**每个网站必须具有：**
- [ ] 工作网站图标（不是默认浏览器图标）
- [ ] 适当的页面标题（每页唯一）
- [ ] 元描述
- [ ] Open Graph图像（1200x630px）
- [ ] 404页面（不是默认服务器错误）
- [ ] 移动视口元标签
- [ ] HTTPS（Cloudflare Pages处理这个）

**另请参阅：** [质量门检查清单](./quality-gates-zh.md)

---

## 项目类型特定标准

不同项目有不同的设计优先级。

### 本地企业网站

**上下文：** 您的主要业务模式（[为本地企业构建网站](../business-model/README-zh.md)）

**设计优先级：**
1. **清晰度胜过创意** - 他们需要客户，不是设计奖项
2. **移动优先** - 大多数搜索是移动的
3. **快速加载** - 紧急搜索需要快速答案
4. **信任信号** - 许可证号，营业年限，真实照片

**常见部分：**
- 带服务区域的英雄
- 服务概述（3-6个关键服务）
- 行动号召（电话，表单）
- 信任标记（持牌，保险，评论）
- 联系信息（电话，时间，位置）

**设计不要：**
- 没有过度的动画
- 没有复杂的导航
- 没有自动播放视频
- 移动设备上没有弹出窗口
- 没有通用库存照片

**模板结构：**
```html
<header>
  <nav>标志 + 电话 + 菜单</nav>
</header>

<main>
  <section class="hero">
    服务 + 位置 + CTA
  </section>

  <section class="services">
    3-6个关键服务
  </section>

  <section class="trust">
    持牌 + 保险 + 评论
  </section>

  <section class="contact">
    表单 + 电话 + 时间 + 地图
  </section>
</main>

<footer>
  联系 + 法律 + 链接
</footer>
```

**另请参阅：** [商业模式指南](../business-model/README-zh.md)

### 着陆页/SaaS

**设计优先级：**
1. **清晰的价值主张**（首屏）
2. **单一转化目标**（不要混淆访问者）
3. **社会证明**（推荐信，标志，指标）
4. **视觉层次**（引导眼睛向下页面）

**常见部分：**
- 英雄（价值主张 + CTA）
- 问题/解决方案
- 功能（3-4个关键功能）
- 社会证明
- 定价（如果适用）
- 最终CTA

**设计不要：**
- 没有多个竞争的CTA
- 没有模糊的利益陈述
- 没有假推荐信
- 没有杂乱的英雄部分

### 作品集/个人网站

**设计优先级：**
1. **首先展示作品**（不是传记）
2. **快速加载**（很多图像）
3. **轻松导航**（到特定项目）
4. **联系可访问性**（使雇佣您容易）

**设计不要：**
- 没有自动播放任何东西
- 没有过度的个人品牌
- 没有隐藏联系信息
- 没有慢速图像画廊

---

## 设计一致性检查清单

在发布任何客户项目之前使用此。

### 视觉一致性 ✅
- [ ] 边框半径值限制为3-4个选项
- [ ] 间距遵循8pt网格（或定义的系统）
- [ ] 字体排印使用定义的比例（最多6-8个大小）
- [ ] 颜色限制为定义的调色板
- [ ] 阴影使用一致的高度系统
- [ ] 没有随机紫色，除非品牌适合
- [ ] 表情符号使用最少（每页<2个）

### 组件一致性 ✅
- [ ] 所有按钮共享核心样式
- [ ] 所有卡片使用相同结构
- [ ] 所有表单遵循相同模式
- [ ] 所有链接行为一致
- [ ] 所有交互元素都有悬停状态
- [ ] 所有悬停效果都是微妙的

### 交互质量 ✅
- [ ] 所有异步操作上的加载状态
- [ ] 所有交互元素工作
- [ ] 没有损坏的链接或按钮
- [ ] 动画不破坏布局
- [ ] 在375px、768px、1024px响应式
- [ ] 移动设备上没有水平滚动

### 内容质量 ✅
- [ ] 没有通用/模糊标语
- [ ] 页脚文本语法正确
- [ ] 没有占位符文本（lorem，test，TODO）
- [ ] 推荐信是真实的或省略
- [ ] 版权年份当前
- [ ] 所有联系信息准确

### 技术质量 ✅
- [ ] 唯一页面标题
- [ ] 元描述存在
- [ ] Open Graph标签配置
- [ ] 网站图标存在（不是默认）
- [ ] 404页面存在
- [ ] 启用HTTPS

**通过率：** 90%+的检查清单项目 = 发布它
**低于80%：** 不准备好客户审查

---

## 常见设计系统错误

### 错误1：太多变量

```css
/* ❌ 糟糕 - 决策瘫痪 */
:root {
  --spacing-1: 4px;
  --spacing-2: 8px;
  --spacing-3: 12px;
  --spacing-4: 16px;
  --spacing-5: 20px;
  --spacing-6: 24px;
  --spacing-7: 28px;
  --spacing-8: 32px;
  /* ... 20多个间距值 */
}

/* ✅ 良好 - 有用的比例 */
:root {
  --space-2: 8px;
  --space-4: 16px;
  --space-6: 24px;
  --space-8: 32px;
  --space-12: 48px;
  --space-16: 64px;
}
```

**规则：** 更少，精心选择的选项 > 许多令人困惑的选项

### 错误2：不一致的组件使用

```html
<!-- ❌ 糟糕 - 组件看起来不同 -->
<button class="px-4 py-2 rounded-md">按钮1</button>
<button class="px-6 py-3 rounded-lg">按钮2</button>
<button class="px-3 py-1 rounded">按钮3</button>

<!-- ✅ 良好 - 一致基础，清晰变体 -->
<button class="btn btn-primary">按钮1</button>
<button class="btn btn-secondary">按钮2</button>
<button class="btn btn-small">按钮3</button>
```

**规则：** 创建组件类，而不是一次性样式

### 错误3：没有设计令牌

```css
/* ❌ 糟糕 - 到处都是魔法数字 */
.card { padding: 24px; }
.section { padding: 96px 0; }
.button { padding: 12px 24px; }
.modal { padding: 32px; }

/* ✅ 良好 - 命名令牌 */
.card { padding: var(--space-6); }
.section { padding: var(--space-24) 0; }
.button { padding: var(--space-3) var(--space-6); }
.modal { padding: var(--space-8); }
```

**规则：** 定义一次，在任何地方使用

---

## AI辅助设计一致性

### 一致性检查提示

在审查AI生成的代码时使用此：

```
审查此代码的设计一致性问题：

[粘贴HTML/CSS]

检查：
1. 不一致的间距值（不在8pt网格上）
2. 随机边框半径值
3. 字体排印不遵循比例
4. 随机变化的阴影值
5. 只使用一次的颜色值
6. 破坏布局的悬停效果
7. 缺少加载状态
8. 非功能性交互元素

列出所有问题并使用设计令牌系统建议修复。
```

### 组件创建提示

```
创建一个[组件类型]遵循这些设计标准：

- 8pt间距网格（8, 16, 24, 32, 48, 64, 96px）
- 边框半径：卡片8px，按钮6px
- 字体排印：text-base（16px）正文，text-xl（20px）标题
- 颜色：[您的品牌颜色]
- 阴影：卡片使用0 4px 6px rgba(0,0,0,0.1)
- 过渡：交互使用0.2s ease

组件应该：
- 在移动设备上工作（375px+）
- 如果交互式，有适当的加载状态
- 使用语义HTML
- 为交互元素包含悬停状态
```

**另请参阅：** [质量聚焦提示](../prompting/quality-focused-prompts-zh.md)

---

## 资源

### 设计系统示例
- [Tailwind UI](https://tailwindui.com/) - 结构良好的组件
- [Shadcn/ui](https://ui.shadcn.com/) - 一致的组件模式
- [Material Design](https://material.io/) - 全面设计系统

### 工具
- [Coolors](https://coolors.co/) - 调色板生成
- [Type Scale](https://typescale.com/) - 字体排印比例计算器
- [Contrast Checker](https://webaim.org/resources/contrastchecker/) - WCAG对比度比率
- Chrome DevTools - 检查间距/大小

### 相关文档
- [质量门](./quality-gates-zh.md) - 发布前检查清单
- [可访问性](./accessibility-zh.md) - WCAG合规
- [性能](./performance-zh.md) - 速度优化
- [SEO](./seo-zh.md) - 搜索优化
- [质量聚焦提示](../prompting/quality-focused-prompts-zh.md) - 质量的AI提示

---

## 关键要点

### 质量 = 一致性

**专业网站有：**
- 可预测的间距
- 一致的组件
- 系统化的字体排印
- 有意的颜色
- 微妙的交互
- 工作功能

**业余网站有：**
- 随机间距
- 不一致的组件
- 随机字体排印
- 任意颜色
- 混乱的交互
- 损坏功能

### 从系统开始，而不是样式

**错误方法：** 构建时为每个元素设置样式

**正确方法：** 首先定义设计令牌，然后使用这些令牌构建组件

### 测试一切

**发布前：**
- 测试所有交互元素
- 检查间距一致性
- 验证字体排印层次结构
- 测试响应式断点
- 移除占位符内容
- 验证元标签

### 设计一致性 = 更高费率

专业、一致的设计证明您的[$300-700定价](../business-model/pricing-economics-zh.md)合理，并将您与廉价替代品区分开来。

---

**相关指南：**
- [质量门检查清单](./quality-gates-zh.md) - 快速发布前检查
- [质量标准概述](./README-zh.md) - 所有质量支柱
- [商业模式](../business-model/README-zh.md) - 质量如何驱动收入
- [工作流程指南](../workflow/README-zh.md) - 何时检查质量

**返回：** [主指南](../../README-zh.md)