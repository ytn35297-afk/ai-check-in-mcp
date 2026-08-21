# 🏥 病娇AI查岗系统 · MCP 代理层（Vercel / Render）

让 AI 能主动查岗：查询老婆/男朋友的手机活动、发 ntfy 推送弹窗的桥梁。实现了 MCP 协议的 `initialize` / `tools/list` / `tools/call`。

## 暴露的工具

| 名称 | 说明 |
|------|------|
| `check_on_wife(limit=10)` | 查询手机最近打开的 App 与累计使用时长 |
| `ntfy_alert(title, content)` | 给手机发 ntfy 推送弹窗（高优先级） |

## 部署（Vercel）

1. 导入本仓库：Vercel.com → Import Git Repository → 部署。
2. 添加环境变量：

   | 变量 | 说明 |
   |------|------|
   | `ORIGIN_API` | 后端 Railway 域名，**务必带 `https://` 前缀**，如 `https://xxx.up.railway.app` |
   | `NTFY_TOPIC` | 你的 ntfy 订阅主题（如 `shushu-checkin`），手机端需订阅同一个主题 |
   | `NTFY_SERVER` | 可选，默认 `https://ntfy.sh`，自托管才需要改 |

3. 部署完成后 MCP 端点地址：`https://你的项目名.vercel.app/mcp`。

## 🆘 备用部署（Render · Vercel 不可用时）

代码零改动，仓库里已带 `render.yaml`，支持一键部署：

1. **render.com** 注册登录（可用 GitHub 账号授权）。
2. New → **Blueprint** → 选择本仓库 → 它会自动读取 `render.yaml`。
3. 按提示填入环境变量（`ORIGIN_API`、`NTFY_TOPIC`，`NTFY_SERVER` 可留空）。
4. 部署完成后 MCP 端点：`https://ai-check-in-mcp.onrender.com/mcp`。

> ⚠️ 免费实例 **15 分钟无请求会自动休眠**，冷启动约 30 秒。用 UptimeRobot 免费版每 10 分钟 GET 一次 `/ping` 就能保活，避免 AI 查岗时等半天。

## 绑定到 AI 角色

把 `/mcp` 地址作为 MCP 工具端点填入你的 AI 角色（Kelivo / Claude Desktop / 其他 MCP 平台），AI 就能调用 `check_on_wife()` 查岗、`ntfy_alert()` 弹窗。

## 手机端接收推送（安卓 · ntfy）

1. Play 商店安装 **ntfy**（开源免费，iOS 也有）。
2. 订阅你设置的 `NTFY_TOPIC` 主题（如 `shushu-checkin`）。
3. 主题里勾选「高优先级」通知，这样 AI 弹窗才会亮屏轰炸。

## 本地运行

```bash
pip install -r requirements.txt
ORIGIN_API=https://xxx.up.railway.app NTFY_TOPIC=你的主题 uvicorn app:app --port 8000
```

---

配套后端见仓库 [ai-check-in-backend](https://github.com/ytn35297-afk/ai-check-in-backend)。
