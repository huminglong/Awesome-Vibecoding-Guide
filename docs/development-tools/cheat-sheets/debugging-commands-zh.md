# 调试命令速查表

在浏览器、终端和开发工具中调试Web应用程序的快速参考。

## Chrome DevTools快捷键

### 打开DevTools
```
Windows/Linux          Mac
F12                    Cmd + Opt + I
Ctrl + Shift + I       Cmd + Opt + I
Ctrl + Shift + C       Cmd + Opt + C (检查元素)
Ctrl + Shift + J       Cmd + Opt + J (控制台)
```

### 导航
```
Windows/Linux          Mac                     操作
Ctrl + [               Cmd + [                 上一个面板
Ctrl + ]               Cmd + ]                 下一个面板
Ctrl + Shift + P       Cmd + Shift + P         命令面板
Ctrl + Shift + D       Cmd + Shift + D         切换设备工具栏
Ctrl + Shift + M       Cmd + Shift + M         切换移动视图
Esc                    Esc                     切换抽屉
Ctrl + L               Cmd + K                 清除控制台
```

### 元素面板
```
Windows/Linux          Mac                     操作
Arrow keys             Arrow keys              导航DOM
Enter                  Enter                   编辑属性
H                      H                       隐藏元素
Delete                 Delete                  删除元素
Ctrl + Z               Cmd + Z                 撤销更改
F2                     F2                      编辑为HTML
Ctrl + F               Cmd + F                 搜索DOM
```

### 控制台
```
Windows/Linux          Mac                     操作
Ctrl + L               Cmd + K                 清除控制台
↑/↓                    ↑/↓                     上一个/下一个命令
Ctrl + U               Cmd + U                 清除当前命令
Tab                    Tab                     自动完成
Shift + Enter          Shift + Enter           多行输入
```

### 源代码/调试器
```
Windows/Linux          Mac                     操作
Ctrl + O               Cmd + O                 打开文件
Ctrl + Shift + O       Cmd + Shift + O         转到符号
Ctrl + G               Cmd + L                 转到行
Ctrl + P               Cmd + P                 转到文件
F8                     F8                      恢复/暂停
F9                     F9                      切换断点
F10                    F10                     跳过
F11                    F11                     进入
Shift + F11            Shift + F11             跳出
Ctrl + Shift + E       Cmd + Shift + E         运行代码片段
```

### 网络面板
```
Windows/Linux          Mac                     操作
Ctrl + E               Cmd + E                 开始/停止记录
Ctrl + F               Cmd + F                 搜索请求
Ctrl + R               Cmd + R                 重新加载（正常）
Ctrl + Shift + R       Cmd + Shift + R         硬重新加载（清除缓存）
Hold Shift + reload    Hold Shift + reload     清空缓存并硬重新加载
```

### 性能面板
```
Windows/Linux          Mac                     操作
Ctrl + E               Cmd + E                 开始/停止记录
Ctrl + Shift + E       Cmd + Shift + E         记录页面加载
```

## Chrome DevTools控制台命令

### 选择
```javascript
// 通过ID选择元素
$('#myId')              // 与document.getElementById('myId')相同

// 通过选择器选择元素
$('.myClass')           // 第一个匹配的元素
$$('.myClass')          // 所有匹配的元素（数组）

// 元素面板中当前选择的元素
$0                      // 当前选择
$1                      // 之前选择
$2                      // 第二个之前选择

// 示例：
$0.style.background = 'red'  // 更改所选元素背景
```

### 检查
```javascript
// 检查元素
inspect($('.myClass'))  // 为元素打开元素面板

// 获取事件监听器
getEventListeners($0)   // 所选元素上的所有监听器

// 监控事件
monitorEvents($0)       // 记录所有事件
monitorEvents($0, ['click', 'mouseenter'])  // 记录特定事件
unmonitorEvents($0)     // 停止监控

// 复制到剪贴板
copy($0)                // 复制所选元素
copy(object)            // 将任何对象复制为JSON
```

### 性能
```javascript
// 测量执行时间
console.time('myOperation')
// ... 要测量的代码
console.timeEnd('myOperation')

// 分析CPU
console.profile('myProfile')
// ... 要分析的代码
console.profileEnd('myProfile')

// 内存使用
console.memory.usedJSHeapSize

// 性能标记
performance.mark('start')
// ... 代码
performance.mark('end')
performance.measure('myMeasure', 'start', 'end')
console.log(performance.getEntriesByType('measure'))
```

