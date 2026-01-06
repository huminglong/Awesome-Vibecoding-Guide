# 核心技术 — 为什么选择这些，为什么它们适用于Vibecoding

本指南解释了我为Vibecoding推荐的核心栈：Astro、Tailwind CSS和Cloudflare Pages（在需要时使用Cloudflare Functions）。这些工具的选择基于三个实际目标：卓越的Core Web Vitals、快速的开发人员反馈循环和最小的运营摩擦（易于托管和维护）。

为什么选择Astro？
- HTML优先的SSG以获得出色的Core Web Vitals：Astro默认在构建时渲染静态HTML，交付最少的初始JavaScript和极快的Time To First Byte（TTFB）和Largest Contentful Paint（LCP）。[→ 性能标准](docs/quality-standards/performance-zh.md)
- 岛屿架构：仅在需要的地方使用客户端框架添加交互性（React、Svelte、Solid等），因此大多数页面保持轻量级。
- 灵活性和互操作性：在Astro页面中使用React组件（或其他框架）——您获得静态优先站点和现代交互性的最佳组合。
- 开发人员人体工程学：简单的基于文件的路由、Markdown友好的内容集合和低摩擦的构建过程使其非常适合快速迭代的vibecoding会话。[→ 提示指南](docs/prompting/README-zh.md)
- SEO和性能默认：图像优化、部分水合和小包对现实世界的SEO和累积布局偏移（CLS）改进友好。[→ 性能标准](docs/quality-standards/performance-zh.md)

**Astro的实施工具：**
- [Zed IDE](docs/development-tools/recommended-tools/zed-zh.md) — 为Astro开发工作流程优化
- [Droid CLI](docs/development-tools/recommended-tools/droid-cli-zh.md) — 组件生成和项目脚手架
- [DevTools MCP](docs/development-tools/mcp-servers/devtools-mcp-zh.md) — Core Web Vitals测试和性能优化
- [阶段4：部署](docs/workflow/phase-4-deployment-zh.md) — 逐步部署指南

为什么选择Tailwind CSS？
- 可预测的实用优先工作流程：使用小型、可组合的实用程序直接在标记中设置样式，加快设计到实施的工作。
- 使用purge/jit的小CSS输出：Tailwind在构建时删除未使用的样式，保持最终CSS很小——有利于Core Web Vitals（更快的FCP）。
- 设计一致性：易于实现一致的间距、排版和响应式实用程序，而无需为每个组件编写自定义CSS文件。
- 快速原型设计：在vibecoding会话期间快速构建UI，并在视觉上迭代，而无需在CSS文件和标记之间切换。

**Tailwind的实施工具：**
- [Shadcn MCP](docs/development-tools/mcp-servers/shadcn-mcp-zh.md) — 专业UI组件库
- [Droid CLI](docs/development-tools/recommended-tools/droid-cli-zh.md) — 组件生成和样式辅助
- [阶段2：开发](docs/workflow/phase-2-development-zh.md) — UI实施工作流程

为什么选择Cloudflare Pages（和Cloudflare Functions）？
- 具有全球边缘网络的静态托管：Pages从边缘CDN提供生成的HTML和资产，以实现全球最小延迟。
- 对于大多数静态网站快速且免费：Cloudflare Pages为Astro生成的静态网站以低/零成本提供出色的性能。
- 无缝无服务器函数：当您需要后端逻辑（联系表单、API）时，Cloudflare Functions允许您添加小型端点而无需运行单独的服务器。
- 有用的额外功能：用于垃圾邮件保护的turnstile集成、从Git轻松部署和快速预览工作流程。

**Cloudflare的实施工具：**
- [托管工具指南](docs/hosting-tools/README-zh.md) — 完整的Cloudflare平台概述
- [Cloudflare Pages](docs/hosting-tools/cloudflare-pages-zh.md) — 逐步部署
- [Cloudflare Functions](docs/hosting-tools/cloudflare-workers-zh.md) — 无服务器后端设置
- [阶段4：部署](docs/workflow/phase-4-deployment-zh.md) — CI/CD和生产工作流程

