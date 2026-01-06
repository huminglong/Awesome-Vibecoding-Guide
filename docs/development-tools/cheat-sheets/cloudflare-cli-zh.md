# Cloudflare CLI (Wrangler) 速查表

Wrangler命令和Cloudflare开发工作流程的快速参考。

## 安装

```bash
# 全局安装
npm install -g wrangler

# 或使用npx（无需安装）
npx wrangler [command]

# 检查版本
wrangler --version

# 更新wrangler
npm update -g wrangler
```

## 身份验证

```bash
# 登录到Cloudflare
wrangler login

# 登出
wrangler logout

# 检查谁已登录
wrangler whoami

# 使用API令牌而不是OAuth
export CLOUDFLARE_API_TOKEN=your_token_here
```

## Cloudflare Pages

### 开发

```bash
# 本地开发服务器
npm run dev
# 或
wrangler pages dev ./dist

# 使用特定端口开发
wrangler pages dev ./dist --port 3000

# 使用实时重新加载开发
wrangler pages dev ./dist --live-reload

# 使用特定绑定开发
wrangler pages dev ./dist --binding KEY=value
```

### 部署

```bash
# 部署到Cloudflare Pages
npm run build && wrangler pages deploy ./dist

# 部署到特定项目
wrangler pages deploy ./dist --project-name=my-site

# 部署到特定分支
wrangler pages deploy ./dist --branch=staging

# 使用提交消息部署
wrangler pages deploy ./dist --commit-message="Deploy v1.2.0"
```

### 项目管理

```bash
# 创建新的Pages项目
wrangler pages project create my-site

# 列出所有项目
wrangler pages project list

# 查看项目详细信息
wrangler pages project get my-site

# 删除项目（小心！）
wrangler pages project delete my-site
```

### 部署管理

```bash
# 列出部署
wrangler pages deployment list --project-name=my-site

# 查看部署详细信息
wrangler pages deployment get <deployment-id>

# 跟踪部署日志
wrangler pages deployment tail
```