### 控制台输出
```javascript
// 不同的日志级别
console.log('信息')
console.info('信息')
console.warn('警告')
console.error('错误')
console.debug('调试')

// 样式化输出
console.log('%c样式化文本', 'color: red; font-size: 20px')
console.log('%c红色 %c蓝色', 'color: red', 'color: blue')

// 表格
console.table([{a: 1, b: 2}, {a: 3, b: 4}])

// 分组
console.group('组')
console.log('项目1')
console.log('项目2')
console.groupEnd()

// 可折叠分组
console.groupCollapsed('隐藏组')
console.log('默认隐藏')
console.groupEnd()

// 计数
console.count('myCounter')  // 每次递增
console.countReset('myCounter')  // 重置计数器

// 断言
console.assert(1 === 2, '这将显示错误')
console.assert(1 === 1, '这将不会显示')

// 堆栈跟踪
console.trace('堆栈跟踪在这里')

// 清除
console.clear()
```

### 调试
```javascript
// 调试器语句
debugger;  // 暂停执行

// 在属性访问时中断
Object.defineProperty(window, 'myVar', {
  get() { debugger; return this._myVar; },
  set(value) { debugger; this._myVar = value; }
})

// 在函数调用时中断
debug(functionName)     // 在函数被调用时中断
undebug(functionName)   // 移除调试

// 监控函数调用
monitor(functionName)   // 在函数被调用时记录
unmonitor(functionName) // 停止监控
```

## 终端调试

### Node.js调试

```bash
# 使用检查器运行
node --inspect app.js

# 运行并在第一行中断
node --inspect-brk app.js

# 在特定端口上使用调试器运行
node --inspect=9229 app.js

# 使用chrome://inspect调试
# 1. 运行：node --inspect app.js
# 2. 打开Chrome：chrome://inspect
# 3. 点击"Open dedicated DevTools for Node"
```

### NPM/Node调试

```bash
# 显示npm调试日志
npm run dev --verbose

# 显示更多调试信息
npm run dev --loglevel=verbose

# 调试npm脚本
DEBUG=* npm run dev

# 检查Node.js版本
node --version

# 检查npm版本
npm --version

# 清除npm缓存
npm cache clean --force

# 重新安装node_modules
rm -rf node_modules package-lock.json
npm install
```

### Astro调试

```bash
# 开发模式（显示错误）
npm run dev

# 使用调试输出构建
npm run build -- --verbose

# 本地预览构建
npm run preview

# 检查Astro错误
npx astro check

# Astro信息（版本、配置）
npx astro info

# 清除Astro缓存
rm -rf .astro node_modules/.astro
```

## 网络检查

### Chrome DevTools网络标签

**过滤请求：**
```
工具栏 → 过滤框

示例：
domain:example.com          # 特定域名的请求
status-code:404             # 按状态代码
method:POST                 # 按HTTP方法
mime-type:image/png         # 按MIME类型
larger-than:1k              # 按大小
-method:OPTIONS             # 排除OPTIONS请求
```

**请求详细信息：**
- **Headers标签** - 请求/响应头
- **Preview标签** - 格式化的响应
- **Response标签** - 原始响应
- **Timing标签** - 请求时序细分
- **Cookies标签** - 发送/接收的cookies

**复制为cURL：**
```
右键单击请求 → 复制 → 复制为cURL

# 在终端中粘贴以重放请求
curl 'https://api.example.com/data' \
  -H 'Authorization: Bearer token' \
  -H 'Content-Type: application/json'
```

**复制为Fetch：**
```
右键单击请求 → 复制 → 复制为fetch

# 在控制台中粘贴以作为JavaScript重放
fetch('https://api.example.com/data', {
  headers: { 'Authorization': 'Bearer token' }
})
```

**限制网络：**
```
工具栏 → No throttling下拉菜单
选项：Fast 3G、Slow 3G、Offline、Custom
```

**阻止请求：**
```
右键单击请求 → Block request URL
右键单击请求 → Block request domain
```

### cURL调试

