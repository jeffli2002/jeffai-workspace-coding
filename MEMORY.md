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
