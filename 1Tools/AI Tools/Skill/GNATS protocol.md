
Prompt:
Please use gnats to search pr. 
Keyword: MPC3E & drop-profile. 
Version: 
Issue: BGP flap


gnats is a remote Juniper service running on  gnats.juniper.net:1529
macOS ──(TCP/IP)──> gnats.juniper.net:1529

copilot已经把如何用它查询pr写进内存，以后会自动连接到 gnats.juniper.net:1529 执行查询





(CVE-2026-35386)                            2026-0417-678815 
(CVE-2026-35385) junos not affected
(CVE-2026-35387)junos not affected
(CVE-2026-35388)junos not affected
(CVE-2026-35414)
 
你问的两部分（Description / Audit-Trail）都支持用 log、长句、段落形式做搜索。
实战建议不要直接整段原样搜，优先拆成 2-4 个“稳定特征词”做 AND 组合，命中和速度更好。
推荐写法（最稳）：

Description~"关键词1" & Description~"关键词2"
Audit-Trail~"关键词1" & Audit-Trail~"关键词2"
如果要带整句，建议再加 1-2 个核心词兜底，避免只靠整句精确片段。