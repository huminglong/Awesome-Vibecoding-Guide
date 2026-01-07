# 国际化（i18n）指南

## 概述

国际化（i18n）使您的站点能够支持多种语言和地区。本指南涵盖了为服务国际受众的基于 Astro 的项目的实施策略。

**何时需要 i18n：**
- 针对多个国家/语言
- 法律要求（欧盟、加拿大需要多语言）
- 市场扩张
- 多样化的用户群

**何时可能不需要：**
- 单一市场/语言
- MVP/早期阶段
- 预算限制
- 小团队

## 关键概念

### 国际化（i18n）与本地化（l10n）

**国际化（i18n）：**
- 技术实施
- 支持多种语言的代码结构
- 由开发者完成一次

**本地化（l10n）：**
- 内容翻译
- 文化适应
- 按语言/地区完成

### 地区格式

**标准格式：** `language-COUNTRY`

示例：
- `en-US` - 英语（美国）
- `en-GB` - 英语（英国）
- `es-ES` - 西班牙语（西班牙）
- `es-MX` - 西班牙语（墨西哥）
- `fr-FR` - 法语（法国）
- `fr-CA` - 法语（加拿大）
- `zh-CN` - 中文（简体，中国）
- `zh-TW` - 中文（繁体，台湾）
- `pt-BR` - 葡萄牙语（巴西）
- `pt-PT` - 葡萄牙语（葡萄牙）

**仅语言：** `en`、`es`、`fr`（当国家不重要时）

### 需要本地化的内容

**内容：**
- UI 文本（按钮、标签、菜单）
- 页面内容（标题、段落）
- 错误消息
- 成功消息
- 表单标签和占位符
- SEO 元标签（标题、描述）

**格式：**
- 日期和时间
- 数字
- 货币
- 电话号码
- 地址

**文化：**
- 图像（人物、旗帜、符号）
- 颜色（含义因文化而异）
- 图标（手势、符号）
- 示例和引用

## Astro i18n 设置
[→ 另请参阅：核心技术 - Astro 框架](../core-technologies-zh.md)

### 项目结构

**方法 1：每个地区单独的页面**

```
src/
├── pages/
│   ├── en/
│   │   ├── index.astro
│   │   ├── about.astro
│   │   └── contact.astro
│   ├── es/
│   │   ├── index.astro
│   │   ├── about.astro (sobre-nosotros.astro)
│   │   └── contact.astro (contacto.astro)
│   └── fr/
│       ├── index.astro
│       ├── about.astro (a-propos.astro)
│       └── contact.astro
```

**URL：**
- `/en/` - 英语主页
- `/es/` - 西班牙语主页
- `/fr/` - 法语主页

**优点：**
- 清晰的分离
- 易于管理
- 每个地区不同的布局

**缺点：**
- 重复代码
- 需要维护更多文件

**方法 2：动态路由（推荐）**

```
src/
├── pages/
│   └── [lang]/
│       ├── index.astro
│       ├── about.astro
│       └── contact.astro
├── i18n/
│   ├── en.json
│   ├── es.json
│   └── fr.json
└── utils/
    └── i18n.ts
```

**优点：**
- 单一代码路径
- 易于添加语言
- 更简洁的代码库

**缺点：**
- 初始设置更复杂

### 配置

**astro.config.mjs：**

```javascript
import { defineConfig } from 'astro/config';

export default defineConfig({
  i18n: {
    defaultLocale: 'en',
    locales: ['en', 'es', 'fr', 'de'],
    routing: {
      prefixDefaultLocale: false, // /en/about 或 /about
      redirectToDefaultLocale: true
    }
  }
});
```

**路由选项：**

```javascript
// 选项 1：为所有地区添加前缀（包括默认地区）
// URL：/en/about、/es/about、/fr/about
{
  prefixDefaultLocale: true
}

// 选项 2：默认地区不加前缀（推荐）
// URL：/about（英语）、/es/about、/fr/about
{
  prefixDefaultLocale: false
}
```

### 翻译文件