```bash
# 简单GET请求
curl https://api.example.com/data

# 带头的GET
curl -H "Authorization: Bearer token" \
     https://api.example.com/data

# 带JSON主体的POST
curl -X POST https://api.example.com/data \
     -H "Content-Type: application/json" \
     -d '{"key": "value"}'

# 带表单数据的POST
curl -X POST https://api.example.com/data \
     -d "name=John" \
     -d "email=john@example.com"

# 显示响应头
curl -i https://api.example.com/data

# 仅显示头
curl -I https://api.example.com/data

# 跟随重定向
curl -L https://api.example.com/data

# 详细（显示完整请求/响应）
curl -v https://api.example.com/data

# 将响应保存到文件
curl https://api.example.com/data -o output.json

# 显示时序信息
curl -w "@curl-timing.txt" https://api.example.com/data

# curl-timing.txt内容：
# time_total: %{time_total}\n

# 上传文件
curl -X POST https://api.example.com/upload \
     -F "file=@/path/to/file.jpg"

# 使用不同的用户代理测试
curl -A "Mozilla/5.0" https://api.example.com/data
```

### HTTPie（比cURL更好）

```bash
# 安装
npm install -g httpie

# GET请求
http GET https://api.example.com/data

# POST JSON
http POST https://api.example.com/data key=value

# 带头的POST
http POST https://api.example.com/data \
     Authorization:"Bearer token" \
     key=value

# 下载文件
http --download https://example.com/file.pdf

# 跟随重定向
http --follow https://example.com/redirect

# 详细
http --verbose GET https://api.example.com/data
```

## 性能分析

### Chrome DevTools性能标签

**记录性能：**
```
1. 打开性能标签
2. 点击记录按钮（●）
3. 与页面交互
4. 点击停止按钮（■）
5. 分析结果
```

**要查找的内容：**
- **红色三角形** - 长任务（> 50ms）
- **黄色部分** - JavaScript执行
- **紫色部分** - 渲染/绘制
- **绿色部分** - 绘制
- **灰色部分** - 空闲时间

**FPS仪表：**
```
1. Cmd/Ctrl + Shift + P
2. 输入"Show frames per second (FPS) meter"
3. 交互时查看实时FPS
```

**性能监视器：**
```
1. Cmd/Ctrl + Shift + P
2. 输入"Show Performance monitor"
3. 查看实时指标：
   - CPU使用率
   - JS堆大小
   - DOM节点
   - JS事件监听器
   - 每秒布局数
```

### Lighthouse

```bash
# 从DevTools运行Lighthouse
1. 打开DevTools
2. 点击"Lighthouse"标签
3. 选择类别
4. 点击"Analyze page load"

# 从命令行运行
npm install -g lighthouse
lighthouse https://example.com
lighthouse https://example.com --view  # 打开报告

# 在CI中运行
npm install -g @lhci/cli
lhci autorun
```

### Chrome DevTools覆盖率

**查找未使用的CSS/JS：**
```
1. Cmd/Ctrl + Shift + P
2. 输入"Show Coverage"
3. 点击重新加载按钮
4. 查看未使用的代码（红色条）
5. 点击文件查看未使用的行
```

**解释结果：**
- 红色条 = 未使用的代码
- 绿色条 = 已使用的代码
- 目标：80%+绿色

## 内存调试

### Chrome DevTools内存标签

**获取堆快照：**
```
1. 打开内存标签
2. 选择"Heap snapshot"
3. 点击"Take snapshot"
4. 分析内存中的对象
```

**比较快照：**
```
1. 获取快照1
2. 与页面交互
3. 获取快照2
4. 比较以查找泄漏
```

**分配时间线：**
```
1. 打开内存标签
2. 选择"Allocation instrumentation on timeline"
3. 点击记录
4. 与页面交互
5. 停止记录
6. 查看随时间的分配
```

**要查找的内容：**
- **分离的DOM节点** - 内存泄漏
- **增长的堆大小** - 可能的泄漏
- **事件监听器** - 可能阻止GC

## 移动调试

### Chrome远程调试（Android）

```
1. 在Android上启用USB调试
2. 通过USB连接手机
3. 在Chrome中打开chrome://inspect
4. 在您的设备上点击"inspect"
5. 正常调试
```

### iOS Safari调试

```
1. 在iOS上启用Web Inspector：
   设置 → Safari → 高级 → Web Inspector

2. 通过USB连接iPhone

3. 在Mac上打开Safari：
   开发 → [您的iPhone] → [页面]

4. 正常调试
```

### 响应式模式

