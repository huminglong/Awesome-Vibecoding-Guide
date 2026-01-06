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