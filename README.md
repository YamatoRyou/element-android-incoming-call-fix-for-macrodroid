# 中文
## 说明
- 此仓库存放特定脚本, 用于解决 [Element for Android](https://github.com/vector-im/element-android):
  - 在后台期间无法自动挂断来电;
  - 在后台期间无法自动处理未接来电;
  - 在后台期间通话结束被叫方挂断后, 主叫卡在 "正在建立连接" 一段时间后提示 "无法建立实时连接";
  - 在前台期间主叫卡在 "远端未能接听" 无法自动返回;
  - 因上述现象导致屏幕常亮而耗尽电池.

## 权限与电池
- 根据脚本的用途, 用户需要为 MacroDroid 赋予下列权限:
  - 无障碍 - MacroDroid (必需)
  - 无障碍 - MacroDroid 界面交互 (必需)
  - **Root 权限 (可能必需, 取决于你的系统)**
  - 通知访问权 (非必需)
  - android.permission.READ_LOGS (非必需)
- 脚本作用域: 
  - 包名: `im.vector.app`
- 电池相关:
  - 允许 MacroDroid 在后台运行
  - 对 MacroDroid 禁用电池优化

## 脚本效果
- 当检测到来电画面时脚本自动开始
- 位于前台 (来电超时后弹出 "远端未能接听"): 在来电画面显示至少 1 分钟 (或稍长于 1 分钟) 后, 脚本自动点击屏幕上的特定元素 ("确认" 按钮) 而挂断来电并返回到锁屏画面
- 位于后台 (来电超时后未弹出 "远端未能接听"): 在来电画面显示至少 1 分钟 (或稍长于 1 分钟) 后, 脚本自动点击屏幕上的特定元素 ("挂断" 按钮) 而挂断来电并返回到锁屏画面
- 脚本运行期间会生成一个半透明的浮动层用于显示实时调试信息
- 超时前手动接听来电或切换到其它应用则自动终止脚本

## 使用方法
- 请先安装 MacroDroid, 版本至少 5.28;
- 安装完成后为 MacroDroid 赋予所需的权限, 并禁用电池优化.

1. 下载 `Element_未接来电超时自动返回_#1.macro` 及 `Element_未接来电超时自动返回_#2.macro` 到手机的内置存储;
2. 启动 MacroDroid, 导入下载的脚本;
   - 导入后不要立即后退, 在脚本名称处随意键入一个字符并删除, 然后再后退, 点击 "保存"
   - 共 2 个脚本, 每个脚本都需要执行一次上述步骤
4. 回到主页, 点击 "变量", 检查是否存在名为 "element_incoming_call_is_triggered" 的变量, 如果没有, 则手动新建一个, 类型为 "布尔";
   - 如果有则忽略本步骤

## 注意事项
`Element_未接来电超时自动返回_#1.macro`
1. **脚本具有读取屏幕内容的能力, 目前只能匹配简体中文**;
2. 脚本使用了以下 Shell 命令用于确定当前 Activity, **但是在某些系统, 执行此命令需要 Root 权限**:  
   `dumpsys activity top | grep ACTIVITY | tail -1`
3. 在某些情况下, 脚本可能无法及时终止:
   当 "来电 -> 拒接" 频繁反复出现时, 脚本计时器可能并不会重新开始而是接续上一次循环

`Element_未接来电超时自动返回_#2.macro`
1. **脚本具有读取屏幕内容的能力, 目前只能匹配简体中文**;




# English
# The script has not been adapted for languages other than Simplified Chinese. When Element for Android is set to other languages, unexpected problems will occur when executing the script.

## Description
- This repository stores specific scripts to solve certain problems with [Element for Android](https://github.com/vector-im/element-android):
   - Unable to automatically hang up calls while in the background;
   - Unable to automatically handle missed calls while in the background;
   - During the background period, after the call ends and the called party hangs up, the caller is stuck on "Establishing Connection" and prompts "Unable to establish real-time connection" after a period of time;
   - While in the foreground, the caller is stuck in "Remote Unable to Answer" and cannot return automatically;
   - Due to the above phenomenon, the screen is always on and the battery is drained.

## Permissions and Battery
- Depending on the purpose of the script, the user needs to grant the following permissions to MacroDroid:
   - Accessibility - MacroDroid (required)
   - Accessibility - MacroDroid interface interaction (required)
   - **Root permissions (may be required, depending on your system)**
   - Notification access (optional)
   - android.permission.READ_LOGS (optional)
- Script scope:
   - Package name: `im.vector.app`
- Battery related:
   - Allow MacroDroid to run in the background
   - Disable battery optimization for MacroDroid
