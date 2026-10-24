# tmux 命令速查表 · tmux Cheatsheet

[![Stars](https://img.shields.io/github/stars/Ezra-Zhao/tmux-cheatsheet?style=social)](https://github.com/Ezra-Zhao/tmux-cheatsheet/stargazers) [![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE) ![Languages](https://img.shields.io/badge/languages-3-blue)

> 🌍 [Español](README.es.md) · [Português](README.pt.md)

一份**一页纸**的 tmux 速查表：30 条真实可用的用法——会话、窗口、窗格、复制模式与缓冲、`~/.tmux.conf` 配置、脚本化。每个条目只讲三件事——**它是干什么的、一条能直接复制粘贴的命令、适用场景标签**。不编造键位：所有 CLI 命令均在本机 tmux 3.4 上用分离会话实际跑过、结果用 `capture-pane` 与 `list-*` 核对；所有前缀键均对照 `tmux list-keys` 打出的默认绑定表逐条核实；配置示例用 `tmux -f` 真实加载、`show-options` 读回验证。先说清一件最容易卡住人的事：**tmux 的默认前缀键是 Ctrl-b**，下文所有 `prefix` 都指它——`prefix c` 就是先按 Ctrl-b、松开、再按 c。

## 面向谁

后端开发者、运维工程师、SRE，以及任何在服务器上跑长任务的人。如果你每次分屏都要想一下是 `%` 还是 `"`、复制模式进去了不知道怎么出来、SSH 一断任务就没了——这张表就是为你准备的。

## 怎么用

1. 直接看 **[CHEATSHEET.md](CHEATSHEET.md)** ——速查表正文，英文命令全球开发者都无压力。
2. 本地要有 tmux：Linux 用发行版包管理器装（`apt install tmux` / `dnf install tmux`），macOS 用 `brew install tmux`；练习不用服务器，本机终端就能练全套。
3. 按"会话 → 窗口 → 窗格 → 复制模式 → 配置 → 脚本化"的顺序，每个条目亲手敲一遍，一小时就顺手了。

## 先跑起来（30 秒验证环境）

```bash
tmux -V                  # 打印版本（如 tmux 3.4），说明装好了
tmux new -s demo         # 建一个叫 demo 的会话并进入（界面底部出现状态栏）
# 在里面按 Ctrl-b 再按 d（detach 分离）：回到普通终端，会话还在后台跑
tmux ls                  # 能看到 demo —— 证明它没随 detach 消失
tmux attach -t demo      # 接回去：刚才的界面原样还在
```

这一趟 detach → attach 往返跑通，tmux 最核心的卖点你就亲手验证过了：**关终端、断 SSH，会话和里面的任务都活着**。

## 一份能直接用的 ~/.tmux.conf

```bash
set -g prefix C-a                              # 前缀键改成 Ctrl-a（顺手的老习惯）
unbind C-b
bind C-a send-prefix
set -g mouse on                                # 鼠标点选窗格、拖边框调大小、滚轮进复制模式
set -g history-limit 10000                     # 回滚历史 10000 行（默认 2000）
set -g base-index 1                            # 窗口从 1 开始编号
set-window-option -g pane-base-index 1         # 窗格也从 1 开始
set -g renumber-windows on                     # 关窗后自动补齐编号，不留空号
bind r source-file ~/.tmux.conf \; display "Reloaded!"   # prefix r 一键重载配置
bind | split-window -h -c "#{pane_current_path}"         # prefix | 左右分屏，继承当前目录
bind - split-window -v -c "#{pane_current_path}"         # prefix - 上下分屏，继承当前目录
```

存进 `~/.tmux.conf` 后重启 tmux（或在已有会话里按 `prefix r` 重载）即生效——注意前缀键从此变成 Ctrl-a。

## 收录了什么

| 分类 | 内容 |
|---|---|
| 🖥️ 会话 Session | 建具名会话 / ls 与 attach 接回 / detach 分离 / 改会话名 / kill-session 与会话树（5 条） |
| 🧩 窗口 Window | 新建与改名 / n、p、数字切换与窗口列表 / 关窗口 / 命令行建窗 / swap 与 move 换位（5 条） |
| 📐 窗格 Pane | % 与 " 分屏 / 方向键、o、q 切换 / z 全屏 zoom / 交换与布局 / 调大小 / 关窗格与 break-pane（6 条） |
| 📋 复制模式与缓冲 | 进复制模式翻历史 / emacs 与 vi 两种复制键位 / 粘贴与缓冲栈 / capture-pane 抓屏 / save-buffer 存文件（5 条） |
| ⚙️ 配置 ~/.tmux.conf | 改前缀 C-a / 鼠标开启 / 历史与编号 / 一键重载 / 分屏键位重绑继承目录（5 条） |
| 🔗 脚本化与杂项 | 后台起会话与 send-keys / pipe-pane 打日志 / list-keys 查键位 / choose-tree 与 kill-server（4 条） |

官方文档只链 https://github.com/tmux/tmux/wiki ，所有命令均对照本机 tmux 3.4 实测（CLI 真跑、键位对 `list-keys`、配置真加载）核实过；示例中的会话名一律用 work / demo 这类占位名，不涉及任何真实服务器。

## 为什么是这 30 条

- **会话存活是第一卖点**：detach/attach 排在最前面——SSH 断线、终端关掉，任务照跑，这是 tmux 区别于普通终端的根本。
- **复制模式把坑说清**：tmux 3.4 默认是 emacs 键位（Ctrl-Space 起选、Alt-w 复制），网上多数教程写的是 vi 键位（Space 起选、Enter 复制）——不对着 `mode-keys` 的实际值看，照抄必翻车，本表两套都写明。
- **配置只给验证过的**：改前缀、鼠标、历史行数、1 起编号、分屏继承当前目录——五条都是装完当天就回本的设置，且全部真加载验证过。
- **危险操作明确标 ⚠️**：kill-session、关窗口、关窗格、kill-server 都写清了后果和确认方式；kill-server 放在最后一条并注明是最后一招。

## 欢迎 PR

发现命令写错了、键位对不上你的版本、翻译不到位——欢迎提 Pull Request。新增条目请保持统一格式（一句话说明 / bash 代码块 / 标签），**只收真实可用的命令，不确定的宁可不收**。

## 📚 更多速查表

同一作者的其他速查表，同样有中/西/葡三语版本：

- [FFmpeg 命令速查表](https://github.com/Ezra-Zhao/ffmpeg-cheatsheet) — 28 条音视频处理命令
- [Git 实战速查表](https://github.com/Ezra-Zhao/git-cheatsheet) — 30 条日常+救命命令
- [Docker 命令速查表](https://github.com/Ezra-Zhao/docker-cheatsheet) — 28 条容器命令
- [curl 命令速查表](https://github.com/Ezra-Zhao/curl-cheatsheet) — 29 条 API 调试命令
- [正则表达式速查表](https://github.com/Ezra-Zhao/regex-cheatsheet) — 30 条实战正则模式
- [jq 命令速查表](https://github.com/Ezra-Zhao/jq-cheatsheet) — 30 条 JSON 处理 filter
- [终端神器速查表](https://github.com/Ezra-Zhao/terminal-superpowers-cheatsheet) — 14 个终端效率工具
- [Adobe 免费替代速查表](https://github.com/Ezra-Zhao/adobe-free-alternatives-cheatsheet) — 16 个 Adobe 免费替代品
- [退订省钱速查表](https://github.com/Ezra-Zhao/desubscribe-cheatsheet) — 10 组退订替代，年省约 $1,260
- [kubectl 命令速查表](https://github.com/Ezra-Zhao/kubectl-cheatsheet) — Kubernetes 常用命令速查
- [SQL 查询速查表](https://github.com/Ezra-Zhao/sql-cheatsheet) — 常用 SQL 查询与函数速查
- [SSH 命令速查表](https://github.com/Ezra-Zhao/ssh-cheatsheet) — 28 条 SSH 连接、密钥与隧道命令
- [Vim 命令速查表](https://github.com/Ezra-Zhao/vim-cheatsheet) — 30 条编辑、搜索替换与宏命令
- [systemd 命令速查表](https://github.com/Ezra-Zhao/systemd-cheatsheet) — 30 条服务管理与日志命令

## License

MIT © 2026 Guangyi Zhao，详见 [LICENSE](LICENSE)。
