# QQbot (DwgxBot)

基于 `qq-botpy` 的 QQ 机器人项目，包含群聊互动、小游戏、红包与数据持久化模块。

## 环境要求

- Python 3.10+
- 已在 QQ 机器人平台创建应用并获取 `appid` / `secret`

## 快速启动

1. 克隆仓库并进入目录

```bash
git clone https://github.com/dwgx/QQbot.git
cd QQbot
```

2. 创建并激活虚拟环境（推荐）

```bash
python -m venv .venv
.venv\Scripts\activate
```

3. 安装依赖

```bash
pip install -r requirements.txt
```

4. 准备配置文件（推荐放在 `other/run/config.yaml`）

```yaml
appid: "你的appid"
secret: "你的secret"
admins:
  - "管理员用户ID"
```

5. 启动机器人

```bash
python Code/QQbot/main/DwgxBot.py
```

## 配置与数据路径

程序支持以下环境变量：

- `DWGXBOT_CONFIG`: 指定配置文件路径
- `DWGXBOT_DATA`: 指定数据文件路径

未设置时默认行为：

- 配置文件优先查找：
  1. `Code/QQbot/main/config.yaml`
  2. 仓库根目录 `config.yaml`
  3. `other/run/config.yaml`
- 数据文件默认写入：`other/run/data.json`
- 日志默认写入：`other/run/dwgxbot.log`

## 项目结构（核心）

- `Code/QQbot/main/`: 机器人主逻辑
- `other/run/`: 运行期配置与数据目录
- `web/`: 相关网页资源和辅助服务代码

## 常见问题

### 1) 启动时报缺少 appid/secret

检查配置文件是否存在，并确认 `appid`、`secret` 字段已填写。

### 2) 运行目录变化后找不到配置文件

设置 `DWGXBOT_CONFIG` 指向绝对路径配置文件，避免依赖当前工作目录。

### 3) 数据文件写入失败

检查 `other/run/` 或 `DWGXBOT_DATA` 指向目录是否有写权限。
