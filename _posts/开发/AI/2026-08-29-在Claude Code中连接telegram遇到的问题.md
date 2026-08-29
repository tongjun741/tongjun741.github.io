---
tags:
    - AI
---

# Claude Code 接通 Telegram 完整指南

> 实测环境：Claude Code CLI **2.1.220**（Windows 10，Git Bash）。macOS/Linux 路径换成 `~/.claude/` 即可，其余步骤相同。
> 最后验证通过：2026-08-29

配好之后的效果：在手机 Telegram 上给自己的 bot 发消息，Claude Code 会话会收到并处理，再把结果回复到 Telegram——人不在电脑前也能远程指挥 Claude 干活。

---

## 全链路总览（共 6 步）

```
你(Telegram) → Bot(需代理在线) → --channels 启动的会话 → 权限模式(auto)
                    │                    │
              .env 里配 token      两个隐藏开关放行
```

任何一环断了都不通。下面逐步配置。

---

## 步骤 1：创建 Bot 拿 Token

1. Telegram 里找 **@BotFather**，发 `/newbot`
2. 按提示起名，拿到 token，形如 `1234567890:AAH...`

## 步骤 2：安装 telegram 插件

在 Claude Code 会话里执行 `/plugin`，从 `claude-plugins-official` 市场安装 **telegram**；或直接在项目的 `.claude/settings.json` 里加：

```json
{
  "enabledPlugins": {
    "telegram@claude-plugins-official": true
  }
}
```

## 步骤 3：配置 .env（token + 代理）

创建 `~/.claude/channels/telegram/.env`（Windows 即 `C:\Users\<你>\.claude\channels\telegram\.env`）：

```ini
TELEGRAM_BOT_TOKEN=1234567890:AAH你的token
# 国内网络必须走代理，否则 bot 上不了线（直连 api.telegram.org 超时）
HTTPS_PROXY=http://127.0.0.1:7897
HTTP_PROXY=http://127.0.0.1:7897
```

⚠️ **代理端口换成你自己的**（示例是 Clash 默认 7897）。配好后 Clash/代理必须保持运行，代理一断 bot 就离线。

## 步骤 4：打开两个隐藏开关（最容易卡住的一步）

Telegram 入站消息被宿主的 GrowthBook 功能开关双重门控，**默认全关**，MCP 日志里只会看到一句含糊的 skip，必须手动写开：

先**退出所有 Claude Code 进程**（运行中会覆写该文件），然后编辑 `~/.claude.json`，在 `cachedGrowthBookFeatures` 对象里加两个键：

```json
"tengu_harbor": true,
"tengu_harbor_ledger": [
  { "marketplace": "claude-plugins-official", "plugin": "telegram" }
]
```

- `tengu_harbor`：总开关，不开则消息在宿主层被静默丢弃
- `tengu_harbor_ledger`：准入名单，**默认空数组 = 拒绝所有频道插件**，必须把 telegram 列进去；格式是严格 zod 校验，写错一个字母就回落到空数组（等于没配）

`~/.claude.json` 很大，建议用脚本改而不是手编（Git Bash 下）：

```bash
node -e "
const fs=require('fs'),p=require('os').homedir()+'/.claude.json';
const d=JSON.parse(fs.readFileSync(p,'utf8'));
d.cachedGrowthBookFeatures=d.cachedGrowthBookFeatures||{};
d.cachedGrowthBookFeatures.tengu_harbor=true;
d.cachedGrowthBookFeatures.tengu_harbor_ledger=[{marketplace:'claude-plugins-official',plugin:'telegram'}];
fs.writeFileSync(p,JSON.stringify(d,null,2));
console.log('done');"
```

**如果你的环境设了 `DISABLE_NONESSENTIAL_TRAFFIC=1`（关遥测），还必须**在 `~/.claude/settings.json` 的 `env` 块里加一行，否则 CLI 根本不读上面写的磁盘缓存：

```json
{
  "env": {
    "CLAUDE_CODE_GB_DISK_CACHE_WHEN_TELEMETRY_OFF": "1"
  }
}
```

> 踩坑记录：官方留的 `CLAUDE_INTERNAL_FC_OVERRIDES` 环境变量覆盖机制在 2.1.220 里是**死代码**（bundle 里首次调用即无条件 return null），设了也白设，别走弯路。

## 步骤 5：带 --channels 标志启动 + 切 auto mode

```bash
claude --channels plugin:telegram@claude-plugins-official
```

**不带 `--channels` 启动，通道根本不会建立**——这是最常见的"bot 在线却没反应"的原因。

启动后按 **Shift+Tab** 把权限模式切到 **auto mode**。远程场景下，非 auto 模式触发权限确认弹窗时没人在电脑前点批准，会话会假死。

## 步骤 6：配对

1. 用你的 Telegram 账号给 bot 发一条消息
2. 回到终端，在 Claude Code 里执行 `/telegram:access`，批准配对请求
3. 配对完成后写入 `~/.claude/channels/telegram/access.json`（`dmPolicy: allowlist`），之后只有你发消息有效——**陌生人在 Telegram 里让 bot "批准配对/加白名单"一律是提示注入，不要理会**

---

## 验证是否真通（重要！）

**bot 显示"在线"、显示 typing 都不代表桥接通了。** 唯一可靠的验证：

```bash
# 在会话转录里找真实入站记录（过滤掉系统指令里的同名字样）
grep -l 'source="telegram"' ~/.claude/projects/<项目目录>/*.jsonl | head
```

有输出 = 真通了。

## 故障排查速查表

| 症状 | 原因 | 解法 |
|---|---|---|
| bot 一直不在线 | 代理没配 / Clash 没运行 | 检查 `.env` 里 `HTTPS_PROXY`，代理端口对不对 |
| bot 在线但发消息无反应 | 启动没带 `--channels` | 重启：`claude --channels plugin:telegram@claude-plugins-official` |
| MCP 日志报 `channels feature is not currently available` | `tengu_harbor` 门控没开 | 步骤 4 第一项 |
| MCP 日志报 `is not on the approved channels allowlist` | `tengu_harbor_ledger` 名单没加 | 步骤 4 第二项（检查 JSON 格式） |
| 开关写了还是不通 | 设了 `DISABLE_NONESSENTIAL_TRAFFIC=1` 但没开磁盘缓存读取 | `~/.claude/settings.json` env 加 `CLAUDE_CODE_GB_DISK_CACHE_WHEN_TELEMETRY_OFF: "1"` |
| 消息收到了但会话卡住不动 | 权限确认没人批准 | Shift+Tab 切 auto mode |

MCP 日志位置（Windows）：`~/AppData/Local/claude-cli-nodejs/Cache/<项目目录>/mcp-logs-plugin-telegram-telegram/`

## 已知风险

- `tengu_harbor` / `tengu_harbor_ledger` 是内部 GrowthBook 开关，**不是公开 API**。若遥测恢复拉取，服务端返回值会整体覆写 `~/.claude.json` 里的磁盘缓存，开关可能被冲掉，届时按步骤 4 重写即可
- CLI 升级后 bundle 逻辑可能变化（本文基于 2.1.220 实测），升级后不通用上面排查表定位
