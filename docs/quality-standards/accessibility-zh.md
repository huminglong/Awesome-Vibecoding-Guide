# 可访问性标准

## 概述

Web可访问性确保网站和应用程序对每个人都可用，包括残疾人。本指南专注于WCAG 2.1 AA级合规性，这是全球许多法律和法规要求的标准（包括ADA、第508节和欧洲可访问性法律）。

**为什么这对您的客户很重要：**
- **法律合规** - 避免诉讼和罚款（美国ADA诉讼和解费用为$5,000-$30,000+）
- **市场覆盖** - 全球15%的人口有某种形式的残疾
- **SEO好处** - 许多可访问性实践提高搜索排名
- **更好的用户体验** - 可访问的网站对所有用户都更容易使用
- **现代标准** - 专业网站符合可访问性标准

## POUR原则

WCAG建立在四个基础原则之上。每个可访问性要求都适合其中之一：

### 1. 可感知
信息和UI组件必须以用户可以感知的方式呈现。

**关键实践：**
- 为非文本内容（图像、图标、音频）提供文本替代
- 为视频和音频提供字幕和转录
- 确保内容可以以不同方式呈现而不失去意义
- 使内容易于看到和听到（颜色对比、文本大小、音频清晰度）

### 2. 可操作
UI组件和导航必须对所有用户都可操作。

**关键实践：**
- 使所有功能都可通过键盘使用
- 给用户足够的时间阅读和使用内容
- 不要设计导致癫痫发作的内容（每秒闪烁不超过3次）
- 帮助用户导航和查找内容（清晰的标题、跳过链接、面包屑）
- 支持除鼠标/触摸之外的多种输入方法

### 3. 可理解
信息和UI操作必须可理解。

**关键实践：**
- 使文本可读和可理解（清晰的语言，定义行话）
- 使内容以可预测的方式出现和操作
- 帮助用户避免和纠正错误（清晰的错误消息、表单验证）
- 提供清晰的指令和标签

### 4. 健壮
内容必须足够健壮，可以被各种用户代理（包括辅助技术）解释。

**关键实践：**
- 使用有效的、语义HTML
- 确保与当前和未来辅助技术的兼容性
- 当HTML语义不足时提供适当的ARIA属性
- 使用实际的屏幕阅读器和辅助技术进行测试

## WCAG 2.1 AA级合规检查清单

此检查清单涵盖最重要的AA级要求。在构建或审查任何网站时使用它。

### 视觉设计和颜色

#### ✅ 颜色对比（1.4.3、1.4.11）
- **常规文本**：与背景的最小4.5:1对比度
- **大文本**（18pt+或14pt+粗体）：最小3:1对比度
- **UI组件**（按钮、表单边框、焦点指示器）：最小3:1对比度
- **图形和图标**：有意义元素的最小3:1对比度

**测试**：使用浏览器DevTools或在线对比度检查器

**常见错误：**
```html
<!-- ❌ 对比度差 - 白色上的浅灰色 -->
<p style="color: #CCCCCC; background: #FFFFFF;">难以阅读的文本</p>

<!-- ✅ 对比度好 - 白色上的深灰色（7:1） -->
<p style="color: #595959; background: #FFFFFF;">易于阅读的文本</p>
```

#### ✅ 不要仅依赖颜色（1.4.1）
使用多个视觉提示（颜色 + 图标、颜色 + 文本、颜色 + 模式）：

```html
<!-- ❌ 仅颜色表示错误 -->
<input type="email" style="border-color: red;">

<!-- ✅ 颜色 + 图标 + 错误文本 -->
<div class="form-field">
  <input type="email" aria-invalid="true" aria-describedby="email-error">
  <p id="email-error" class="error">
    <span class="error-icon" aria-hidden="true">⚠</span>
    请输入有效的电子邮件地址
  </p>
</div>
```

#### ✅ 调整文本大小（1.4.4）
文本必须可调整到200%而不丢失内容或功能：

```css
/* ✅ 使用相对单位，而不是固定像素 */
body {
  font-size: 1rem; /* 16px默认 */
}

h1 {
  font-size: 2.5rem; /* 随用户的字体设置缩放 */
}

/* ❌ 避免文本使用固定像素大小 */
.bad-text {
  font-size: 14px; /* 不尊重用户偏好 */
}
```

### 语义HTML和文档结构

#### ✅ 正确的标题层次（1.3.1、2.4.6）
标题必须遵循逻辑顺序（h1 → h2 → h3）而不跳过级别：

```html
<!-- ✅ 正确的层次 -->
<h1>页面标题</h1>
  <h2>主要部分</h2>
    <h3>子部分</h3>

<!-- ❌ 不正确 - 从h1跳到h3 -->
<h1>页面标题</h1>
  <h3>部分</h3> <!-- 应该是h2 -->
```

**为什么重要**：屏幕阅读器用户通过标题导航。清晰的层次结构有助于他们理解页面结构。

#### ✅ 语义HTML元素（1.3.1、4.1.2）
为预期目的使用正确的HTML元素：

