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

<!-- ❌ 关键词堆砌 -->
<title>Plumber Austin, Austin Plumber, Plumbing Austin, Best Plumber Austin</title>

<!-- ❌ 不具描述性 -->
<title>Home</title>

<!-- ❌ 缺少品牌 -->
<title>Plumbing Services in Austin</title>
```

### Meta描述最佳实践

**规则：**
- **长度**：150-160个字符（更长会被截断）
- **引人注目**：包含行动号召
- **关键词**：自然包含主要关键词
- **唯一**：每个页面应该有唯一的描述
- **准确**：必须准确描述页面内容

**示例：**
```html
<!-- 主页 -->
<meta name="description" content="Smith Plumbing provides 24/7 emergency plumbing services in Austin, TX. Licensed, insured, and highly rated. Call (512) 555-0100 for fast, reliable service.">

<!-- 服务页面 -->
<meta name="description" content="Expert water heater repair and installation in Austin. Same-day service available. We work on all brands and models. Free estimates. Call (512) 555-0100 today.">

<!-- 博客文章 -->
<meta name="description" content="Learn how to fix a leaky faucet in 5 simple steps. This DIY guide includes photos and tools you'll need. Save money and stop that annoying drip today.">

<!-- 联系页面 -->
<meta name="description" content="Contact Smith Plumbing for a free quote on plumbing services in Austin, TX. Available 24/7 for emergencies. Call (512) 555-0100 or fill out our online form.">
```

**糟糕的示例：**
```html
<!-- ❌ 太短 -->
<meta name="description" content="Plumbing services.">

<!-- ❌ 太长（250+个字符） -->
<meta name="description" content="Smith Plumbing is the leading provider of comprehensive plumbing services in Austin, TX and surrounding areas. We offer emergency plumbing, water heater repair and installation, drain cleaning, sewer line repair, fixture installation, leak detection, and much more. Our licensed and insured plumbers are available 24/7.">

<!-- ❌ 不引人注目 -->
<meta name="description" content="This is the homepage for Smith Plumbing.">

<!-- ❌ 关键词堆砌 -->
<meta name="description" content="Plumber Austin plumbing Austin plumber emergency plumber 24/7 plumber best plumber Austin TX.">
```

### Open Graph标签（Facebook、LinkedIn）

Open Graph标签控制您的页面在社交媒体上分享时的显示方式。

```html
<!-- 基本Open Graph标签 -->
<meta property="og:title" content="Professional Plumbing Services in Austin, TX | Smith Plumbing">
<meta property="og:description" content="24/7 emergency plumbing services. Licensed, insured, highly rated. Call (512) 555-0100 for fast, reliable service.">
<meta property="og:image" content="https://www.smithplumbing.com/images/og-image.jpg">
<meta property="og:url" content="https://www.smithplumbing.com/">
<meta property="og:type" content="website">
<meta property="og:site_name" content="Smith Plumbing">
<meta property="og:locale" content="en_US">

<!-- 文章特定（用于博客文章） -->
<meta property="og:type" content="article">
<meta property="article:author" content="John Smith">
<meta property="article:published_time" content="2025-01-15T09:00:00Z">
<meta property="article:modified_time" content="2025-01-20T14:30:00Z">
<meta property="article:section" content="DIY Tips">
<meta property="article:tag" content="plumbing">
<meta property="article:tag" content="DIY">

<!-- 业务特定 -->
<meta property="og:type" content="business.business">
<meta property="business:contact_data:street_address" content="123 Main St">
<meta property="business:contact_data:locality" content="Austin">
<meta property="business:contact_data:region" content="TX">
<meta property="business:contact_data:postal_code" content="78701">
<meta property="business:contact_data:country_name" content="USA">
```

**OG图像要求：**
- **尺寸**：1200 × 630像素（推荐）
- **格式**：JPG或PNG
- **最大大小**：8MB以下
- **宽高比**：1.91:1
- **内容**：包含品牌，避免在小尺寸时难以阅读的文本

### Twitter Card标签

Twitter Cards控制您的页面在Twitter/X上分享时的显示方式。

```html
<!-- 摘要卡（默认） -->
<meta name="twitter:card" content="summary">
<meta name="twitter:site" content="@smithplumbing">
<meta name="twitter:creator" content="@johnsmith">
<meta name="twitter:title" content="Professional Plumbing Services in Austin, TX">
<meta name="twitter:description" content="24/7 emergency plumbing services. Licensed, insured, highly rated. Call (512) 555-0100.">
<meta name="twitter:image" content="https://www.smithplumbing.com/images/twitter-card.jpg">
<meta name="twitter:image:alt" content="Smith Plumbing logo and phone number">

<!-- 大图像卡（用于英雄图像） -->
<meta name="twitter:card" content="summary_large_image">
<meta name="twitter:site" content="@smithplumbing">
<meta name="twitter:title" content="How to Fix a Leaky Faucet: Step-by-Step Guide">
<meta name="twitter:description" content="Learn how to fix a leaky faucet in 5 simple steps. Includes photos and tool list.">
<meta name="twitter:image" content="https://www.smithplumbing.com/blog/leaky-faucet-hero.jpg">
<meta name="twitter:image:alt" content="Plumber fixing a kitchen faucet">
```

**Twitter图像要求：**
- **摘要卡**：1:1宽高比（144 × 144最小，4096 × 4096最大）
- **大图像卡**：2:1宽高比（300 × 157最小，4096 × 4096最大）
- **格式**：JPG、PNG、WEBP、GIF
- **最大大小**：5 MB

### 完整Meta标签示例

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">

  <!-- 主要SEO -->
  <title>Professional Plumbing Services in Austin, TX | Smith Plumbing</title>
  <meta name="description" content="Smith Plumbing provides 24/7 emergency plumbing services in Austin, TX. Licensed, insured, and highly rated. Call (512) 555-0100 for fast, reliable service.">
  <meta name="keywords" content="plumber austin, emergency plumbing, water heater repair, drain cleaning">
  <link rel="canonical" href="https://www.smithplumbing.com/">
  <meta name="robots" content="index, follow">

  <!-- Open Graph -->
  <meta property="og:title" content="Professional Plumbing Services in Austin, TX | Smith Plumbing">
  <meta property="og:description" content="24/7 emergency plumbing services. Licensed, insured, highly rated. Call (512) 555-0100.">
  <meta property="og:image" content="https://www.smithplumbing.com/images/og-image.jpg">
  <meta property="og:url" content="https://www.smithplumbing.com/">
  <meta property="og:type" content="website">
  <meta property="og:site_name" content="Smith Plumbing">

  <!-- Twitter -->
  <meta name="twitter:card" content="summary_large_image">
  <meta name="twitter:site" content="@smithplumbing">
  <meta name="twitter:title" content="Professional Plumbing Services in Austin, TX">
  <meta name="twitter:description" content="24/7 emergency plumbing. Licensed, insured, highly rated. Call (512) 555-0100.">
  <meta name="twitter:image" content="https://www.smithplumbing.com/images/twitter-card.jpg">

  <!-- 附加 -->
  <meta name="author" content="Smith Plumbing">
  <meta name="theme-color" content="#0066cc">

  <!-- Schema.org结构化数据 -->
  <script type="application/ld+json">
    {
      "@context": "https://schema.org",
      "@type": "LocalBusiness",
      "name": "Smith Plumbing",
      ...
    }
  </script>
</head>
<body>
  ...
</body>
</html>
```

