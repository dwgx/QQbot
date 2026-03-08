# QQbot（DwgxBot）

这是一个基于 `qq-botpy` 的 QQ 机器人项目，包含群聊互动、定时任务和基础数据持久化能力。

## 功能概览

- 基础消息响应
- 管理员功能扩展
- 运行日志与数据持久化
- 可按模块逐步扩展

## 环境要求

- Python 3.10+
- 已在 QQ 机器人平台申请 `appid` / `secret`

## 快速启动

```bash
git clone https://github.com/dwgx/QQbot.git
cd QQbot
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
python Code/QQbot/main/DwgxBot.py
```

## 配置文件

推荐创建 `other/run/config.yaml`：

```yaml
appid: "你的appid"
secret: "你的secret"
admins:
  - "管理员用户ID"
```

## 可用环境变量

- `DWGXBOT_CONFIG`：指定配置文件路径
- `DWGXBOT_DATA`：指定数据文件路径

## 目录说明

- `Code/QQbot/main/`：主逻辑入口
- `other/run/`：运行期配置、数据、日志
- `web/`：网页与辅助资源

## 常见问题

### 启动报 appid/secret 缺失

检查配置文件路径与字段名是否正确。

### 数据文件无法写入

检查目标目录是否存在写权限。