```html
<!-- ✅ 语义标记 -->
<nav aria-label="主导航">
  <ul>
    <li><a href="/">首页</a></li>
    <li><a href="/about">关于</a></li>
    <li><a href="/contact">联系</a></li>
  </ul>
</nav>

<main>
  <article>
    <h1>文章标题</h1>
    <p>文章内容...</p>
  </article>
  
  <aside>
    <h2>相关链接</h2>
    <ul>
      <li><a href="/related1">相关文章1</a></li>
    </ul>
  </aside>
</main>

<footer>
  <p>&copy; 2025 公司名称</p>
</footer>

<!-- ❌ 非语义div -->
<div class="nav">
  <div class="nav-item">首页</div>
  <div class="nav-item">关于</div>
</div>
```

**关键语义元素：**
- `<header>`、`<nav>`、`<main>`、`<article>`、`<section>`、`<aside>`、`<footer>`
- `<button>`用于可点击操作（不是`<div onclick="">`）
- `<a>`用于导航（不是`<div>`或`<span>`）
- `<ul>`/`<ol>`用于列表
- `<table>`用于表格数据（不用于布局）

#### ✅ 地标和页面区域（1.3.1、2.4.1）
使用ARIA地标定义页面区域：

```html
<!-- ✅ 具有地标的清晰页面结构 -->
<body>
  <a href="#main" class="skip-link">跳转到主要内容</a>
  
  <header>
    <nav aria-label="主导航">
      <ul>
        <li><a href="/">首页</a></li>
        <li><a href="/about">关于</a></li>
      </ul>
    </nav>
  </header>
  
  <main id="main">
    <h1>页面标题</h1>
    <article>
      <h2>文章标题</h2>
      <p>内容...</p>
    </article>
  </main>
  
  <aside aria-label="侧边栏">
    <h2>相关内容</h2>
    <!-- 侧边栏内容 -->
  </aside>
  
  <footer>
    <p>&copy; 2025 公司名称</p>
  </footer>
</body>
```

### 键盘导航

#### ✅ 键盘可访问（2.1.1、2.1.2）
所有交互元素必须可以通过键盘访问（Tab、Enter、Space、箭头键）：

**测试检查清单：**
1. 您可以通过Tab浏览所有交互元素吗？
2. 焦点顺序逻辑吗？
3. 您可以使用Enter或Space激活按钮/链接吗？
4. 您可以使用Escape关闭模态框吗？
5. 您可以使用箭头键导航下拉菜单吗？
6. 没有键盘陷阱（您总是可以Tab出去）吗？

```html
<!-- ✅ 键盘可访问按钮 -->
<button onclick="submitForm()">提交</button>

<!-- ❌ 不可键盘访问 -->
<div onclick="submitForm()">提交</div>

<!-- ✅ 使div可键盘访问（如果必须使用div） -->
<div role="button" tabindex="0" onclick="submitForm()"
     onkeydown="if(event.key==='Enter'||event.key===' ')submitForm()">
  提交
</div>
```

#### ✅ 可见焦点指示器（2.4.7）
用户必须能够看到哪个元素具有键盘焦点：

```css
/* ✅ 清晰的焦点指示器 */
a:focus,
button:focus,
input:focus {
  outline: 3px solid #0066cc;
  outline-offset: 2px;
}

/* ❌ 永远不要在没有替换的情况下移除焦点轮廓 */
*:focus {
  outline: none; /* 不要这样做！ */
}

/* ✅ 自定义焦点样式（如果您不喜欢浏览器默认） */
.custom-button:focus {
  outline: 3px solid #0066cc;
  box-shadow: 0 0 0 4px rgba(0, 102, 204, 0.2);
}
```

**Tailwind CSS方法：**
```html
<button class="focus:outline-none focus:ring-4 focus:ring-blue-500 focus:ring-offset-2">
  点击我
</button>
```

#### ✅ 跳过链接（2.4.1）
为键盘用户提供跳过重复内容的方法：

```html
<!-- ✅ 跳过链接（页面上第一个可聚焦元素） -->
<a href="#main" class="skip-link">跳转到主要内容</a>

<style>
  .skip-link {
    position: absolute;
    top: -40px;
    left: 0;
    background: #000;
    color: #fff;
    padding: 8px;
    text-decoration: none;
    z-index: 100;
  }
  
  .skip-link:focus {
    top: 0;
  }
</style>
```

### 图像和替代文本

#### ✅ 图像的Alt文本（1.1.1）
每个`<img>`必须有`alt`属性：

```html
<!-- ✅ 信息性图像 - 描述重要的内容 -->
<img src="chart.png" alt="销售额从Q1到Q2增长了25%">

<!-- ✅ 功能性图像（例如，按钮/链接） - 描述功能 -->
<a href="/search">
  <img src="search-icon.png" alt="搜索">
</a>

<!-- ✅ 装饰性图像 - 空alt -->
<img src="decorative-border.png" alt="">

<!-- ❌ 缺少alt -->
<img src="important-chart.png">

<!-- ❌ 冗余alt -->
<img src="photo.jpg" alt="一张显示...的照片">
<!-- 更好： -->
<img src="photo.jpg" alt="团队在办公室庆祝产品发布">
```

**Alt文本指南：**
- **简洁** - 目标是150个字符以下
- **不要以"图像"或"图片"开头** - 屏幕阅读器已经宣布它是图像
- **描述内容和功能** - 图像传达什么信息？
- **上下文很重要** - Alt文本应该适合周围内容
- **装饰性图像** - 使用`alt=""`（不是缺少alt属性）
- **复杂图像** - 考虑使用`longdesc`或附近的文本描述

