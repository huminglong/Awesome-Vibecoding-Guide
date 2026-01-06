# 术语表 📖

本指南中使用的所有技术术语、概念和缩写的综合参考指南。术语按类别组织，并在每个部分内按字母顺序排列，以便于导航。

---

## 核心Vibecoding概念

**Act Mode（执行模式）**: 一种开发模式，AI代理执行预先编写的计划，根据保存在markdown文件中的规范逐步实现任务。

**BYOK (Bring Your Own Key - 自带密钥)**: 一种方法，开发者使用自己从AI提供商获取的API密钥，而不是为捆绑服务付费，从而实现更大的成本控制和灵活性。

**Cascade**: Windsurf的AI代理，通过自动引用文档和工作流程来帮助完成编码任务。

**Context Compression（上下文压缩）**: 总结或压缩对话历史以释放AI模型上下文窗口空间的过程，同时保留关键信息。
→ [了解更多: 上下文管理](context-management/README-zh.md)

**Context Debt（上下文债务）**: 对话中不必要或过时信息的积累，会随着时间的推移降低AI性能，类似于软件开发中的技术债务。
→ [了解更多: 上下文管理](context-management/README-zh.md)

**Context Window（上下文窗口）**: AI模型在单次对话或交互中可以考虑和记住的最大文本量（以token衡量）。
→ [了解更多: 上下文管理](context-management/README-zh.md)

**Dialog Compression（对话压缩）**: 参见 **Context Compression（上下文压缩）**。

**Markdown Memory Pattern（Markdown记忆模式）**: 一种工作流程策略，将重要信息保存到.md文件中而不是保留在对话中，减少上下文使用并提供持久的知识存储。
→ [了解更多: 上下文管理](context-management/README-zh.md)

**Plan Mode（计划模式）**: 一种开发模式，在编写任何代码之前，您与AI讨论并创建全面的项目计划，通常将计划保存到markdown文件中。
→ [了解更多: 工作流程阶段](workflow/README-zh.md)

**Spec-Driven Development（规范驱动开发）**: 一种方法，在编码之前创建详细的规范和计划，使AI代理能够更准确、一致地实现功能。
→ [了解更多: 工作流程阶段](workflow/README-zh.md)

**The 60-85% Rule（60-85%规则）**: 上下文管理指南，建议在使用60-85%的上下文窗口时AI性能最佳；超过85%可能会降低响应质量。

**Vibecoder（Vibecoder开发者）**: 使用AI助手作为软件开发中协作伙伴的开发者，强调高效的工作流程和高性价比的工具组合。
→ [了解更多: 简介与哲学](introduction/README-zh.md)

**Vibecoding**: 一种软件开发方法，您与AI作为编码伙伴协作，使用自然语言和结构化工作流程高效地构建应用程序。
→ [了解更多: 简介与哲学](introduction/README-zh.md)

---

## 开发工具与软件

**AmpCode**: 一种替代的AI编码助手CLI工具，提供免费层级，支持MCP服务器和积极开发。

**Claude Code**: Anthropic的官方AI辅助编码CLI工具，集成了Claude的语言模型功能。
→ [了解更多: 开发工具](development-tools/README-zh.md)

**CLI (Command-Line Interface - 命令行界面)**: 基于文本的界面，您通过输入命令与软件交互，而不是在图形界面中点击按钮。

**Cursor**: 一个AI驱动的代码编辑器（IDE），具有内置的AI辅助和对外部AI模型的支持。
→ [了解更多: 开发工具](development-tools/README-zh.md)

**Droid CLI**: 一个具有自主规划能力、权限管理和自带密钥支持的命令行编码代理。
→ [了解更多: Droid CLI指南](development-tools/recommended-tools/droid-cli-zh.md)

**Gemini CLI**: Google的命令行界面，用于访问Gemini AI模型，为开发工作提供慷慨的免费层级。

**Git**: 一个版本控制系统，跟踪代码随时间的变化，允许您保存版本、与他人协作以及撤销错误。

