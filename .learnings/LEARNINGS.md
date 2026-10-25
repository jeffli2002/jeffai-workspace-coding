# Learnings

## 2026-10-25 — 飞书告警必须用 accountId=abe + 配对 open_id
- 现象：cron failureAlert 配置的 `to: user:ou_aeb3984fc66be7c78e396255f7c7a11b` 发报失败，报 `99992351 invalid open_id`；换 `ou_a266c509f74545b9cbe260720a8be507` 报 `99992361 open_id cross app`。
- 结论：必须同时指定 `accountId=abe`（来自 feishu-pairing.json）+ 配对 open_id `ou_a266c509f74545b9cbe260720a8be507`，发送才成功（messageId om_x100b62e032a630acb087098f0cc66fa）。
- 后续：cron job f07475a4 (sync-supabase-30m) 的 failureAlert.to 仍是旧的无效 open_id，需老板/有权限时更新为新值（本次 cron update 被权限限制：restricted to the current cron job）。