#### ✅ SVG可访问性
```html
<!-- ✅ 可访问的SVG图标 -->
<svg role="img" aria-labelledby="icon-title">
  <title id="icon-title">设置</title>
  <path d="..."/>
</svg>

<!-- ✅ 装饰性SVG -->
<svg aria-hidden="true" focusable="false">
  <path d="..."/>
</svg>
```

### 表单和输入

#### ✅ 表单标签（1.3.1、3.3.2）
每个输入必须有可见的、关联的标签：

```html
<!-- ✅ 正确的标签关联 -->
<label for="email">电子邮件地址</label>
<input type="email" id="email" name="email" required>

<!-- ✅ 替代：将输入包装在标签中 -->
<label>
  电子邮件地址
  <input type="email" name="email" required>
</label>

<!-- ❌ 缺少标签 -->
<input type="email" placeholder="电子邮件"> <!-- 占位符不是标签！ -->

<!-- ✅ 如果必须视觉上隐藏标签（罕见！） -->
<label for="search" class="sr-only">搜索</label>
<input type="search" id="search" placeholder="搜索...">
```

**屏幕阅读器仅类：**
```css
.sr-only {
  position: absolute;
  width: 1px;
  height: 1px;
  padding: 0;
  margin: -1px;
  overflow: hidden;
  clip: rect(0, 0, 0, 0);
  white-space: nowrap;
  border-width: 0;
}
```

#### ✅ 必填字段指示（3.3.2）
清楚地指示必填字段：

```html
<!-- ✅ 多个指示器 -->
<label for="name">
  全名 <span class="required" aria-label="必填">*</span>
</label>
<input type="text" id="name" required aria-required="true">

<!-- ✅ 文本指示器 -->
<label for="phone">电话号码（必填）</label>
<input type="tel" id="phone" required aria-required="true">
```

#### ✅ 错误消息（3.3.1、3.3.3）
提供清晰、具体的错误消息：

```html
<!-- ✅ 可访问的错误消息 -->
<div class="form-field">
  <label for="email">电子邮件地址</label>
  <input 
    type="email" 
    id="email" 
    name="email"
    aria-invalid="true"
    aria-describedby="email-error"
  >
  <p id="email-error" class="error" role="alert">
    <span class="error-icon" aria-hidden="true">⚠</span>
    请输入有效的电子邮件地址（例如，name@example.com）
  </p>
</div>

<!-- ❌ 模糊错误 -->
<p>无效输入</p> <!-- 哪个字段？有什么问题？如何修复？ -->

<!-- ✅ 好错误 -->
<p>电子邮件地址必须包含@符号和域名（例如，name@example.com）</p>
```

**具有多个错误的表单的错误摘要：**
```html
<div role="alert" class="error-summary">
  <h2>此表单中有3个错误</h2>
  <ul>
    <li><a href="#email-field">电子邮件地址无效</a></li>
    <li><a href="#phone-field">电话号码必填</a></li>
    <li><a href="#zip-field">邮政编码格式错误</a></li>
  </ul>
</div>
```

#### ✅ 输入目的（1.3.5）
使用autocomplete属性识别输入目的：

```html
<form>
  <label for="name">全名</label>
  <input type="text" id="name" autocomplete="name">
  
  <label for="email">电子邮件</label>
  <input type="email" id="email" autocomplete="email">
  
  <label for="phone">电话</label>
  <input type="tel" id="phone" autocomplete="tel">
  
  <label for="address">地址</label>
  <input type="text" id="address" autocomplete="street-address">
  
  <label for="city">城市</label>
  <input type="text" id="city" autocomplete="address-level2">
  
  <label for="zip">邮政编码</label>
  <input type="text" id="zip" autocomplete="postal-code">
</form>
```

### ARIA（可访问的富互联网应用程序）

ARIA填补HTML语义不足的空白。**规则#1：如果HTML语义有效，不要使用ARIA。**

#### 常见ARIA属性

**aria-label** - 提供可访问名称：
```html
<button aria-label="关闭对话框">×</button>
<nav aria-label="主导航">...</nav>
```

**aria-labelledby** - 引用另一个元素作为可访问名称：
```html
<section aria-labelledby="section-title">
  <h2 id="section-title">关于我们</h2>
  ...
</section>
```

**aria-describedby** - 提供额外描述：
```html
<input
  type="password"
  aria-describedby="password-requirements"
>
<p id="password-requirements">
  必须至少8个字符，包含1个数字和1个特殊字符
</p>
```

**aria-hidden** - 从屏幕阅读器隐藏内容：
```html
<!-- 装饰性图标，不宣布 -->
<span class="icon" aria-hidden="true">★</span>
<span>特色</span>
```

**aria-live** - 宣布动态内容更改：
```html
<!-- 礼貌：等待用户暂停 -->
<div aria-live="polite" aria-atomic="true" class="status-message">
  <!-- 状态消息出现在这里 -->
</div>

<!-- 断言：立即中断 -->
<div aria-live="assertive" role="alert">
  <!-- 关键错误出现在这里 -->
</div>
```