## 结构化数据（Schema.org JSON-LD）

结构化数据帮助搜索引擎理解您的内容，并可能导致搜索结果中的丰富摘要（星级评分、价格、活动日期等）。

**格式**：使用JSON-LD（Google推荐的格式）
**位置**：在`<head>`中或`<body>`末尾
**测试**：使用Google的丰富结果测试

### LocalBusiness Schema（对本地企业至关重要）

非常适合水管工、电工、餐厅、沙龙等。

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "LocalBusiness",
  "name": "Smith Plumbing",
  "image": "https://www.smithplumbing.com/images/logo.jpg",
  "url": "https://www.smithplumbing.com",
  "telephone": "+15125550100",
  "priceRange": "$$",
  "address": {
    "@type": "PostalAddress",
    "streetAddress": "123 Main Street",
    "addressLocality": "Austin",
    "addressRegion": "TX",
    "postalCode": "78701",
    "addressCountry": "US"
  },
  "geo": {
    "@type": "GeoCoordinates",
    "latitude": 30.2672,
    "longitude": -97.7431
  },
  "openingHoursSpecification": [
    {
      "@type": "OpeningHoursSpecification",
      "dayOfWeek": [
        "Monday",
        "Tuesday",
        "Wednesday",
        "Thursday",
        "Friday"
      ],
      "opens": "08:00",
      "closes": "18:00"
    },
    {
      "@type": "OpeningHoursSpecification",
      "dayOfWeek": "Saturday",
      "opens": "09:00",
      "closes": "14:00"
    }
  ],
  "aggregateRating": {
    "@type": "AggregateRating",
    "ratingValue": "4.8",
    "reviewCount": "127"
  },
  "review": [
    {
      "@type": "Review",
      "author": {
        "@type": "Person",
        "name": "Jane Doe"
      },
      "reviewRating": {
        "@type": "Rating",
        "ratingValue": "5"
      },
      "datePublished": "2025-01-15",
      "reviewBody": "Excellent service! Fast, professional, and reasonably priced. Highly recommend Smith Plumbing."
    }
  ],
  "areaServed": [
    {
      "@type": "City",
      "name": "Austin"
    },
    {
      "@type": "City",
      "name": "Round Rock"
    },
    {
      "@type": "City",
      "name": "Cedar Park"
    }
  ],
  "hasOfferCatalog": {
    "@type": "OfferCatalog",
    "name": "Plumbing Services",
    "itemListElement": [
      {
        "@type": "Offer",
        "itemOffered": {
          "@type": "Service",
          "name": "Emergency Plumbing",
          "description": "24/7 emergency plumbing services"
        }
      },
      {
        "@type": "Offer",
        "itemOffered": {
          "@type": "Service",
          "name": "Water Heater Repair",
          "description": "Repair and installation of all water heater brands"
        }
      }
    ]
  }
}
</script>
```

**更具体的类型：**
- `Plumber` - 用于管道业务
- `Electrician` - 用于电气承包商
- `HomeAndConstructionBusiness` - 一般承包商
- `Restaurant` - 餐厅和咖啡馆
- `BeautySalon` - 美发沙龙、美甲沙龙
- `Attorney` - 律师事务所
- `Dentist` - 牙科诊所
- `MedicalClinic` - 医疗诊所

### Organization Schema

用于公司信息和品牌。

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Organization",
  "name": "Smith Plumbing",
  "alternateName": "Smith Plumbing & Heating",
  "url": "https://www.smithplumbing.com",
  "logo": "https://www.smithplumbing.com/images/logo.jpg",
  "image": "https://www.smithplumbing.com/images/team.jpg",
  "description": "Professional plumbing services in Austin, TX since 2005. Licensed, insured, and highly rated.",
  "email": "info@smithplumbing.com",
  "telephone": "+15125550100",
  "address": {
    "@type": "PostalAddress",
    "streetAddress": "123 Main Street",
    "addressLocality": "Austin",
    "addressRegion": "TX",
    "postalCode": "78701",
    "addressCountry": "US"
  },
  "contactPoint": [
    {
      "@type": "ContactPoint",
      "telephone": "+15125550100",
      "contactType": "customer service",
      "availableLanguage": ["English", "Spanish"],
      "areaServed": "US"
    },
    {
      "@type": "ContactPoint",
      "telephone": "+15125550101",
      "contactType": "emergency",
      "availableLanguage": "English",
      "hoursAvailable": "24/7"
    }
  ],
  "sameAs": [
    "https://www.facebook.com/smithplumbing",
    "https://www.instagram.com/smithplumbing",
    "https://twitter.com/smithplumbing",
    "https://www.linkedin.com/company/smithplumbing",
    "https://www.yelp.com/biz/smith-plumbing-austin"
  ],
  "founder": {
    "@type": "Person",
    "name": "John Smith"
  },
  "foundingDate": "2005-03-15",
  "numberOfEmployees": {
    "@type": "QuantitativeValue",
    "value": 12
  }
}
</script>
```

