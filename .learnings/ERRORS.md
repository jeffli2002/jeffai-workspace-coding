
## 2026-09-14 sync-supabase NXDOMAIN
- sync_supabase.py 三个模块全部失败：[Errno -2] Name or service not known
- 诊断：njxjuvxosvwvluxefrzg.supabase.co 在 8.8.8.8 和本机 DNS 均 NXDOMAIN；本机网络/其他域名解析正常
- 结论：Supabase 项目已被删除或重建，需更新 /root/.openclaw/credentials/supabase.json