**aria-expanded** - 指示元素是否展开：
```html
<button
  aria-expanded="false"
  aria-controls="dropdown-menu"
  onclick="toggleDropdown()"
>
  菜单
</button>
<ul id="dropdown-menu" hidden>
  <li><a href="/home">首页</a></li>
  <li><a href="/about">关于</a></li>
</ul>
```

**aria-current** - 指示当前项目：
```html
<nav>
  <a href="/home" aria-current="page">首页</a>
  <a href="/about">关于</a>
  <a href="/services">服务</a>
  <a href="/contact">联系</a>
</nav>
```

#### ✅ ARIA Live区域
对于动态内容更新：

```html
<!-- 状态消息（礼貌） -->
<div role="status" aria-live="polite" aria-atomic="true">
  <!-- "购物车中添加了5件商品" -->
</div>

<!-- 紧急警报（断言） -->
<div role="alert" aria-live="assertive">
  <!-- "错误：付款失败" -->
</div>

<!-- 加载指示器 -->
<div aria-live="polite" aria-busy="true">
  加载中...
</div>
```

### 交互组件

#### ✅ 模态对话框（2.4.3）
```html
<div
  role="dialog"
  aria-modal="true"
  aria-labelledby="dialog-title"
  aria-describedby="dialog-description"
  class="modal"
>
  <h2 id="dialog-title">确认操作</h2>
  <p id="dialog-description">您确定要删除此项目吗？此操作无法撤销。</p>
  <button onclick="confirmAction()">确认</button>
  <button onclick="closeDialog()">取消</button>
</div>

<script>
// 模态框的可访问性要求：
// 1. 焦点陷阱 - Tab在模态框内循环
// 2. Escape键关闭
// 3. 焦点管理 - 打开时聚焦第一个可聚焦元素
// 4. 关闭时返回焦点到触发元素
// 5. 防止背景滚动
// 6. aria-modal="true"从屏幕阅读器隐藏背景
</script>
```

#### ✅ 下拉菜单
```html
<div class="dropdown">
  <button
    aria-haspopup="true"
    aria-expanded="false"
    aria-controls="dropdown-menu"
    onclick="toggleDropdown()"
  >
    菜单
  </button>
  <ul id="dropdown-menu" role="menu" hidden>
    <li role="none"><a href="/home" role="menuitem">首页</a></li>
    <li role="none"><a href="/about" role="menuitem">关于</a></li>
    <li role="none"><a href="/contact" role="menuitem">联系</a></li>
  </ul>
</div>

<script>
// 键盘要求：
// - 箭头键导航菜单项
// - Enter激活项目
// - Escape关闭菜单
// - Tab关闭菜单并移动到下一个元素
</script>
```

#### ✅ 手风琴
```html
<div class="accordion">
  <h3>
    <button
      aria-expanded="false"
      aria-controls="panel1"
      onclick="toggleAccordion('panel1')"
    >
      第1部分
    </button>
  </h3>
  <div id="panel1" role="region" aria-labelledby="accordion1" hidden>
    <p>第1部分的内容...</p>
  </div>
  
  <h3>
    <button
      aria-expanded="false"
      aria-controls="panel2"
      onclick="toggleAccordion('panel2')"
    >
      第2部分
    </button>
  </h3>
  <div id="panel2" role="region" aria-labelledby="accordion2" hidden>
    <p>第2部分的内容...</p>
  </div>
</div>
```

#### ✅ 标签页
```html
<div class="tabs">
  <div role="tablist" aria-label="内容部分">
    <button
      role="tab"
      aria-selected="true"
      aria-controls="panel1"
      id="tab1"
      onclick="switchTab('panel1')"
    >
      标签1
    </button>
    <button
      role="tab"
      aria-selected="false"
      aria-controls="panel2"
      id="tab2"
      onclick="switchTab('panel2')"
    >
      标签2
    </button>
  </div>
  
  <div
    id="panel1"
    role="tabpanel"
    aria-labelledby="tab1"
    tabindex="0"
  >
    <p>标签1的内容...</p>
  </div>
  
  <div
    id="panel2"
    role="tabpanel"
    aria-labelledby="tab2"
    tabindex="0"
    hidden
  >
    <p>标签2的内容...</p>
  </div>
</div>

<script>
// 键盘要求：
// - 箭头键在标签之间移动
// - Tab键移动到面板内容
// - Home/End跳转到第一个/最后一个标签
</script>
```

## Astro特定的可访问性模式

### 可访问的Astro组件

```astro
---
// src/components/AccessibleButton.astro
interface Props {
  variant?: 'primary' | 'secondary';
  type?: 'button' | 'submit' | 'reset';
  ariaLabel?: string;
  disabled?: boolean;
}

const { variant = 'primary', type = 'button', ariaLabel, disabled = false } = Astro.props;
---

<button
  type={type}
  class={`btn btn-${variant}`}
  aria-label={ariaLabel}
  disabled={disabled}
>
  <slot />
</button>

<style>
  .btn {
    padding: 0.75rem 1.5rem;
    border-radius: 0.375rem;
    font-weight: 600;
    cursor: pointer;
    transition: all 0.2s;
  }
  
  .btn:focus {
    outline: 3px solid #0066cc;
    outline-offset: 2px;
  }
  
  .btn-primary {
    background-color: #0066cc;
    color: white;
  }
  
  .btn-secondary {
    background-color: #f3f4f6;
    color: #1f2937;
  }
  
  .btn:disabled {
    opacity: 0.5;
    cursor: not-allowed;
  }
</style>
```

