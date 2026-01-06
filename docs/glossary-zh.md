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