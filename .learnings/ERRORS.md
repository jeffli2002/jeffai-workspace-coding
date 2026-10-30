
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

## 2026-09-25 — sync-supabase-30m 失败：Supabase 项目域名 NXDOMAIN
- 现象：sync_supabase.py 三个模块（Memories/Documents/Tasks）全部报 `[Errno -2] Name or service not known`
- 排查：本机 DNS 正常（github.com 可解析），supabase.co 主域可解析；项目域名 njxjuvxosvwvluxefrzg.supabase.co 在 8.8.8.8 / 1.1.1.1 均返回 NXDOMAIN
- 结论：Supabase 项目很可能已被删除或项目 ref 变更，非网络抖动
- 行动：需老板确认 Supabase 项目状态，并更新 /root/.openclaw/credentials/supabase.json 中的 url/service_key

## 2026-09-25 sync-supabase-30m 失败
- 脚本 sync_supabase.py 全部模块报 DNS 解析失败
- 根因: njxjuvxosvwvluxefrzg.supabase.co 在本地及 8.8.8.8/1.1.1.1 均 NXDOMAIN（项目可能被暂停/删除或 URL 变更）
- 处理: 需老板在 Supabase Dashboard 确认项目状态，恢复或更新 /root/.openclaw/credentials/supabase.json

## 2026-09-26 | sync-supabase-30m 失败：Supabase 项目域名 NXDOMAIN
- 现象：sync_supabase.py 三个模块（Memories/Documents/Tasks）全部 `[Errno -2] Name or service not known`
- 根因：`njxjuvxosvwvluxefrzg.supabase.co` DNS 解析 NXDOMAIN（本机 DNS 与 8.8.8.8 均确认），通用 DNS 正常 → Supabase 项目已被删除/释放，或 credentials 中的 url 已失效
- 处置：需人工到 Supabase Dashboard 确认项目状态；更新 `/root/.openclaw/credentials/supabase.json` 的 url/service_key 后重跑脚本

## 2026-09-27 — sync-supabase-30m 失败：Supabase 项目域名 NXDOMAIN
- **现象**：sync_supabase.py 三个模块（Memories/Documents/Tasks）全部报 `[Errno -2] Name or service not known`
- **诊断**：本机 DNS 正常（github.com、supabase.co 均可解析）；经 Cloudflare DoH 公共 DNS 复核，`njxjuvxosvwvluxefrzg.supabase.co` 返回 NXDOMAIN(Status 3)
- **结论**：Supabase 项目本身已不存在（免费项目长期暂停后被删除的可能性最大），非本机网络问题
- **待办**：老板需在 Supabase 重建/恢复项目，用新 URL + service_key 更新 `/root/.openclaw/credentials/supabase.json`，之后重跑脚本验证
2026-09-27 10:30 - Supabase sync failed: DNS NXDOMAIN for njxjuvxosvwvluxefrzg.supabase.co - project may be deleted
2026-09-27 10:30 - Supabase sync failed: DNS NXDOMAIN for njxjuvxosvwvluxefrzg.supabase.co - project may be deleted

## 2026-09-27 — sync-supabase-30m: Supabase 项目域名 NXDOMAIN
- **现象**: sync_supabase.py 三个模块全部失败，报 `[Errno -2] Name or service not known`
- **诊断**: `njxjuvxosvwvluxefrzg.supabase.co` 在本机 DNS 和 8.8.8.8 均 NXDOMAIN；但 `supabase.co` 主域正常解析 → 非本机网络问题，是 Supabase 项目被删除/暂停回收
- **处理**: 需要老板确认 Supabase 项目状态；若已重建项目，需更新 `/root/.openclaw/credentials/supabase.json` 中的 url/service_key
- **复查**: 下次 cron 运行前若凭证未更新会继续失败

## 2026-10-08 sync-supabase-30m DNS failure

**错误**: `[Errno -2] Name or service not known` 在连接 Supabase 时
**触发**: cron job `sync-supabase-30m`
**根因**: 本机 VM 无法解析 `njxjuvxosvwvluxefrzg.supabase.co` DNS（网络隔离/防火墙问题）
**影响**: Memories/Documents/Tasks 三个模块同步全部失败
**处置**: 非脚本问题，无需老板介入；需检查 VM 网络出口策略或 Supabase IP白名单