### Article Schema（博客文章）

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Article",
  "headline": "How to Fix a Leaky Faucet: Complete DIY Guide",
  "image": [
    "https://www.smithplumbing.com/blog/leaky-faucet-1x1.jpg",
    "https://www.smithplumbing.com/blog/leaky-faucet-4x3.jpg",
    "https://www.smithplumbing.com/blog/leaky-faucet-16x9.jpg"
  ],
  "datePublished": "2025-01-15T09:00:00-06:00",
  "dateModified": "2025-01-20T14:30:00-06:00",
  "author": {
    "@type": "Person",
    "name": "John Smith",
    "url": "https://www.smithplumbing.com/about/john-smith",
    "jobTitle": "Master Plumber",
    "image": "https://www.smithplumbing.com/images/john-smith.jpg"
  },
  "publisher": {
    "@type": "Organization",
    "name": "Smith Plumbing",
    "logo": {
      "@type": "ImageObject",
      "url": "https://www.smithplumbing.com/images/logo.jpg",
      "width": 600,
      "height": 60
    }
  },
  "description": "Learn how to fix a leaky faucet in 5 simple steps with our complete DIY guide. Includes photos, tools needed, and troubleshooting tips.",
  "articleBody": "Full article text here...",
  "wordCount": 1250,
  "keywords": "leaky faucet, fix faucet, DIY plumbing, faucet repair",
  "articleSection": "DIY Tips",
  "inLanguage": "en-US",
  "isAccessibleForFree": true,
  "mainEntityOfPage": {
    "@type": "WebPage",
    "@id": "https://www.smithplumbing.com/blog/how-to-fix-leaky-faucet"
  }
}
</script>
```

**文章图像要求：**
- 多种宽高比（1x1、4x3、16x9）
- 最小宽度：1200px
- 格式：JPG、PNG、WebP

### Product Schema（电子商务）

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Product",
  "name": "Premium Kitchen Faucet - Brushed Nickel",
  "image": [
    "https://www.example.com/faucet-front.jpg",
    "https://www.example.com/faucet-side.jpg",
    "https://www.example.com/faucet-top.jpg"
  ],
  "description": "High-quality kitchen faucet with pull-down sprayer. Brushed nickel finish resists spots and fingerprints.",
  "sku": "FK-8875-BN",
  "mpn": "8875",
  "brand": {
    "@type": "Brand",
    "name": "KitchenPro"
  },
  "offers": {
    "@type": "Offer",
    "url": "https://www.example.com/products/kitchen-faucet-bn",
    "priceCurrency": "USD",
    "price": "249.99",
    "priceValidUntil": "2025-12-31",
    "availability": "https://schema.org/InStock",
    "seller": {
      "@type": "Organization",
      "name": "Smith Plumbing Supply"
    },
    "shippingDetails": {
      "@type": "OfferShippingDetails",
      "shippingRate": {
        "@type": "MonetaryAmount",
        "value": "0",
        "currency": "USD"
      },
      "shippingDestination": {
        "@type": "DefinedRegion",
        "addressCountry": "US"
      },
      "deliveryTime": {
        "@type": "ShippingDeliveryTime",
        "businessDays": {
          "@type": "OpeningHoursSpecification",
          "dayOfWeek": ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday"]
        },
        "cutoffTime": "16:00:00-06:00",
        "handlingTime": {
          "@type": "QuantitativeValue",
          "minValue": 0,
          "maxValue": 1,
          "unitCode": "DAY"
        },
        "transitTime": {
          "@type": "QuantitativeValue",
          "minValue": 3,
          "maxValue": 5,
          "unitCode": "DAY"
        }
      }
    }
  },
  "aggregateRating": {
    "@type": "AggregateRating",
    "ratingValue": "4.7",
    "reviewCount": "89",
    "bestRating": "5",
    "worstRating": "1"
  },
  "review": [
    {
      "@type": "Review",
      "author": {
        "@type": "Person",
        "name": "Sarah Johnson"
      },
      "reviewRating": {
        "@type": "Rating",
        "ratingValue": "5",
        "bestRating": "5"
      },
      "datePublished": "2025-01-10",
      "reviewBody": "Beautiful faucet and easy to install. The pull-down sprayer works great."
    }
  ]
}
</script>
```

### Service Schema

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Service",
  "serviceType": "Water Heater Repair",
  "provider": {
    "@type": "LocalBusiness",
    "name": "Smith Plumbing",
    "telephone": "+15125550100",
    "address": {
      "@type": "PostalAddress",
      "streetAddress": "123 Main Street",
      "addressLocality": "Austin",
      "addressRegion": "TX",
      "postalCode": "78701"
    }
  },
  "areaServed": {
    "@type": "City",
    "name": "Austin"
  },
  "description": "Professional water heater repair and installation services. We work on all brands and models including tank and tankless water heaters.",
  "offers": {
    "@type": "Offer",
    "priceCurrency": "USD",
    "price": "95",
    "description": "Starting price for diagnostic service call"
  },
  "availableChannel": {
    "@type": "ServiceChannel",
    "serviceUrl": "https://www.smithplumbing.com/services/water-heater-repair",
    "servicePhone": {
      "@type": "ContactPoint",
      "telephone": "+15125550100",
      "availableLanguage": ["English", "Spanish"]
    }
  },
  "hoursAvailable": {
    "@type": "OpeningHoursSpecification",
    "dayOfWeek": ["Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"],
    "opens": "00:00",
    "closes": "23:59"
  }
}
</script>
```

### Breadcrumb Schema

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "BreadcrumbList",
  "itemListElement": [
    {
      "@type": "ListItem",
      "position": 1,
      "name": "Home",
      "item": "https://www.smithplumbing.com/"
    },
    {
      "@type": "ListItem",
      "position": 2,
      "name": "Services",
      "item": "https://www.smithplumbing.com/services"
    },
    {
      "@type": "ListItem",
      "position": 3,
      "name": "Water Heater Repair",
      "item": "https://www.smithplumbing.com/services/water-heater-repair"
    }
  ]
}
</script>
```

