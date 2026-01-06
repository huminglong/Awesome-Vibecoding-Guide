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

**方法 2：Astro 内置 i18n 路由**

```astro
// astro.config.mjs
import { defineConfig } from 'astro/config';
import { defaultLocale, localeRoutes } from 'astro:i18n/routing';

export default defineConfig({
  i18n: {
    defaultLocale: 'en',
    locales: ['en', 'es', 'fr'],
    routing: {
      prefixDefaultLocale: false,
    },
  },
});
```

### 实施步骤

**步骤 1：配置 Astro**

```javascript
// astro.config.mjs
import { defineConfig } from 'astro/config';

export default defineConfig({
  i18n: {
    defaultLocale: 'en',
    locales: ['en', 'es', 'fr', 'zh-CN'],
    routing: {
      prefixDefaultLocale: false,
    },
  },
});
```

**步骤 2：创建翻译文件**

```javascript
// src/i18n/ui.ts
export const messages = {
  en: {
    nav: {
      home: 'Home',
      about: 'About',
      contact: 'Contact',
    },
    hero: {
      title: 'Welcome to Our Site',
      subtitle: 'Building amazing experiences',
    },
  },
  es: {
    nav: {
      home: 'Inicio',
      about: 'Acerca de',
      contact: 'Contacto',
    },
    hero: {
      title: 'Bienvenido a Nuestro Sitio',
      subtitle: 'Construyendo experiencias increíbles',
    },
  },
  fr: {
    nav: {
      home: 'Accueil',
      about: 'À propos',
      contact: 'Contact',
    },
    hero: {
      title: 'Bienvenue sur Notre Site',
      subtitle: 'Créer des expériences incroyables',
    },
  },
};
```

**步骤 3：创建语言切换器**

```astro
---
import { getLocale } from 'astro:i18n';
import { messages } from '../i18n/ui';

const locale = getLocale(Astro.url);
const t = messages[locale];
---

<nav>
  <a href="/">{t.nav.home}</a>
  <a href="/about">{t.nav.about}</a>
  <a href="/contact">{t.nav.contact}</a>

  <div class="language-switcher">
    <a href="/en">English</a>
    <a href="/es">Español</a>
    <a href="/fr">Français</a>
  </div>
</nav>
```

**步骤 4：翻译内容**

```astro
---
import { getLocale } from 'astro:i18n';
import { messages } from '../i18n/ui';

const locale = getLocale(Astro.url);
const t = messages[locale];
---

<section class="hero">
  <h1>{t.hero.title}</h1>
  <p>{t.hero.subtitle}</p>
</section>
```

### 高级功能

**动态内容翻译**

```javascript
// src/i18n/ui.ts
export const messages = {
  en: {
    greeting: (name: string) => `Hello, ${name}!`,
    itemCount: (count: number) => `${count} item${count !== 1 ? 's' : ''}`,
  },
  es: {
    greeting: (name: string) => `¡Hola, ${name}!`,
    itemCount: (count: number) => `${count} elemento${count !== 1 ? 's' : ''}`,
  },
};
```

**复数形式**

```javascript
// src/i18n/ui.ts
export const messages = {
  en: {
    items: {
      zero: 'No items',
      one: '1 item',
      other: '{count} items',
    },
  },
  es: {
    items: {
      zero: 'Sin elementos',
      one: '1 elemento',
      other: '{count} elementos',
    },
  },
};
```

**日期和数字格式化**

```astro
---
import { getLocale } from 'astro:i18n';

const locale = getLocale(Astro.url);
const date = new Date();
const price = 1234.56;
---

<p>Date: {date.toLocaleDateString(locale)}</p>
<p>Price: {price.toLocaleString(locale, { style: 'currency', currency: 'USD' })}</p>
```

### SEO 优化

**hreflang 标签**

```astro
---
import { getLocales } from 'astro:i18n';

const locales = getLocales();
---

<head>
  {locales.map(locale => (
    <link rel="alternate" hreflang={locale} href={`/${locale}/`} />
  ))}
  <link rel="alternate" hreflang="x-default" href="/" />
</head>
```

