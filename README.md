## 说明
- 此仓库存放特定脚本, 用于解决 [Element for Android](https://github.com/vector-im/element-android):
  - 在后台期间无法自动挂断来电;
  - 在后台期间无法自动处理未接来电;
  - 在后台期间通话结束被叫方挂断后, 主叫卡在 "正在建立连接" 一段时间后提示 "无法建立实时连接";
  - 在前台期间主叫卡在 "远端未能接听" 无法自动返回;
  - 因上述现象导致屏幕常亮及长时间响铃而耗尽电池.

## 权限与电池
- 根据脚本的用途, 用户需要为 MacroDroid 赋予下列权限:
  - 无障碍 - MacroDroid 界面交互 (涉及到自动点击, 必需)
  - android.permission.DUMP (执行特定 Shell 命令时必需)
  - android.permission.PACKAGE_USAGE_STATS (执行特定 Shell 命令时必需)
  - android:get_usage_stats (AppOps 权限, 执行特定 Shell 命令时必需)
  - android.permission.READ_LOGS (当启用基于系统日志的触发器为必需)
  - 允许叠加层 (当在来电界面上显示调试信息时为必需)
- 脚本作用域: 
  - 包名: `im.vector.app`
- 电池相关:
  - 允许 MacroDroid 在后台运行
  - 对 MacroDroid 禁用电池优化
  - 允许 MacroDroid 开机自启动及自动唤醒

## 脚本触发
- 当检测到屏幕显示来电画面时脚本自动触发 (因此弹出通知不会触发脚本)
- 脚本内置 2 种触发器:
   - 基于屏幕内容: 默认启用, 由于 MacroDroid 对屏幕内容读取频率的限制 (默认为 2 秒), 有时触发比较慢
   - 基于系统日志 (logcat): 默认禁用, 触发较快, 但比较耗电

## 脚本效果
- 适用于语音通话与视频通话
- Element 分别在不同的场合下的处理策略:  
  | 应用状态 | 屏幕状态 | 效果 |
  | ----- | ----- | ----- |
  | 位于前台 | 从其它应用自动切换到来电画面 | 来电画面显示至少 1 分钟 (或稍长于 1 分钟) 后, 脚本自动点击屏幕上的特定元素 ("确认" 按钮) 而挂断来电并返回到上一个画面 |
  | 位于前台 | 从锁屏自动切换到来电画面 | 来电画面显示至少 1 分钟 (或稍长于 1 分钟) 后, 脚本自动点击屏幕上的特定元素 ("确认" 按钮) 而挂断来电并返回到锁屏画面 |
  | 位于后台 | 从锁屏自动切换到来电画面 | 来电画面显示至少 1 分钟 (或稍长于 1 分钟) 后, 脚本自动点击屏幕上的特定元素 ("挂断" 按钮) 而挂断来电并返回到锁屏画面 |
- 脚本运行期间会生成一个半透明的浮动层用于显示实时调试信息
- 超时前手动接听
- 通话计时开始时振动
- 超时或切换到其它应用则自动终止脚本, 浮动层消失

## 通过 ADB 授予部分特殊权限
手机启用开发者选项及 USB 调试后连接到电脑, 在终端或命令提示符执行以下命令:  
```
adb shell
pm grant com.arlosoft.macrodroid android.permission.DUMP
pm grant com.arlosoft.macrodroid android.permission.PACKAGE_USAGE_STATS
appops set com.arlosoft.macrodroid android:get_usage_stats allow
```
如果启用基于系统日志的触发器, 额外执行以下命令:  
```
pm grant com.arlosoft.macrodroid android.permission.READ_LOGS
```  
**Android 13 及更高版本不推荐使用, 因为该权限被 Google 限制为一次性权限, 随触发器启用并于手机下次重启后失效, 并且无法通过 ADB 完成授权, 必须随启用触发器后在屏幕上手动授权. 如果你的手机安装了 LSPosed, 可以额外安装 [DisableLogRequest](https://modules.lsposed.org/module/com.queallytech.disablelogrequest/) 尝试解决此问题.**

## 使用方法
- 请先安装 MacroDroid, 版本至少 5.28;
- 安装完成后为 MacroDroid 赋予所需的权限, 并禁用电池优化.

1. 下载 `Element_未接来电超时自动返回_#1.macro` 及 `Element_未接来电超时自动返回_#2.macro` 到手机的内置存储;
2. 启动 MacroDroid, 导入下载的脚本;
   - 导入后不要立即后退, 在脚本名称处随意键入一个字符并删除, 然后再后退, 点击 "保存"
   - 共 2 个脚本, 每个脚本都需要执行一次上述步骤
4. 回到主页, 点击 "变量", 检查是否存在名为 "element_incoming_call_is_triggered" 的变量, 如果没有, 则手动新建一个, 类型为 "布尔";
   - 如果有则忽略本步骤
5. 锁定手机, 然后使用 Element 呼叫该手机以测试脚本, 如果来电后被迅速挂断, 则需要授予特殊权限

## 注意事项
`Element_未接来电超时自动返回_#1.macro`
1. **脚本具有读取屏幕内容的能力, 目前只能匹配简体中文**;
2. 脚本使用了以下 Shell 命令用于确定当前 Activity, **需要授予特殊权限**:  
   `dumpsys activity top | grep ACTIVITY | tail -1 | cut -d " " -f 4`
3. 在某些情况下, 脚本可能无法及时终止:
   当 "来电 -> 拒接" 频繁反复出现时, 脚本计时器可能并不会重新开始而是接续上一次循环
4. Element for Android 的中文翻译并不规范, 未来可能会随版本更新而更换关键词导致脚本失效.

`Element_未接来电超时自动返回_#2.macro`
1. **脚本具有读取屏幕内容的能力, 目前只能匹配简体中文**;
2. Element for Android 的中文翻译并不规范, 未来可能会随版本更新而更换关键词导致脚本失效.

## 隐私
虽然此脚本具有读取屏幕内容的能力, 但并不会将屏幕上的内容发送给作者. 脚本内容已公开.