### Person Schema

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Person",
  "name": "John Smith",
  "url": "https://www.smithplumbing.com/about/john-smith",
  "image": "https://www.smithplumbing.com/images/john-smith.jpg",
  "jobTitle": "Master Plumber & Owner",
  "worksFor": {
    "@type": "Organization",
    "name": "Smith Plumbing"
  },
  "email": "john@smithplumbing.com",
  "telephone": "+15125550100",
  "address": {
    "@type": "PostalAddress",
    "addressLocality": "Austin",
    "addressRegion": "TX",
    "addressCountry": "US"
  },
  "sameAs": [
    "https://www.linkedin.com/in/johnsmith",
    "https://twitter.com/johnsmith"
  ],
  "knowsAbout": ["Plumbing", "Water Heaters", "Drain Cleaning", "Pipe Repair"],
  "alumniOf": {
    "@type": "CollegeOrUniversity",
    "name": "Austin Community College"
  },
  "hasCredential": [
    {
      "@type": "EducationalOccupationalCredential",
      "credentialCategory": "license",
      "name": "Master Plumber License",
      "recognizedBy": {
        "@type": "Organization",
        "name": "Texas State Board of Plumbing Examiners"
      }
    }
  ]
}
</script>
```

### FAQ Schema

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "FAQPage",
  "mainEntity": [
    {
      "@type": "Question",
      "name": "How much does a plumber cost in Austin?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Most plumbers in Austin charge $95-$150 for a service call, which typically includes the first hour of labor. Additional labor is usually $75-$125 per hour. Emergency and after-hours service may cost 1.5-2x the regular rate."
      }
    },
    {
      "@type": "Question",
      "name": "Do you offer same-day plumbing service?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Yes, we offer same-day service for most plumbing issues in Austin. Call us before 2 PM on weekdays, and we can typically schedule a visit the same day. Emergency service is available 24/7."
      }
    },
    {
      "@type": "Question",
      "name": "Are you licensed and insured?",
      "acceptedAnswer": {
        "@type": "Answer",
        "text": "Yes, Smith Plumbing is fully licensed (Texas Master Plumber License #M-12345) and insured. We carry both liability insurance and workers compensation insurance to protect our customers and employees."
      }
    }
  ]
}
</script>
```

### Review Schema（用于评论页面）

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "Review",
  "itemReviewed": {
    "@type": "LocalBusiness",
    "name": "Smith Plumbing",
    "address": {
      "@type": "PostalAddress",
      "streetAddress": "123 Main Street",
      "addressLocality": "Austin",
      "addressRegion": "TX",
      "postalCode": "78701"
    },
    "priceRange": "$$"
  },
  "author": {
    "@type": "Person",
    "name": "Michael Rodriguez"
  },
  "reviewRating": {
    "@type": "Rating",
    "ratingValue": "5",
    "bestRating": "5",
    "worstRating": "1"
  },
  "datePublished": "2025-01-18",
  "reviewBody": "Smith Plumbing saved the day! Had a burst pipe emergency and they came out within an hour. Professional, efficient, and reasonably priced. Highly recommend!"
}
</script>
```

## 技术SEO

### Sitemap.xml

站点地图帮助搜索引擎发现和抓取您的页面。

**Astro站点地图生成：**
```bash
# 安装
npm install @astrojs/sitemap

# 在astro.config.mjs中配置
import { defineConfig } from 'astro/dist/core/config';
import sitemap from '@astrojs/sitemap';

export default defineConfig({
  site: 'https://www.smithplumbing.com',
  integrations: [
    sitemap({
      changefreq: 'weekly',
      priority: 0.7,
      lastmod: new Date(),
      # 自定义每页
      serialize(item) {
        if (item.url === 'https://www.smithplumbing.com/') {
          item.priority = 1.0;
          item.changefreq = 'daily';
        }
        if (item.url.includes('/blog/')) {
          item.changefreq = 'monthly';
          item.priority = 0.6;
        }
        return item;
      },
      # 排除页面
      filter: (page) => !page.includes('/admin/') && !page.includes('/draft/')
    })
  ]
});
```

**手动站点地图示例：**
```xml
<?xml version="1.0" encoding="UTF-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
  <url>
    <loc>https://www.smithplumbing.com/</loc>
    <lastmod>2025-01-20</lastmod>
    <changefreq>daily</changefreq>
    <priority>1.0</priority>
  </url>
  <url>
    <loc>https://www.smithplumbing.com/services</loc>
    <lastmod>2025-01-15</lastmod>
    <changefreq>weekly</changefreq>
    <priority>0.8</priority>
  </url>
  <url>
    <loc>https://www.smithplumbing.com/services/water-heater-repair</loc>
    <lastmod>2025-01-10</lastmod>
    <changefreq>monthly</changefreq>
    <priority>0.7</priority>
  </url>
  <url>
    <loc>https://www.smithplumbing.com/blog/how-to-fix-leaky-faucet</loc>
    <lastmod>2025-01-20</lastmod>
    <changefreq>monthly</changefreq>
    <priority>0.6</priority>
  </url>
</urlset>
```

**优先级指南：**
- 主页：1.0
- 主要服务页面：0.8-0.9
- 单个服务页面：0.7-0.8
- 博客文章：0.5-0.6
- 实用页面（隐私、条款）：0.3-0.4

**提交给Google：**
1. 添加到robots.txt：`Sitemap: https://www.smithplumbing.com/sitemap.xml`
2. 在Google Search Console中提交
3. 自动抓取更新

### Robots.txt

控制搜索引擎可以抓取哪些页面。

```txt
# 允许所有爬虫访问所有内容
User-agent: *
Allow: /

# 阻止特定目录
Disallow: /admin/
Disallow: /private/
Disallow: /api/
Disallow: /temp/
Disallow: /*.pdf$

# 阻止查询参数
Disallow: /*?*

# 站点地图位置
Sitemap: https://www.smithplumbing.com/sitemap.xml
Sitemap: https://www.smithplumbing.com/sitemap-blog.xml

# 抓取延迟（可选，如果服务器无法处理激进抓取）
# Crawl-delay: 10

# 特定机器人规则
User-agent: Googlebot
Allow: /

User-agent: Bingbot
Allow: /

# 阻止坏机器人（可选）
User-agent: AhrefsBot
Disallow: /

User-agent: SemrushBot
Disallow: /
```

**常见错误：**
```txt
# ❌ 不要阻止CSS/JS（Google需要这些）
Disallow: /css/
Disallow: /js/

# ❌ 不要意外阻止所有内容
User-agent: *
Disallow: /  # 这会阻止所有页面！

# ✅ 正确
User-agent: *
Allow: /
```

**测试**：使用Google Search Console > robots.txt测试器

### 规范URL

防止重复内容问题。

```html
<!-- 每个页面都应该有规范URL -->
<link rel="canonical" href="https://www.smithplumbing.com/services/water-heater-repair">
```

**使用场景：**
- **www与非www** - 选择一个作为规范
- **HTTP与HTTPS** - 始终使用HTTPS作为规范
- **查询参数** - 指向干净的URL
- **相似内容** - 指向主要版本
- **分页** - 每个页面对自己规范

**示例：**
```html
<!-- 通过以下方式访问页面：https://www.smithplumbing.com/services?id=123 -->
<link rel="canonical" href="https://www.smithplumbing.com/services/water-heater-repair">

<!-- 通过以下方式访问页面：http://smithplumbing.com/services（无www，无https） -->
<link rel="canonical" href="https://www.smithplumbing.com/services">

<!-- 通过以下方式访问页面：https://www.smithplumbing.com/services/?utm_source=facebook -->
<link rel="canonical" href="https://www.smithplumbing.com/services">
```

