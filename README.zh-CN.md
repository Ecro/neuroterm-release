[English](README.md) | 简体中文 | [日本語](README.ja.md)

# NeuroTerm

**让编码智能体（Claude Code、Codex）也能使用的串口和 SSH 终端。**

NeuroTerm 是一款面向嵌入式开发者的免费串口和 SSH 终端，支持 Windows 和 Linux。它可以打开串口（UART）、带 SFTP 侧栏的 SSH、WSL、本地 Shell 以及 TCP / RTT 标签页，最多同时 20 个会话。通过内置的 MCP 服务器 **Terminal Use**，Claude Code 和 Codex 可以读取你的串口和 SSH 控制台，并且只在你允许时才输入。

[官网](https://neuroterm.dev) · [下载](https://neuroterm.dev/download) · [文档](https://neuroterm.dev/docs) · [演示视频](https://www.youtube.com/watch?v=9ddM6887Hpw) · [反馈问题](https://github.com/Ecro/neuroterm-issues/issues/new/choose)

![NeuroTerm：Claude Code 与串口标签页并排](https://neuroterm.dev/media/neuroterm-preview.png)

> 应用界面目前只有英文。下文中的菜单和按钮名称保留英文原文，与应用中显示的一致。

## 为什么需要它

编码智能体自己打开 `/dev/ttyUSB0` 时很容易卡住：串口不会自己关闭，读取它的工具调用就一直不返回（见 [anthropics/claude-code#6237](https://github.com/anthropics/claude-code/issues/6237)）。在 NeuroTerm 中，串口一直在一个你能看到的标签页里保持打开。智能体在那里读取启动日志；在它执行命令、输入或复位开发板之前，标签页会先征求你的同意。详见 [Claude Code and Codex on your serial port](https://neuroterm.dev/claude-code-serial-port)（英文）。

## Terminal Use（MCP 服务器）

NeuroTerm 会替你把名为 `neuroterm` 的 MCP 服务器注册到 Claude Code 和 Codex。

1. 打开 **Settings → Coding agents → Setup**，在智能体所在主机那一行选择 **Set up…**，再点击 Terminal Use 旁边的 **Set up…**。
2. 在 NeuroTerm 的 **Local** 或 **WSL** 标签页中启动 `claude` 或 `codex`。在 Windows 上，请从 WSL 标签页启动 Codex。
3. 试着提问，例如："Read the boot log in my serial tab"（读取我串口标签页里的启动日志）。

之后，智能体就可以在你的串口、SSH、WSL、Local 和 TCP 标签页中工作。读取不需要批准。执行命令、输入或复位开发板时，会先在该标签页中显示请求（Allow 或 Deny，45 秒），除非你已在本次会话中允许该智能体，或打开了 Allow automatically。

| 工具 | 作用 |
|---|---|
| `tabs` | 列出标签页和已保存的配置；打开、拆分、聚焦或关闭标签页（打开、拆分和关闭需要你的 Allow）。 |
| `terminal_run` | 在标签页中执行命令，返回每条命令的输出和退出码（需要你的 Allow）。 |
| `terminal_observe` | 以纯文本读取标签页的新输出，附带 NeuroTerm 标记的错误、警告和启动阶段。 |
| `terminal_act` | 为 REPL、U-Boot、登录提示和全屏程序输入文本和按键（需要你的 Allow）。 |
| `serial_port` | 列出串口及其 USB 身份；为烧录借用串口标签页的端口，用完再还回（需要你的 Allow）。 |
| `export_serial_log` | 把串口标签页的输出（最多 1 MiB）写入智能体可以搜索的文件。 |
| `reset_serial_device` | 通过 RTS、DTR 或 break 复位开发板（需要你的 Allow）。 |
| `plugin_*` | 构建、检查、运行和截图 NeuroTerm 插件：`plugin_tools`、`plugin_guide`、`plugin_scaffold`、`plugin_validate`、`plugin_apply`、`plugin_remove`、`plugin_logs`、`plugin_fork`、`plugin_capture`。 |

完整参考：[Terminal Use (MCP) 文档](https://neuroterm.dev/docs/agent-serial-access)（英文）。

## 其他功能

- **串口自动重连**：USB 转串口适配器重新插入后换了端口名（COM7 变成 COM9，`/dev/ttyUSB0` 变成 `/dev/ttyUSB1`），同一个标签页会自动重连。
- **错误标记**：日志滚动时自动标出启动阶段和崩溃转储。
- 智能体在等你时发出 **Agent alerts**（提醒）；重启后用 **Agent resume** 继续之前的对话。
- 分屏、工作区、SFTP 侧栏、插件，以及可选的本地 AI（Neuro），它根据你自己的文档回答问题。

## 下载

Windows 10/11（`.exe`、`.msi`）和 Linux（`.deb`、`.rpm`、`.AppImage`）：见 [最新版本](https://github.com/Ecro/neuroterm-release/releases/latest) 或 [neuroterm.dev/download](https://neuroterm.dev/download)。暂不支持 macOS。

以下链接始终指向最新版本：

```sh
# Ubuntu / Debian
curl -L -o neuroterm.deb https://neuroterm.dev/dl/deb && sudo apt install ./neuroterm.deb

# Fedora / RHEL
curl -L -o neuroterm.rpm https://neuroterm.dev/dl/rpm && sudo dnf install ./neuroterm.rpm
```

```powershell
# Windows (PowerShell)
Invoke-WebRequest https://neuroterm.dev/dl/exe -OutFile NeuroTerm-setup.exe; .\NeuroTerm-setup.exe
```

另有 `https://neuroterm.dev/dl/msi` 和 `https://neuroterm.dev/dl/appimage`。

## 许可

NeuroTerm 是专有软件，个人在家或在工作中都可以免费使用，没有使用限制。员工超过 10 人的公司如果向员工统一部署，需要购买 Commercial 席位。详见 [价格](https://neuroterm.dev/pricing) 和 [条款](https://neuroterm.dev/terms)（英文）。本仓库只存放发布的安装包，不包含源代码。请在 [Ecro/neuroterm-issues](https://github.com/Ecro/neuroterm-issues) 反馈问题和提出功能需求。
