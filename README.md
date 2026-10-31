# NeuroTerm

**The serial and SSH terminal your coding agent can use.**

NeuroTerm is a free serial and SSH terminal for embedded developers on Windows and Linux. It opens serial (UART), SSH with an SFTP sidebar, WSL, local shell and TCP / RTT tabs, up to 20 sessions at once. Through **Terminal Use**, its built-in MCP server, Claude Code and Codex can read your serial and SSH consoles and type only when you allow.

[Website](https://neuroterm.dev) · [Download](https://neuroterm.dev/download) · [Docs](https://neuroterm.dev/docs) · [Demo video](https://www.youtube.com/watch?v=9ddM6887Hpw) · [Report an issue](https://github.com/Ecro/neuroterm-issues/issues/new/choose)

![NeuroTerm: Claude Code beside a serial tab](https://neuroterm.dev/media/neuroterm-preview.png)

## Why

A coding agent that opens `/dev/ttyUSB0` itself tends to hang: a serial port never closes, so a tool call that reads it never returns (see [anthropics/claude-code#6237](https://github.com/anthropics/claude-code/issues/6237)). In NeuroTerm the port stays open in a tab you can watch. The agent reads the boot log there, and before it runs a command, types or resets the board, the tab asks you. More: [Claude Code and Codex on your serial port](https://neuroterm.dev/claude-code-serial-port).

## Terminal Use (MCP server)

NeuroTerm registers its MCP server, named `neuroterm`, with Claude Code and Codex for you.

1. Open **Settings → Coding agents → Setup**, choose **Set up…** in the row of the host where your agent runs, then **Set up…** next to Terminal Use.
2. Start `claude` or `codex` in a NeuroTerm **Local** or **WSL** tab. On Windows, start Codex from a WSL tab.
3. Ask, for example: "Read the boot log in my serial tab".

The agent can then work in your serial, SSH, WSL, Local and TCP tabs. Reading needs no approval. Running a command, typing or resetting the board shows a request in that tab first (Allow or Deny, 45 s), unless you allowed that agent for the session or turned on Allow automatically.

| Tool | What it does |
|---|---|
| `tabs` | Lists your tabs and saved profiles; opens, splits, focuses or closes a tab (opening, splitting and closing need your Allow). |
| `terminal_run` | Runs commands in a tab and returns each one's output and exit code (with your Allow). |
| `terminal_observe` | Reads a tab's new output as plain text, with the errors, warnings and boot stages NeuroTerm marked. |
| `terminal_act` | Types text and keys for REPLs, U-Boot, login prompts and full-screen programs (with your Allow). |
| `serial_port` | Lists serial ports with their USB identity; borrows a serial tab's port for flashing, then gives it back (with your Allow). |
| `export_serial_log` | Writes a serial tab's output (up to 1 MiB) to a file the agent can search. |
| `reset_serial_device` | Resets the board through RTS, DTR or a break (with your Allow). |
| `plugin_*` | Build, check, run and capture NeuroTerm plugins: `plugin_tools`, `plugin_guide`, `plugin_scaffold`, `plugin_validate`, `plugin_apply`, `plugin_remove`, `plugin_logs`, `plugin_fork`, `plugin_capture`. |

Full reference: [Terminal Use (MCP) docs](https://neuroterm.dev/docs/agent-serial-access).

## Also in the box

- **Serial auto-reconnect**: when a USB adapter comes back under a new port name (COM7 as COM9, `/dev/ttyUSB0` as `/dev/ttyUSB1`), the same tab reconnects.
- **Error markers**: boot stages and crash dumps are marked as the log scrolls.
- **Agent alerts** when the agent is waiting for you, and **Agent resume** after a restart.
- Split panes, workspaces, an SFTP sidebar, plugins, and an optional local AI (Neuro) that answers from your own documents.

## Download

Windows 10/11 (`.exe`, `.msi`) and Linux (`.deb`, `.rpm`, `.AppImage`): see the [latest release](https://github.com/Ecro/neuroterm-release/releases/latest) or [neuroterm.dev/download](https://neuroterm.dev/download). macOS is not supported yet.

These URLs always point at the latest release:

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

Also `https://neuroterm.dev/dl/msi` and `https://neuroterm.dev/dl/appimage`.

## License

NeuroTerm is proprietary software, free for individuals at home or at work, with no usage limits. A company with more than 10 employees that rolls it out to its staff needs Commercial seats. See [pricing](https://neuroterm.dev/pricing) and [terms](https://neuroterm.dev/terms). This repository holds the release binaries; the source code is not published here. Report bugs and request features at [Ecro/neuroterm-issues](https://github.com/Ecro/neuroterm-issues).