**自引用规范（推荐所有页面使用）：**
```html
<!-- 即使没有重复，也指定规范 -->
<link rel="canonical" href="https://www.smithplumbing.com/current-page-url">
```

### URL结构最佳实践

**良好的URL结构：**
- **描述性** - URL描述页面内容
- **简短** - 越短越好（60个字符以下理想）
- **关键词** - 包含主要关键词
- **连字符** - 使用连字符分隔单词（不是下划线）
- **小写** - 始终使用小写
- **无特殊字符** - 避免使用&、%、#等
- **逻辑层次** - 反映站点结构

**示例：**
```
✅ 良好URL：
https://www.smithplumbing.com/services/water-heater-repair
https://www.smithplumbing.com/blog/fix-leaky-faucet
https://www.smithplumbing.com/locations/south-austin

❌ 糟糕URL：
https://www.smithplumbing.com/page.php?id=123&cat=5
https://www.smithplumbing.com/services/Water_Heater_Repair
https://www.smithplumbing.com/svcs/wtr-htr-rpr
https://www.smithplumbing.com/this-is-a-really-long-url-that-goes-on-and-on
```

**层次结构示例：**
```
/ (主页)
/services/ (服务概述)
  /services/emergency-plumbing
  /services/water-heater-repair
  /services/drain-cleaning
/locations/ (位置概述)
  /locations/south-austin
  /locations/round-rock
/blog/ (博客主页)
  /blog/category/diy-tips
    /blog/category/diy-tips/fix-leaky-faucet
/about/
/contact/
```

### 内部链接策略

内部链接帮助用户导航并分配页面权威性。

**最佳实践：**
- **链接到重要页面** - 更多内部链接 = 更重要
- **描述性锚文本** - 自然使用关键词
- **从新内容链接到旧内容** - 恢复旧内容
- **从高流量页面链接** - 将权威性传递给重要页面
- **导航链接** - 每个页面应该从主页3次点击内可达
- **上下文链接** - 内容中的链接比侧边栏/页脚更有价值

**示例：**
```html
<!-- ✅ 良好锚文本（描述性关键词） -->
<p>
  Our <a href="/services/water-heater-repair">water heater repair services</a>
  are available 24/7 in Austin.
</p>

<!-- ❌ 糟糕锚文本（通用） -->
<p>
  We offer water heater repair services. <a href="/services/water-heater-repair">Click here</a>
  to learn more.
</p>

<!-- ✅ 上下文链接 -->
<p>
  If you're experiencing low water pressure, it could be due to sediment buildup
  in your water heater. Learn more about
  <a href="/blog/signs-your-water-heater-needs-maintenance">signs your water heater needs maintenance</a>
  and how to prevent problems.
</p>
```

**内部链接清单：**
- 每个页面都从至少一个其他页面链接？
- 孤立页面（无内部链接）已识别并修复？
- 重要页面有许多指向它们的内部链接？
- 损坏的内部链接已修复（404）？
- 深层页面（从主页4+次点击）已减少？

## 内容SEO

### 页面结构和标题层次

**H1-H6层次规则：**
- **每页一个H1** - 主要页面主题/关键词
- **逻辑顺序** - H1 → H2 → H3（不要跳过级别）
- **描述性** - 标题描述部分内容
- **关键词** - 自然包含关键词

**示例结构：**
```html
<h1>Water Heater Repair Services in Austin, TX</h1>

<h2>Professional Water Heater Repair</h2>
<p>Content about repair services...</p>

<h2>Common Water Heater Problems</h2>

<h3>No Hot Water</h3>
<p>Content about this problem...</p>

<h3>Water Not Hot Enough</h3>
<p>Content about this problem...</p>

<h3>Strange Noises</h3>
<p>Content about this problem...</p>

<h2>Water Heater Brands We Service</h2>
<p>Content about brands...</p>

<h2>Why Choose Smith Plumbing?</h2>

<h3>Licensed & Insured</h3>
<p>Content...</p>

<h3>Same-Day Service</h3>
<p>Content...</p>

<h2>Contact Us for Water Heater Repair</h2>
<p>Content with call-to-action...</p>
```

### 关键词放置

**在哪里包含您的主要关键词：**
1. **标题标签** - 靠近开头
2. **H1** - 完全匹配或接近变体
3. **第一段** - 在前100个词内
4. **H2/H3** - 在一些子标题中使用变体
5. **URL** - 干净、富含关键词的URL
6. **Meta描述** - 自然包含
7. **Alt文本** - 对于相关图像
8. **正文内容** - 2-3%关键词密度（自然，不堆砌）

**示例（主要关键词："water heater repair austin"）：**
```html
<title>Water Heater Repair Austin | Same-Day Service | Smith Plumbing</title>
<meta name="description" content="Expert water heater repair in Austin, TX. Same-day service, all brands. Licensed & insured. Call (512) 555-0100 for fast, reliable repair.">

<h1>Water Heater Repair Services in Austin, TX</h1>

<p>
  Need <strong>water heater repair in Austin</strong>? Smith Plumbing provides
  fast, professional repair services for all brands and models. We're available
  24/7 for emergency repairs and offer same-day service throughout the Austin area.
</p>

<h2>Professional Austin Water Heater Repair</h2>
<p>Content with natural keyword variations...</p>

<h2>Common Water Heater Problems We Fix</h2>
<p>Content...</p>
```

**关键词密度：**
- **主要关键词**：总词数的2-3%
- **相关关键词**：合计5-10%
- **自然语言**：绝不强制关键词

### 图像SEO优化

**文件命名：**
```
❌ 糟糕：
IMG_1234.jpg
DSC00567.jpg
image (1).jpg

✅ 良好：
water-heater-repair-austin.jpg
plumber-fixing-water-heater.jpg
tankless-water-heater-installation.jpg
```

**Alt文本：**
```html
<!-- ✅ 带关键词的描述性alt文本 -->
<img
  src="water-heater-repair-austin.jpg"
  alt="Licensed plumber repairing gas water heater in Austin home"
  width="800"
  height="600"
  loading="lazy"
>

<!-- ❌ 缺少或糟糕的alt文本 -->
<img src="IMG_1234.jpg" alt="image">
```

**图像优化清单：**
- **格式**：WebP与JPG回退（或仅JPG）
- **大小**：压缩（照片200KB以下）
- **尺寸**：正确大小（不要用HTML/CSS缩小）
- **描述性文件名**：带连字符的关键词
- **Alt文本**：描述性（不是关键词堆砌）
- **加载**：首屏以下图像延迟加载
- **宽度/高度属性**：防止布局偏移（CLS）