**src/i18n/en.json：**
```json
{
  "nav": {
    "home": "Home",
    "about": "About",
    "services": "Services",
    "contact": "Contact"
  },
  "home": {
    "hero": {
      "title": "Professional Plumbing Services",
      "subtitle": "24/7 Emergency Service in Austin, TX",
      "cta": "Get Free Quote"
    },
    "features": {
      "title": "Why Choose Us",
      "licensed": "Licensed & Insured",
      "licensed_desc": "Fully licensed master plumbers",
      "fast": "Same-Day Service",
      "fast_desc": "Available 24/7 for emergencies",
      "quality": "Quality Guaranteed",
      "quality_desc": "100% satisfaction guarantee"
    }
  },
  "contact": {
    "title": "Contact Us",
    "name": "Full Name",
    "email": "Email Address",
    "phone": "Phone Number",
    "message": "Message",
    "submit": "Send Message",
    "success": "Message sent successfully!",
    "error": "Error sending message. Please try again."
  },
  "common": {
    "loading": "Loading...",
    "error": "An error occurred",
    "retry": "Try Again",
    "cancel": "Cancel",
    "save": "Save",
    "delete": "Delete",
    "edit": "Edit",
    "close": "Close"
  }
}
```

**src/i18n/es.json：**
```json
{
  "nav": {
    "home": "Inicio",
    "about": "Nosotros",
    "services": "Servicios",
    "contact": "Contacto"
  },
  "home": {
    "hero": {
      "title": "Servicios Profesionales de Plomería",
      "subtitle": "Servicio de Emergencia 24/7 en Austin, TX",
      "cta": "Cotización Gratis"
    },
    "features": {
      "title": "Por Qué Elegirnos",
      "licensed": "Licenciado y Asegurado",
      "licensed_desc": "Plomeros maestros totalmente licenciados",
      "fast": "Servicio el Mismo Día",
      "fast_desc": "Disponible 24/7 para emergencias",
      "quality": "Calidad Garantizada",
      "quality_desc": "Garantía de satisfacción 100%"
    }
  },
  "contact": {
    "title": "Contáctenos",
    "name": "Nombre Completo",
    "email": "Correo Electrónico",
    "phone": "Número de Teléfono",
    "message": "Mensaje",
    "submit": "Enviar Mensaje",
    "success": "¡Mensaje enviado exitosamente!",
    "error": "Error al enviar mensaje. Por favor intente de nuevo."
  },
  "common": {
    "loading": "Cargando...",
    "error": "Ocurrió un error",
    "retry": "Intentar de Nuevo",
    "cancel": "Cancelar",
    "save": "Guardar",
    "delete": "Eliminar",
    "edit": "Editar",
    "close": "Cerrar"
  }
}
```

### 翻译工具

**src/utils/i18n.ts：**

```typescript
import en from '../i18n/en.json';
import es from '../i18n/es.json';
import fr from '../i18n/fr.json';

export const languages = {
  en: 'English',
  es: 'Español',
  fr: 'Français'
};

export const defaultLang = 'en';

const translations = {
  en,
  es,
  fr
};

export type Language = keyof typeof translations;

export function getLangFromUrl(url: URL): Language {
  const [, lang] = url.pathname.split('/');
  if (lang in translations) return lang as Language;
  return defaultLang;
}

export function useTranslations(lang: Language) {
  return function t(key: string): string {
    const keys = key.split('.');
    let value: any = translations[lang];

    for (const k of keys) {
      value = value?.[k];
    }

    if (!value) {
      console.warn(`Translation missing: ${key} for lang: ${lang}`);
      return key;
    }

    return value;
  };
}

// 获取带有后备的翻译
export function getTranslation(lang: Language, key: string, fallback?: string): string {
  const t = useTranslations(lang);
  const value = t(key);

  if (value === key && fallback) {
    return fallback;
  }

  return value;
}
```

### 在组件中使用翻译

**src/pages/[lang]/index.astro：**

```astro
---
import { getLangFromUrl, useTranslations } from '../../utils/i18n';
import Layout from '../../layouts/Layout.astro';

const lang = getLangFromUrl(Astro.url);
const t = useTranslations(lang);
---

<Layout title={t('home.hero.title')} lang={lang}>
  <main>
    <section class="hero">
      <h1>{t('home.hero.title')}</h1>
      <p>{t('home.hero.subtitle')}</p>
      <a href={`/${lang}/contact`} class="cta-button">
        {t('home.hero.cta')}
      </a>
    </section>

    <section class="features">
      <h2>{t('home.features.title')}</h2>

      <div class="feature">
        <h3>{t('home.features.licensed')}</h3>
        <p>{t('home.features.licensed_desc')}</p>
      </div>

      <div class="feature">
        <h3>{t('home.features.fast')}</h3>
        <p>{t('home.features.fast_desc')}</p>
      </div>

      <div class="feature">
        <h3>{t('home.features.quality')}</h3>
        <p>{t('home.features.quality_desc')}</p>
      </div>
    </section>
  </main>
</Layout>
```

### 语言切换器组件

**src/components/LanguageSwitcher.astro：**