### 可访问的导航组件

```astro
---
// src/components/Navigation.astro
const currentPath = Astro.url.pathname;
const navItems = [
  { href: '/', label: '首页' },
  { href: '/about', label: '关于' },
  { href: '/services', label: '服务' },
  { href: '/contact', label: '联系' },
];
---

<nav aria-label="主导航">
  <ul>
    {navItems.map((item) => (
      <li>
        <a
          href={item.href}
          aria-current={currentPath === item.href ? 'page' : undefined}
        >
          {item.label}
        </a>
      </li>
    ))}
  </ul>
</nav>

<style>
  nav ul {
    display: flex;
    list-style: none;
    gap: 1.5rem;
    padding: 0;
    margin: 0;
  }
  
  nav a {
    text-decoration: none;
    color: #1f2937;
    font-weight: 500;
    padding: 0.5rem 0;
    border-bottom: 2px solid transparent;
  }
  
  nav a:hover,
  nav a:focus {
    color: #0066cc;
  }
  
  nav a:focus {
    outline: 3px solid #0066cc;
    outline-offset: 2px;
  }
  
  nav a[aria-current="page"] {
    border-bottom-color: #0066cc;
    color: #0066cc;
  }
</style>
```

### 可访问的模态框组件

```astro
---
// src/components/Modal.astro
interface Props {
  id: string;
  title: string;
}

const { id, title } = Astro.props;
---

<div id={id} class="modal-backdrop" aria-hidden="true">
  <div
    role="dialog"
    aria-modal="true"
    aria-labelledby={`${id}-title`}
    class="modal"
  >
    <header>
      <h2 id={`${id}-title`}>{title}</h2>
      <button
        aria-label="关闭对话框"
        onclick="closeModal()"
        class="close-button"
      >
        ×
      </button>
    </header>
    <div class="modal-content">
      <slot />
    </div>
  </div>
</div>

<script>
  // 焦点陷阱和键盘处理
  let focusableElements: HTMLElement[] = [];
  let firstFocusable: HTMLElement;
  let lastFocusable: HTMLElement;
  let previousFocus: HTMLElement;

  function openModal() {
    const modal = document.querySelector('.modal') as HTMLElement;
    const backdrop = document.querySelector('.modal-backdrop') as HTMLElement;
    
    // 保存之前的焦点
    previousFocus = document.activeElement as HTMLElement;
    
    // 显示模态框
    backdrop.setAttribute('aria-hidden', 'false');
    modal.setAttribute('aria-hidden', 'false');
    
    // 获取可聚焦元素
    focusableElements = Array.from(
      modal.querySelectorAll(
        'button, [href], input, select, textarea, [tabindex]:not([tabindex="-1"])'
      )
    );
    
    firstFocusable = focusableElements[0];
    lastFocusable = focusableElements[focusableElements.length - 1];
    
    // 聚焦第一个元素
    firstFocusable?.focus();
    
    // 防止背景滚动
    document.body.style.overflow = 'hidden';
  }

  function closeModal() {
    const modal = document.querySelector('.modal') as HTMLElement;
    const backdrop = document.querySelector('.modal-backdrop') as HTMLElement;
    
    // 隐藏模态框
    backdrop.setAttribute('aria-hidden', 'true');
    modal.setAttribute('aria-hidden', 'true');
    
    // 恢复焦点
    previousFocus?.focus();
    
    // 恢复滚动
    document.body.style.overflow = '';
  }

  function handleKeyDown(e: KeyboardEvent) {
    if (e.key === 'Escape') {
      closeModal();
    } else if (e.key === 'Tab') {
      if (e.shiftKey) {
        if (document.activeElement === firstFocusable) {
          e.preventDefault();
          lastFocusable?.focus();
        }
      } else {
        if (document.activeElement === lastFocusable) {
          e.preventDefault();
          firstFocusable?.focus();
        }
      }
    }
  }

  // 添加键盘事件监听器
  document.addEventListener('keydown', handleKeyDown);
</script>

<style>
  .modal-backdrop {
    position: fixed;
    inset: 0;
    background: rgba(0, 0, 0, 0.5);
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 1000;
  }

  .modal {
    background: white;
    border-radius: 0.5rem;
    padding: 2rem;
    max-width: 500px;
    width: 90%;
    max-height: 90vh;
    overflow-y: auto;
  }

  .modal header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 1.5rem;
  }

  .close-button {
    background: none;
    border: none;
    font-size: 1.5rem;
    cursor: pointer;
    padding: 0.5rem;
  }

  .close-button:focus {
    outline: 3px solid #0066cc;
    outline-offset: 2px;
  }
</style>
```

### 可访问的表单组件