**元数据翻译**

```astro
---
import { getLocale } from 'astro:i18n';
import { messages } from '../i18n/ui';

const locale = getLocale(Astro.url);
const t = messages[locale];
---

<head>
  <title>{t.meta.title}</title>
  <meta name="description" content={t.meta.description} />
  <meta property="og:title" content={t.meta.ogTitle} />
  <meta property="og:description" content={t.meta.ogDescription} />
</head>
```

### 最佳实践

**1. 保持翻译文件结构化**

```javascript
// 好的做法
export const messages = {
  en: {
    nav: { /* ... */ },
    hero: { /* ... */ },
    footer: { /* ... */ },
  },
};

// 避免扁平结构
export const messages = {
  en: {
    navHome: 'Home',
    navAbout: 'About',
    heroTitle: 'Welcome',
    // ... 太多键
  },
};
```

**2. 使用一致的键名**

```javascript
// 好的做法
export const messages = {
  en: {
    buttons: {
      submit: 'Submit',
      cancel: 'Cancel',
    },
  },
};

// 避免不一致
export const messages = {
  en: {
    submitButton: 'Submit',
    cancelBtn: 'Cancel',
    confirm: 'Confirm',
  },
};
```

**3. 提供上下文信息**

```javascript
// 好的做法
export const messages = {
  en: {
    forms: {
      email: {
        label: 'Email',
        placeholder: 'Enter your email',
        error: 'Please enter a valid email',
      },
    },
  },
};
```

**4. 处理缺失的翻译**

```javascript
// src/i18n/ui.ts
export const messages = {
  en: { /* ... */ },
  es: { /* ... */ },
};

export function t(key: string, locale: string) {
  const keys = key.split('.');
  let value = messages[locale];

  for (const k of keys) {
    value = value?.[k];
  }

  if (value === undefined) {
    console.warn(`Missing translation for key: ${key} in locale: ${locale}`);
    return key; // 返回键作为后备
  }

  return value;
}
```

### 常见问题

**Q: 如何处理 RTL（从右到左）语言？**

```astro
---
import { getLocale } from 'astro:i18n';

const locale = getLocale(Astro.url);
const rtlLocales = ['ar', 'he', 'fa'];
const isRTL = rtlLocales.includes(locale);
---

<html dir={isRTL ? 'rtl' : 'ltr'} lang={locale}>
```

**Q: 如何翻译动态内容？**

```javascript
// 使用 API 或 CMS
export async function getStaticPaths() {
  const posts = await getPosts();
  const locales = ['en', 'es', 'fr'];

  return locales.flatMap(locale =>
    posts.map(post => ({
      params: { id: post.id },
      props: { post, locale },
    }))
  );
}
```

**Q: 如何处理图片本地化？**

```astro
---
import { getLocale } from 'astro:i18n';

const locale = getLocale(Astro.url);
---

<img
  src={`/images/hero-${locale}.jpg`}
  alt={`Hero image for ${locale}`}
/>
```

### 工具和资源

**翻译管理平台**
- [Crowdin](https://crowdin.com/)
- [POEditor](https://poeditor.com/)
- [Lokalise](https://lokalise.com/)

**Astro i18n 库**
- [astro-i18next](https://github.com/patrickkettner/astro-i18next)
- [astro-i18n](https://github.com/withastro/astro/tree/main/packages/integrations/i18n)

**相关文档**
- [Astro i18n 指南](https://docs.astro.build/en/guides/internationalization/)
- [MDN Web Docs - 国际化](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Intl)

## 总结

国际化是一个强大的功能，可以让您的站点触达全球受众。通过正确实施 i18n，您可以：

- 扩展到新市场
- 改善用户体验
- 满足法律要求
- 建立全球品牌

记住，i18n 是一个持续的过程。定期审查和更新翻译，确保它们保持准确和相关。

返回：[技术栈](README-zh.md)