**GitHub**: 一个基于云的平台，用于托管Git仓库，实现协作、代码共享和项目管理。

**GitHub Speckit**: GitHub的规范工具，用于创建结构化的项目计划（作者已从其工具栈中放弃，转而使用Clavix）。

**IDE (Integrated Development Environment - 集成开发环境)**: 一个软件应用程序，在一个地方提供编写、测试和调试代码的综合工具。

**Clavix**: 一个开源的提示工程框架，使用CLEAR方法论（简洁、逻辑、明确、自适应、反思）将粗略的想法转化为完善的提示、完整的PRD和可实施的任务列表。
→ [了解更多: Clavix指南](development-tools/recommended-tools/clavix-zh.md)

**OpenSpec**: 一个规范框架工具，将纯语言描述转化为全面的项目计划（已移至荣誉提及，被Clavix取代）。
→ [了解更多: OpenSpec指南](development-tools/honorable-mentions/openspec-cli-zh.md)

**Qwen CLI**: 阿里巴巴的免费开源编码助手CLI，为开发提供本地或基于云的AI辅助。
→ [了解更多: 开发工具](development-tools/README-zh.md)

**Terminal（终端）**: 基于文本的界面，通过输入命令与计算机操作系统交互。

**TRAE**: 一个AI驱动的开发IDE，具有双模式（IDE + SOLO）。SOLO对Pro用户广泛可用，实现从PRD到预览/发布的端到端自动化。Trae包括对高级模型（如`Gemini-3-Pro-Preview`）的内置访问，并提供慷慨的配额（Pro：约600次快速请求/月 + 无限慢速请求），使其成为一个强大的价值选项；上下文限制仍取决于模型。

**Traycer.ai**: 一个AI驱动的开发规划和验证工具（由于速度和成本问题，作者已从其工具栈中放弃）。

**VS Code (Visual Studio Code)**: 微软的免费、流行的代码编辑器，具有广泛的插件支持和自定义选项。

**Warp**: 一个现代的、GPU加速的终端模拟器，具有AI驱动的命令辅助和个性化选项。

**Windsurf**: 一个AI增强的代码编辑器，具有Cascade代理和高级上下文管理功能。
→ [了解更多: Windsurf指南](development-tools/recommended-tools/windsurf-zh.md)

**Zed**: 一个快速的、AI优先的代码编辑器，专为AI辅助开发而构建，具有最小的资源使用和原生AI集成。
→ [了解更多: Zed指南](development-tools/recommended-tools/zed-zh.md)

---

## MCP (Model Context Protocol - 模型上下文协议)

**Context7 MCP**: 一个MCP服务器，提供对框架文档和技术知识的访问，帮助AI助手通过检索相关文档来解决问题。
→ [了解更多: Context7 MCP](development-tools/mcp-servers/context7-mcp-zh.md)

**DevTools MCP**: 一个用于浏览器自动化和测试的MCP服务器，使AI代理能够自动导航网站、收集错误并使用Chrome DevTools调试前端问题。
→ [了解更多: DevTools MCP](development-tools/mcp-servers/devtools-mcp-zh.md)

**MCP (Model Context Protocol - 模型上下文协议)**: 一种标准协议，允许AI助手连接到外部数据源和工具，将其功能扩展到内置知识之外。
→ [了解更多: MCP服务器](development-tools/mcp-servers/README-zh.md)

**MCP Server（MCP服务器）**: 实现模型上下文协议的服务，为AI助手提供对特定外部资源（如文档、数据库或开发工具）的访问。
→ [了解更多: MCP服务器](development-tools/mcp-servers/README-zh.md)

**Sequential Thinking MCP**: 一个MCP服务器，强制AI助手在提供解决方案之前系统地、分步骤地思考问题，提高调试质量并防止冲动决策。
→ [了解更多: Sequential Thinking MCP](development-tools/mcp-servers/sequential-thinking-mcp-zh.md)

**Shadcn MCP**: 一个MCP服务器，使AI助手能够浏览、搜索和安装来自shadcn/ui库的专业UI组件。
→ [了解更多: Shadcn MCP](development-tools/mcp-servers/shadcn-mcp-zh.md)

