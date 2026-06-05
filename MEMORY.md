# 黎镭的长期记忆

## 2025-06-04

### Claude Code + CC Switch 代理问题

**问题：** 使用 CC Switch 本地路由（127.0.0.1:15721）时，Claude Code 报错：
- 模型名解析错误（`claude-sonnet-4-6[1m]` 带 ANSI 转义序列）
- 502 状态码（no body）
- 提示检查 inference gateway

**根因：** 环境变量 `HTTPS_PROXY`/`ALL_PROXY`/`HTTP_PROXY` 等代理设置干扰了 Claude Code 对本地 CC Switch 服务（127.0.0.1:15721）的连接。

**解决方案：**
```bash
env -u HTTPS_PROXY -u ALL_PROXY -u HTTP_PROXY -u https_proxy -u http_proxy -u all_proxy claude
```

或者直接 unset 这些变量后启动 Claude Code。

**教训：** 使用本地 API 路由/网关时，代理环境变量会劫持本地连接请求，导致诡异错误。以后遇到类似 "本地服务不可达但服务明明在运行" 的情况，优先检查代理变量。

## 2025-06-05

### Claude Code + CC Switch 模型映射配置

**问题：** Claude Code 启动报错 `There's an issue with the selected model (claude-opus-4-8[1m]). It may not exist or you may not have access to it.`

**根因：** CC Switch 的 provider 配置里只有 `env`（API 地址和 Key），没有 `modelAliases`。Claude Code 请求 `claude-opus-4-8` 等 Anthropic 模型名，但 Kimi API 只认 `kimi-k2p6`/`kimi-k2p5` 等自己的模型名。CC Switch 没有配置模型映射，导致请求直接转发给 Kimi API 被拒。

**修复：** 在 CC Switch 的 provider 配置（数据库 `providers` 表的 `settings_config` 字段）里添加 `modelAliases`：

```json
{
  "env": {
    "ANTHROPIC_BASE_URL": "https://api.kimi.com/coding/v1",
    "ANTHROPIC_AUTH_TOKEN": "sk-kimi-..."
  },
  "modelAliases": {
    "claude-opus-4-8": "kimi-k2p6",
    "claude-opus-4-8-20250514": "kimi-k2p6",
    "claude-sonnet-4-6": "kimi-k2p6",
    "claude-sonnet-4-6-20251022": "kimi-k2p6",
    "claude-haiku-4-5": "kimi-k2p5",
    "claude-haiku-4-5-20251001": "kimi-k2p5"
  }
}
```

**关键洞察：**
1. `modelAliases` 是 CC Switch 的功能，不是 Claude Code 的功能。配置在 CC Switch 的 provider 级别，不是 `~/.claude/settings.json`。
2. `~/.claude/settings.json` 里的 `modelAliases` 会被 Claude Code 忽略，因为它不认识这个字段。
3. 修改 provider 配置后，需要重启 CC Switch 或重新加载配置才能生效。

**排查路径：**
1. 确认 proxy 环境变量已 unset（昨天的教训）
2. 检查 CC Switch 是否在运行（`lsof -i :15721`）
3. 检查 provider 配置是否有 `modelAliases`（`sqlite3 ~/.cc-switch/cc-switch.db`）
4. 检查 `~/.claude/settings.json` 是否有 JSON 语法错误（今天发现的逗号问题）

**关联人：** 黎镭（ou_aeb3984fc66ae7c78e396255f7c7a11b）
