
## 2026-09-14 sync-supabase NXDOMAIN
- sync_supabase.py 三个模块全部失败：[Errno -2] Name or service not known
- 诊断：njxjuvxosvwvluxefrzg.supabase.co 在 8.8.8.8 和本机 DNS 均 NXDOMAIN；本机网络/其他域名解析正常
- 结论：Supabase 项目已被删除或重建，需更新 /root/.openclaw/credentials/supabase.json

## 2026-09-14 — Supabase 同步失败：项目域名 NXDOMAIN
- **现象**: sync_supabase.py 三个模块 (Memories/Documents/Tasks) 全部报 `[Errno -2] Name or service not known`
- **根因**: 配置的 `njxjuvxosvwvluxefrzg.supabase.co` DNS 解析为 NXDOMAIN（域名不存在）；本机网络和 DNS 正常（supabase.com 可访问）
- **结论**: Supabase 项目很可能已被删除（免费版长期暂停后会被清除），非本机故障
- **处置**: 需老板登录 Supabase 控制台确认项目状态；如已删除需重建项目并更新 `/root/.openclaw/credentials/supabase.json`

## 2026-09-15 01:00 - sync-supabase-30m DNS NXDOMAIN
- What: sync_supabase.py failed for all modules (memories/documents/tasks) with "Name or service not known"
- Root cause: project host njxjuvxosvwvluxefrzg.supabase.co returns NXDOMAIN on both local DNS (183.60.83.19) and 8.8.8.8 — Supabase project likely paused (free-tier auto-pause) or deleted
- Action needed: owner must restore/unpause project in Supabase dashboard, or update /root/.openclaw/credentials/supabase.json if project was recreated

## 2026-09-15 04:30 CST — sync-supabase-30m 失败：Supabase 项目域名 NXDOMAIN
- 现象：sync_supabase.py 连接后三个模块（Memories/Documents/Tasks）全部同步失败，`[Errno -2] Name or service not known`
- 根因：`njxjuvxosvwvluxefrzg.supabase.co` 在 8.8.8.8 / 1.1.1.1 均返回 NXDOMAIN → Supabase 项目疑似被删除（非本机网络/DNS 问题，supabase.co 主域可解析）
- 处置：已通知老板。需要：恢复/重建 Supabase 项目，或更新 `/root/.openclaw/credentials/supabase.json` 的 url + service_key
- 注意：cron job 的 failureAlert(after:3) 统计的是 agentTurn 崩溃次数，脚本失败但 agent 正常结束时不会累加，内置告警不会触发 → 需手动告警