**Task Manager MCP**: 一个MCP服务器，在不同的AI对话会话中维护任务列表，确保开发上下文得到保留，任务在完成前需要批准。
→ [了解更多: Task Manager MCP](development-tools/mcp-servers/task-manager-mcp-zh.md)

---

## AI/ML与LLM术语

**Agent（代理）**: 一个AI程序，可以自主执行任务，如编写代码、调试问题或分析需求，只需最少的人工干预。

**API Key（API密钥）**: 唯一标识符，用于验证您对AI服务或API的访问，通常保密并安全存储。

**Caching（缓存）**: 存储以前检索或生成的数据以供重用，降低成本并提高AI交互中的响应时间。

**Fine-tuning（微调）**: 在特定数据上训练现有AI模型的过程，使其在特定任务上表现更好。

**Frontier Model（前沿模型）**: 可用的最先进的AI模型，通常来自主要的AI研究公司，如OpenAI、Anthropic或Google。

**Hallucination（幻觉）**: 当AI模型生成听起来合理但事实错误或虚构的信息时。

**Inference（推理）**: AI模型根据您的输入提示生成输出（如代码或文本）的过程。

**LLM (Large Language Model - 大语言模型)**: 在大量文本数据上训练的AI系统，可以理解和生成类似人类的文本，用于编码辅助和对话等任务。

**Model Provider（模型提供商）**: 通过API提供AI模型访问的公司或服务，如OpenAI、Anthropic或Google。

**Model Routing（模型路由）**: 智能选择为不同类型的任务使用哪个AI模型，以优化成本、速度或质量。

**Open-Source Model（开源模型）**: 代码和权重公开可用的AI模型，允许任何人使用、修改或在本地运行。

**Prompt（提示）**: 您提供给AI模型的文本输入，以请求特定的响应或操作。
→ [了解更多: 提示指南](prompting/README-zh.md)

**Prompt Engineering（提示工程）**: 制作有效提示以从AI模型获得更好结果的实践。
→ [了解更多: 提示指南](prompting/README-zh.md)

**Rate Limiting（速率限制）**: 在特定时间段内您可以向AI服务发出的请求数量的限制。

**Token（令牌）**: AI模型处理的基本文本单位，大致相当于一个单词或单词的一部分。AI定价和限制通常以token衡量。

---

## AI模型提供商

**Factory AI**: 一个AI平台，提供具有竞争力的token定价和高级缓存，通过Droid CLI工具以降低的成本提供对GPT-5-high和codex等模型的访问。
→ [了解更多: AI模型提供商](ai-model-providers/README-zh.md)

**GLM (GLM Coding Plan)**: 一个高性价比的AI模型提供商，提供最先进的开源模型和慷慨的使用限制，作为作者开发工作的主要LLM。
→ [了解更多: GLM Coding Plan](ai-model-providers/glm-coding-plan-zh.md)

**Synthetic.new**: 一个隐私优先的AI模型提供商，提供对20+前沿开源模型的访问，具有竞争力的定价和高速率限制，强调代码隐私和快速性能。
→ [了解更多: AI模型提供商](ai-model-providers/README-zh.md)

---

## Web开发与技术

**Astro**: 一个现代Web框架，专注于生成具有最少JavaScript的静态HTML，针对快速加载的网站和出色的性能分数进行了优化。
→ [了解更多: 核心技术](core-technologies-zh.md)

**bcrypt**: 一种密码哈希算法，通过将用户密码转换为不可逆的加密字符串来安全存储。

**CDN (Content Delivery Network - 内容分发网络)**: 全球分布的服务器网络，从最近的服务器向用户提供Web内容，提高加载速度。

**Cloudflare Functions**: 在Cloudflare的边缘网络上运行的无服务器后端函数，允许您为静态站点添加动态功能。
→ [了解更多: Cloudflare Workers](hosting-tools/cloudflare-workers-zh.md)

