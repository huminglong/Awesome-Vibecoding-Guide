# 贡献 🤝

感谢您对改进Awesome Vibecoding指南的兴趣！本文档提供了为仓库做出贡献的综合指南，特别关注在整个文档中保持一致的交叉引用。

## 目录

1. [入门](#入门)
2. [贡献类型](#贡献类型)
3. [交叉引用标准](#交叉引用标准)
4. [写作指南](#写作指南)
5. [文档结构](#文档结构)
6. [审查流程](#审查流程)
7. [样式指南](#样式指南)

---

## 入门

### 如何贡献

**对于小更改：**
1. Fork仓库
2. 按照以下指南进行更改
3. 通过阅读受影响的部分来测试您的更改
4. 提交带有清晰描述的拉取请求

**对于重大更改：**
1. 打开一个问题以讨论您提议的更改
2. 等待维护者反馈和批准
3. 按照这些指南实施更改
4. 提交对您的问题的拉取引用

**对于错误修复：**
1. 检查现有问题以避免重复
2. 如有必要，打开一个带有重现步骤的新问题
3. 按照指南修复问题
4. 包括测试或验证步骤

---

## 贡献类型

**高优先级贡献：**
- ✅ 交叉引用改进和添加
- ✅ 工具文档更新
- ✅ 工作流程澄清
- ✅ 新工具推荐
- ✅ 成本优化策略

**中优先级贡献：**
- ✅ 语法和清晰度改进
- ✅ 结构和导航增强
- ✅ 示例和用例
- ✅ 性能优化

**低优先级贡献：**
- ✅ 风格改进
- ✅ 小的格式更改
- ✅ 冗余内容删除

---

## 交叉引用标准

### 何时添加交叉引用

**始终链接当：**
- ✅ 部分提到具有专用文档的工具
- ✅ 工作流程阶段相互引用
- ✅ 一个部分中的概念在其他地方详细解释
- ✅ 实施细节引用更大的概念
- ✅ 业务考虑与技术实施相关

**链接类型和示例：**

**1. 导航链接**
```markdown
[描述性链接文本](../path/to/document.md) — 用于直接导航

示例：
- [核心技术](../core-technologies-zh.md) — 推荐技术栈
- [阶段1：规划](../workflow/phase-1-planning-zh.md) — 项目结构
- [托管工具](../hosting-tools/README-zh.md) — 完整托管指南
```

**2. 部分特定链接**
```markdown
[描述性链接文本](../path/to/document.md#specific-section) — 用于定向信息

示例：
- [阶段0工具选择](../workflow/phase-0-vibecoder-preparation-zh.md#tool-setup) — 设置详细信息
- [免费替代方案](../ai-model-providers/README-zh.md#honorable-mentions) — 预算选项
- [成本优化](../workflow/phase-0-vibecoder-preparation-zh.md#cost-effective-development-strategy) — 省钱提示
```

**3. 工具特定链接**
```markdown
[工具提及](../development-tools/tool-name-zh.md) — 用于详细工具信息

示例：
- [Zed IDE](../development-tools/recommended-tools/zed-zh.md) — 主要开发环境
- [DevTools MCP](../development-tools/mcp-servers/devtools-mcp-zh.md) — 测试和调试
- [GLM编码计划](../ai-model-providers/glm-coding-plan-zh.md) — 主要AI提供商
```

**4. 概念链接**
```markdown
[概念引用](../path/to/document-zh.md) — 用于详细解释

示例：
- [上下文管理](../context-management/README-zh.md) — 高效工作流程实践
- [Vibecoding哲学](../introduction/philosophy-zh.md) — 核心原则
- [MCP服务器](../development-tools/README-zh.md#mcp-servers) — 工具集成
```

### 交叉引用质量检查清单

**在提交任何贡献之前：**

- [ ] 所有主要概念链接到相关部分
- [ ] 工作流程阶段引用适当的工具和下一步
- [ ] 业务考虑在相关时集成
- [ ] 新部分包括"相关阅读"或"下一步"
- [ ] 导航链接使用描述性文本（避免"点击这里"）
- [ ] 交叉引用保持上下文（无需阅读目标即可理解）
- [ ] 内部链接工作（通过点击验证）
- [ ] 链接文本与目标文档标题一致

### 常见交叉引用模式

**工具集成部分：**
```markdown
## [技术]的实施工具

**开发工具：**
- [工具名称](../path/to/tool-zh.md) — 使用描述
- [工具名称](../path/to/tool-zh.md) — 特定集成优势

**工作流程集成：**
- [阶段X](../workflow/phase-x-name-zh.md) — 如何融入流程
- [相关阶段](../workflow/phase-y-name-zh.md) — 依赖关系和连接

**预算考虑：**
- [成本比较](../ai-model-providers/README-zh.md#pricing) — 财务影响
- [免费替代方案](../development-tools/honorable-mentions/README-zh.md) — 预算选项
```

**工作流程跨阶段集成：**
```markdown
## 先决条件和下一步

**需要完成：**
- [上一阶段](../workflow/phase-x-previous-zh.md) — 首先需要什么
- [设置文档](../path/to/setup-zh.md) — 先决条件
- [工具配置](../development-tools/README-zh.md) — 环境准备

**为以下做准备：**
- [下一阶段](../workflow/phase-y-next-zh.md) — 接下来是什么
- [相关文档](../path/to/document-zh.md) — 支持信息

**相关阅读：**
- [概念解释](../path/to/concept-zh.md) — 详细背景
- [实际示例](../path/to/examples-zh.md) — 实施细节
```

### 应避免的交叉引用反模式

**❌ 不要：**
- 链接到不存在的文件
- 使用通用链接文本如"点击这里"或"这里"
- 创建循环引用
- 链接到不存在的部分
- 通过更改文件名而不更新引用来破坏链接
- 过度链接（链接每个其他词）

**✅ 要做：**
- 验证所有链接工作
- 使用描述性、有意义的链接文本
- 保持交叉引用平衡和有帮助
- 移动文件时更新引用
- 测试导航流程
- 添加链接时考虑用户体验

---

## 写作指南

### 内容标准

**保持部分：**
- ✅ **简洁** - 快速切入要点
- ✅ **可操作** - 读者可以立即遵循步骤
- ✅ **一致** - 匹配指南的实用、基于经验的语调
- ✅ **最新** - 反映当前工具和最佳实践

**避免：**
- ❌ 没有实际应用的理论讨论
- ❌ 过时的工具推荐
- ❌ 过于复杂的解释
- ❌ 没有正当理由的个人偏好

### 语音和语调

**保持指南的语音：**
- 实用和基于经验
- 自信但不教条
- 注重成本和效率
- 适合中级开发人员
- 行动导向语言

**示例：**
```markdown
✅ 好的："使用Cloudflare Pages进行托管 - 它是免费且快速的"
❌ 避免："有人可能会考虑Cloudflare Pages可能提供优势"
```

### 技术准确性

**确保所有推荐都是：**
- 基于真实商业经验
- 经过测试和验证
- 性价比高且实用
- 当前维护和支持

**推荐工具时：**
- 包括定价信息
- 提及预算限制的替代方案
- 提供设置说明或链接
- 解释为什么选择此工具而非替代方案

---

## 文档结构

### 文件组织

```
docs/
├── README.md                 # 主索引
├── introduction/             # 哲学和方法
│   ├── README.md
│   └── philosophy.md
├── workflow/                 # 核心方法论
│   ├── README.md
│   ├── phase-0-vibecoder-preparation.md
│   ├── phase-1-planning.md
│   ├── phase-2-development.md
│   ├── phase-3-testing-debugging.md
│   └── phase-4-deployment.md
├── development-tools/        # 工具文档
│   ├── README.md
│   ├── recommended-tools/
│   ├── mcp-servers/
│   └── honorable-mentions/
├── ai-model-providers/       # AI服务文档
│   ├── README.md
│   ├── glm-coding-plan.md
│   ├── synthetic-new.md
│   ├── factory-ai.md
│   └── honorable-mentions/
├── core-technologies.md      # 技术栈推荐
├── hosting-tools/           # 基础设施指南
│   └── README.md
├── context-management/      # 工作流程效率
│   └── README.md
├── glossary.md              # 关键术语
└── contributing.md          # 此文件
```

### 部分结构模板

**新工具文档：**
```markdown
# 工具名称

## 概述
[工具做什么，为什么推荐]

## 定价
[当前定价，成本比较]

## 主要功能
[主要能力，优势]

## 集成设置
[逐步配置]

## 工作流程集成
**使用于：**
- [阶段X](../workflow/phase-x-zh.md) — 特定用例
- [阶段Y](../workflow/phase-y-zh.md) — 另一个用例

**工具集成：**
- [相关工具](../development-tools/related-tool-zh.md) — 它们如何协同工作
- [设置指南](../development-tools/setup-zh.md) — 配置

## 最佳实践
[推荐使用模式]

## 何时使用
[此工具表现出色的场景]

## 替代方案
- [免费选项](../development-tools/honorable-mentions/free-tool-zh.md) — 预算替代方案
- [高级选项](../development-tools/premium-tool-zh.md) — 功能比较

返回：[开发工具](../development-tools/README-zh.md)
```

**新工作流程阶段：**
```markdown
# 阶段X：[名称]

**工具：** [所需工具和技术]

## 概述
[此阶段完成什么]

## 先决条件
- [上一阶段完成](../workflow/phase-x-previous-zh.md)
- [设置要求](../path/to/setup-zh.md)

## 逐步过程
[详细实施步骤]

## 最佳实践
[经过验证的方法和提示]

## 常见陷阱
[要避免什么以及为什么]

## 质量保证
[如何验证完成]

## 下一步
- [阶段Y](../workflow/phase-y-next-zh.md) — 接下来是什么
- [相关文档](../path/to/document-zh.md) — 支持信息

返回：[工作流程概述](../workflow/README-zh.md)
```

---

## 审查流程

### 自我审查清单

在提交任何贡献之前，验证：

**内容质量：**
- [ ] 信息准确且最新
- [ ] 示例经过测试并按描述工作
- [ ] 定价和工具信息当前
- [ ] 技术细节正确
- [ ] 语调匹配指南语音

**结构和导航：**
- [ ] 部分标题遵循一致的层次结构
- [ ] 目录在适用时更新
- [ ] 交叉引用准确且有帮助
- [ ] 导航流程有意义
- [ ] 文件组织遵循既定模式

**交叉引用：**
- [ ] 所有链接工作并指向正确目标
- [ ] 链接文本描述性和有意义
- [ ] 交叉引用增强而非混乱
- [ ] 相关概念正确连接
- [ ] 工作流程阶段引用适当的工具和下一步

**写作质量：**
- [ ] 语法和拼写正确
- [ ] 句子清晰简洁
- [ ] 技术术语解释或链接
- [ ] 提供可操作建议
- [ ] 内容为指南增加价值

### 测试您的更改

**导航测试：**
1. 通读受影响的部分
2. 点击所有交叉引用以确保它们工作
3. 验证导航流程有意义
4. 检查断开的链接或缺失的部分

**内容验证：**
1. 测试任何代码示例或命令
2. 验证工具定价和可用性
3. 检查步骤按描述工作
4. 确保与提及的工具兼容

**用户体验：**
1. 作为新用户阅读部分
2. 验证信息流逻辑合理
3. 检查交叉引用提供价值
4. 确保内容可访问且可操作

---

## 样式指南

### 写作风格

**首选语言：**
- 使用主动语态
- 用第二人称("你")进行直接指导
- 使用现在时进行说明
- 保持对话式但专业
- 包括具体示例和数字

**格式偏好：**
```markdown
# 主标题 (## 用于主要部分)
## 子部分标题

**工具名称和重要术语** 用粗体
`代码示例` 和 `命令` 用反引号
- 列表用项目符号
- 编号列表用于逐步过程
```

### 交叉引用格式

**标准模式：**

**导航：**
```markdown
返回：[部分名称](../path/to/document-zh.md)
下一步：[部分名称 → 部分标题](../path/to/document-zh.md)
```

**相关信息：**
```markdown
**相关阅读：**
- [概念名称](../path/to/concept-zh.md) — 简短描述
- [工具指南](../path/to/tool-zh.md) — 如何使用

**工具集成：**
- [工具名称](../development-tools/tool-zh.md) — 主要使用
- [设置指南](../development-tools/setup-zh.md) — 配置
```

### 代码和命令示例

**格式一致：**
```markdown
**终端命令：**
```bash
npm install package-name
npm run build
```

**配置示例：**
```toml
# wrangler.toml
name = "my-project"
compatibility_date = "2023-10-30"
```

**API示例：**
```typescript
// TypeScript/JavaScript
const response = await fetch('/api/endpoint');
const data = await response.json();
```
```

---

## 快速参考

### 常见交叉引用模式

| 源部分 | 目标类型 | 链接格式 |
|---------------|-------------|-------------|
| 工作流程阶段 | 工具 | [工具名称](../development-tools/tool-zh.md) — 使用描述 |
| 工作流程阶段 | 其他阶段 | [阶段名称](../workflow/phase-name-zh.md) — 接下来是什么 |
| 工具文档 | 工作流程阶段 | [阶段X](../workflow/phase-x-zh.md) — 何时使用此工具 |
| 概念 | 实施 | [相关指南](../path/to/guide-zh.md) — 实际应用 |
| 业务内容 | 技术 | [技术栈](../core-technologies-zh.md) — 实施细节 |

### 文件命名约定

- 使用连字符用于文件名：`phase-1-planning.md`
- 保持描述性但简洁的名称：`glm-coding-plan.md` (不是 `glm-coding-plan-detailed-guide.md`)
- 使用 `README.md` 作为目录索引
- 避免在文件名中使用空格和特殊字符

### 链接验证命令

```bash
# 检查断开的链接（如果您安装了linkchecker）
linkchecker --check-extern docs/

# 或手动验证关键导航路径：
# - 主README → 所有主要部分
# - 工作流程阶段 → 下一步/上一步阶段
# - 工具文档 → 相关工作流程阶段
# - 修改文件中的所有交叉引用
```

---

## 问题和支持

**如果您需要贡献帮助：**
1. 检查现有问题以获取类似讨论
2. 打开一个带有您问题的新问题
3. 标记维护者以获得特定指导

**对于紧急问题：**
- 关键断开的链接
- 过时的定价信息
- 与安全相关的问题

**对于增强讨论：**
- 新工具推荐
- 工作流程改进
- 文档结构更改

---

感谢您为Awesome Vibecoding指南做出贡献！您的改进有助于使此资源对整个社区更有价值。

**返回索引：** [顶级README](../README-zh.md)