```
Chrome DevTools:
Cmd/Ctrl + Shift + M    # 切换设备工具栏

选项：
- 选择设备预设
- 设置自定义尺寸
- 旋转设备
- 限制网络
- 显示媒体查询
- 显示标尺
```

## 布局调试

### 可视化布局问题

```javascript
// 显示所有元素轮廓
[].forEach.call(
  document.querySelectorAll('*'),
  el => el.style.outline = '1px solid red'
)

// 显示具有特定属性的元素
[].forEach.call(
  document.querySelectorAll('*'),
  el => {
    if (getComputedStyle(el).position === 'absolute') {
      el.style.outline = '2px solid blue'
    }
  }
)
```

### Chrome布局偏移区域

```
1. 打开DevTools
2. Cmd/Ctrl + Shift + P
3. 输入"Show Layout Shift Regions"
4. 重新加载页面
5. 查看发生偏移的蓝色矩形
```

### Chrome图层边框

```
1. 打开DevTools
2. Cmd/Ctrl + Shift + P
3. 输入"Show layer borders"
4. 查看正在合成的内容
```

### 强制元素状态

```
Chrome DevTools元素面板：
1. 选择元素
2. 右键单击
3. 强制状态 → :hover/:active/:focus/:visited
```

## 调试提示

### 在DOM更改时中断

```
Chrome DevTools元素面板：
1. 右键单击元素
2. Break on...
   - 子树修改
   - 属性修改
   - 节点移除
```

### XHR/Fetch断点

```
Chrome DevTools源代码面板：
1. 展开"XHR/fetch Breakpoints"
2. 点击+添加URL模式
3. 调试器在匹配请求时中断
```

### 事件监听器断点

```
Chrome DevTools源代码面板：
1. 展开"Event Listener Breakpoints"
2. 检查要中断的事件：
   - Mouse → click
   - Keyboard → keydown
   - Form → submit
   - 等等
```

### 条件断点

```
Chrome DevTools源代码面板：
1. 右键单击行号
2. "Add conditional breakpoint"
3. 输入条件（例如，"userId === 123"）
4. 仅在条件为真时中断
```

### 日志点（非中断断点）

```
Chrome DevTools源代码面板：
1. 右键单击行号
2. "Add logpoint"
3. 输入消息（例如，"User ID: {userId}"）
4. 记录而不中断
```

## 快速调试清单

### 页面未加载
```
☐ 检查控制台是否有错误
☐ 检查网络标签是否有失败的请求
☐ 检查是否启用了JavaScript
☐ 检查广告拦截器是否干扰
☐ 硬重新加载（Ctrl+Shift+R / Cmd+Shift+R）
☐ 检查CORS错误
☐ 检查API是否正在运行
```

### 样式问题
```
☐ 检查元素
☐ 检查计算样式
☐ 检查CSS是否加载（网络标签）
☐ 检查CSS特异性（DevTools显示）
☐ 检查继承样式
☐ 检查:hover/:focus状态
☐ 检查移动/响应式视图
☐ 清除浏览器缓存
```

### JavaScript错误
```
☐ 检查控制台是否有错误消息
☐ 检查堆栈跟踪
☐ 添加console.log()语句
☐ 添加断点
☐ 检查变量是否已定义
☐ 检查函数参数
☐ 检查async/await使用
☐ 检查promise拒绝
```

### 性能问题
```
☐ 运行Lighthouse审计
☐ 检查性能标签
☐ 检查网络标签（瀑布图）
☐ 检查覆盖率标签（未使用的代码）
☐ 检查布局偏移
☐ 检查长任务（> 50ms）
☐ 检查图像大小
☐ 检查JavaScript包大小
```

### 网络问题
```
☐ 检查网络标签是否有失败的请求
☐ 检查请求/响应头
☐ 检查请求负载
☐ 检查响应状态代码
☐ 检查CORS头
☐ 检查API端点是否正确
☐ 使用cURL测试请求
☐ 检查网络限制
```

---

**专业提示：**
- 当console.log不够时使用`debugger;`语句
- 使用Chrome DevTools → 源代码 → "Pause on exceptions"
- 调查缓慢时使用性能标签记录所有内容
- 使用覆盖率标签查找未使用的CSS/JS
- 使用网络标签 → "Block request URL"测试回退
- 通过chrome://inspect（Android）或Safari开发菜单（iOS）进行移动调试