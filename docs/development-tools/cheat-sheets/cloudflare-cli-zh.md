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

## Cloudflare Workers

### 开发

```bash
# 初始化新的Worker
wrangler init my-worker

# 启动本地开发服务器
wrangler dev

# 使用远程资源开发（D1、KV等）
wrangler dev --remote

# 使用特定端口开发
wrangler dev --port 8787

# 使用本地模式开发（更快，但没有远程资源）
wrangler dev --local
```

### 部署

```bash
# 部署Worker
wrangler deploy

# 部署到特定环境
wrangler deploy --env production

# 使用名称部署
wrangler deploy --name my-worker

# 发布（部署的别名）
wrangler publish
```

### Worker管理

```bash
# 列出Workers
wrangler list

# 删除Worker
wrangler delete my-worker

# 查看Worker详细信息
wrangler status

# 跟踪Worker日志
wrangler tail

# 跟踪特定Worker
wrangler tail my-worker
```

## D1数据库

### 数据库管理

```bash
# 创建数据库
wrangler d1 create my-database

# 列出数据库
wrangler d1 list

# 删除数据库
wrangler d1 delete my-database

# 获取数据库信息
wrangler d1 info my-database
```

### 运行SQL命令

```bash
# 执行SQL命令
wrangler d1 execute my-database --command="SELECT * FROM users"

# 执行SQL文件
wrangler d1 execute my-database --file=schema.sql

# 使用远程数据库执行
wrangler d1 execute my-database --remote --command="SELECT * FROM users"

# 使用本地数据库执行
wrangler d1 execute my-database --local --command="SELECT * FROM users"
```

### 迁移

```bash
# 创建迁移
wrangler d1 migrations create my-database create_users_table

# 列出迁移
wrangler d1 migrations list my-database

# 应用迁移（本地）
wrangler d1 migrations apply my-database --local

# 应用迁移（远程/生产）
wrangler d1 migrations apply my-database --remote
```

### 示例：完整的D1设置

```bash
# 1. 创建数据库
wrangler d1 create my-app-db

# 2. 记录数据库ID，添加到wrangler.toml：
# [[d1_databases]]
# binding = "DB"
# database_name = "my-app-db"
# database_id = "xxxx-xxxx-xxxx-xxxx"

# 3. 创建迁移
wrangler d1 migrations create my-app-db create_users

# 4. 编辑迁移文件（migrations/0001_create_users.sql）：
# CREATE TABLE users (
#   id INTEGER PRIMARY KEY AUTOINCREMENT,
#   email TEXT UNIQUE NOT NULL,
#   name TEXT NOT NULL,
#   created_at INTEGER DEFAULT (unixepoch())
# );

# 5. 本地应用迁移（用于测试）
wrangler d1 migrations apply my-app-db --local

# 6. 本地测试查询
wrangler d1 execute my-app-db --local --command="INSERT INTO users (email, name) VALUES ('test@example.com', 'Test User')"
wrangler d1 execute my-app-db --local --command="SELECT * FROM users"

# 7. 应用迁移到生产
wrangler d1 migrations apply my-app-db --remote

# 8. 验证生产
wrangler d1 execute my-app-db --remote --command="SELECT * FROM users"
```

## KV（键值存储）

### 命名空间管理

```bash
# 创建KV命名空间
wrangler kv:namespace create MY_KV

# 创建预览命名空间（用于wrangler dev）
wrangler kv:namespace create MY_KV --preview

# 列出命名空间
wrangler kv:namespace list

# 删除命名空间
wrangler kv:namespace delete --namespace-id=xxxx
```

### 键值操作

```bash
# 放置键值
wrangler kv:key put --namespace-id=xxxx "my-key" "my-value"

# 从文件放置
wrangler kv:key put --namespace-id=xxxx "my-key" --path=./file.txt

# 获取值
wrangler kv:key get --namespace-id=xxxx "my-key"

# 列出键
wrangler kv:key list --namespace-id=xxxx

# 使用前缀列出键
wrangler kv:key list --namespace-id=xxxx --prefix="user:"

# 删除键
wrangler kv:key delete --namespace-id=xxxx "my-key"
```

### 批量操作

```bash
# 批量放置（从JSON文件）
# 格式：[{"key": "key1", "value": "value1"}, ...]
wrangler kv:bulk put --namespace-id=xxxx ./data.json

# 批量删除（从JSON文件）
# 格式：["key1", "key2", ...]
wrangler kv:bulk delete --namespace-id=xxxx ./keys.json
```

### 示例：完整的KV设置

```bash
# 1. 创建命名空间
wrangler kv:namespace create MY_CACHE

# 输出：
# 将以下内容添加到您的wrangler.toml：
# [[kv_namespaces]]
# binding = "MY_CACHE"
# id = "xxxx"

# 2. 创建预览命名空间
wrangler kv:namespace create MY_CACHE --preview

# 输出：
# 添加到wrangler.toml：
# preview_id = "yyyy"

# 3. 添加到wrangler.toml：
# [[kv_namespaces]]
# binding = "MY_CACHE"
# id = "xxxx"
# preview_id = "yyyy"

# 4. 在代码中使用：
# export default {
#   async fetch(request, env) {
#     await env.MY_CACHE.put('key', 'value');
#     const value = await env.MY_CACHE.get('key');
#     return new Response(value);
#   }
# }
```

