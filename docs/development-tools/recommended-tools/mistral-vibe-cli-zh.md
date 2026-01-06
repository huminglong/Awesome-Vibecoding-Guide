# Mistral Vibe CLI — 开源编码代理 🚀

> **开源冠军**：Mistral Vibe CLI是一个强大的开源命令行编码助手，由最先进的Devstral 2模型驱动。它将代理编码带到您的终端，采用"安全第一"的方法和深入的项目感知。

## 为什么Mistral Vibe是一个"Vibe"

Mistral Vibe不仅仅是另一个CLI工具；它是一个完全成熟的编码代理，理解您的整个项目上下文。它结合了Mistral的新Devstral 2模型的力量和流畅、交互式CLI体验，感觉对您的工作流程来说是原生的。

### 主要区别

**由Devstral 2驱动：**
- 基于Devstral 2（123B）和Devstral Small 2（24B）模型
- **SOTA开源模型**：在SWE-bench Verified上达到72.2%
- **高性价比**：在现实世界任务中比Claude Sonnet高7倍的成本效率

**项目感知上下文：**
- 自动扫描您的文件结构和Git状态
- 理解您的整个代码库，而不仅仅是您正在编辑的文件
- 为复杂重构启用架构级推理

**强大的工具集：**
- **交互式聊天**：分解复杂任务的对话界面
- **文件操作**：精确读取、写入和修补文件
- **智能搜索**：使用`grep`的递归代码搜索（ripgrep支持）
- **Shell集成**：在有状态终端中执行shell命令

**安全和控制：**
- **安全第一**：默认情况下具有工具执行批准
- **高度可配置**：通过`config.toml`自定义模型、提供商和工具权限
- **透明**：开源（Apache 2.0），因此您确切知道它在做什么

## 安装和设置

开始使用Mistral Vibe非常快。

**一键安装（Linux/macOS）：**
```bash
curl -LsSf https://mistral.ai/vibe/install.sh | bash
```

**使用uv：**
```bash
uv tool install mistral-vibe
```

**使用pip：**
```bash
pip install mistral-vibe
```

**集成：**
Mistral Vibe也可作为**[Zed](./zed-zh.md)**中的扩展使用，允许您直接在您喜欢的IDE中使用它。

## 定价：目前免费！💸

目前，Mistral Vibe CLI和为其提供动力的Devstral 2模型通过Mistral API**免费使用**。

- **Devstral 2（123B）**：通过API免费（目前）
- **Devstral Small 2（24B）**：通过API免费（目前）

*注意：免费期后，将应用API定价（Devstral 2每百万令牌$0.40/$2.00）。*

## 何时使用Mistral Vibe

| 场景          | 使用Mistral Vibe                   |
| ------------- | ---------------------------------- |
| **预算编码**  | ✅ **完美选择**（免费SOTA模型）     |
| **开源偏好**  | ✅ **最佳选择**（开源权重和代码）   |
| **隐私/控制** | ✅ **绝佳选择**（支持运行本地模型） |
| **复杂重构**  | ✅ **强力竞争者**（项目感知上下文） |

## Vibe的专业提示

### 1. 交互模式
只需运行`vibe`进入交互循环。使用`@`引用文件以进行智能自动完成和上下文加载。

### 2. 自动批准
为了更快的迭代，使用`--auto-approve`标志（或使用`Shift+Tab`切换）让代理在无需手动确认的情况下执行工具。
```bash
vibe --auto-approve
```

### 3. 智能引用
不要复制粘贴路径。使用智能引用系统：
```bash
> 重构@src/main.py文件以改进错误处理
```

## 结论

Mistral Vibe CLI是vibecoder工具包的绝佳补充。它提供高级、代理编码体验，**免费**（目前），由与行业最佳相媲美的模型驱动。如果您正在寻找Claude Code的开源替代方案，或者只是想尝试新的Devstral模型，Mistral Vibe是必须尝试的。

---

**相关：**
- [Mistral AI](../../ai-model-providers/honorable-mentions/mistral-ai-zh.md) — 魔力背后的提供商
- [Zed](./zed-zh.md) — 官方IDE集成

返回: [开发工具](../README-zh.md)