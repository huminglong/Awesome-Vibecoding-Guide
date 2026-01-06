# 质量标准概述

确保您的vibecoding项目中的专业级质量。本部分涵盖了每个面向客户的网站应满足的基本质量标准：可访问性、SEO和性能。

## 为什么质量标准很重要

**对于您的业务：**
- 专业声誉
- 客户满意度和推荐
- 竞争优势
- **证明您的定价合理性** - 参见[商业模式：定价和经济学](../business-model/pricing-economics-zh.md)
- 减少支持问题
- **更高的利润率** - 质量工作要求溢价率（[价值主张](../business-model/value-proposition-zh.md)）

**对于您的客户：**
- 更好的Google排名（SEO + 性能）
- 更多客户（可访问性 + 移动）
- 法律合规（可访问性）
- 更低的跳出率（性能）
- 专业形象

**对于最终用户：**
- 快速、响应式体验
- 对每个人都有效（包括残疾）
- 易于在搜索中找到
- 移动友好

## 四大支柱

### 1. 可访问性（a11y）
**[完整指南 →](./accessibility-zh.md)**

使您的网站对每个人都可用，包括残疾人。

**关键标准：**
- WCAG 2.1 AA级合规
- 屏幕阅读器兼容性
- 键盘导航
- 颜色对比要求
- 语义HTML

**为什么重要：**
- 全球15%的人口有残疾
- 许多司法管辖区的法律要求
- 对每个人都更好的用户体验
- 改善SEO

**快速获胜：**
运行Lighthouse可访问性审计并修复问题。

---

### 2. SEO（搜索引擎优化）
**[完整指南 →](./seo-zh.md)**

使您的网站在搜索引擎中可被发现，特别是Google。

**关键元素：**
- HTML meta标签（title、description、Open Graph）
- 结构化数据（Schema.org JSON-LD）
- Sitemap.xml和robots.txt
- 内容优化
- 小型企业的本地SEO

**为什么重要：**
- 93%的在线体验从搜索开始
- Google第一页获得91.5%的流量
- 本地搜索为服务企业转化80%+
- 自然流量是免费的

**快速获胜：**
为每个页面添加适当的meta标签和结构化数据。

---

### 3. 性能优化
**[完整指南 →](./performance-zh.md)**

使您的网站快速加载并平稳运行。

**关键指标：**
- Core Web Vitals（LCP、FID/INP、CLS）
- 页面加载时间 < 3秒
- 移动性能
- 图像优化
- 包大小减少

**为什么重要：**
- 53%的用户放弃加载时间>3秒的网站
- Google使用性能作为排名因素
- 更好的性能 = 更高的转化
- 移动用户期望速度

**快速获胜：**
优化图像并实现延迟加载。

---

### 4. 设计一致性
**[完整指南 →](./design-consistency-zh.md)**

通过系统设计和一致模式防止"vibe编码"外观。

**关键元素：**
- 间距节奏（8pt网格系统）
- 排版层次
- 组件一致性
- 边框半径和提升标准
- 复制质量和内容标准

**为什么重要：**
- 视觉润色证明高级定价的合理性
- 与廉价/匆忙的工作区分开来
- 减少客户修订请求
- 专业外观建立信任
- 一致的UX减少困惑

**快速获胜：**
建立设计令牌并在整个项目中一致地使用它们。

---

## 质量门控

**[发货前检查清单 →](./quality-gates-zh.md)**

在发布任何客户项目之前，使用质量门控检查清单捕获常见问题：

**关键7项（不可协商）：**
1. 所有交互元素都工作
2. 移动响应式（最小375px）
3. 存在meta标签
4. 无占位符内容
5. 异步操作的加载状态
6. 一致的间距和对齐
7. 真实或零推荐

**项目类型特定门控：**
- 本地商业网站
- 着陆页/SaaS
- 作品集/个人网站

**时间投资**：5-10分钟防止发货后数小时的修复。

