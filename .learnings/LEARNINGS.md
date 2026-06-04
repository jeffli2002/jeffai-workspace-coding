# 学习记录

## 2025-06-04 - Claude Code + CC Switch 代理陷阱

**问题：** CC Switch 本地路由报 502，模型名解析错误（带 ANSI `[1m]`）。

**根因：** 环境变量 `HTTPS_PROXY`/`ALL_PROXY`/`HTTP_PROXY` 等代理设置把对 `127.0.0.1:15721` 的本地请求也代理走了，导致 CC Switch 无法正常工作。

**修复：** 清理代理环境变量后启动 Claude Code：
```bash
env -u HTTPS_PROXY -u ALL_PROXY -u HTTP_PROXY -u https_proxy -u http_proxy -u all_proxy claude
```

**关键洞察：** 代理环境变量对本地服务（localhost/127.0.0.1）的影响在不同工具中表现不一致。有些工具会自动绕过 localhost，有些不会。Claude Code 属于后者。以后排查本地服务问题时，这是必查项。

**关联人：** 黎镭（ou_aeb3984fc66ae7c78e396255f7c7a11b）