**响应式图像：**
```html
<picture>
  <source
    srcset="water-heater-large.webp"
    type="image/webp"
    media="(min-width: 1024px)"
  >
  <source
    srcset="water-heater-medium.webp"
    type="image/webp"
    media="(min-width: 768px)"
  >
  <source srcset="water-heater-small.webp" type="image/webp">
  <img
    src="water-heater-small.jpg"
    alt="Licensed plumber repairing gas water heater"
    width="800"
    height="600"
    loading="lazy"
  >
</picture>
```

### 内容长度建议

**按页面类型：**
- **主页**：最少500-800字
- **服务页面**：800-1,500字
- **位置页面**：600-1,000字
- **博客文章**：1,000-2,500字（综合指南更长）
- **关于页面**：500-1,000字
- **联系页面**：300-500字

**质量胜过数量：**
- 不要人为增加字数
- 每个句子都应该增加价值
- 全面回答用户问题
- 用标题、列表、图像分隔文本

## 本地SEO（对小企业至关重要）

### Google Business Profile

**最重要的本地SEO因素！**

**设置清单：**
1. **声明您的业务** - 搜索"Google Business Profile"
2. **验证所有权** - 明信片、电话或电子邮件
3. **完成所有部分：**
   - 业务名称（与网站匹配）
   - 地址（NAP一致性）
   - 电话号码（NAP一致性）
   - 网站URL
   - 类别（主要+次要）
   - 营业时间（包括特殊时间）
   - 服务区域
   - 业务描述（750个字符，富含关键词）
   - 属性（女性拥有、LGBTQ+友好等）
   - 产品/服务
4. **添加照片：**
   - 徽标
   - 封面照片
   - 团队照片
   - 工作照片
   - 内部/外部
   - 产品/服务
   - **目标：100+张照片**
5. **获取评论：**
   - 询问满意客户
   - 使其简单（发送直接链接）
   - 回应所有评论
   - **目标：50+评论，4.5+评分**
6. **定期发布：**
   - 更新
   - 优惠
   - 活动
   - 博客文章
   - **目标：每月2-4篇帖子**

**Google Business Profile描述示例：**
```
Smith Plumbing is Austin's trusted plumbing company since 2005. We provide 24/7 emergency plumbing, water heater repair and installation, drain cleaning, leak detection, and all residential and commercial plumbing services. Our licensed, insured master plumbers serve Austin, Round Rock, Cedar Park, and surrounding areas. Same-day service available. Family-owned and operated with over 1,000 five-star reviews. Call (512) 555-0100 for fast, professional plumbing service.
```

### NAP一致性

NAP = 名称、地址、电话号码

**必须在任何地方都完全相同：**
- 网站
- Google Business Profile
- Facebook
- Yelp
- BBB
- 行业目录
- 引用
- Schema标记

**示例（选择一种格式并在任何地方使用）：**
```
✅ 一致的NAP：
Smith Plumbing
123 Main Street, Austin, TX 78701
(512) 555-0100

❌ 不一致（不同格式损害SEO）：
网站：Smith Plumbing, 123 Main St, Austin TX 78701, 512-555-0100
Google：Smith Plumbing & Heating, 123 Main Street, Austin, Texas 78701, (512) 555-0100
Yelp：Smith Plumbing Inc., 123 Main St., Austin, TX, 512.555.0100
```

**检查NAP一致性：**
1. Google您的业务名称+城市
2. 审核所有列表
3. 更新不正确的列表
4. 删除重复列表

### 本地引用

引用是您的业务NAP在其他网站上的提及。

**顶级引用来源（免费）：**
- Google Business Profile（必需！）
- Bing Places
- Apple Maps
- Yelp
- Facebook
- Better Business Bureau
- Yellow Pages
- Angie's List / Angi
- HomeAdvisor（如果您想要潜在客户）
- Thumbtack（如果您想要潜在客户）
- Nextdoor

**行业特定：**
- **水管工**：PlumberDirectory、Plumbing-Heating-Cooling Contractors Association
- **电工**：Electrical Contractor Directory
- **餐厅**：TripAdvisor、OpenTable、Zomato
- **零售**：Merchant Circle、Manta

**本地目录：**
- Austin Chamber of Commerce
- 本地商业协会
- 社区网站
- 本地博客/新闻网站

**引用建设技巧：**
- **一致性**：使用完全相同的NAP格式
- **准确性**：仔细检查所有信息
- **完整性**：填写整个个人资料
- **类别**：选择相关类别
- **照片**：向列表添加照片
- **描述**：使用富含关键词的描述
- **链接**：链接回您的网站

### 本地关键词和"附近"优化

**本地关键词格式：**
- `[服务] + [城市]` - "plumber austin"
- `[服务] + near me` - "plumber near me"
- `[服务] + [社区]` - "plumber south austin"
- `[服务] + in [城市]` - "plumber in austin"
- `[城市] + [服务]` - "austin plumber"

**在以下位置定位本地关键词：**
- 主页（城市焦点）
- 服务页面（城市+服务）
- 位置页面（特定社区）
- 博客文章（本地角度）

**"附近"优化：**
```html
<!-- Google使用Schema.org中的您的地址 -->
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "LocalBusiness",
  "name": "Smith Plumbing",
  "address": {
    "@type": "PostalAddress",
    "streetAddress": "123 Main Street",
    "addressLocality": "Austin",
    "addressRegion": "TX",
    "postalCode": "78701"
  },
  "geo": {
    "@type": "GeoCoordinates",
    "latitude": 30.2672,
    "longitude": -97.7431
  }
}
</script>
```

**多个服务区域的位置页面：**
```
/locations/south-austin
/locations/north-austin
/locations/round-rock
/locations/cedar-park
/locations/pflugerville
```

每个位置页面应该有：
- 唯一内容（不是重复！）
- 提及本地地标/社区
- 本地电话号码（如果不同）
- 服务区域描述
- 嵌入式Google地图
- 方向
- 来自该地区的推荐
- 本地结构化数据

### 评论Schema标记

帮助在搜索结果中显示星级评分。