```astro
---
import { languages, getLangFromUrl } from '../utils/i18n';

const currentLang = getLangFromUrl(Astro.url);
const currentPath = Astro.url.pathname.replace(`/${currentLang}`, '');
---

<div class="language-switcher">
  {Object.entries(languages).map(([lang, label]) => (
    <a
      href={`/${lang}${currentPath}`}
      class:list={['lang-link', { active: lang === currentLang }]}
      aria-current={lang === currentLang ? 'true' : undefined}
    >
      {label}
    </a>
  ))}
</div>

<style>
  .language-switcher {
    display: flex;
    gap: 0.5rem;
  }

  .lang-link {
    padding: 0.5rem 1rem;
    text-decoration: none;
    color: #333;
    border-radius: 4px;
  }

  .lang-link.active {
    background: #0066cc;
    color: white;
    font-weight: bold;
  }

  .lang-link:hover {
    background: #e0e0e0;
  }

  .lang-link.active:hover {
    background: #0052a3;
  }
</style>
```

## AI 辅助翻译工作流程
[→ 另请参阅：提示指南](../prompting/README-zh.md)

### 初始翻译

**给 AI 的提示：**

```
将此 JSON 文件翻译为西班牙语（es-MX 方言）：

[粘贴 en.json]

要求：
- 精确维护 JSON 结构
- 使用自然、对话式的西班牙语
- 墨西哥西班牙语方言
- 商业网站的专业语调
- 保持占位符完整（例如 {name}）
- 保持 HTML 标签完整

提供完整的翻译 JSON 文件。
```

**对于多种语言：**

```
将 en.json 翻译为以下语言：
1. 西班牙语（es-MX）- 墨西哥西班牙语
2. 法语（fr-FR）- 法国法语
3. 德语（de-DE）- 德国德语

对于每种语言：
- 使用适当的方言
- 专业商务语调
- 自然对话风格
- 维护 JSON 结构

为每种语言提供单独的 JSON 文件。
```

### 上下文感知翻译

**为了获得更好的翻译，提供上下文：**

```
将此联系表单翻译为西班牙语：

上下文：
- 管道业务网站
- 针对德克萨斯州奥斯汀的房主
- 许多客户说西班牙语
- 语调：专业但友好，不过于正式

内容：
{
  "contact": {
    "title": "Contact Us",
    "subtitle": "Get a free quote today",
    ...
  }
}

请翻译为墨西哥西班牙语（es-MX）。
```

### 母语者审查

**AI 翻译应由母语者审查：**

```markdown
# 翻译审查清单

对于每种语言：
- [ ] 语法正确
- [ ] 拼写正确
- [ ] 语调符合品牌（专业/随意/友好）
- [ ] 习语在目标文化中有意义
- [ ] 没有尴尬的措辞
- [ ] 没有冒犯性词汇或含义
- [ ] 技术术语翻译正确
- [ ] 行动号召具有吸引力
- [ ] 格式适当（日期、数字等）
```

**聘请母语者用于：**
- AI 翻译的最终审查
- 文化适宜性检查
- 语调和品牌声音匹配
- 技术术语验证

**预算：**
- 专业翻译每字 $0.10-0.30
- 或在 Upwork 上聘请审查员：$20-50/小时
- 或使用 Fiverr：$20-100/500 字

## RTL（从右到左）语言支持

### RTL 语言

**常见的 RTL 语言：**
- 阿拉伯语（ar）
- 希伯来语（he）
- 波斯语/法尔西语（fa）
- 乌尔都语（ur）

### CSS 注意事项

**使用逻辑属性：**

```css
/* ❌ 不要使用方向性属性 */
.element {
  margin-left: 1rem;
  padding-right: 2rem;
  text-align: left;
}

/* ✅ 使用逻辑属性 */
.element {
  margin-inline-start: 1rem;
  padding-inline-end: 2rem;
  text-align: start;
}
```

**自动 RTL：**

```css
/* 在 html 元素上设置方向 */
html[dir="rtl"] {
  direction: rtl;
}

html[dir="ltr"] {
  direction: ltr;
}

/* 或按语言 */
html[lang="ar"],
html[lang="he"] {
  direction: rtl;
}
```

**布局镜像：**

```css
/* 使用 CSS 变换图标 */
html[dir="rtl"] .icon-arrow-right {
  transform: scaleX(-1);
}

/* 或提供 RTL 特定图标 */
html[dir="rtl"] .icon-arrow {
  background-image: url('/icons/arrow-left.svg');
}

html[dir="ltr"] .icon-arrow {
  background-image: url('/icons/arrow-right.svg');
}
```

