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

## 2025-06-12

### Codex CLI + APP 登录失败

**问题：** Codex CLI 和 Codex APP 登录失败，报错：
- CLI: `auth no Codex credentials were found` + `reachability one or more required provider endpoints are unreachable`
- APP: `Token exchange failed: error sending request for url (https://auth.openai.com/oauth/token)`
- APP 后续: `403 Forbidden: Country, region, or territory not supported`

**根因：** 多层代理配置问题 + Codex APP 本身不支持代理：
1. **Shell 环境变量**：`HTTP_PROXY=http://127.0.0.1:7897`, `HTTPS_PROXY=http://127.0.0.1:7897`, `ALL_PROXY=socks5://127.0.0.1:7993`
2. **macOS 系统代理**：所有网络接口的 HTTP/HTTPS 代理都指向 `7993`（SOCKS5 端口），协议不匹配
3. **7897 端口 SSL 问题**：HTTP 代理端口的 TLS 握手失败
4. **Codex APP 不支持代理**：GitHub Issue #10555，APP 完全不支持 HTTP/SOCKS5 代理配置
5. **Codex APP OAuth 地区限制**：即使通过 Proxifier 强制代理，APP 的 token exchange 返回 403（地区不支持）

**CLI 解决方案：**
```bash
# 去掉 HTTP 代理变量（7897 端口 SSL 有问题）
unset HTTP_PROXY HTTPS_PROXY
# 保留 SOCKS5 代理
export ALL_PROXY="socks5://127.0.0.1:7993"
codex login
```

**APP 解决方案：**
- Codex APP 目前不支持代理，在国内无法使用
- 即使通过 Proxifier 强制 SOCKS5 代理，OAuth 登录仍返回 403（地区不支持）
- 建议用 CLI 代替，功能相同

**CLI MCP 警告（可忽略）：**
- `codex_apps` MCP 启动失败是已知 bug（GitHub Issue #16360）
- 原因：`codex_apps` 使用 reqwest 库，编译时未启用 SOCKS5 feature
- 解决方案：同时配置 HTTP_PROXY + ALL_PROXY，或忽略该警告（不影响核心功能）

**教训：**
1. 代理工具的 HTTP 端口和 SOCKS5 端口不能混用——系统代理配置里 HTTP 代理必须指向 HTTP 端口，不能指向 SOCKS5 端口
2. 桌面应用不读 shell 环境变量，只读系统代理设置
3. 检查所有网络接口的代理配置，不只 Wi-Fi
4. Codex APP 在国内目前无法使用，等 OpenAI 修复 Issue #10555

**关联人：** 黎镭（ou_aeb3984fc66ae7c78e396255f7c7a11b）

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