---

## 这些标准如何关联

质量支柱相互依赖并相互加强：

### 可访问性 → SEO
- **语义HTML**改善搜索引擎理解
- **图像上的Alt文本**帮助搜索引擎索引视觉内容
- **正确的标题层次**改善爬虫的内容结构
- **快速、可访问的网站**在搜索结果中排名更好

**了解更多**：[可访问性指南](./accessibility-zh.md) → [SEO指南](./seo-zh.md)

### 性能 → SEO
- **Core Web Vitals**是直接的Google排名因素
- **页面速度**影响搜索排名
- **移动性能**对移动优先索引至关重要
- **快速网站**被搜索引擎更频繁地抓取

**了解更多**：[性能指南](./performance-zh.md) → [SEO指南](./seo-zh.md)

### 可访问性 → 性能
- **语义HTML**需要更少的CSS/JavaScript
- **正确的结构**减少DOM复杂性
- **键盘导航**通常意味着更轻的交互
- **干净的标记**改善渲染性能

**了解更多**：[可访问性指南](./accessibility-zh.md) → [性能指南](./performance-zh.md)

### 设计一致性 → 所有支柱
- **系统设计**确保遵循可访问性标准
- **干净、一致的代码**改善性能
- **专业外观**改善用户参与指标（有助于SEO）
- **组件重用**减少代码复杂性和错误

**了解更多**：[设计一致性指南](./design-consistency-zh.md)

### 质量基础
```
       设计一致性
              |
              |  （系统方法）
              |
      可访问性
          /   \
         /     \
        /       \
       /         \
   SEO --------- 性能
```

**关键见解**：设计一致性为所有其他质量支柱提供基础。系统的设计方法自然导致更好的可访问性、SEO和性能。当您使用一致的模式和标准构建时，专业质量随之而来。

---

## 质量标准检查清单

在部署任何客户网站之前，确保满足这些标准：

### 可访问性 ✅
- [ ] WCAG AA级合规（Lighthouse审计90+）
- [ ] 全部使用语义HTML
- [ ] 正确的标题层次（h1-h6）
- [ ] 所有图像都有alt文本
- [ ] 颜色对比度≥ 4.5:1
- [ ] 键盘导航工作
- [ ] 焦点指示器可见
- [ ] 表单正确标记
- [ ] 需要的地方有ARIA标签
- [ ] axe DevTools中无可访问性错误

### SEO ✅
- [ ] 每个页面都有唯一的标题标签
- [ ] Meta描述（150-160个字符）
- [ ] 社交分享的Open Graph标签
- [ ] Schema.org结构化数据（LocalBusiness/Organization）
- [ ] 生成并提交Sitemap.xml
- [ ] 配置Robots.txt
- [ ] 设置规范URL
- [ ] 移动友好（Google测试通过）
- [ ] 启用HTTPS
- [ ] 配置Google Search Console
- [ ] 安装Google Analytics

### 性能 ✅
- [ ] Lighthouse性能分数≥ 90
- [ ] Core Web Vitals全部"良好"（绿色）
  - LCP < 2.5s
  - FID/INP < 100ms
  - CLS < 0.1
- [ ] 图像优化（WebP格式，压缩）
- [ ] 实施延迟加载
- [ ] CSS最小化
- [ ] JavaScript最小化
- [ ] 无渲染阻塞资源
- [ ] 配置Cloudflare缓存
- [ ] 总页面大小< 1MB
- [ ] 在Fast 3G上< 3秒加载

### 设计一致性 ✅
- [ ] 间距遵循8pt网格系统（无随机值）
- [ ] 边框半径限制为3-4个值，一致使用
- [ ] 排版使用定义的规模（最多6-8个大小）
- [ ] 颜色来自定义的调色板（无随机紫色/霓虹色）
- [ ] 所有组件共享核心样式模式
- [ ] 悬停效果微妙且不破坏布局
- [ ] 所有异步操作的加载状态
- [ ] 所有交互元素功能正常
- [ ] 复制具体（不是通用标语）
- [ ] 推荐真实或完全省略
- [ ] 无占位符内容（lorem、test、TODO）

