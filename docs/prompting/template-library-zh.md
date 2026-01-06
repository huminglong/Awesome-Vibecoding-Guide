# 提示模板库 📚

**常见开发场景的复制粘贴就绪提示**

**如何使用**：找到您的场景，复制模板，填写{{PLACEHOLDERS}}，发送给AI。

---

## 目录

1. [功能实施模板](#功能实施模板)
2. [调试模板](#调试模板)
3. [重构模板](#重构模板)
4. [测试模板](#测试模板)
5. [文档模板](#文档模板)
6. [代码审查模板](#代码审查模板)
7. [性能模板](#性能模板)
8. [安全模板](#安全模板)

---

## 功能实施模板

### 模板1：简单功能添加

```
在{{FILE_PATH}}中实施{{FEATURE_NAME}}。

要求：
- {{REQUIREMENT_1}}
- {{REQUIREMENT_2}}
- {{REQUIREMENT_3}}

遵循{{REFERENCE_FILE}}中{{REFERENCE_FEATURE}}使用的模式。

成功标准：
- {{CRITERION_1}}
- {{CRITERION_2}}
- {{CRITERION_3}}
```

**示例填写：**
```
在src/components/RegisterForm.tsx中实施电子邮件验证。

要求：
- 在模糊时验证（不是每次击键）
- 在输入字段下方显示错误消息
- 使用标准电子邮件正则表达式检查格式
- 必填字段（空 = 错误）

遵循src/components/RegisterForm.tsx中密码验证使用的模式（第45-67行）。

成功标准：
- 无效电子邮件 → 红色边框 + 错误消息
- 有效电子邮件 → 绿色边框，无错误
- 模糊时为空字段 → 错误消息'Email is required'
```

---

### 模板2：API端点创建

```
创建{{HTTP_METHOD}} {{ENDPOINT_PATH}}端点。

文件：{{FILE_PATH}}

请求：
{{REQUEST_SCHEMA}}

响应：
成功（{{SUCCESS_STATUS_CODE}}）：{{SUCCESS_RESPONSE_SCHEMA}}
错误（{{ERROR_STATUS_CODE}}）：{{ERROR_RESPONSE_SCHEMA}}

业务逻辑：
- {{LOGIC_1}}
- {{LOGIC_2}}

验证：
- {{VALIDATION_1}}
- {{VALIDATION_2}}

遵循{{REFERENCE_FILE}}中{{REFERENCE_ENDPOINT}}的模式。

包括：
- 输入验证（express-validator）
- 错误处理（try-catch）
- {{ADDITIONAL_MIDDLEWARE}}中间件
```

**示例填写：**
```
创建POST /api/posts端点。

文件：src/api/posts.ts

请求：
{
  title: 字符串（必需，最多200个字符），
  content: 字符串（必需，最多5000个字符），
  tags: 字符串[]（可选，最多5个标签）
}

响应：
成功（201）：{ data: { id, title, content, tags, authorId, createdAt } }
错误（400）：{ error: { code: 'VALIDATION_ERROR', message: string } }

业务逻辑：
- 将帖子与认证用户关联（req.user.id）
- 清理内容中的HTML
- 标准化标签（小写，修剪）

验证：
- 标题必需，最大长度
- 内容必需，最大长度
- 标签数组，最多5个项目

遵循src/api/users.ts中POST /api/users的模式。

包括：
- 输入验证（express-validator）
- 错误处理（try-catch）
- 身份验证中间件（requireAuth）
```

---

### 模板3：React组件创建

```
创建{{COMPONENT_NAME}}组件。

文件：{{FILE_PATH}}

Props：
{{PROPS_INTERFACE}}

行为：
- {{BEHAVIOR_1}}
- {{BEHAVIOR_2}}
- {{BEHAVIOR_3}}

样式：
- {{STYLING_APPROACH}}
- {{SPECIFIC_STYLES}}

状态管理（如果需要）：
- {{STATE_DESCRIPTION}}

遵循{{REFERENCE_COMPONENT}}的{{PATTERN_DESCRIPTION}}模式。

使用现有组件：
- {{EXISTING_COMPONENT_1}}
- {{EXISTING_COMPONENT_2}}
```

**示例填写：**
```
创建UserCard组件。

文件：src/components/UserCard.tsx

Props：
{
  user: {
    id: string,
    name: string,
    email: string,
    avatar?: string,
    role: 'user' | 'admin'
  },
  onClick?: (userId: string) => void,
  showRole?: boolean
}

行为：
- 显示用户头像（如果没有提供则显示默认头像）
- 显示姓名和电子邮件
- 可选显示角色徽章
- 点击卡片 → 使用userId调用onClick
- 悬停效果（轻微缩放）

样式：
- Tailwind CSS实用类
- 卡片：白色背景，圆角，悬停时阴影
- 头像：圆形，48x48px
- 角色徽章：小徽章，管理员蓝色，用户灰色

遵循PostCard的卡片组件模式（src/components/PostCard.tsx）。

使用现有组件：
- Avatar（src/components/common/Avatar.tsx）
- Badge（src/components/common/Badge.tsx）
```

---

## 调试模板

### 模板4：运行时错误调试

```
修复{{FILE_PATH}}中的{{ERROR_TYPE}}。

错误消息：
{{ERROR_MESSAGE}}

堆栈跟踪：
{{RELEVANT_STACK_TRACE}}

上下文：
- 文件：{{FILE}}:{{LINE}}
- 操作：{{USER_ACTION}}
- 环境：{{ENVIRONMENT}}
- 开始发生：{{WHEN}}
- {{ADDITIONAL_CONTEXT}}

期望 vs 实际：
- 期望：{{EXPECTED_BEHAVIOR}}
- 实际：{{ACTUAL_BEHAVIOR}}

已尝试：
- {{ATTEMPT_1}} → {{RESULT_1}}
- {{ATTEMPT_2}} → {{RESULT_2}}

相关代码：
{{CODE_SNIPPET}}

{{HYPOTHESIS}}（如果您有假设）
```

**示例填写：**
```
修复登录流程中的空指针错误。

错误消息：
TypeError: Cannot read property 'id' of undefined

堆栈跟踪：
  at handleLogin (src/api/auth.ts:45:31)
  at validateAndLogin (src/api/auth.ts:23:12)

上下文：
- 文件：src/api/auth.ts:45
- 操作：用户提交了登录表单
- 环境：开发
- 开始发生：昨天添加电子邮件验证后
- 仅影响未验证用户

期望 vs 实际：
- 期望：为未验证用户返回401
- 实际：500错误崩溃

已尝试：
- 添加console.log → 用户为undefined
- 检查User.findByEmail → 为未验证用户返回null

相关代码：
```typescript
async function handleLogin(req, res) {
  const { email, password } = req.body;
  const user = await User.findByEmail(email);
  const isValid = await bcrypt.compare(password, user.password);
  if (!isValid) {
    return res.status(401).json({ error: 'Invalid credentials' });
  }
  const token = jwt.sign({ id: user.id }, SECRET); // 第45行错误
  return res.json({ token });
}
```

假设：User.findByEmail为未验证用户返回null，但我们没有检查就访问user.id。
```

---

### 模板5：逻辑错误调试

```
修复{{FUNCTION/COMPONENT}}中不正确的{{BEHAVIOR}}。

错误描述：
{{WHAT'S_WRONG}}

期望行为：
{{DETAILED_EXPECTED}}

实际行为：
{{DETAILED_ACTUAL}}

测试用例：
输入：{{INPUT}}
期望输出：{{EXPECTED_OUTPUT}}
实际输出：{{ACTUAL_OUTPUT}}

代码（{{FILE}}）：
{{CODE_SNIPPET}}

分析：
{{YOUR_ANALYSIS}}（如果您已识别问题）
```

---

## 重构模板

### 模板6：组件重构

```
将{{COMPONENT_NAME}}从{{OLD_APPROACH}}重构为{{NEW_APPROACH}}。

文件：{{FILE_PATH}}

当前问题：
- {{PROBLEM_1}}
- {{PROBLEM_2}}

期望状态：
- {{GOAL_1}}
- {{GOAL_2}}

必须保持：
- {{PRESERVATION_1}}（关键 - 原因：{{REASON}}）
- {{PRESERVATION_2}}

允许的更改：
- {{ALLOWED_CHANGE_1}}
- {{ALLOWED_CHANGE_2}}

当前代码：
{{CODE_SNIPPET}}

成功标准：
- 测试仍然通过
- {{SPECIFIC_REQUIREMENT_1}}
- {{SPECIFIC_REQUIREMENT_2}}
```

**示例填写：**
```
将UserProfile从类组件重构为带钩子的函数组件。

文件：src/components/UserProfile.tsx

当前问题：
- 冗长（200行）
- 生命周期方法分散
- 难以提取可重用逻辑

期望状态：
- 函数组件
- 合并的useEffect钩子
- 可重用逻辑的自定义钩子
- ~100行

必须保持：
- 所有功能（关键 - 其他组件依赖它）
- Props接口不变
- 卸载时订阅清理（关键 - 防止内存泄漏）

允许的更改：
- 转换为函数组件
- 使用钩子（useState、useEffect）
- 提取自定义钩子

当前代码：
[类组件代码]

成功标准：
- 测试无需修改通过
- 订阅清理验证
- 代码< 100行
```

---

### 模板7：架构重构

```
将{{SYSTEM}}架构从{{OLD_PATTERN}}重构为{{NEW_PATTERN}}。

第{{N}}阶段（共{{TOTAL}}阶段）：{{PHASE_NAME}}

基本原理：
{{WHY_REFACTORING}}

范围（此阶段）：
受影响文件：
- {{FILE_1}}（{{ACTION}}）
- {{FILE_2}}（{{ACTION}}）

不更改文件：
- {{UNCHANGED_FILE_1}}
- {{UNCHANGED_FILE_2}}

要求：
{{DETAILED_PHASE_REQUIREMENTS}}

向后兼容性：
{{HOW_TO_MAINTAIN}}

测试：
- {{TEST_REQUIREMENT_1}}
- {{TEST_REQUIREMENT_2}}
```

---

## 测试模板

### 模板8：单元测试生成

```
为{{FUNCTION/CLASS/COMPONENT}}编写全面测试。

要测试的代码（{{FILE}}）：
{{CODE_SNIPPET}}

测试文件：{{TEST_FILE_PATH}}
测试框架：{{FRAMEWORK}}

要覆盖的场景：

快乐路径：
- [ ] {{HAPPY_SCENARIO_1}}
- [ ] {{HAPPY_SCENARIO_2}}

错误情况：
- [ ] {{ERROR_SCENARIO_1}}
- [ ] {{ERROR_SCENARIO_2}}

边缘情况：
- [ ] {{EDGE_SCENARIO_1}}
- [ ] {{EDGE_SCENARIO_2}}

{{ADDITIONAL_CATEGORY}}：
- [ ] {{ADDITIONAL_SCENARIO_1}}

模拟：
- 模拟{{DEPENDENCY_1}}
- 模拟{{DEPENDENCY_2}}

测试结构：
```typescript
describe('{{NAME}}', () => {
  describe('{{FUNCTIONALITY}}', () => {
    it('{{TEST_CASE}}', () => {});
  });
});
```

请编写完整、可运行的测试。
```

---

### 模板9：集成测试

```
为{{FEATURE}}编写集成测试。

功能描述：
{{DESCRIPTION}}

测试文件：{{TEST_FILE_PATH}}
测试框架：{{FRAMEWORK}}

设置：
- {{SETUP_REQUIREMENT_1}}
- {{SETUP_REQUIREMENT_2}}

测试流程：
1. {{STEP_1}}
2. {{STEP_2}}
3. {{STEP_3}}

断言：
- {{ASSERTION_1}}
- {{ASSERTION_2}}

清理：
- {{CLEANUP_1}}
- {{CLEANUP_2}}
```

---

## 文档模板

### 模板10：函数/API文档

```
为{{FUNCTION/API}}生成{{DOC_TYPE}}文档。

代码：
{{CODE_SNIPPET}}

文档格式：{{FORMAT}}

包括：
- {{ELEMENT_1}}
- {{ELEMENT_2}}
- {{ELEMENT_3}}

示例：
提供{{N}}个使用示例，显示：
- {{EXAMPLE_SCENARIO_1}}
- {{EXAMPLE_SCENARIO_2}}

{{ADDITIONAL_REQUIREMENTS}}
```

**示例填写：**
```
为calculateDiscount函数生成JSDoc文档。

代码：
```typescript
function calculateDiscount(subtotal: number, discountCode?: string) {
  if (!discountCode) return 0;
  const discount = DISCOUNTS[discountCode];
  if (!discount) throw new Error('Invalid discount code');
  return subtotal * (discount.percentage / 100);
}
```

文档格式：JSDoc

包括：
- 函数描述
- 每个参数的@param文档
- @returns文档
- @throws文档（对于无效代码）
- @example带3个使用示例

示例：
提供3个使用示例，显示：
- 无折扣代码
- 有效折扣代码
- 无效折扣代码（错误情况）
```

---

### 模板11：README/指南创建

```
为{{SYSTEM/FEATURE}}创建{{DOC_TYPE}}。

文件：{{FILE_PATH}}

目标受众：{{AUDIENCE}}

包括的章节：
1. {{SECTION_1}}
2. {{SECTION_2}}
3. {{SECTION_3}}

语调：{{TONE}}（例如，技术性、初学者友好、教程风格）

包括：
- {{REQUIREMENT_1}}
- {{REQUIREMENT_2}}

格式：Markdown

{{ADDITIONAL_CONTEXT}}
```

---

## 代码审查模板

### 模板12：拉取请求审查

```
审查PR #{{NUMBER}}：{{TITLE}}

更改的文件：
- {{FILE_1}}（{{CHANGES}}）
- {{FILE_2}}（{{CHANGES}}）

审查标准：

{{CATEGORY_1}}（优先级：{{PRIORITY}}）：
- [ ] {{CRITERION_1}}
- [ ] {{CRITERION_2}}

{{CATEGORY_2}}：
- [ ] {{CRITERION_3}}
- [ ] {{CRITERION_4}}

上下文：
{{FEATURE_DESCRIPTION}}

特定关注点：
1. {{CONCERN_1}}
2. {{CONCERN_2}}

输出格式：
1. 严重问题（合并前必须修复）
2. {{CATEGORY_1}}问题
3. {{CATEGORY_2}}问题
4. 测试覆盖缺口
5. 总体评估（批准/请求更改）
```

---

### 模板13：安全审计

```
{{SYSTEM/FEATURE}}的安全审计。

范围：
- {{FILE_1}}
- {{FILE_2}}

审计：

{{SECURITY_CATEGORY_1}}：
- [ ] {{CHECK_1}}
- [ ] {{CHECK_2}}

{{SECURITY_CATEGORY_2}}：
- [ ] {{CHECK_3}}
- [ ] {{CHECK_4}}

当前实现：
{{CODE_OR_DESCRIPTION}}

请提供：
1. 严重漏洞（必须立即修复）
2. 中等优先级问题
3. 最佳实践改进
4. 需要的具体代码更改

对于每个问题：
- 漏洞是什么
- 如何可以被利用
- 如何修复
- 修复的代码示例
```

---

## 性能模板

### 模板14：性能优化

```
优化{{SYSTEM/FEATURE}}性能。

问题：
{{CURRENT_PERFORMANCE_ISSUE}}

目标：
当前：{{CURRENT_METRIC}}
目标：{{TARGET_METRIC}}

性能数据：
- {{MEASUREMENT_1}}：{{VALUE}}
- {{MEASUREMENT_2}}：{{VALUE}}

文件：
- {{FILE_1}}
- {{FILE_2}}

当前实现：
{{CODE_OR_DESCRIPTION}}

约束：
- {{CONSTRAINT_1}}
- {{CONSTRAINT_2}}

优化想法：
- {{IDEA_1}}
- {{IDEA_2}}

什么是最好的方法？请实施最有效的解决方案。
```

---

### 模板15：性能分析

```
分析{{SYSTEM/FEATURE}}的性能。

上下文：
{{DESCRIPTION}}

当前指标：
- {{METRIC_1}}：{{VALUE}}
- {{METRIC_2}}：{{VALUE}}

要分析的文件：
- {{FILE_1}}
- {{FILE_2}}

关注点：
- {{AREA_1}}
- {{AREA_2}}

请识别：
1. 性能瓶颈
2. 改进机会
3. 每个优化的预估影响
4. 推荐的实施顺序
```

---

## 安全模板

### 模板16：安全修复

```
修复{{SYSTEM/FEATURE}}中的安全漏洞。

漏洞类型：{{TYPE}}
严重性：{{SEVERITY}}

问题：
{{VULNERABILITY_DESCRIPTION}}

利用场景：
{{HOW_IT_CAN_BE_EXPLOITED}}

受影响代码（{{FILE}}）：
{{CODE_SNIPPET}}

修复要求：
- {{REQUIREMENT_1}}
- {{REQUIREMENT_2}}

测试：
- {{TEST_REQUIREMENT_1}}
- {{TEST_REQUIREMENT_2}}

请提供：
1. 修复的代码
2. 修复说明
3. 验证修复的测试用例
```

---

### 模板17：输入验证

```
为{{ENDPOINT/FUNCTION}}添加全面输入验证。

当前代码（{{FILE}}）：
{{CODE_SNIPPET}}

要验证的输入：
- {{INPUT_1}}：{{TYPE}} - {{VALIDATION_RULES}}
- {{INPUT_2}}：{{TYPE}} - {{VALIDATION_RULES}}
- {{INPUT_3}}：{{TYPE}} - {{VALIDATION_RULES}}

验证库：{{LIBRARY}}

错误处理：
- 无效输入 → 返回{{ERROR_RESPONSE}}
- 验证错误格式：{{FORMAT}}

安全考虑：
- {{CONSIDERATION_1}}
- {{CONSIDERATION_2}}
```

---

## 快速参考：模板选择

```
┌──────────────────────────┐
│  选择您的模板                          │
├──────────────────────────────────────────┤
│  新功能        → 模板1, 2, 3          │
│  错误修复      → 模板4, 5             │
│  代码改进      → 模板6, 7             │
│  添加测试      → 模板8, 9             │
│  编写文档      → 模板10, 11           │
│  审查代码      → 模板12, 13           │
│  加速代码      → 模板14, 15           │
│  安全修复      → 模板16, 17           │
└──────────────────────────────────────────┘
```

---

## 使用提示

### 1. 定制模板

**不要盲目使用模板：**
- 删除无关部分
- 添加项目特定要求
- 调整为您的代码风格
- 引用您的实际文件/模式

### 2. 保存您自己的模板

**创建项目特定模板：**

```
/docs/templates/prompts/
├── feature-api-endpoint.md
├── feature-react-component.md
├── debug-backend.md
├── refactor-component.md
└── test-integration.md
```

每个针对您的项目定制。

### 3. 模板变量

**常见占位符：**

```
{{FILE_PATH}} → src/components/UserCard.tsx
{{COMPONENT_NAME}} → UserCard
{{HTTP_METHOD}} → POST
{{ENDPOINT_PATH}} → /api/users
{{REQUIREMENT_N}} → 具体要求描述
{{CRITERION_N}} → 成功标准
{{ERROR_TYPE}} → NullPointerError、ValidationError等
{{REFERENCE_FILE}} → src/components/ExampleComponent.tsx
```

### 4. 组合模板

**从多个模板混合元素：**

示例：功能 + 测试
```
[使用模板3进行组件创建]
[然后使用模板8进行测试生成]
```

结果：组件 + 全面测试在两个提示中。

---

## 模板维护

### 何时更新模板：

- ✅ 您发现更好的提示模式
- ✅ 项目约定更改
- ✅ 采用新工具/库
- ✅ 重复提示可以模板化
- ✅ 模板产生不良结果

### 模板质量检查清单：

- [ ] 有清晰的占位符（{{VARIABLE_NAME}}）
- [ ] 包含所有必要上下文
- [ ] 引用项目模式
- [ ] 定义成功标准
- [ ] 指定约束
- [ ] 提供使用示例

---

## 下一步

1. **尝试模板** → 复制一个，填写占位符，测试它
2. **衡量结果** → 需要多少次迭代？输出质量如何？
3. **精炼模板** → 根据结果改进
4. **创建自定义模板** → 用于您的常见任务
5. **与团队分享** → 团队一致的提示

---

**相关文档：**
- [基础](./foundations-zh.md) → 核心提示原则
- [任务特定模式](./task-specific-patterns-zh.md) → 详细示例
- [高级技术](./advanced-techniques-zh.md) → 专家策略
- [故障排除指南](../troubleshooting/README-zh.md) → 当提示失败时

**返回：** [提示指南主页](./README-zh.md)