```html
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "LocalBusiness",
  "name": "Smith Plumbing",
  "aggregateRating": {
    "@type": "AggregateRating",
    "ratingValue": "4.8",
    "reviewCount": "127",
    "bestRating": "5",
    "worstRating": "1"
  },
  "review": [
    {
      "@type": "Review",
      "author": {
        "@type": "Person",
        "name": "Jane Doe"
      },
      "reviewRating": {
        "@type": "Rating",
        "ratingValue": "5",
        "bestRating": "5"
      },
      "datePublished": "2025-01-15",
      "reviewBody": "Excellent service! Fast, professional, and reasonably priced."
    }
  ]
}
</script>
```

**评论生成策略：**
1. **在正确时间询问** - 成功完成工作后
2. **使其简单** - 发送Google评论表单的直接链接
3. **提供选项** - Google、Facebook、Yelp
4. **不要激励** - 不为评论付费（违反Google的TOS）
5. **回应所有** - 感谢正面，专业处理负面
6. **监控** - 为新评论设置警报

**获取Google评论链接：**
1. 打开您的Google Business Profile
2. 主页 → "获取更多评论" → 复制链接
3. 格式：`https://g.page/r/[YOUR_CODE]/review`

## 性能对SEO的影响

### Core Web Vitals作为排名因素

Google使用Core Web Vitals作为排名信号（自2021年6月起）。

**三个指标：**
1. **LCP（最大内容绘制）** - 加载性能
   - **目标**：< 2.5秒
   - **SEO影响**：高

2. **FID/INP（首次输入延迟/交互到下次绘制）** - 交互性
   - **FID目标**：< 100ms（已弃用）
   - **INP目标**：< 200ms（替代）
   - **SEO影响**：中

3. **CLS（累积布局偏移）** - 视觉稳定性
   - **目标**：< 0.1
   - **SEO影响**：中

**查看完整详情**：[性能标准](./performance-zh.md)

### 移动优先索引

Google主要使用您网站的移动版本进行索引和排名。

**要求：**
- **响应式设计** - 必须在移动设备上良好工作
- **相同内容** - 移动版本具有所有重要内容
- **快速加载** - 移动性能至关重要
- **触摸友好** - 按钮/链接易于点击（44×44px最小）
- **可读文本** - 无微小字体（16px最小）
- **无侵入性插页** - 避免在移动设备上使用全屏弹出窗口

**测试移动友好性：**
- Google移动友好测试
- Chrome DevTools移动模拟
- Lighthouse移动审计
- 真实设备测试

### 页面速度作为排名因素

**速度影响：**
- **跳出率** - 慢速网站 = 更高跳出率
- **用户体验** - 慢速 = 沮丧的用户
- **抓取预算** - 慢速网站抓取频率较低
- **转化** - 每100ms延迟 = 1%收入损失（亚马逊数据）

**速度目标：**
- **可交互时间**：< 3.8秒
- **速度指数**：< 3.4秒
- **总阻塞时间**：< 300ms

**查看完整优化指南**：[性能标准](./performance-zh.md)

## Astro特定SEO

### 静态生成SEO优势

Astro的静态站点生成提供SEO优势：

**优势：**
- **快速加载** - 无服务器处理时间
- **缓存内容** - 从CDN边缘位置提供服务
- **可靠** - 无数据库/服务器故障
- **可抓取** - 纯HTML，易于机器人抓取
- **小型JavaScript** - 最小JS = 快速TTI

**最适合：**
- 营销站点
- 商业网站
- 博客
- 文档
- 着陆页

### SEO组件模式

**可重用SEO组件：**

```astro
---
// src/components/SEO.astro
export interface Props {
  title: string;
  description: string;
  image?: string;
  canonical?: string;
  type?: 'website' | 'article' | 'product';
  publishedTime?: string;
  modifiedTime?: string;
}

const {
  title,
  description,
  image = '/images/og-default.jpg',
  canonical,
  type = 'website',
  publishedTime,
  modifiedTime
} = Astro.props;

const siteUrl = 'https://www.smithplumbing.com';
const canonicalURL = canonical || new URL(Astro.url.pathname, siteUrl).toString();
const imageUrl = new URL(image, siteUrl).toString();
---

<!-- Primary Meta Tags -->
<title>{title}</title>
<meta name="title" content={title} />
<meta name="description" content={description} />
<link rel="canonical" href={canonicalURL} />

<!-- Open Graph / Facebook -->
<meta property="og:type" content={type} />
<meta property="og:url" content={canonicalURL} />
<meta property="og:title" content={title} />
<meta property="og:description" content={description} />
<meta property="og:image" content={imageUrl} />
<meta property="og:site_name" content="Smith Plumbing" />

{publishedTime && <meta property="article:published_time" content={publishedTime} />}
{modifiedTime && <meta property="article:modified_time" content={modifiedTime} />}

<!-- Twitter -->
<meta name="twitter:card" content="summary_large_image" />
<meta name="twitter:url" content={canonicalURL} />
<meta name="twitter:title" content={title} />
<meta name="twitter:description" content={description} />
<meta name="twitter:image" content={imageUrl} />
```

**用法：**
```astro
---
// src/pages/services/water-heater-repair.astro
import SEO from '@components/SEO.astro';
import Layout from '@layouts/Layout.astro';
---

<Layout>
  <SEO
    title="Water Heater Repair Austin | Same-Day Service | Smith Plumbing"
    description="Expert water heater repair in Austin, TX. Same-day service, all brands. Licensed & insured. Call (512) 555-0100."
    image="/images/water-heater-repair-og.jpg"
  />

  <main>
    <h1>Water Heater Repair Services in Austin, TX</h1>
    <!-- Content -->
  </main>
</Layout>
```

### 元数据管理

**全局SEO配置：**

```typescript
// src/config/seo.ts
export const siteConfig = {
  name: 'Smith Plumbing',
  url: 'https://www.smithplumbing.com',
  description: 'Professional plumbing services in Austin, TX since 2005.',
  author: 'Smith Plumbing',
  phone: '(512) 555-0100',
  email: 'info@smithplumbing.com',
  address: {
    street: '123 Main Street',
    city: 'Austin',
    state: 'TX',
    zip: '78701'
  },
  social: {
    facebook: 'https://www.facebook.com/smithplumbing',
    instagram: 'https://www.instagram.com/smithplumbing',
    twitter: 'https://twitter.com/smithplumbing'
  },
  defaultOgImage: '/images/og-default.jpg'
};
```

**动态meta标签：**
```astro
---
// src/pages/blog/[slug].astro
import { siteConfig } from '@config/seo';

export async function getStaticPaths() {
  const posts = await getCollection('blog');
  return posts.map(post => ({
    params: { slug: post.slug },
    props: { post }
  }));
}

const { post } = Astro.props;
const { title, description, publishedAt, image } = post.data;
---

<SEO
  title={`${title} | ${siteConfig.name}`}
  description={description}
  image={image || siteConfig.defaultOgImage}
  type="article"
  publishedTime={publishedAt.toISOString()}
/>
```