**Cloudflare Pages**: 一个免费的静态网站托管平台，具有全球CDN、从Git自动部署以及对无服务器函数的内置支持。
→ [了解更多: Cloudflare Pages](hosting-tools/cloudflare-pages-zh.md)

**Cloudflare Workers**: 一个无服务器计算平台，在Cloudflare的全球网络上运行JavaScript代码，每天提供100,000次免费请求。
→ [了解更多: Cloudflare Workers](hosting-tools/cloudflare-workers-zh.md)

**CORS (Cross-Origin Resource Sharing - 跨源资源共享)**: 一种安全机制，控制哪些网站可以从不同域访问您的API或资源。

**D1 Database**: Cloudflare的无服务器SQL数据库服务，设计为与Cloudflare Workers和Pages无缝协作。
→ [了解更多: D1数据库](hosting-tools/cloudflare-d1-zh.md)

**Edge Functions（边缘函数）**: 在靠近用户的CDN边缘服务器上运行的小段代码，为动态内容提供快速响应时间。

**Island Architecture（岛屿架构）**: 一种Web开发模式，页面的大部分是静态HTML，带有小型交互式JavaScript组件"岛屿"，提高性能。

**Jamstack**: 一种Web开发架构，强调JavaScript、API和标记（静态文件），产生快速、安全和可扩展的网站。

**JWT (JSON Web Token)**: 在各方之间以JSON对象形式安全传输信息的方式，通常用于Web应用程序中的用户身份验证。

**Middleware（中间件）**: 位于不同应用程序组件之间的软件，通常用于身份验证、日志记录或在到达主应用程序之前处理请求。

**Next.js**: 一个流行的React框架，支持服务器端渲染、静态站点生成和API路由，用于构建全栈Web应用程序。

**React**: 一个用于使用可重用组件构建用户界面的JavaScript库，由Meta（Facebook）维护。

**SSG (Static Site Generation - 静态站点生成)**: 在构建时而不是用户请求时将所有网页构建为静态HTML文件的过程，导致更快的加载速度。

**SSR (Server-Side Rendering - 服务器端渲染)**: 为每个请求在服务器上生成网页HTML，允许动态内容同时改善SEO和初始加载时间。

**Svelte**: 一个现代JavaScript框架，在构建时将组件编译为高效的原生JavaScript，导致更小的包大小。

**Tailwind CSS**: 一个实用优先的CSS框架，提供小型、可组合的类来直接为HTML设置样式，实现快速UI开发而无需编写自定义CSS。
→ [了解更多: 核心技术](core-technologies-zh.md)

**Turnstile**: Cloudflare的隐私优先的CAPTCHA替代方案，用于保护网站免受垃圾邮件和机器人攻击。

**WebSocket**: 一种通信协议，在Web浏览器和服务器之间提供全双工（双向）通信，实现实时功能。

---

## 软件开发实践

**Acceptance Criteria（验收标准）**: 功能或任务必须满足的特定条件，才能被认为完成并可接受。

**ADR (Architecture Decision Record - 架构决策记录)**: 记录重要架构决策、其上下文和后果以供将来参考的文档。

**API (Application Programming Interface - 应用程序编程接口)**: 允许不同软件应用程序相互通信的规则和协议集。

**Branch（分支）**: 在Git中，一个独立的开发线，允许您在不影响主代码库的情况下开发功能。

**CI/CD (Continuous Integration/Continuous Deployment - 持续集成/持续部署)**: 自动化测试代码更改并将其部署到生产环境的过程，减少手动工作和错误。

**Code Review（代码审查）**: 让其他开发者检查您的代码以确保质量、错误和符合标准的实践，然后才将其合并。

**Commit（提交）**: 在Git中，使用描述性消息保存代码更改快照。

**Debugging（调试）**: 查找和修复代码中的错误或意外行为的过程。

**Deployment（部署）**: 将代码发布到用户可以访问的服务器或平台的过程。

**Edge Case（边缘情况）**: 可能导致错误的不寻常或罕见情况，如果在代码中未正确处理。

**Feature Branch（功能分支）**: 专门为开发单个功能而创建的Git分支，将其与主代码库隔离直到完成。