```astro
---
// src/components/ContactForm.astro
---

<form class="contact-form" method="POST">
  <div class="form-field">
    <label for="name">姓名</label>
    <input
      type="text"
      id="name"
      name="name"
      required
      aria-required="true"
      autocomplete="name"
    >
  </div>

  <div class="form-field">
    <label for="email">电子邮件</label>
    <input
      type="email"
      id="email"
      name="email"
      required
      aria-required="true"
      autocomplete="email"
      aria-describedby="email-error"
    >
    <p id="email-error" class="error" role="alert"></p>
  </div>

  <div class="form-field">
    <label for="message">消息</label>
    <textarea
      id="message"
      name="message"
      rows="5"
      required
      aria-required="true"
    ></textarea>
  </div>

  <button type="submit">发送消息</button>
</form>

<div role="status" aria-live="polite" class="form-status"></div>

<style>
  .form-field {
    margin-bottom: 1.5rem;
  }

  .form-field label {
    display: block;
    margin-bottom: 0.5rem;
    font-weight: 600;
  }

  .form-field input,
  .form-field textarea {
    width: 100%;
    padding: 0.75rem;
    border: 2px solid #e5e7eb;
    border-radius: 0.375rem;
    font-size: 1rem;
  }

  .form-field input:focus,
  .form-field textarea:focus {
    outline: 3px solid #0066cc;
    outline-offset: 2px;
    border-color: #0066cc;
  }

  .form-field .error {
    color: #dc2626;
    font-size: 0.875rem;
    margin-top: 0.5rem;
  }

  button[type="submit"] {
    background-color: #0066cc;
    color: white;
    padding: 0.75rem 2rem;
    border: none;
    border-radius: 0.375rem;
    font-weight: 600;
    cursor: pointer;
  }

  button[type="submit"]:focus {
    outline: 3px solid #0066cc;
    outline-offset: 2px;
  }

  .form-status {
    margin-top: 1rem;
    padding: 0.75rem;
    border-radius: 0.375rem;
  }

  .form-status.success {
    background-color: #d1fae5;
    color: #065f46;
  }

  .form-status.error {
    background-color: #fee2e2;
    color: #991b1b;
  }
</style>
```

## AI辅助可访问性

### 生成可访问代码的提示

#### 初始组件创建
```
创建一个Astro的可访问[组件类型]组件，它：
- 遵循WCAG 2.1 AA级标准
- 包括适当的ARIA属性
- 具有完整的键盘导航支持
- 包括可见的焦点指示器
- 与屏幕阅读器一起工作
- 具有语义HTML结构

要求：
[列出具体要求]

请包括解释可访问性功能的注释。
```

#### 可访问性审查提示
```
审查此代码是否符合WCAG 2.1 AA级可访问性：

[粘贴代码]

检查：
1. 语义HTML使用
2. 键盘导航
3. ARIA属性（正确使用，不过度使用）
4. 焦点管理
5. 颜色对比（如果存在样式）
6. 图像的替代文本
7. 表单标签和错误消息
8. 标题层次

为发现的任何问题提供具体修复。
```

#### 表单可访问性提示
```
创建一个用于[目的]的可访问表单，包括：
- 所有输入的正确标签关联
- 清晰的必填字段指示器
- 输入目的（autocomplete属性）
- 具有可访问错误消息的客户端验证
- 表单顶部的错误摘要
- 动态消息的ARIA live区域
- 键盘可访问的提交按钮

遵循WCAG 2.1 AA级标准。
```

#### 交互组件提示
```
创建一个可访问的[模态框/下拉菜单/手风琴/标签页]组件，它：
- 具有适当的ARIA角色和属性
- 实现键盘导航（指定键）
- 适当地管理焦点
- 向屏幕阅读器宣布状态更改
- 在没有JavaScript的情况下工作作为后备
- 包括可见的焦点指示器

使用vanilla JavaScript或指定您是否需要框架。
```

### AI可访问性审查工作流程

**步骤1：初始生成**
- 使用AI生成具有提示中可访问性要求的组件
- 请求解释可访问性功能的注释

**步骤2：自动化测试**
- 运行Lighthouse可访问性审计
- 运行axe DevTools扫描
- 使用WAVE扩展检查

**步骤3：AI审查**
- 要求AI审查代码的可访问性问题
- 提供有关任何自动化工具警告的上下文
- 请求具体修复

**步骤4：手动测试**
- 自己测试键盘导航
- 使用实际屏幕阅读器（NVDA/VoiceOver）测试
- 使用工具验证颜色对比
- 测试表单验证

**步骤5：迭代改进**
- 记录发现的任何问题
- 使用AI修复特定问题
- 修复后重新测试

### AI可访问性工作的上下文

将其包含在您的.cursorrules或项目上下文中：

```markdown
# 可访问性标准

所有代码必须符合WCAG 2.1 AA级标准：

## 始终包括：
- 语义HTML（header、nav、main、article、section、aside、footer）
- 正确的标题层次（h1-h6，不跳过级别）
- 所有图像的Alt文本（装饰性使用alt=""）
- 所有输入的表单标签（使用<label for="id">）
- 键盘可访问性（所有交互元素可聚焦）
- 可见焦点指示器（轮廓或自定义焦点样式）
- 仅在HTML语义不足时使用ARIA属性
- 颜色对比：文本4.5:1，大文本和UI组件3:1

## 永远不要：
- 在没有替换的情况下移除焦点轮廓
- 使用divs/spans作为按钮（使用<button>）
- 跳过标题级别
- 仅依赖颜色传达信息
- 使用占位符作为标签替换
- 创建键盘陷阱
- 使用正tabindex值（tabindex="1"、"2"等）

## 使用以下测试：
- 仅键盘（Tab、Enter、Space、Escape、箭头键）
- 屏幕阅读器（Windows上的NVDA，Mac上的VoiceOver）
- Lighthouse可访问性审计（目标：100分）
- axe DevTools
```

