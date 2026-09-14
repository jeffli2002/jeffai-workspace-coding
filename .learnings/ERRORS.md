
## 2026-09-14 sync-supabase NXDOMAIN
- sync_supabase.py 三个模块全部失败：[Errno -2] Name or service not known
- 诊断：njxjuvxosvwvluxefrzg.supabase.co 在 8.8.8.8 和本机 DNS 均 NXDOMAIN；本机网络/其他域名解析正常
- 结论：Supabase 项目已被删除或重建，需更新 /root/.openclaw/credentials/supabase.json

## 2026-09-14 — Supabase 同步失败：项目域名 NXDOMAIN
- **现象**: sync_supabase.py 三个模块 (Memories/Documents/Tasks) 全部报 `[Errno -2] Name or service not known`
- **根因**: 配置的 `njxjuvxosvwvluxefrzg.supabase.co` DNS 解析为 NXDOMAIN（域名不存在）；本机网络和 DNS 正常（supabase.com 可访问）
- **结论**: Supabase 项目很可能已被删除（免费版长期暂停后会被清除），非本机故障
- **处置**: 需老板登录 Supabase 控制台确认项目状态；如已删除需重建项目并更新 `/root/.openclaw/credentials/supabase.json`
