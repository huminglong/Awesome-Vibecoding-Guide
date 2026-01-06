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