### 图标放置

**方向性图标需要翻转：**

```html
<!-- 箭头图标、V 形符号等 -->
<span class="icon" data-icon="arrow-right">→</span>

<style>
  html[dir="rtl"] [data-icon="arrow-right"]::before {
    content: "←"; /* 翻转箭头 */
  }
</style>
```

**非方向性图标不翻转：**
- 搜索图标（🔍）
- 设置图标（⚙️）
- 用户图标（👤）
- 关闭图标（✕）

### 文本方向

```html
<!-- 设置 dir 属性 -->
<html lang="ar" dir="rtl">

<!-- 或动态设置 -->
<html lang={lang} dir={isRTL(lang) ? 'rtl' : 'ltr'}>
```

```typescript
// utils/i18n.ts
export const RTL_LANGUAGES = ['ar', 'he', 'fa', 'ur'];

export function isRTL(lang: string): boolean {
  return RTL_LANGUAGES.includes(lang);
}
```

## 格式化

### 日期和时间

**使用 Intl.DateTimeFormat：**

```typescript
// utils/format.ts

export function formatDate(date: Date, lang: string): string {
  return new Intl.DateTimeFormat(lang, {
    year: 'numeric',
    month: 'long',
    day: 'numeric'
  }).format(date);
}

export function formatTime(date: Date, lang: string): string {
  return new Intl.DateTimeFormat(lang, {
    hour: 'numeric',
    minute: '2-digit',
    hour12: lang === 'en-US' // 美国使用 12 小时制，其他国家使用 24 小时制
  }).format(date);
}

export function formatDateTime(date: Date, lang: string): string {
  return new Intl.DateTimeFormat(lang, {
    year: 'numeric',
    month: 'long',
    day: 'numeric',
    hour: 'numeric',
    minute: '2-digit'
  }).format(date);
}
```

**用法：**

```astro
---
import { formatDate } from '../utils/format';

const lang = getLangFromUrl(Astro.url);
const date = new Date();
---

<p>{formatDate(date, lang)}</p>
<!-- en: January 15, 2025 -->
<!-- es: 15 de enero de 2025 -->
<!-- fr: 15 janvier 2025 -->
```

### 数字

```typescript
export function formatNumber(num: number, lang: string): string {
  return new Intl.NumberFormat(lang).format(num);
}

// 示例：
formatNumber(1234.56, 'en-US') // "1,234.56"
formatNumber(1234.56, 'de-DE') // "1.234,56"
formatNumber(1234.56, 'fr-FR') // "1 234,56"
```

### 货币

```typescript
export function formatCurrency(
  amount: number,
  currency: string,
  lang: string
): string {
  return new Intl.NumberFormat(lang, {
    style: 'currency',
    currency: currency
  }).format(amount);
}

// 示例：
formatCurrency(99.99, 'USD', 'en-US') // "$99.99"
formatCurrency(99.99, 'EUR', 'de-DE') // "99,99 €"
formatCurrency(99.99, 'GBP', 'en-GB') // "£99.99"
formatCurrency(99.99, 'MXN', 'es-MX') // "MX$99.99"
```

## URL 结构

### 策略

**选项 1：子目录（推荐）**

```
example.com/          （英语 - 默认）
example.com/es/       （西班牙语）
example.com/fr/       （法语）
```

**优点：**
- 易于实施
- 对 SEO 有利
- 用户友好的 URL
- 单个域名

**缺点：**
- 默认语言不明确

**选项 2：子域名**

```
example.com           （英语）
es.example.com        （西班牙语）
fr.example.com        （法语）
```

**优点：**
- 清晰的语言分离
- 可以托管在不同的服务器上

**缺点：**
- 多个 SSL 证书
- 更多 DNS 配置
- 更难管理
- Cookie 共享问题

**选项 3：查询参数**

```
example.com/?lang=en
example.com/?lang=es
```

**优点：**
- 实施简单

**缺点：**
- SEO 效果差
- 不用户友好
- 不推荐

**选项 4：顶级域名（TLD）**

```
example.com    （英语）
example.es     （西班牙语）
example.fr     （法语）
```

**优点：**
- 最强的本地 SEO 信号
- 用户对本地域名的信任

**缺点：**
- 昂贵（多个域名）
- 复杂的管理
- 独立的站点

## 多语言网站的 SEO

### Hreflang 标签

**告诉搜索引擎语言版本：**