这三个如何协同工作以实现Vibecoding
- 以最少的设置在本地构建，在几秒钟内迭代，并在没有运营头痛的情况下全球部署。
- 静态优先页面（Astro）+ 小型、有针对性的交互性（React岛屿）+ 实用程序样式（Tailwind）= 小负载和高CWV分数。
- 为表单或集成添加无服务器端点（Cloudflare Functions），同时保持主站点静态和可缓存。
- 非常适合作品集网站、营销网站、博客和小型产品UI——所有受益于快速加载和易于更新的内容。

您将立即注意到的具体好处
- 更小的包大小和更少的运行时JS成本 → 更好的First Contentful Paint（FCP）和LCP。[→ 性能标准](docs/quality-standards/performance-zh.md)
- 由于可预测的样式和静态渲染，布局偏移更少 → 更好的CLS。[→ 性能标准](docs/quality-standards/performance-zh.md)
- 更快的开发迭代（热重新加载/快速构建），因此您可以专注于设计、文案和氛围。[→ 工作流程指南](docs/workflow/README-zh.md)
- 简单的托管和部署：推送到Git，让Cloudflare Pages从边缘提供优化的资产。[→ 托管工具](docs/hosting-tools/README-zh.md)

集成和常见模式
- Astro中的React组件用于交互式小部件（表单、地图、媒体播放器）。
- TypeScript用于组件和无服务器端点的安全性。
- 公共表单上的Turnstile（Cloudflare CAPTCHA）用于垃圾邮件保护。
- 通过构建时或边缘实用程序进行图像优化，以保持页面轻量。
- 联系表单模式：客户端向/api/contact端点提交POST，该端点由Cloudflare Function支持，验证Turnstile并通过无服务器电子邮件集成转发电子邮件。

实用入门检查清单
- Node：使用Node.js v20+以获得可预测的构建。
- 安装和引导：npm install → npm run dev用于本地开发。
- 为生产构建：npm run build → 将dist/部署到Cloudflare Pages。
- 本地Pages预览（如果您想在本地测试函数）：npx wrangler pages dev dist --local
- 验证交互：测试跨核心页面的导航、响应行为和任何联系表单流程（带有Turnstile验证）。

保持高性能的快速提示
- 对于公共内容，首选SSG页面；仅在需要的岛屿中使用客户端JS。
- 使用Tailwind的purge/jit，以便发送到生产的CSS最少。
- 保持大型媒体延迟加载和优化（调整大小并提供Web友好格式）。
- 在边缘积极缓存静态内容（Cloudflare默认），并酌情为动态端点使用短期缓存。

何时考虑其他选择
- 具有类似应用程序路由和重客户端状态的大型单页Web应用程序可能更适合完整的客户端框架方法（Next.js/Remix/Vite+React SPA）——但对于网站和营销页面，Astro通常更简单、更快。
- 如果您需要对每个请求的个性化内容进行繁重的服务器渲染，请使用边缘函数，但要注意缓存策略和服务器成本。

## 实施工具和设置

**此栈的基本开发工具：**
- [Zed IDE](docs/development-tools/recommended-tools/zed-zh.md) — 为Astro + Tailwind工作流程优化
- [Droid CLI](docs/development-tools/recommended-tools/droid-cli-zh.md) — 用于组件生成的AI辅助
- [DevTools MCP](docs/development-tools/mcp-servers/devtools-mcp-zh.md) — Core Web Vitals测试和优化

**部署工具：**
- [Cloudflare Pages](docs/hosting-tools/README-zh.md) — 推荐的托管平台
- [Cloudflare Functions](docs/hosting-tools/README-zh.md) — 用于表单/API的无服务器端点

**AI模型配置：**
- [GLM Coding Plan](docs/ai-model-providers/glm-coding-plan-zh.md) — Astro开发的推荐LLM
- [预算替代方案](docs/ai-model-providers/README-zh.md#honorable-mentions) — 高性价比选项

**工作流程集成：**
- [阶段2：开发](docs/workflow/phase-2-development-zh.md) — 实际实施步骤
- [阶段4：部署](docs/workflow/phase-4-deployment-zh.md) — 生产部署指南

**业务考虑：**
- [托管成本优化](docs/hosting-tools/README-zh.md#cost-comparison) — 与传统托管相比节省95%
- [客户项目设置](docs/hosting-tools/README-zh.md#recommended-combinations) — 不同项目类型的预配置栈

## 实施设置检查清单