## 2026-10-10 | sync-supabase-30m 持续失败：Supabase 项目域名 NXDOMAIN
- **现象**：`sync_supabase.py` 每 30 分钟运行均失败，Memories/Documents/Tasks 全模块报 `[Errno -2] Name or service not known`，exit 1。
- **诊断**：脚本里 "✓ 已连接到 Supabase" 有误导性——`create_client` 不发网络请求，仅初始化。真实失败发生在首次 API 调用。`getent hosts` 与 `nslookup`（本地 DNS 183.60.83.19 + Google 8.8.8.8）对 `njxjuvxosvwvluxefrzg.supabase.co` 均返回 NXDOMAIN。
- **结论**：非本机网络问题；Supabase 项目疑似被删除或 project ref 变更。
- **待办**：老板确认 Supabase 项目状态，更新 `/root/.openclaw/credentials/supabase.json`（url + service_key）；恢复前该 cron 会持续失败。

## 2026-10-14 — Supabase sync failed: project DNS NXDOMAIN
- Task: cron sync-supabase-30m, script `second-brain/scripts/supabase/sync_supabase.py`
- What happened: All 3 modules (Memories/Documents/Tasks) failed with `[Errno -2] Name or service not known`. Verified via 8.8.8.8 and 1.1.1.1: `njxjuvxosvwvluxefrzg.supabase.co` is NXDOMAIN globally — the Supabase project appears deleted (or free-tier pause expired). Local DNS/network is fine.
- Action needed: Boss must check Supabase dashboard — restore/recreate project and update `/root/.openclaw/credentials/supabase.json` (url + service_key). Do NOT keep retrying sync until credentials are fixed.

## 2026-10-17 — sync-supabase-30m 失败: Supabase 项目 DNS NXDOMAIN
- 现象: sync_supabase.py 三个模块 (memories/documents/tasks) 全部 "Name or service not known"
- 排查: 本机 DNS 正常 (google.com 可解析); 8.8.8.8 / 1.1.1.1 均返回 NXDOMAIN for njxjuvxosvwvluxefrzg.supabase.co; supabase.co 主域正常
- 结论: Supabase 项目 njxjuvxosvwvluxefrzg 已被删除或长期暂停后回收, 非脚本/网络问题
- 处理: 需老板确认项目状态, 恢复或新建项目并更新 /root/.openclaw/credentials/supabase.json
- 2026-10-19 | sync-supabase-30m 失败 | Supabase 项目域名 njxjuvxosvwvluxefrzg.supabase.co 解析 NXDOMAIN（本机 DNS 正常，supabase.co 主域可解析），疑似项目被暂停/删除（免费层不活跃自动暂停）。需要老板到 Supabase 控制台恢复项目或更新 credentials/supabase.json。

## 2026-10-21 - sync-supabase-30m 失败: Supabase 项目域名 NXDOMAIN
- 现象: sync_supabase.py 三个模块全部报 "Name or service not known"
- 诊断: njxjuvxosvwvluxefrzg.supabase.co 在 8.8.8.8 和本机 DNS 均返回 NXDOMAIN；supabase.co 主域正常
- 结论: Supabase 免费版项目疑似被暂停（DNS 记录被移除），非本机网络问题
- 处理: 需老板到 Supabase Dashboard 恢复/解除暂停项目；恢复后重跑脚本即可
## 2026-10-21 - Supabase sync DNS failure

- What happened: `python3 /root/.openclaw/workspace/projects/second-brain/scripts/supabase/sync_supabase.py` failed for Memories, Documents, and Tasks with `[Errno -2] Name or service not known`.
- What to do differently: Before sync, verify DNS/network reachability to Supabase host; consider preflight check and clearer failure alert when name resolution fails.