**另见**：[设计一致性指南](./design-consistency-zh.md) | [质量门控](./quality-gates-zh.md)

---

## 测试您的标准

### 自动化测试

**Lighthouse（Chrome DevTools）：**
```
1. 打开DevTools（F12）
2. Lighthouse标签
3. 选择：性能、可访问性、最佳实践、SEO
4. 设备：移动
5. 点击"分析页面加载"
6. 目标：所有分数≥ 90
```

**其他工具：**
- [WAVE浏览器扩展](https://wave.webaim.org/extension/) - 可访问性
- [axe DevTools](https://www.deque.com/axe/devtools/) - 可访问性
- [Google PageSpeed Insights](https://pagespeed.web.dev/) - 性能
- [Google Search Console](https://search.google.com/search-console) - SEO
- [Schema标记验证器](https://validator.schema.org/) - 结构化数据

### 手动测试

**可访问性：**
- 仅使用键盘（Tab键）导航整个网站
- 使用屏幕阅读器测试（Windows上的NVDA，Mac上的VoiceOver）
- 使用浏览器工具检查颜色对比
- 测试表单的正确标签和错误消息

**SEO：**
- 在Google中搜索"site:yourdomain.com"（查看索引内容）
- 在浏览器检查器中检查meta标签
- 在Search Console中验证结构化数据
- 使用Google的移动友好测试在移动上测试

**性能：**
- 在具有3G限制的真实移动设备上测试
- 在网络限制的情况下检查加载时间
- 验证图像延迟加载
- 在Chrome用户体验报告中测试Core Web Vitals

---

## 常见质量问题

### 可访问性失败

**缺少Alt文本：**
```html
<!-- ❌ 不好 -->
<img src="logo.png">

<!-- ✅ 好 -->
<img src="logo.png" alt="公司名称 - 家庭服务">
```

**糟糕的标题结构：**
```html
<!-- ❌ 不好 -->
<h1>欢迎</h1>
<h3>我们的服务</h3>
<h2>关于我们</h2>

<!-- ✅ 好 -->
<h1>欢迎</h1>
<h2>我们的服务</h2>
<h2>关于我们</h2>
```

**低对比度：**
```css
/* ❌ 不好（2.5:1比例） */
color: #777;
background: #fff;

/* ✅ 好（4.5:1比例） */
color: #555;
background: #fff;
```

### SEO失败

**缺少或重复的标题：**
```html
<!-- ❌ 不好 -->
<title>首页</title>

<!-- ✅ 好 -->
<title>John's Plumbing - 德克萨斯州奥斯汀的24/7紧急服务</title>
```

**无Meta描述：**
```html
<!-- ❌ 不好 -->
<!-- 无meta描述 -->

<!-- ✅ 好 -->
<meta name="description" content="John's Plumbing在德克萨斯州奥斯汀提供24/7紧急管道服务。持证、保险，并受到500+本地客户的信任。立即致电！">
```

**缺少结构化数据：**
```html
<!-- ❌ 不好 -->
<!-- 无结构化数据 -->

<!-- ✅ 好 -->
<script type="application/ld+json">
{
  "@context": "https://schema.org",
  "@type": "LocalBusiness",
  "name": "John's Plumbing",
  "telephone": "+1-512-555-0123",
  "address": {
    "@type": "PostalAddress",
    "streetAddress": "123 Main St",
    "addressLocality": "Austin",
    "addressRegion": "TX",
    "postalCode": "78701"
  }
}
</script>
```

### 性能失败

**未优化的图像：**
```html
<!-- ❌ 不好（5MB原始照片） -->
<img src="hero.jpg">

<!-- ✅ 好（优化、响应式、延迟加载） -->
<img
  src="hero-800.webp"
  srcset="hero-400.webp 400w, hero-800.webp 800w, hero-1200.webp 1200w"
  sizes="(max-width: 600px) 400px, (max-width: 1200px) 800px, 1200px"
  alt="专业水管工修理水槽"
  loading="lazy"
>
```

**渲染阻塞资源：**
```html
<!-- ❌ 不好 -->
<link rel="stylesheet" href="styles.css">
<script src="app.js"></script>

<!-- ✅ 好 -->
<link rel="stylesheet" href="styles.css">
<script src="app.js" defer></script>
```

---

## 不同项目类型的质量标准

### 本地商业网站（水管工、电工等）

**优先级：**
1. **性能**（移动速度至关重要）
2. **SEO**（本地搜索可发现性）
3. **可访问性**（基本合规）

**重点领域：**
- 移动优先优化
- 本地SEO（Google Business Profile集成）
- 点击通话功能
- 快速加载时间（通常是紧急搜索）
- Google Maps集成

**目标分数：**
- 性能：95+
- SEO：100
- 可访问性：90+

---

### 专业服务（律师、会计师等）

**优先级：**
1. **可访问性**（专业形象、法律合规）
2. **SEO**（可信度和可发现性）
3. **性能**（桌面和移动）

**重点领域：**
- WCAG AA合规
- 专业外观
- 内容丰富（服务、资质）
- 桌面体验优化
- 安全（HTTPS、隐私）

**目标分数：**
- 可访问性：100
- SEO：100
- 性能：90+

---

### 电子商务/产品网站

**优先级：**
1. **性能**（转化优化）
2. **SEO**（产品可发现性）
3. **可访问性**（法律合规、覆盖范围）

**重点领域：**
- 快速产品页面加载
- 产品架构标记
- 图像优化（很多照片）
- 移动结账优化
- Core Web Vitals

**目标分数：**
- 性能：90+（产品更难）
- SEO：100
- 可访问性：95+

---

## AI辅助质量保证

### 使用AI检查标准

**可访问性审计提示：**
```
"审计此HTML是否符合WCAG 2.1 AA级合规：

[粘贴HTML]

检查：
- 语义HTML
- 标题层次
- Alt文本
- ARIA标签
- 表单标签
- 颜色对比（如果提供样式）
- 键盘导航支持

列出所有问题及其严重性（关键/高/中/低）并提供修复。"
```

**SEO审计提示：**
```
"此页面的SEO审计：

URL: [url]
HTML: [粘贴head部分]

检查：
- 标题标签（唯一、描述性、<60个字符）
- Meta描述（引人注目、150-160个字符）
- Open Graph标签
- 结构化数据（Schema.org）
- 标题结构
- 图像alt文本
- 内部链接

提供改进建议。"
```

**性能审计提示：**
```
"分析此网站的性能问题：

Lighthouse报告：[粘贴JSON或摘要]
网络瀑布图：[描述或截图]

识别：
1. 最大瓶颈
2. 快速获胜
3. 图像优化机会
4. JavaScript优化
5. CSS优化

提供优先操作项。"
```

---

## 质量标准工作流程

### 对于每个新项目

**阶段1：规划**
- [ ] 识别目标受众及其需求
- [ ] 确定关键质量标准
- [ ] 设置性能预算
- [ ] 规划SEO策略（关键词、本地/全国）

**阶段2：开发**
- [ ] 从一开始使用语义HTML
- [ ] 在添加到网站之前优化图像
- [ ] 随时编写描述性alt文本
- [ ] 为每个页面添加meta标签
- [ ] 早期实施结构化数据

**阶段3：测试**
- [ ] 运行Lighthouse审计（所有类别）
- [ ] 使用键盘导航测试
- [ ] 使用屏幕阅读器测试
- [ ] 在检查器中验证SEO标签
- [ ] 在移动上检查性能

**阶段4：优化**
- [ ] 修复所有关键可访问性问题
- [ ] 添加缺失的SEO元素
- [ ] 优化性能瓶颈
- [ ] 验证结构化数据
- [ ] 在真实设备上测试

**阶段5：发布**
- [ ] 最终Lighthouse审计（记录分数）
- [ ] 将站点地图提交给Google Search Console
- [ ] 设置Google Analytics
- [ ] 验证移动友好测试通过
- [ ] 监控Core Web Vitals

**阶段6：监控**
- [ ] 每周：检查Search Console的问题
- [ ] 每月：审查分析和排名
- [ ] 每季度：重新运行完整质量审计
- [ ] 持续：监控Core Web Vitals

---

## 资源

### 官方标准

**可访问性：**
- [WCAG 2.1指南](https://www.w3.org/WAI/WCAG21/quickref/)
- [WebAIM资源](https://webaim.org/resources/)
- [A11y项目](https://www.a11yproject.com/)

**SEO：**
- [Google搜索中心](https://developers.google.com/search)
- [Schema.org](https://schema.org/)
- [Google SEO入门指南](https://developers.google.com/search/docs/fundamentals/seo-starter-guide)

**性能：**
- [Web.dev性能](https://web.dev/performance/)
- [Core Web Vitals](https://web.dev/vitals/)
- [Google PageSpeed Insights](https://pagespeed.web.dev/)

### 工具

**测试：**
- [Lighthouse](https://developer.chrome.com/docs/lighthouse/) - 一体化审计
- [WAVE](https://wave.webaim.org/) - 可访问性
- [axe DevTools](https://www.deque.com/axe/devtools/) - 可访问性
- [WebPageTest](https://www.webpagetest.org/) - 性能
- [Google Rich Results Test](https://search.google.com/test/rich-results) - 结构化数据

**优化：**
- [Squoosh](https://squoosh.app/) - 图像压缩
- [SVGOMG](https://jakearchibald.github.io/svgomg/) - SVG优化
- [Cloudflare图像优化](https://www.cloudflare.com/products/cloudflare-images/) - 自动图像优化

---

## 关键要点

### 质量 = 专业 = 有利可图

**更好的质量标准导致：**
- 更高的客户满意度
- 更多推荐
- 证明高级定价的合理性
- 更少的支持问题
- 更好的作品集作品

### 从标准开始，而不是事后修复

**从一开始就构建质量：**
- 比以后修复更容易
- 总体时间更少
- 更好的结果
- 避免技术债务

### 尽可能自动化

**使用工具捕获问题：**
- Lighthouse用于自动化审计
- CI/CD在每次部署时运行测试
- 浏览器扩展用于快速检查
- AI审查代码是否符合标准

### 客户沟通

**向客户展示价值：**
- "您的网站在Google的测试中得分95+"
- "对所有用户完全可访问"
- "针对搜索引擎优化"
- "在2秒内加载"

这将您与竞争对手区分开来，并证明您的定价合理性。

---

**详细指南：**
- [可访问性指南 →](./accessibility-zh.md) - WCAG合规和包容性设计
- [SEO指南 →](./seo-zh.md) - 搜索引擎优化最佳实践
- [性能指南 →](./performance-zh.md) - 速度优化和Core Web Vitals
- [设计一致性指南 →](./design-consistency-zh.md) - 防止"vibe编码"外观
- [质量门控检查清单 →](./quality-gates-zh.md) - 发货前质量验证

**相关文档：**
- [质量导向提示](../prompting/quality-focused-prompts-zh.md) - 专业输出的AI提示
- [测试策略](../workflow/phase-3-testing-debugging-zh.md) - 质量保证测试
- [客户管理](../business-model/client-management-zh.md) - 沟通质量价值
- [核心技术](../core-technologies-zh.md) - Astro性能优势

**返回**：[主指南](../../README-zh.md)