## SEO工具

### Google Search Console

**监控SEO性能必不可少。**

**设置：**
1. 转到search.google.com/search-console
2. 添加属性（域或URL前缀）
3. 验证所有权（HTML文件、DNS、标签或Google Analytics）
4. 提交站点地图

**监控内容：**
- **性能** - 点击次数、展示次数、CTR、位置
- **覆盖率** - 已索引页面、错误、警告
- **Core Web Vitals** - 性能问题
- **移动可用性** - 移动友好问题
- **安全问题** - 手动操作、安全问题

**关键报告：**
1. **搜索结果** - 哪些关键词驱动流量
2. **页面** - 哪些页面表现最佳
3. **国家/地区** - 地理分布
4. **设备** - 桌面与移动与平板
5. **搜索外观** - 丰富结果性能

### Google Analytics 4

**设置：**
1. 在analytics.google.com创建GA4属性
2. 获取测量ID（G-XXXXXXXXXX）
3. 添加到Astro站点：

```astro
---
// src/layouts/Layout.astro
const GA_MEASUREMENT_ID = import.meta.env.PUBLIC_GA_ID;
---

<!DOCTYPE html>
<html lang="en">
<head>
  <!-- Google Analytics -->
  <script async src={`https://www.googletagmanager.com/gtag/js?id=${GA_MEASUREMENT_ID}`}></script>
  <script define:vars={{ GA_MEASUREMENT_ID }}>
    window.dataLayer = window.dataLayer || [];
    function gtag(){dataLayer.push(arguments);}
    gtag('js', new Date());
    gtag('config', GA_MEASUREMENT_ID);
  </script>

  <!-- Rest of head -->
</head>
<body>
  <slot />
</body>
</html>
```

**SEO关键指标：**
- **有机流量** - 来自搜索引擎的流量
- **跳出率** - 单页会话
- **平均会话持续时间** - 网站停留时间
- **每次会话页数** - 页面深度
- **热门着陆页** - 来自搜索的入口点

### Lighthouse SEO审计

**如何运行：**
1. 打开Chrome DevTools（F12）
2. 单击"Lighthouse"选项卡
3. 选择"SEO"类别
4. 单击"分析页面加载"

**检查内容：**
- 存在Meta描述
- 存在标题标签且长度合适
- 可抓取链接
- 有效的robots.txt
- 成功的HTTP状态代码
- 图像宽高比
- 字体大小
- 点击目标大小
- 以及更多...

**目标分数**：95-100

### Schema标记验证器

**Google丰富结果测试：**
- URL：search.google.com/test/rich-results
- 测试：所有结构化数据类型
- 显示：丰富结果外观预览
- 用途：测试单个页面

**Schema.org验证器：**
- URL：validator.schema.org
- 测试：JSON-LD、Microdata、RDFa
- 显示：结构化数据树
- 用途：验证schema语法

**Google Search Console（实时数据）：**
- 报告 → 增强功能
- 显示：丰富结果性能和错误
- 用途：监控实时站点

## AI辅助SEO

### 使用AI生成Meta描述

**提示模板：**
```
为此页面编写引人注目的meta描述（150-160个字符）：

页面URL：[URL]
主要关键词：[keyword]
页面内容：[简要摘要]

描述应该：
- 自然包含主要关键词
- 有明确的行动号召
- 引人注目且值得点击
- 准确描述页面内容
- 保持在150-160字符限制内

提供3个变体。
```

**示例：**
```
为此页面编写引人注目的meta描述（150-160个字符）：

页面URL：/services/water-heater-repair
主要关键词：water heater repair austin
页面内容：We provide professional water heater repair services in Austin. Same-day service, licensed plumbers, all brands. Available 24/7 for emergencies.

描述应该：
- 自然包含主要关键词
- 有明确的行动号召
- 引人注目且值得点击
- 准确描述页面内容
- 保持在150-160字符限制内

提供3个变体。
```

### 使用AI创建Schema标记

**提示模板：**
```
为[业务类型]创建Schema.org JSON-LD结构化数据：

业务详情：
- 名称：[name]
- 地址：[address]
- 电话：[phone]
- 网站：[url]
- 服务：[list]
- 营业时间：[hours]
- 评分：[rating]
- 评论数量：[count]

使用LocalBusiness schema类型（或更具体的如Plumber、Electrician等）。
包含所有相关属性。
```

### 使用AI进行内容优化

**SEO内容审查提示：**
```
审查此页面内容的SEO优化：

[粘贴内容]

主要关键词：[keyword]
目标字数：[count]

分析：
1. 关键词使用（频率、位置、变体）
2. 标题结构（H1-H6层次）
3. 内容质量和完整性
4. 内部链接机会
5. 行动号召有效性
6. 可读性（句子长度、段落长度）
7. 缺失元素（图像、列表等）

提供具体的改进建议。
```

**本地SEO内容提示：**
```
为[城市]的[业务类型]编写[字数]字的SEO优化内容：

主要关键词：[keyword]
次要关键词：[keywords]
独特卖点：[USPs]
服务区域：[areas]

内容应该：
- 针对本地搜索者
- 在整个过程中自然包含位置
- 提及本地地标/社区
- 在第一段包含主要关键词
- 使用带关键词变体的H2/H3子标题
- 包含明确的行动号召
- 对话式和有帮助（不是关键词堆砌）

目标受众：[description]
```

## 交叉引用

**相互依赖的质量标准：**
- **[性能标准](./performance-zh.md)** - Core Web Vitals是直接的Google排名因素
- **[可访问性标准](./accessibility-zh.md)** - 可访问标记改善SEO（语义HTML、alt文本、正确结构）
- **[这些标准如何关联](./README-zh.md#how-these-standards-relate)** - 理解质量三角形

**相关文档：**
- [客户管理](../business-model/client-management-zh.md) - 向客户解释SEO价值
- [测试和调试](../workflow/phase-3-testing-debugging-zh.md) - 工作流程中的SEO测试
- [Cloudflare Pages](../hosting-tools/cloudflare-pages-zh.md) - SEO的托管配置
- [质量标准概述](./README-zh.md) - 返回质量标准主页

---

**记住**：SEO是长期投资。结果需要3-6个月才能显现。专注于为目标受众创建真正有帮助的内容，排名会随之而来。