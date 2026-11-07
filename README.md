# phone-adb

一个让 AI 助手通过**无线 adb** 控制 Android 手机的技能包（Skill），免 root。
## 灵感来源
出门在外没带电脑，想临时卸载个应用、调个 DPI？phone-adb 的
灵感来自电脑端的「搞机工具箱」——把最常用的 adb 操作搬进 AI 对话里，
应急够用，但受限于无线连接和系统权限，它不能完全替代电脑。

## 能干什么

- 安装 APK、卸载 / 禁用应用（用户级，免 root）
- 截图、限时录屏
- 修改 DPI、动画倍率
- 推送 / 拉取文件
- 直接执行 shell 命令

## 前置条件

- 目标手机：**Android 11+**，开启开发者选项和「无线调试」
- 控制端：装有 adb（Operit 平板 / 电脑 platform-tools 均可）
- 手机与控制端处于**同一 WLAN**

## 安装（Operit 平台）

1. 把整个 `phone-adb` 文件夹放进 `/sdcard/Download/Operit/skills/`
2. 在软件技能列表里确认 `phone-adb` 开关开启

> 其它支持 SKILL.md 的 AI 助手平台，按各自 Skill 目录规则放置即可。

## 使用流程

1. 手机开启无线调试，打开主页面，记下"IP地址和端口"
2. **首次使用**：点「使用配对码配对设备」，把配对码和配对端口告诉 AI，执行 `adb pair`
3. 把"IP地址和端口"告诉 AI，执行 `adb connect`，手机上允许调试
4. 之后直接用自然语言下指令，例如：
   - "帮我卸载某某应用"
   - "装这个 APK"
   - "截个屏看看"

## 注意事项

- 无线调试的端口每次重开都会变，断连后重新报一次"IP地址和端口"即可
- 部分厂商（OPPO / realme 等）系统应用受保护，无 root 删不掉，AI 会降级为禁用或如实说明
- 破坏性操作前，AI 会先向你确认

## 目录结构

```
phone-adb/
├── SKILL.md    # 技能定义（AI 的执行手册）
├── README.md
└── LICENSE
```

## 许可

[MIT License](LICENSE)