**Main Branch（主分支）**: 包含生产就绪代码的主要Git分支（通常称为"main"或"master"）。

**Merge Request / Pull Request（合并请求/拉取请求）**: 将代码更改从一个Git分支合并到另一个分支的请求，通常包括代码审查以获得批准。

**Migration（迁移）**: 更新数据库结构（模式）或将数据从一个系统移动到另一个系统的脚本或过程。

**MVP (Minimum Viable Product - 最小可行产品)**: 具有足够功能以可用并为早期用户提供价值的最简单产品版本。

**PRD (Product Requirements Document - 产品需求文档)**: 描述产品应该做什么、谁将使用它以及它解决什么问题的文档。

**Push（推送）**: 在Git中，将本地代码更改上传到远程仓库（如GitHub）。

**Refactoring（重构）**: 重构现有代码以提高其质量、可读性或性能，而不改变其功能。

**Regression（回归）**: 代码更改意外破坏以前正常工作的现有功能。

**Remote Repository（远程仓库）**: 存储在服务器（如GitHub）上的Git仓库版本，允许备份和协作。

**REST API**: 使用HTTP方法（GET、POST、PUT、DELETE）与服务器资源交互创建Web API的标准方式。

**Rollback（回滚）**: 将代码恢复到以前版本，通常在部署在生产环境中引起问题时进行。

**SaaS (Software as a Service - 软件即服务)**: 通过互联网作为服务提供的软件，通常通过订阅模式。

**Schema（模式）**: 数据库的结构和组织，定义表、列和关系。

**Serverless（无服务器）**: 一种云计算模型，您编写按需运行的函数而无需管理服务器，仅按执行时间付费。

**Staging Environment（预发布环境）**: 一个与生产环境密切相似的测试环境，用于在向用户发布之前验证更改。

**Technical Debt（技术债务）**: 由于选择快速、简单的解决方案而不是需要更长时间的更好方法而导致的未来返工的隐含成本。

**Unit Test（单元测试）**: 验证单个、小段代码（如函数）在隔离状态下正确工作的测试。

**User Story（用户故事）**: 从最终用户角度描述功能的描述，通常遵循"作为[用户]，我想要[目标]，以便[原因]"的格式。

**Version Control（版本控制）**: 一个系统（如Git），跟踪文件随时间的变化，允许您查看历史记录并恢复到以前的版本。

---

## Core Web Vitals与性能

**Bundle Size（包大小）**: 浏览器必须下载以运行Web应用程序的JavaScript和CSS的总文件大小，影响加载速度。

**CLS (Cumulative Layout Shift - 累积布局偏移)**: 一个Core Web Vitals指标，衡量视觉稳定性 - 页面内容在加载期间意外移动的程度。
→ [了解更多: 性能标准](quality-standards/performance-zh.md)

**Core Web Vitals (CWV)**: Google的一套衡量网站性能和用户体验的指标，包括加载速度、交互性和视觉稳定性。
→ [了解更多: 性能标准](quality-standards/performance-zh.md)

**FCP (First Contentful Paint - 首次内容绘制)**: 一个性能指标，衡量屏幕上首次出现内容所需的时间。
→ [了解更多: 性能标准](quality-standards/performance-zh.md)

**Hydration（水合）**: 通过附加JavaScript事件处理程序和状态使静态HTML页面可交互的过程。

**Lazy Loading（懒加载）**: 延迟加载图像或内容直到需要时（例如，滚动到视图中），改善初始页面加载时间。

**LCP (Largest Contentful Paint - 最大内容绘制)**: 一个Core Web Vitals指标，衡量最大的可见内容元素加载所需的时间。
→ [了解更多: 性能标准](quality-standards/performance-zh.md)

**PageSpeed**: Google的工具和指标，用于衡量网站性能并提供优化建议。
→ [了解更多: 性能标准](quality-standards/performance-zh.md)

**Partial Hydration（部分水合）**: 仅加载交互式组件的JavaScript，而不是整个页面，减少初始包大小。