## 2026-10-21 22:08 - sync-supabase-30m 失败 + 脚本改进（DNS 预检）
- 现象: sync_supabase.py 三个模块全报 `[Errno -2] Name or service not known`
- 根因确认: njxjuvxosvwvluxefrzg.supabase.co 在本机 DNS、Google DoH、Cloudflare DoH、223.5.5.5/腾讯 DoH 全部 NXDOMAIN（supabase.co 主域正常）→ Supabase 项目被删除/暂停回收
- 改进: scripts/supabase/sync_supabase.py 新增 socket DNS 预检（preflight），失败时直接报明确原因，create_client 不再"假成功"
- 教训: supabase-py 的 create_client 不做连通性校验，永远不要把它的成功当成连接成功
- 处理: 已发飞书 failure alert（om_x100b62b83a0acca8b07c02cdd3d799a），待老板恢复项目或更新 credentials/supabase.json

## 2026-10-22 00:03 - sync-supabase-30m 失败：Supabase 项目域名 NXDOMAIN（持续第 N 次）
- 现象: sync_supabase.py exit 1，DNS 预检直接拦截，0 个模块同步
- 诊断: njxjuvxosvwvluxefrzg.supabase.co 在本机 DNS / 8.8.8.8 / 1.1.1.1 全部 NXDOMAIN；supabase.co 主域正常 → 项目已被暂停/回收
- 处置: 已发飞书 failure alert（om_x100b62ba7558e8a4b32e113b6462486）；需老板恢复项目或更新 credentials/supabase.json
- 注意: cron run 的 status 始终为 "ok"（agent turn 正常返回），所以 cron 自带 failureAlert（after=3）不会触发；失败告警只能手动发消息

## 2026-10-22 — sync_supabase.py 失败：Supabase 项目域名 NXDOMAIN
- 现象：`python3 /root/.openclaw/workspace/projects/second-brain/scripts/supabase/sync_supabase.py` exit 1，报 "DNS 解析失败 njxjuvxosvwvluxefrzg.supabase.co"。
- 排查：本机 DNS 正常（supabase.co / api.github.com 可解析），8.8.8.8 / 1.1.1.1 / 223.5.5.5 三家公网解析器对该域名均返回 NXDOMAIN → 项目在公网已不存在（被删除或暂停回收），非本机网络问题。
- 处理：已发飞书 failure alert 给老板，要求在 Supabase Dashboard 恢复项目并更新 /root/.openclaw/credentials/supabase.json。
- 下次做法：遇到此类 NXDOMAIN，先用 3 家公网 DNS 交叉验证再判定是本机网络还是项目本身失效，避免误判为网络抖动。

## 2026-10-22 sync-supabase-30m 失败（DNS NXDOMAIN）
- 现象: sync_supabase.py 退出码 1，报 `DNS 解析失败: njxjuvxosvwvluxefrzg.supabase.co`
- 排查: 本地 getent + Google DoH 均返回 NXDOMAIN；supabase.com / google.com 解析正常 → 非本机网络问题，是该项目域名已不存在（项目被删除/回收）
- 处理: 已飞书告警老板，等待恢复项目或更新 /root/.openclaw/credentials/supabase.json 的 url/ref
- 下次: 再遇到类似域名解析失败，先用 DoH 复核并对比同域根域解析结果，再判定是本地网络还是项目失效

## 2026-10-22 15:02 - sync-supabase-30m 失败（Supabase 项目域名 NXDOMAIN，持续故障）
- 现象: sync_supabase.py exit 1，报 `DNS 解析失败: njxjuvxosvwvluxefrzg.supabase.co -> [Errno -2] Name or service not known`，0 个模块同步
- 排查: 本机 DNS(183.60.83.19) / 8.8.8.8 / 1.1.1.1 对该域名均 NXDOMAIN；supabase.co 与 api.supabase.com 解析正常 → 非本机网络问题，Supabase 项目已被删除/暂停回收
- 兜底排查: 全盘只有 njxjuvxosvwvluxefrzg 一个 project ref（workspace/credentials 共 30 处引用），无新 ref 可切换；无 supabase CLI / access token 可用，无法自行恢复
- 处理: 已发飞书 failure alert 给老板（ou_aeb3984fc66ae7c78e396255f7c7a11b），要求在 Supabase Dashboard 恢复项目或更新 /root/.openclaw/credentials/supabase.json
- 遗留: cron run 的 lastStatus 仍为 ok（agent turn 正常返回），failureAlert(after=3) 不会触发，告警只能手动发