## R2（对象存储）

### 存储桶管理

```bash
# 创建存储桶
wrangler r2 bucket create my-bucket

# 列出存储桶
wrangler r2 bucket list

# 删除存储桶
wrangler r2 bucket delete my-bucket
```

### 对象操作

```bash
# 放置对象
wrangler r2 object put my-bucket/path/to/file.jpg --file=./local-file.jpg

# 获取对象
wrangler r2 object get my-bucket/path/to/file.jpg --file=./downloaded-file.jpg

# 列出对象
wrangler r2 object list my-bucket

# 使用前缀列出
wrangler r2 object list my-bucket --prefix="uploads/"

# 删除对象
wrangler r2 object delete my-bucket/path/to/file.jpg
```

### 示例：完整的R2设置

```bash
# 1. 创建存储桶
wrangler r2 bucket create my-app-uploads

# 2. 添加到wrangler.toml：
# [[r2_buckets]]
# binding = "MY_BUCKET"
# bucket_name = "my-app-uploads"

# 3. 上传测试文件
wrangler r2 object put my-app-uploads/test.txt --file=./test.txt

# 4. 验证上传
wrangler r2 object list my-app-uploads

# 5. 在代码中使用：
# export default {
#   async fetch(request, env) {
#     const object = await env.MY_BUCKET.get('test.txt');
#     return new Response(await object.text());
#   }
# }
```

## 配置

### wrangler.toml

```toml
# 基本配置
name = "my-worker"
main = "src/index.js"
compatibility_date = "2024-01-01"

# 对于Pages Functions
pages_build_output_dir = "./dist"

# 环境变量
[vars]
ENVIRONMENT = "production"
API_URL = "https://api.example.com"

# 密钥（改用wrangler secret put）
# [secrets]
# API_KEY = "use wrangler secret put API_KEY"

# D1数据库
[[d1_databases]]
binding = "DB"
database_name = "my-database"
database_id = "xxxx-xxxx-xxxx-xxxx"

# KV命名空间
[[kv_namespaces]]
binding = "MY_KV"
id = "xxxx"
preview_id = "yyyy"

# R2存储桶
[[r2_buckets]]
binding = "MY_BUCKET"
bucket_name = "my-bucket"

# Durable Objects
[[durable_objects.bindings]]
name = "MY_DO"
class_name = "MyDurableObject"

# 特定环境的配置
[env.staging]
name = "my-worker-staging"
vars = { ENVIRONMENT = "staging" }

[env.production]
name = "my-worker-production"
vars = { ENVIRONMENT = "production" }
```

## 密钥管理

```bash
# 设置密钥（交互式）
wrangler secret put API_KEY

# 从文件设置密钥
cat ./secret.txt | wrangler secret put API_KEY

# 列出密钥（仅名称，不包括值）
wrangler secret list

# 删除密钥
wrangler secret delete API_KEY

# 为特定环境设置密钥
wrangler secret put API_KEY --env production
```

## 环境变量

```bash
# 在wrangler.toml中设置：
# [vars]
# MY_VAR = "value"

# 或通过命令行
wrangler dev --var MY_VAR:value

# 在代码中访问：
# env.MY_VAR
```

## 跟踪日志（实时日志记录）

```bash
# 跟踪当前Worker的日志
wrangler tail

# 跟踪特定Worker
wrangler tail my-worker

# 使用过滤器跟踪
wrangler tail --status error
wrangler tail --method POST
wrangler tail --search "user-login"

# 使用格式跟踪
wrangler tail --format json
wrangler tail --format pretty

# 跟踪Pages Functions
wrangler pages deployment tail
```

## 常见工作流程

### 在Cloudflare Pages上创建新的Astro站点

```bash
# 1. 创建Astro项目
npm create astro@latest my-site
cd my-site

# 2. 安装wrangler（可选，用于本地开发）
npm install -D wrangler

# 3. 构建
npm run build

# 4. 本地测试
npx wrangler pages dev ./dist

# 5. 部署
npx wrangler pages deploy ./dist --project-name=my-site

# 6. 连接到Git（通过Cloudflare仪表板）
# 转到仪表板，连接GitHub仓库，推送时自动部署
```

### 向现有项目添加D1数据库

```bash
# 1. 创建数据库
wrangler d1 create my-app-db

# 2. 将绑定添加到wrangler.toml
# [[d1_databases]]
# binding = "DB"
# database_name = "my-app-db"
# database_id = "paste-id-from-step-1"

# 3. 创建架构迁移
wrangler d1 migrations create my-app-db init

# 4. 编辑迁移文件，添加架构
# migrations/0001_init.sql

# 5. 本地应用以进行测试
wrangler d1 migrations apply my-app-db --local

# 6. 本地测试
wrangler pages dev ./dist --d1 DB=my-app-db

# 7. 应用到生产
wrangler d1 migrations apply my-app-db --remote

# 8. 部署
npm run build && wrangler pages deploy ./dist
```