## 测试工具和工作流程

### 浏览器DevTools - Chrome Lighthouse

**如何运行：**
1. 打开Chrome DevTools（F12）
2. 点击"Lighthouse"标签
3. 选择"可访问性"类别
4. 选择"桌面"或"移动"
5. 点击"分析页面加载"

**目标分数**：95-100（100是理想的）

**发现的常见问题：**
- 缺少alt文本
- 低颜色对比
- 缺少表单标签
- 缺少ARIA属性
- 不正确的标题顺序

### axe DevTools

**安装**：Chrome/Firefox/Edge扩展（免费）

**如何使用：**
1. 安装扩展
2. 打开DevTools
3. 点击"axe DevTools"标签
4. 点击"扫描我的所有页面"
5. 按严重程度审查问题（关键、严重、中等、轻微）

**好处：**
- 自动捕获约57%的WCAG问题
- 为每个问题提供具体修复
- 在页面中突出显示受影响的元素
- 为客户导出报告

### WAVE浏览器扩展

**安装**：Chrome/Firefox/Edge扩展（免费）

**如何使用：**
1. 安装扩展
2. 点击工具栏中的WAVE图标
3. 审查页面上的颜色编码注释
4. 检查侧边栏的摘要

**它显示：**
- 错误（红色）- 必须修复
- 警告（黄色）- 需要审查
- 功能（绿色）- 存在可访问性功能
- 结构元素
- ARIA使用

### 屏幕阅读器测试

#### Windows：NVDA（免费）

**下载**：https://www.nvaccess.org/download/

**基本命令：**
- **Ctrl** - 停止阅读
- **Insert + 向下箭头** - 阅读全部
- **H** - 下一个标题
- **Shift + H** - 上一个标题
- **K** - 下一个链接
- **B** - 下一个按钮
- **F** - 下一个表单字段
- **Tab** - 下一个交互元素

#### Mac：VoiceOver（内置）

**激活**：Cmd + F5

**基本命令：**
- **VO键** - Control + Option（一起按住）
- **VO + A** - 开始阅读
- **VO + 向右箭头** - 下一个项目
- **VO + 向左箭头** - 上一个项目
- **VO + Space** - 激活元素
- **VO + U** - 打开转子（标题、链接等）

#### 测试检查清单：
1. 您可以使用屏幕阅读器导航整个页面吗？
2. 所有图像都得到适当描述吗？
3. 表单字段是否清晰标记？
4. 错误消息是否宣布？
5. 您可以从标题理解页面结构吗？
6. 按钮/链接的目的清楚吗？
7. 动态更新是否宣布（ARIA live区域）？

### 手动键盘测试

**仅使用键盘测试（无鼠标）：**

1. **Tab浏览页面**
   - 所有交互元素都可到达？
   - 焦点顺序逻辑吗？
   - 焦点在所有元素上可见吗？

2. **测试交互元素**
   - 按钮使用Enter/Space激活？
   - 链接使用Enter激活？
   - 下拉菜单使用箭头键导航？

3. **测试模态框**
   - 可以用键盘打开吗？
   - 可以用Escape关闭吗？
   - 焦点是否正确管理？
   - 关闭时焦点是否返回到触发器？

4. **测试表单**
   - 所有字段都可以用Tab到达吗？
   - 错误消息是否宣布？
   - 可以用键盘纠正错误吗？

5. **测试自定义组件**
   - 手风琴使用Enter打开/关闭？
   - 标签页使用箭头键导航？
   - 所有功能都可以通过键盘使用吗？

### 颜色对比测试

**工具：**
- **Chrome DevTools** - 检查元素 → 颜色选择器显示对比度
- **WebAIM对比度检查器** - https://webaim.org/resources/contrastchecker/
- **颜色对比分析器** - 桌面应用程序（Windows/Mac）

**要求：**
- 常规文本：4.5:1最小
- 大文本（18pt+或14pt+粗体）：3:1最小
- UI组件和图形：3:1最小

**常见问题：**
- 白色背景上的浅灰色文本
- 彩色背景上的彩色文本
- 按钮边框和焦点指示器
- 链接颜色

### CI/CD中的自动化测试

**Lighthouse CI：**
```bash
# 安装
npm install -g @lhci/cli

# 运行
lhci autorun --collect.url=http://localhost:3000

# 在GitHub Actions中
- name: Lighthouse CI
  run: |
    npm install -g @lhci/cli
    lhci autorun --collect.url=http://localhost:3000
```

**axe-core（Playwright/Cypress）：**
```javascript
// Playwright
import { test, expect } from '@playwright/test';
import AxeBuilder from '@axe-core/playwright';

test('should not have accessibility violations', async ({ page }) => {
  await page.goto('http://localhost:3000');
  const accessibilityScanResults = await new AxeBuilder({ page }).analyze();
  expect(accessibilityScanResults.violations).toEqual([]);
});
```