## 2026-10-22 18:02 - sync-supabase-30m 失败（Supabase 项目域名持续 NXDOMAIN）
- 现象: sync_supabase.py exit 1，`DNS 解析失败: njxjuvxosvwvluxefrzg.supabase.co -> [Errno -2] Name or service not known`，0 模块同步
- 排查: dig @8.8.8.8 / @1.1.1.1 均 NXDOMAIN；supabase.com、api.supabase.com 解析正常 → 项目域名公网已不存在（非本机网络）
- 历史确认: cron run 历史显示自 2026-09 起持续同一错误，累计 30+ 轮；lastStatus 仍为 ok，故 failureAlert(after=3) 永不触发，只能手动飞书告警
- 处理: 已发飞书 failure alert（om_x100b62aa6b2d00acce9e370d11eafa7），并建议修复前暂停该 30 分钟任务
- 下次做法: 该类"项目域名消失"故障无需每轮重复全网排查，确认无新 ref 后直接告警 + 建议暂停任务，等待老板提供新凭证

## 2026-10-23 — sync_supabase.py: Supabase 项目域名 NXDOMAIN
- 现象：脚本以 DNS 解析失败退出（code 1），`njxjuvxosvwvluxefrzg.supabase.co` 解析不到。
- 排查：supabase.com / api.supabase.com 均可解析连通，排除本机网络问题；全工作区仅此一个项目 ref（credentials + 4 处源码硬编码）。
- 结论：Supabase 项目被删除或项目 ref 失效，需人工在 Dashboard 恢复或换新 URL/key。
- 后续：换 URL 时需同步修改 credentials/supabase.json 与 second-brain 内 4 处硬编码 URL。
- 2026-10-25 复发：cron sync-supabase-30m 再次以同一原因失败；已向老板发 failure alert（需在 Dashboard 恢复项目或更新凭据）。若短期无法恢复应暂停该 cron，避免每 30 分钟刷屏。

## 2026-10-25 — sync-supabase-30m DNS 解析失败
- 现象：脚本 `scripts/supabase/sync_supabase.py` 退出码 1，`nxxjuvxosvwvluxefrzg.supabase.co` DNS 无法解析。
- 原因：Supabase 项目域名公网不存在（项目删除/回收），需在 Dashboard 恢复或更新 credentials/supabase.json。
- 改进：下次遇到同类失败先检查 credentials 文件中的 project_url 是否仍有效，再重试；失败后按任务要求发 failure alert。

## 2026-10-26 11:11 — sync-supabase-30m DNS 解析失败（第 N 次复发）
- 现象：脚本真实执行 exit 1，`njxjuvxosvwvluxefrzg.supabase.co` → Errno -2
- 复核：Google DoH + Cloudflare DoH 均 Status=3 (NXDOMAIN)，权威 NS 为 cloudflare；supabase.co / api.supabase.com 正常 → 项目域名公网不存在，非本机网络
- 处理：已发飞书 failure alert（om_x100b62d81c7630bcb321deaa8c8683d）
- 改进建议（长期）：该 cron 自 2026-09 起 2000+ 轮同因失败，failureAlert(after=3) 因 run status=ok 永不触发，只能手动发；建议在脚本层检测 NXDOMAIN 后自动 disable 该 job，或修复 Supabase 域名

## 2026-10-27 — sync-supabase-30m: Supabase 项目域名 NXDOMAIN
- 发生了什么：运行 second-brain/scripts/supabase/sync_supabase.py 失败，DNS 解析 njxjuvxosvwvluxefrzg.supabase.co 报 NXDOMAIN（Cloudflare/Google DNS 复核均无记录；supabase.co 本身可解析）。
- 怎么处理：不要反复重试该脚本消耗资源；先确认项目是否被删除/暂停回收，恢复项目或更新 /root/.openclaw/credentials/supabase.json 中的 url（同时注意 src/app/api/rd-memos/route.ts、src/app/page.tsx 有硬编码 URL）。飞书告警直接用 message 工具发到 user:ou_aeb...（feishu-lark-agent CLI 因缺少 FEISHU_APP_ID/SECRET 不可用）。

