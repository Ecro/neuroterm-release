[English](README.md) | [简体中文](README.zh-CN.md) | 日本語

# NeuroTerm

**コーディングエージェント（Claude Code、Codex）も使えるシリアル / SSH ターミナル。**

NeuroTerm は組み込み開発者向けの無料のシリアル / SSH ターミナルで、Windows と Linux に対応しています。シリアル（UART）、SFTP サイドバー付きの SSH、WSL、ローカルシェル、TCP / RTT のタブを開けて、最大 20 セッションを同時に扱えます。内蔵の MCP サーバー **Terminal Use** を通じて、Claude Code と Codex があなたのシリアル / SSH コンソールを読み、あなたが許可したときだけ入力します。

[Web サイト](https://neuroterm.dev) · [ダウンロード](https://neuroterm.dev/download) · [ドキュメント](https://neuroterm.dev/docs) · [デモ動画](https://www.youtube.com/watch?v=9ddM6887Hpw) · [不具合の報告](https://github.com/Ecro/neuroterm-issues/issues/new/choose)

![NeuroTerm：Claude Code とシリアルタブを並べた画面](https://neuroterm.dev/media/neuroterm-preview.png)

> アプリの画面は今のところ英語のみです。以下のメニュー名やボタン名は、アプリの表示どおり英語のままにしています。

## なぜ必要か

コーディングエージェントが自分で `/dev/ttyUSB0` を開くと、よく固まります。シリアルポートは閉じないので、それを読むツール呼び出しがいつまでも返ってこないのです（[anthropics/claude-code#6237](https://github.com/anthropics/claude-code/issues/6237) 参照）。NeuroTerm では、ポートはあなたが見ているタブの中で開いたままです。エージェントはそこでブートログを読み、コマンドの実行、入力、ボードのリセットの前には、タブがあなたに確認します。詳しくは [Claude Code and Codex on your serial port](https://neuroterm.dev/claude-code-serial-port)（英語）。

## Terminal Use（MCP サーバー）

NeuroTerm は `neuroterm` という名前の MCP サーバーを Claude Code と Codex に自動で登録します。

1. **Settings → Coding agents → Setup** を開き、エージェントが動くホストの行で **Set up…** を選び、Terminal Use の横の **Set up…** を押します。
2. NeuroTerm の **Local** または **WSL** タブで `claude` か `codex` を起動します。Windows では Codex を WSL タブから起動してください。
3. たとえば "Read the boot log in my serial tab"（シリアルタブのブートログを読んで）と頼みます。

これでエージェントは、シリアル、SSH、WSL、Local、TCP の各タブで作業できます。読み取りに承認は要りません。コマンドの実行、入力、ボードのリセットは、まずそのタブに確認が出ます（Allow か Deny、45 秒）。そのセッションでエージェントを許可済みか、Allow automatically をオンにしている場合は出ません。

| ツール | できること |
|---|---|
| `tabs` | タブと保存済みプロファイルの一覧。タブを開く、分割、フォーカス、閉じる（開く・分割・閉じるにはあなたの Allow が必要）。 |
| `terminal_run` | タブでコマンドを実行し、各コマンドの出力と終了コードを返す（Allow が必要）。 |
| `terminal_observe` | タブの新しい出力をプレーンテキストで読む。NeuroTerm が付けたエラー、警告、ブート段階のマーク付き。 |
| `terminal_act` | REPL、U-Boot、ログインプロンプト、全画面プログラム向けに文字やキーを入力（Allow が必要）。 |
| `serial_port` | シリアルポートを USB の識別情報付きで一覧。書き込みのためにシリアルタブのポートを借りて、終わったら返す（Allow が必要）。 |
| `export_serial_log` | シリアルタブの出力（最大 1 MiB）を、エージェントが検索できるファイルに書き出す。 |
| `reset_serial_device` | RTS、DTR、break でボードをリセット（Allow が必要）。 |
| `plugin_*` | NeuroTerm プラグインの作成、検査、実行、キャプチャ：`plugin_tools`、`plugin_guide`、`plugin_scaffold`、`plugin_validate`、`plugin_apply`、`plugin_remove`、`plugin_logs`、`plugin_fork`、`plugin_capture`。 |

詳しいリファレンス：[Terminal Use (MCP) ドキュメント](https://neuroterm.dev/docs/agent-serial-access)（英語）。

## そのほかの機能

- **シリアルの自動再接続**：USB シリアルアダプターを挿し直してポート名が変わっても（COM7 が COM9 に、`/dev/ttyUSB0` が `/dev/ttyUSB1` に）、同じタブが再接続します。
- **エラーマーカー**：ログが流れるそばから、ブート段階やクラッシュダンプに印を付けます。
- エージェントがあなたを待っているときの **Agent alerts**（通知）と、再起動後に会話を続ける **Agent resume**。
- 画面分割、ワークスペース、SFTP サイドバー、プラグイン、そして手元のドキュメントから答えるオプションのローカル AI（Neuro）。

## ダウンロード

Windows 10/11（`.exe`、`.msi`）と Linux（`.deb`、`.rpm`、`.AppImage`）：[最新リリース](https://github.com/Ecro/neuroterm-release/releases/latest) または [neuroterm.dev/download](https://neuroterm.dev/download) から。macOS にはまだ対応していません。

次の URL は常に最新リリースを指します。

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

ほかに `https://neuroterm.dev/dl/msi` と `https://neuroterm.dev/dl/appimage` もあります。

## ライセンス

NeuroTerm はプロプライエタリソフトウェアで、個人なら自宅でも職場でも無料、利用制限もありません。従業員 10 人を超える企業が社員向けに一括導入する場合は Commercial シートが必要です。[料金](https://neuroterm.dev/pricing) と [利用規約](https://neuroterm.dev/terms) をご覧ください（英語）。このリポジトリはリリース用のバイナリだけを置いており、ソースコードは公開していません。不具合の報告や機能の要望は [Ecro/neuroterm-issues](https://github.com/Ecro/neuroterm-issues) へ。
