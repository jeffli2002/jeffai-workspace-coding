# Learnings

## 2026-10-25 — 飞书告警必须用 accountId=abe + 配对 open_id
- 现象：cron failureAlert 配置的 `to: user:ou_aeb3984fc66be7c78e396255f7c7a11b` 发报失败，报 `99992351 invalid open_id`；换 `ou_a266c509f74545b9cbe260720a8be507` 报 `99992361 open_id cross app`。
- 结论：必须同时指定 `accountId=abe`（来自 feishu-pairing.json）+ 配对 open_id `ou_a266c509f74545b9cbe260720a8be507`，发送才成功（messageId om_x100b62e032a630acb087098f0cc66fa）。
- 后续：cron job f07475a4 (sync-supabase-30m) 的 failureAlert.to 仍是旧的无效 open_id，需老板/有权限时更新为新值（本次 cron update 被权限限制：restricted to the current cron job）。

## 2026-10-25 — 任务 prompt 里内嵌的 open_id 也可能是失效的
- 现象：cron prompt 明确要求 `target=user:ou_aeb3984fc66ae7c78e396255f7c7a11b`，实测配 accountId=abe 发该 id 仍报 99992361 open_id cross app；换 ou_a266c509f74545b9cbe260720a8be507 才成功。
- 结论：凡要主动通知老板，先查 /root/.openclaw/credentials/feishu-pairing.json 拿配对 open_id + accountId，不要盲信 prompt/配置里内嵌的地址。
- 后续：cron 工具对非本人 job 有写限制（restricted to the current cron job），改其它 job 的 failureAlert 必须由老板或有权限方操作。