```astro
---
const lang = getLangFromUrl(Astro.url);
const alternates = ['en', 'es', 'fr'];
const currentPath = Astro.url.pathname.replace(`/${lang}`, '');
---

<head>
  <!-- 自引用 -->
  <link rel="alternate" hreflang={lang} href={`https://example.com/${lang}${currentPath}`} />

  <!-- 其他语言版本 -->
  {alternates.filter(l => l !== lang).map(l => (
    <link rel="alternate" hreflang={l} href={`https://example.com/${l}${currentPath}`} />
  ))}

  <!-- 默认/后备 -->
  <link rel="alternate" hreflang="x-default" href={`https://example.com/en${currentPath}`} />
</head>
```

**结果：**
```html
<link rel="alternate" hreflang="en" href="https://example.com/en/about" />
<link rel="alternate" hreflang="es" href="https://example.com/es/about" />
<link rel="alternate" hreflang="fr" href="https://example.com/fr/about" />
<link rel="alternate" hreflang="x-default" href="https://example.com/en/about" />
```

### 特定语言的站点地图

**为每种语言生成站点地图：**

```xml
<!-- sitemap-en.xml -->
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://example.com/en/</loc>
    <xhtml:link rel="alternate" hreflang="es" href="https://example.com/es/" />
    <xhtml:link rel="alternate" hreflang="fr" href="https://example.com/fr/" />
  </url>
</urlset>
```

**提交所有站点地图：**
```
https://example.com/sitemap-en.xml
https://example.com/sitemap-es.xml
https://example.com/sitemap-fr.xml
```

### 重复内容处理

**Hreflang 防止重复内容问题：**
- Google 理解页面是翻译版本
- 每个语言版本可以在适当的地区排名
- 不同语言的"重复"内容不会受到惩罚

## 常见陷阱

### 1. 硬编码字符串

```astro
<!-- ❌ 硬编码 -->
<button>Submit</button>

<!-- ✅ 翻译 -->
<button>{t('common.submit')}</button>
```

### 2. 日期格式假设

```typescript
// ❌ 假设美国日期格式
const dateString = `${month}/${day}/${year}`;

// ✅ 使用 Intl
const dateString = new Intl.DateTimeFormat(lang).format(date);
```

### 3. 文本扩展

**文本长度因语言而异：**

英语： "Submit"
西班牙语： "Enviar"
德语： "Einreichen"
法语： "Soumettre"

**为扩展设计：**
```css
/* 允许按钮增长 */
.button {
  min-width: 120px; /* 不是固定宽度 */
  padding: 0.5rem 1rem;
  white-space: nowrap;
}
```

### 4. 文化不敏感

**图像：**
- 手势有不同的含义
- 颜色有不同的含义（白色在某些文化中意味着死亡）
- 符号可能具有冒犯性

**示例：**
- 竖大拇指：在某些中东国家具有冒犯性
- OK 手势（👌）：在某些国家具有冒犯性
- 红色：好运（中国），危险（西方国家）

**解决方案：** 使用文化中立的图像或本地化图像。

### 5. 字符串连接

```typescript
// ❌ 字符串连接
const message = `Hello ${name}, you have ${count} messages`;

// ✅ 翻译中的占位符
// en.json: "greeting": "Hello {name}, you have {count} messages"
// fr.json: "greeting": "Bonjour {name}, vous avez {count} messages"

const t = useTranslations(lang);
const message = t('greeting')
  .replace('{name}', name)
  .replace('{count}', count.toString());
```

---

## 相关文档

**核心技术：**
- [核心技术](../core-technologies-zh.md) - Astro 框架基础和设置
- [开发工具](../development-tools/README-zh.md) - 国际化项目的工具

**质量标准：**
- [SEO 标准](../quality-standards/seo-zh.md) - Hreflang 标签和 i18n SEO 优化
- [无障碍标准](../quality-standards/accessibility-zh.md) - 多语言的 WCAG
- [性能标准](../quality-standards/performance-zh.md) - 优化多语言网站

**开发工作流程：**
- [提示指南](../prompting/README-zh.md) - AI 辅助翻译技术
- [工作流程阶段](../workflow/README-zh.md) - 将 i18n 集成到开发流程中
- [阶段 2：开发](../workflow/phase-2-development-zh.md) - 构建多语言功能

**业务考虑：**
- [客户管理](../business-model/client-management-zh.md) - i18n 项目定价
- [商业模式](../business-model/README-zh.md) - 国际服务货币化

**故障排除：**
- [故障排除指南](../troubleshooting/README-zh.md) - 常见 i18n 问题

---

**记住：** 从主要市场语言开始，随着扩张添加其他语言。在需要之前不要过度设计 i18n。