## 2026-10-27 09:08 — sync-supabase-30m DNS 解析失败（持续复发）
- 现象：脚本真实执行 exit 1，`njxjuvxosvwvluxefrzg.supabase.co` → Errno -2
- 复核：dig @8.8.8.8 / @1.1.1.1 均无 A 记录；supabase.co、api.supabase.com 解析与连通正常（api 404 属预期）→ 项目域名公网已不存在
- 处理：已发飞书 failure alert（om_x100b62cbd20acca0ce3408cb90bc12f）给老板，请其恢复项目或更新凭据，并建议恢复前暂停此 cron
- 注意：cron failureAlert(after=3) 不生效——job 的 lastRunStatus 恒为 ok（agent 正常返回），只能手动发 message 告警

## 2026-10-27 — Supabase 同步 DNS NXDOMAIN
- 现象：`sync_supabase.py` 报 `DNS 解析失败: njxjuvxosvwvluxefrzg.supabase.co -> Name or service not known`，退出码 1。
- 排查：本机 nslookup + Google DNS + Cloudflare DNS 三重确认权威 NXDOMAIN；同域 `supabase.co` 正常解析 → 非本机网络/脚本 bug，Supabase 项目已被删除或暂停回收。
- 处理：已通过飞书向老板发出 failure alert（项目 ref 恢复或更新 `/root/.openclaw/credentials/supabase.json` 即可自愈）；cron 保持 30 分钟重试。
- 下次做类似排查：先用权威公共 DNS 复核 NXDOMAIN，再判定是凭据问题还是平台侧资源消失。

## 2026-10-27 — sync_supabase.py fails: NXDOMAIN for project domain
- What: `python3 .../scripts/supabase/sync_supabase.py` exits 1; `njxjuvxosvwvluxefrzg.supabase.co` returns NXDOMAIN.
- Verified: host DNS is healthy (supabase.com / api.supabase.com resolve OK) → not a local network problem; the Supabase project itself no longer exists (deleted/paused/reclaimed).
- Do differently: don't re-investigate the network next run — check the project domain only, and escalate the credential/url fix to the owner instead of retrying blindly.

## 2026-10-29 sync-supabase-30m 失败
- 现象：sync_supabase.py 退出码 1，DNS 解析失败 njxjuvxosvwvluxefrzg.supabase.co
- 诊断：外网 DNS 正常，仅该 Supabase 项目域名无解析记录 → 项目疑似被删除/暂停回收
- 处理：已通过飞书向老板发送 failure alert（om_x100b611ab54af8b4cfe32856e671fcd）；需老板在 Dashboard 恢复项目或更新 /root/.openclaw/credentials/supabase.json
- 注意：failureAlert 配置的 open_id 是 ou_aeb3984fc66ae7c78e396255f7c7a11b，转发时务必逐字符核对（曾因错抄成 c7c8 导致 400）

## 2026-10-30 05:31 — sync_supabase.py NXDOMAIN（复发，同因）
- 现象：退出码 1，DNS 解析失败 njxjuvxosvwvluxefrzg.supabase.co -> [Errno -2]
- 复核：getent hosts 确认该子域 NXDOMAIN，supabase.co 正常 → 仍为 Supabase 项目侧域名失效，非本机网络
- 处理：已发飞书 failure alert（om_x100b610ff9dc78a0b4a14cd24e039db）给老板；需恢复项目或更新 credentials/supabase.json
- 长期：建议脚本检测 NXDOMAIN 后自动 disable 本 cron，避免每 30 分钟无效重试 + 重复告警

## 2026-10-30 10:02 — sync_supabase.py NXDOMAIN（复发，同因）
- 现象：退出码 1，`DNS 解析失败: njxjuvxosvwvluxefrzg.supabase.co -> [Errno -2] Name or service not known`
- 复核：8.8.8.8 与 1.1.1.1 均 NXDOMAIN，supabase.co 主域解析正常 → Supabase 项目侧域名失效，非本机网络/脚本问题
- 处理：已发飞书 failure alert（om_x100b610be37eaca4cebf613e8eae9de）给老板，需恢复项目或更新 /root/.openclaw/credentials/supabase.json
- 累计：09-14 起同一原因复发 30+ 次，脚本级 NXDOMAIN 自愈不可能；建议脚本检测到 NXDOMAIN 时自动 disable 本 cron 或改为每日一次，避免每 30 分钟重复告警