### 添加KV存储

```bash
# 1. 创建命名空间
wrangler kv:namespace create MY_CACHE

# 2. 创建预览命名空间
wrangler kv:namespace create MY_CACHE --preview

# 3. 添加到wrangler.toml（使用上面的ID）
# [[kv_namespaces]]
# binding = "MY_CACHE"
# id = "production-id"
# preview_id = "preview-id"

# 4. 本地测试
wrangler pages dev ./dist

# 5. 部署
npm run build && wrangler pages deploy ./dist
```

### 添加R2存储

```bash
# 1. 创建存储桶
wrangler r2 bucket create my-uploads

# 2. 添加到wrangler.toml
# [[r2_buckets]]
# binding = "UPLOADS"
# bucket_name = "my-uploads"

# 3. 本地测试
wrangler pages dev ./dist

# 4. 部署
npm run build && wrangler pages deploy ./dist
```

## 故障排除

### 常见问题

#### "Error: No account_id found"
```bash
# 首先登录
wrangler login

# 或在wrangler.toml中设置账户ID
account_id = "your-account-id"

# 查找账户ID
wrangler whoami
```

#### "Error: Not authorized"
```bash
# 重新认证
wrangler logout
wrangler login

# 或检查API令牌权限
# 令牌需要Workers、Pages、D1、KV、R2权限（根据需要）
```

#### "Error: 'wrangler' is not recognized"
```bash
# 全局安装
npm install -g wrangler

# 或使用npx
npx wrangler [command]

# 检查PATH
echo $PATH

# 如需要，重新安装Node.js
```

#### 本地开发与D1/KV不工作
```bash
# 使用--remote标志使用生产资源（小心！）
wrangler pages dev ./dist --remote

# 或使用--local进行本地D1
wrangler pages dev ./dist --local

# 对于D1，首先本地应用迁移
wrangler d1 migrations apply my-db --local
```

#### 部署失败，显示"Invalid binding"
```bash
# 检查wrangler.toml语法
# 确保ID正确
# 确保命名空间/存储桶存在

# 列出资源
wrangler d1 list
wrangler kv:namespace list
wrangler r2 bucket list

# 如需要，重新创建
```

### 调试模式

```bash
# 启用调试输出
wrangler dev --debug

# 详细输出
wrangler deploy --verbose

# 显示帮助
wrangler --help
wrangler pages --help
wrangler d1 --help
```

## 有用的别名

添加到您的shell配置（`.bashrc`、`.zshrc`）：

```bash
# Wrangler快捷方式
alias wd='wrangler dev'
alias wdp='wrangler pages dev ./dist'
alias wb='npm run build'
alias wbd='npm run build && wrangler pages dev ./dist'
alias wdeploy='npm run build && wrangler pages deploy ./dist'

# D1快捷方式
alias d1list='wrangler d1 list'
alias d1exec='wrangler d1 execute'
alias d1migrate='wrangler d1 migrations apply'

# KV快捷方式
alias kvlist='wrangler kv:namespace list'
alias kvget='wrangler kv:key get'
alias kvput='wrangler kv:key put'

# R2快捷方式
alias r2list='wrangler r2 bucket list'
alias r2objects='wrangler r2 object list'
```

## 快速参考

### 最常用的命令

```bash
# 开发
wrangler pages dev ./dist              # 本地开发服务器
wrangler dev                            # Worker开发服务器
wrangler dev --remote                   # 使用远程资源开发

# 部署
wrangler pages deploy ./dist            # 部署Pages
wrangler deploy                         # 部署Worker

# D1数据库
wrangler d1 create my-db                # 创建数据库
wrangler d1 migrations create my-db name  # 创建迁移
wrangler d1 migrations apply my-db --local  # 本地应用
wrangler d1 migrations apply my-db --remote # 应用到生产
wrangler d1 execute my-db --command="..."   # 运行SQL

# KV存储
wrangler kv:namespace create MY_KV      # 创建命名空间
wrangler kv:key put --namespace-id=xxx "key" "value"  # 放置
wrangler kv:key get --namespace-id=xxx "key"          # 获取
wrangler kv:key list --namespace-id=xxx               # 列出

# R2存储
wrangler r2 bucket create my-bucket     # 创建存储桶
wrangler r2 object put bucket/key --file=file  # 上传
wrangler r2 object get bucket/key --file=file  # 下载
wrangler r2 object list bucket          # 列出

# 密钥
wrangler secret put SECRET_NAME         # 设置密钥
wrangler secret list                    # 列出密钥

# 日志
wrangler tail                           # 跟踪日志
wrangler pages deployment tail          # 跟踪Pages日志
```

---

**专业提示：**
- 使用`wrangler dev --remote`测试生产数据（小心！）
- 在应用到生产之前始终本地测试D1迁移
- 在wrangler.toml中使用特定环境的配置
- 将密钥保存在Wrangler密钥中，而不是代码中
- 使用`wrangler tail`进行实时调试