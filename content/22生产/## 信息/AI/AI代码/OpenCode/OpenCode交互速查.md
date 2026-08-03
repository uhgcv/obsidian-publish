# OpenCode 交互速查

> 桌面版（Electron）操作指南。所有 keybind 经 **Settings → Shortcuts** 配置（tui.json 仅终端 TUI，桌面版会删除该文件，无用）。

## 修改提问重新发送
- `/undo`：回退上一轮——撤销 AI 改动+回答，把你的原提问重新显示出来。改写后按 Enter 重发。
- 连续 `/undo` 回退多轮；`/redo` 重做。
- 文件改动也会被撤销（需 Git 仓库）。

## 分支对话（Fork）
- **命令面板**（`ctrl+p`）搜 **"Fork from message"**：从你选的消息分叉出新会话，原会话不变。
- 快捷键：**Alt+F**（在 Settings → Shortcuts 绑 "session fork"）。
- 分支后用 `/sessions`（`ctrl+l`）在分支间切换。

## 导出对话
- `/export`：当前会话导出为 Markdown，用 `$EDITOR` 打开。
- Windows 须先设 EDITOR：`setx EDITOR "code --wait"`（VS Code）或 `setx EDITOR notepad`，重启桌面版生效。
- 分享链接：`/share` 生成 URL 复制到剪贴板；`/unshare` 撤销。

## 归档会话（Archive）
- **作用**：把会话从主列表隐藏（不删数据），用于清理杂乱。
- **操作**：命令面板搜 **"Archive session"**，或会话列表右键菜单。
- **为何消失**：归档后默认从会话列表排除（"Archived sessions are excluded by default"）。
- **找回**：会话列表里切"显示归档"过滤，对该会话执行 **Unarchive** 恢复。数据始终在 opencode.db。

## 删除会话（Delete）
- **操作**：命令面板搜 **"Delete session"**；TUI 默认 `ctrl+d`（桌面版在 Settings → Shortcuts 绑）。
- **区别**：删除 = **永久移除**（不可恢复）；归档 = 隐藏可恢复。删前不确定就归档。

## 跳转回答（替代滚轮）
- **↑/↓** = 跳上一条/下一条回答（在 Settings → Shortcuts 绑 "messages previous/next" 到 ↑/↓，并从 "history previous/next" 解绑）。
- `ctrl+g` 跳第一条；`ctrl+alt+g` 跳最后一条。

## 会话列表
- `/sessions`（`ctrl+l`）：列出并切换会话。归档项默认隐藏。

## keybind 配置要点
- **桌面版**：Settings → Shortcuts UI（唯一方式；不在任何用户可编辑配置文件）。
- **终端 TUI**：`~/.config/opencode/tui.json`（桌面版会删除此文件，仅 CLI 用）。
- 桌面版与 TUI keybind 相互独立。
