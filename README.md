# QQbot (DwgxBot)

> QQ 群聊骰子赌博机器人，附带加密聊天 demo 和一堆桌面小工具。
>
> A QQ group chatbot with dice gambling, encrypted web chat, and PySide6 desktop utilities.

## Overview / 概述

A QQ channel/group bot built on Tencent's official `qq-botpy` SDK. It listens for @-mentions, parses text commands, and runs a token-based dice betting system complete with a dealer ("boss") role, red-envelope handouts, and account tracking. Data persists to local JSON.

The repo also includes:
- An experimental WebSocket + AES encrypted chat server with static frontend (`Code/web/`)
- A collection of PySide6 desktop tools (`other/pyside6tool/`)

---

QQbot 是个基于腾讯 `qq-botpy` SDK 的群聊机器人，核心玩法是代币骰子押注——下注大小单双或指定点数，摇骰子开奖，赢了翻倍输了归零。支持庄家（负责人）系统、红包收发、账户余额查询。数据用本地 JSON 文件存。

仓库里还塞了两坨别的东西：
- `Code/web/`：WebSocket + AES 加密的聊天服务端 demo 和静态前端页面
- `other/pyside6tool/`：一堆 PySide6 写的桌面小工具（端口查看、二维码生成、文件分类、注释清理之类的）

## Features / 功能

### Bot / 机器人 (`Code/QQbot/main/`)

| Command | What it does |
|---------|-------------|
| `大100 单50` | Bet 100 on "big", 50 on "odd" |
| `sh` / `🎲` | Roll once |
| `sh3` / `🎲3` | Roll three times |
| `我当老板` / `不当老板` | Become / quit dealer |
| `查看老板` / `boss` | Check current dealer |
| `hb <amount> <count>` | Send a red envelope |
| `领取` | Claim red envelope |
| `撤回` | Recall red envelope |
| `ye` / `查看账户` | Check balance & history |
| `复读 <text>` | Echo text back |
| `取消` / `qx` | Cancel current game |
| `规则` | Game rules link |

- Odds table: big/small 1.97x, odd/even 2.9x, exact number up to ~6.9x
- Dealer account starts with 1,000,000 tokens
- Admin list controls who can issue red envelopes
- Async data persistence with `asyncio` locks for concurrency safety
- Graceful shutdown on SIGINT/SIGTERM

### Web Chat Demo / 网页聊天 (`Code/web/`)

- WebSocket chat server (`ChatServer`) on `0.0.0.0:20008`
- AES session-key encryption for messages
- Static frontend (HTML/CSS/JS) in `public/`

### Desktop Tools / 桌面工具 (`other/pyside6tool/`)

- `PortInfoApp.py` — port scanner
- `QRCGenerator.py` — QR code generator
- `FileClassifierTool.py` — file classifier
- `ConverterGUI.py` — converter
- `CodeCleaner.py` / `exegesiskiller.py` — code/comment cleaner
- `LibUninstaller.py` — dependency uninstaller

## Tech Stack / 技术栈

- **Language**: Python (main), HTML/CSS/JS (frontend)
- **QQ Bot SDK**: `qq-botpy` 1.2.1
- **Async/Network**: `aiohttp`, `websockets`, `APScheduler`
- **Crypto**: `cryptography` (AES for web chat)
- **Config/Data**: `PyYAML`, stdlib `json`
- **Desktop GUI**: `PySide6` 6.8.x

Full deps in [`requirements.txt`](requirements.txt).

## Project Structure / 项目结构

```
QQbot/
├── Code/
│   ├── QQbot/
│   │   ├── main/
│   │   │   ├── DwgxBot.py        # Bot entry, event dispatch
│   │   │   ├── DataManager.py    # Config + JSON persistence
│   │   │   ├── Gambling.py       # Dice betting & odds
│   │   │   ├── Boss.py           # Dealer/boss logic
│   │   │   ├── RedEnvelope.py    # Red envelope send/claim
│   │   │   └── Assist.py         # Balance, history queries
│   │   └── resource/             # Emoji/GIF assets
│   └── web/
│       ├── Sever.py              # WebSocket + AES chat server
│       ├── package.json
│       └── public/               # Static frontend
├── other/
│   ├── pyside6tool/              # PySide6 desktop tools
│   └── run/                      # Runtime config/data/backups
├── requirements.txt
├── setup.py
├── LICENSE
└── SECURITY.md
```

## Getting Started / 快速开始

### Prerequisites / 前置要求

- Python 3.10+
- A registered QQ bot `appid` and `secret` from the QQ Bot Open Platform

### Install / 安装

```bash
git clone https://github.com/dwgx/QQbot.git
cd QQbot
python -m venv .venv
# Windows
.venv\Scripts\activate
# Linux/macOS
# source .venv/bin/activate
pip install -r requirements.txt
```

### Configure / 配置

Create a config file (lookup order: `Code/QQbot/main/config.yaml` -> project root `config.yaml` -> `other/run/config.yaml`, or set `DWGXBOT_CONFIG` env var):

```yaml
appid: "your_appid"
secret: "your_secret"
admins:
  - "admin_user_id"
```

### Run / 运行

```bash
python Code/QQbot/main/DwgxBot.py
```

Bot reads config, initializes data, and responds to @ commands. Logs and data go to `other/run/` (`dwgxbot.log`, `data.json`).

Optional — run the web chat server:

```bash
python Code/web/Sever.py
```

<!-- TODO: confirm exact frontend serving/deployment steps for Code/web/public — the bundled package.json has no build/start script. -->

## Usage / 使用方法

在群里 @ 机器人发指令：

- `@bot 规则` — get game rules link
- `@bot 我当老板` — become dealer
- `@bot 大100 单50` — bet 100 on big, 50 on odd
- `@bot sh` — roll dice
- `@bot ye` — check balance
- `@bot hb 100 5` — send red envelope (100 tokens, 5 people)
- `@bot 领取` — claim red envelope
- `@bot 复读 你好` — echo "你好"

## Configuration / 配置项

Config file fields:
- `appid` — QQ bot application ID (required)
- `secret` — QQ bot application secret (required)
- `admins` — admin user ID list (controls red envelope permissions)

Environment variables:
- `DWGXBOT_CONFIG` — config file path override
- `DWGXBOT_DATA` — data JSON file path override

Web chat server listens on `0.0.0.0:20008` by default (configurable in `Sever.py`).

## CI

`.github/workflows/ci.yml` runs `python -m compileall` on push to `main` and PRs. Syntax check only, no test suite.

<!-- TODO: no test suite present; add tests if needed. -->

## Status / 状态

个人练习项目，WIP 状态。核心机器人能跑，赔率和部分 URL 有硬编码占位。`Code/web` 和 `other/pyside6tool` 是独立附带的东西。

Personal practice project, WIP. The core bot works; odds and some URLs are hardcoded placeholders. The web chat and PySide6 tools are independent extras.

## License / 许可证

[MIT License](LICENSE) - Copyright (c) 2024 dwgx