**Pa11y：**
```bash
# 安装
npm install -g pa11y

# 运行
pa11y http://localhost:3000

# 使用特定标准
pa11y --standard WCAG2AA http://localhost:3000
```

## AI生成代码中的常见可访问性陷阱

### 陷阱#1：Div按钮
```html
<!-- ❌ AI经常生成这个 -->
<div class="button" onclick="submit()">提交</div>

<!-- ✅ 修复 -->
<button onclick="submit()">提交</button>
```

**为什么错误**：Div不可键盘访问，不被宣布为按钮，不支持Enter/Space键。

### 陷阱#2：缺少表单标签
```html
<!-- ❌ AI生成占位符作为"标签" -->
<input type="email" placeholder="电子邮件地址">

<!-- ✅ 修复 -->
<label for="email">电子邮件地址</label>
<input type="email" id="email" placeholder="you@example.com">
```

**为什么错误**：占位符在输入时消失，不是可访问标签，导致可用性问题。

### 陷阱#3：糟糕的颜色对比
```css
/* ❌ AI经常建议时髦但不可访问的颜色 */
.text {
  color: #999; /* 浅灰色 */
  background: #fff; /* 白色 */
}
/* 对比度：2.8:1 - 不符合WCAG */

/* ✅ 修复 */
.text {
  color: #666; /* 更深的灰色 */
  background: #fff;
}
/* 对比度：5.7:1 - 通过WCAG */
```

### 陷阱#4：过度使用ARIA
```html
<!-- ❌ AI添加不必要的ARIA -->
<button role="button" aria-label="提交">提交</button>

<!-- ✅ 修复 - HTML语义足够 -->
<button>提交</button>
```

**为什么错误**：冗余ARIA增加复杂性。ARIA的第一条规则：如果HTML有效，不要使用ARIA。

### 陷阱#5：缺少Alt文本
```html
<!-- ❌ AI生成没有alt的图像 -->
<img src="product.jpg">

<!-- ✅ 修复 -->
<img src="product.jpg" alt="优质皮革钱包，棕色">
```

### 陷阱#6：无焦点指示器
```css
/* ❌ AI生成"干净"的设计 */
* {
  outline: none;
}

/* ✅ 修复 - 始终有可见焦点 */
*:focus {
  outline: 3px solid #0066cc;
  outline-offset: 2px;
}
```

## 可访问性检查清单

### 发布前检查清单

在发布任何页面或组件之前，验证：

#### 视觉设计
- [ ] 所有文本与背景的对比度至少为4.5:1
- [ ] 大文本（18pt+或14pt+粗体）对比度至少为3:1
- [ ] UI组件（按钮、边框、焦点指示器）对比度至少为3:1
- [ ] 信息不仅通过颜色传达
- [ ] 文本可以调整到200%而不破坏布局

#### HTML结构
- [ ] 使用语义HTML元素（header、nav、main、article、section、aside、footer）
- [ ] 标题遵循逻辑层次（h1 → h2 → h3，不跳过）
- [ ] 所有图像都有alt属性（装饰性使用alt=""）
- [ ] 所有表单输入都有关联的标签
- [ ] 使用地标定义页面区域

#### 键盘导航
- [ ] 所有交互元素都可以通过Tab访问
- [ ] 焦点顺序逻辑且直观
- [ ] 所有按钮都可以通过Enter/Space激活
- [ ] 模态框可以用Escape关闭
- [ ] 焦点指示器可见且清晰
- [ ] 没有键盘陷阱

#### ARIA使用
- [ ] 仅在HTML语义不足时使用ARIA
- [ ] ARIA属性正确使用
- [ ] 动态内容使用ARIA live区域
- [ ] 交互组件有适当的ARIA角色
- [ ] 状态变化正确宣布

#### 表单
- [ ] 所有输入都有可见标签
- [ ] 必填字段清楚指示
- [ ] 错误消息具体且可访问
- [ ] 表单验证提供清晰的反馈
- [ ] 使用autocomplete属性

#### 测试
- [ ] 通过Lighthouse可访问性审计（目标：95-100）
- [ ] 通过axe DevTools扫描
- [ ] 通过WAVE扩展检查
- [ ] 通过键盘导航测试
- [ ] 通过屏幕阅读器测试（至少一个）

## 资源

### 官方指南
- [WCAG 2.1指南](https://www.w3.org/WAI/WCAG21/quickref/)
- [WebAIM WCAG检查清单](https://webaim.org/standards/wcag/checklist)
- [ARIA创作实践](https://www.w3.org/WAI/ARIA/apg/)

### 工具
- [axe DevTools](https://www.deque.com/axe/)
- [WAVE](https://wave.webaim.org/)
- [Lighthouse](https://developers.google.com/web/tools/lighthouse/)
- [WebAIM对比度检查器](https://webaim.org/resources/contrastchecker/)

### 学习资源
- [WebAIM可访问性教程](https://webaim.org/techniques/)
- [MDN可访问性](https://developer.mozilla.org/en-US/docs/Web/Accessibility)
- [A11Y项目](https://www.a11yproject.com/)

---

**记住**：可访问性不是附加功能——它是专业网站的基本要求。从一开始就构建可访问性比以后修复更容易、更便宜。