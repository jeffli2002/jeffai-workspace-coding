
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
## 2026-09-17 - sync-supabase-30m DNS failure
- Script `sync_supabase.py` failed for Memories/Documents/Tasks with `[Errno -2] Name or service not known`; treat as network/DNS/Supabase URL issue.
- Feishu alert via `message` tool with `channel=feishu` + `target=user:ou_...` worked; local `feishu.py` needed env and hardcoded creds hit `open_id cross app`.

## 2026-09-17 11:30 - Supabase 同步 DNS 失败

**任务**: sync-supabase-30m 定时任务
**错误**: [Errno -2] Name or service not known
**影响模块**: Memories, Documents, Tasks（全部失败）
**原因**: DNS 解析失败，无法连接到 Supabase 服务器
**处理**: 需要检查网络连接或 DNS 配置

---

## 2026-09-18 15:00 — sync-supabase-30m 同步失败
- 任务: Second Brain 本机→Supabase 定时同步
- 现象: Memories/Documents/Tasks 全部失败, `[Errno -2] Name or service not known`
- 判断: DNS 解析失败 — 网络故障或 SUPABASE_URL 无效/项目暂停
- 待办: 检查 DNS、检查 SUPABASE_URL 环境变量、确认 Supabase 项目状态
- 备注: message 工具无默认 target，failure alert 未能推送，需配置告警 channel

## 2026-09-19 sync-supabase-30m 失败：Supabase 项目域名 NXDOMAIN
- 现象：sync_supabase.py 三个模块全部报 "Name or service not known"
- 排查：supabase.com 正常解析；项目域名 njxjuvxosvwvluxefrzg.supabase.co 在本地 DNS、8.8.8.8、1.1.1.1 均 NXDOMAIN → 项目已被删除或长期暂停
- 处理：需老板介入——恢复/重建 Supabase 项目并更新 /root/.openclaw/credentials/supabase.json，或停用该 cron

## 2026-09-23 sync-supabase-30m 失败 (NXDOMAIN)
- 现象: sync_supabase.py 三模块全部失败，Errno -2 Name or service not known
- 根因: Supabase 项目 njxjuvxosvwvluxefrzg 域名 NXDOMAIN（8.8.8.8 与本地 DNS 均确认），项目已删除/释放，不可自愈
- 处理: 已通过飞书告警老板；需更新 /root/.openclaw/credentials/supabase.json
- 教训: DNS NXDOMAIN 对 Supabase 意味着项目没了，不必重试；直接告警

## 2026-09-25 11:30 CST — sync_supabase.py DNS NXDOMAIN
- 现象: 定时任务 sync-supabase-30m 失败, memories/documents/tasks 全部报 "[Errno -2] Name or service not known"
- 诊断: 本机 DNS 正常 (google.com 可解析), 但 njxjuvxosvwvluxefrzg.supabase.co 返回 NXDOMAIN → Supabase 项目疑似被删除/项目 ref 失效
- 处置: 需老板确认 Supabase 项目状态并更新 /root/.openclaw/credentials/supabase.json