**TTFB (Time To First Byte - 首字节时间)**: 一个性能指标，衡量浏览器从服务器接收第一个数据字节所需的时间。

---

## 文件格式与配置

**Environment Variables（环境变量）**: 与代码分开存储的配置值（如API密钥），通常在.env文件中，用于安全和灵活性。

**JSON (JavaScript Object Notation)**: 用于存储和交换数据的文本格式，使用人类可读的键值对和数组。

**Markdown (.md)**: 一种轻量级文本格式，使用简单符号进行格式化（如#表示标题，*表示列表），作为纯文本易于阅读。

**YAML (YAML Ain't Markup Language)**: 一种人类可读的数据格式，通常用于配置文件，使用缩进来显示结构。

---

## 开发工作流程与工具

**Branch Strategy（分支策略）**: 组织项目中Git分支的计划，如使用功能分支进行开发和主分支进行生产。

**Build Process（构建过程）**: 将源代码转换为可部署应用程序的步骤，包括编译、打包和优化。

**Build Time（构建时间）**: 代码被编译和准备部署的阶段，与用户交互的运行时相对。

**Deployment Script（部署脚本）**: 处理将代码发布到生产服务器过程的自动化命令。

**Hot Reload（热重载）**: 一个开发功能，当您更改代码时自动在浏览器中更新应用程序，而不会丢失状态。

**Local Development（本地开发）**: 在自己的计算机上运行和测试应用程序，然后将其部署到服务器。

**Package Manager（包管理器）**: 一个工具（如npm、yarn或pnpm），用于安装和管理项目的代码库和依赖项。

**Plugin（插件）**: 扩展主要应用程序功能的附加软件，如VS Code扩展。

**Production Environment（生产环境）**: 您的应用程序为真实用户运行的实时服务器/系统。

**Sandbox（沙箱）**: 用于安全测试代码而不影响生产系统的隔离环境。

**Task Breakdown（任务分解）**: 将大型功能分解为更小、可管理的任务，可以单独实现和测试。

**Workflow（工作流程）**: 为高效完成开发任务而遵循的步骤或过程序列。

---

## 安全与身份验证

**API Endpoint（API端点）**: API中执行特定功能的特定URL，如/api/login用于用户身份验证。

**Authentication（身份验证）**: 验证用户身份的过程，通常通过用户名和密码。

**Authorization（授权）**: 确定经过身份验证的用户被允许访问哪些操作或资源。

**CSRF (Cross-Site Request Forgery - 跨站请求伪造)**: 一种安全漏洞，攻击者诱骗用户在他们已认证的网站上执行不需要的操作。

**Hash（哈希）**: 一种单向加密函数，将数据（如密码）转换为无法逆转的固定长度字符串。

**Input Validation（输入验证）**: 检查用户输入以确保其符合预期格式且不包含恶意代码的处理。

**OAuth**: 一种授权标准，允许用户授予应用程序访问其信息的权限，而无需共享密码。

**Password Hashing（密码哈希）**: 将明文密码转换为不可逆的加密字符串，然后将其存储在数据库中。

**Rate Limiting（速率限制）**: 限制用户在时间段内可以向API发出的请求数量，防止滥用。

**Sanitization（净化）**: 通过删除或转义潜在危险字符来清理用户输入。

**Session（会话）**: 用户和应用程序之间的交互期间，通常通过cookie或令牌维护。

**SQL Injection（SQL注入）**: 一种安全漏洞，攻击者通过用户输入插入恶意SQL代码来操纵数据库。

**XSS (Cross-Site Scripting - 跨站脚本)**: 一种安全漏洞，攻击者将恶意脚本注入其他用户查看的网站。

---

## 开发概念

**Abstraction（抽象）**: 在简单接口后面隐藏复杂实现细节，使代码更易于使用和理解。

**Async/Await**: 一种JavaScript语法，用于以更可读、顺序的方式处理异步操作。

**Boilerplate**: 在许多地方需要但实现之间变化不大的标准、重复代码。

**Breaking Change（破坏性更改）**: 导致现有功能停止工作的代码更改。

**Codebase（代码库）**: 项目的完整源代码集合。

**Component（组件）**: 一个可重用的、自包含的代码片段（如按钮或表单），可以在多个地方使用。

**Dependency（依赖项）**: 项目依赖以正常运行的外部代码库或包。

**Design Pattern（设计模式）**: 解决常见编程问题的可重用解决方案，提供经过验证的代码结构方法。

**DRY (Don't Repeat Yourself - 不要重复自己)**: 一个编码原则，强调代码重用以避免重复和维护问题。

**Endpoint（端点）**: 参见 **API Endpoint（API端点）**。

**Fallback（备选方案）**: 当主要方法失败或不可用时使用的替代选项。

**Framework（框架）**: 提供通用功能的结构化代码基础，允许您通过遵循其模式更快地构建应用程序。

**Library（库）**: 为您的项目提供特定功能的预编写代码集合。

**Localhost**: 用于访问在自己计算机上运行的应用程序的地址（通常为http://localhost或127.0.0.1）。

**Monorepo（单体仓库）**: 包含多个相关项目或包的单个仓库，一起管理。

**Namespace（命名空间）**: 组织代码元素（如变量或函数）以避免命名冲突的容器。

**npm (Node Package Manager)**: JavaScript的默认包管理器，用于安装和管理代码库。

**Payload（负载）**: 在API请求或响应中发送的实际数据，不包括头部和元数据。

**Polyfill**: 为不原生支持它的旧浏览器提供现代功能的代码。

**Query（查询）**: 从数据库或API请求数据。

**Repository (Repo)（仓库）**: 代码的存储位置，通常用Git管理，包含所有项目文件及其历史记录。

**Scope（作用域）**: 变量或函数可访问和使用的代码部分。

**SDK (Software Development Kit - 软件开发工具包)**: 用于在特定平台上开发应用程序的工具、库和文档集合。

**Single Responsibility Principle（单一职责原则）**: 一个设计原则，说明每个模块或函数应该只有一个改变的理由。

**Stack（技术栈）**: 项目中使用的技术组合，如"Astro + Tailwind + Cloudflare Pages"。

**Stack Trace（堆栈跟踪）**: 显示导致错误的函数调用序列的报告，用于调试。

**Syntax（语法）**: 编写有效代码的规则和结构。

**Type（类型）**: 数据的分类（如字符串、数字或布尔值），确定可以对其执行的操作。

**TypeScript**: 通过添加可选类型检查扩展JavaScript的编程语言，以获得更好的错误检测。

---

## 缩写快速参考

- **ADR**: 架构决策记录
- **API**: 应用程序编程接口
- **BYOK**: 自带密钥
- **CDN**: 内容分发网络
- **CI/CD**: 持续集成/持续部署
- **CLI**: 命令行界面
- **CLS**: 累积布局偏移
- **CORS**: 跨源资源共享
- **CSRF**: 跨站请求伪造
- **CWV**: Core Web Vitals
- **DRY**: 不要重复自己
- **FCP**: 首次内容绘制
- **IDE**: 集成开发环境
- **JSON**: JavaScript对象表示法
- **JWT**: JSON Web Token
- **LCP**: 最大内容绘制
- **LLM**: 大语言模型
- **MCP**: 模型上下文协议
- **MVP**: 最小可行产品
- **npm**: Node包管理器
- **OAuth**: 开放授权
- **PRD**: 产品需求文档
- **REST**: 表述性状态转移
- **ROI**: 投资回报率
- **SaaS**: 软件即服务
- **SDK**: 软件开发工具包
- **SEO**: 搜索引擎优化
- **SQL**: 结构化查询语言
- **SSG**: 静态站点生成
- **SSR**: 服务器端渲染
- **TTFB**: 首字节时间
- **UI**: 用户界面
- **URL**: 统一资源定位符
- **UX**: 用户体验
- **XSS**: 跨站脚本
- **YAML**: YAML Ain't Markup Language

---

返回索引：[顶级README](../README-zh.md)