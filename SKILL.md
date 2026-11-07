---
name: phone-adb
description: 通过无线 adb 控制一台 Android 手机（Android 11+）：安装 APK、卸载/禁用应用、截图录屏、改 DPI 与动画、传文件、执行 shell 命令等。
---

# phone-adb：无线 adb 控制 Android 手机

## 适用场景
用户要求对手机执行任何 adb 层操作时启用本技能：装/卸应用、禁用系统组件、截图录屏、调整显示参数、推送/拉取文件、直接跑 shell 命令。

## 前置条件
- 目标手机：Android 11+，已开启开发者选项与「无线调试」，与控制端处于同一 WLAN
- 控制端：装有 adb（platform-tools）；首次使用需在手机「无线调试」里用「使用配对码配对设备」完成配对：`adb pair <ip:port> <6位配对码>`
- 本技能假定 adb 在 Linux 终端环境执行（如 Operit 的 super_admin:terminal）

## 设备信息（使用前填写 / 更新）
- 示例：realme RMX3700，Android 16（SDK 36），arm64-v8a
- 可查询：`adb shell getprop ro.product.model`、`getprop ro.build.version.release`、`getprop ro.product.cpu.abi`
- 配对成功后无需再次 pair，直接 connect 即可

## 执行通道
所有命令通过终端环境执行 adb，格式：
`adb -s <ip:port> <命令>`

## 连接与重连
1. 先 `adb devices` 检查连接状态；`device` 表示在线可用，`offline`/`unauthorized`/消失表示断连。
2. 手机端无线调试的 IP:端口每次重开都会变。断连时先让用户报「无线调试」主页面显示的"IP地址和端口"，然后 `adb connect <ip:port>`。
3. 若提示 unauthorized，让用户在手机上点"允许调试"。
4. 若同时存在模拟器等无关设备（如 emulator-5554），忽略它们，只操作目标手机。

## 常用命令模板
- 安装 APK：`adb -s <ip:port> install -r <apk路径>`（Linux 环境可访问 /sdcard 与 /storage）
- 卸载（用户级，免 root）：`adb -s <ip:port> shell pm uninstall --user 0 <包名>`
- 禁用（用户级，免 root）：`adb -s <ip:port> shell pm disable-user --user 0 <包名>`
- 恢复：`pm install-existing <包名>` / `pm enable <包名>`
- 查包名：`pm list packages | grep -iE '关键词'`
- 查已禁用：`pm list packages -d`
- 清数据：`pm clear <包名>`
- 截图：`adb -s <ip:port> exec-out screencap -p > <本地路径>.png`
- 录屏（限时）：`adb -s <ip:port> shell screenrecord --time-limit 30 /sdcard/screen.mp4`，随后 `adb pull`
- 推/拉文件：`adb -s <ip:port> push <本地> <手机路径>`；`adb -s <ip:port> pull <手机路径> <本地>`
- 改 DPI：`adb -s <ip:port> shell wm density 420`；还原 `wm density reset`
- 动画倍率调 0：`adb -s <ip:port> shell settings put global window_animation_scale 0`（同设 transition_animation_scale / animator_duration_scale，还原值为 1）
- 直跑命令：`adb -s <ip:port> shell "<命令>"`

## 已知案例与约束边界
1. 破坏性操作（恢复出厂、rm 删除、清存储等）必须先向用户确认再执行。
2. 部分厂商（OPPO/realme 等）系统应用受保护，无 root 时 pm uninstall / disable-user 会失败（如 com.nearme.instant.platform 报 DELETE_FAILED_INTERNAL_ERROR 或 SecurityException）。失败后如实说明，改提手动设置、重启重试或 root 方案，不要反复硬试。
3. 不要在未确认设备在线时连续执行批量命令。
4. 命令报错时先读报错信息再判断，不猜测、不掩盖。

## 期望输出
执行后向用户报告：命令结果（成功/失败）、失败原因、已做的兜底或下一步建议。
