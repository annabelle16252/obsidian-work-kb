# DDOS

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

DDOS
# Commands #
> show ddos-protection protocols snmp statistics Packet types: 1, Received traffic: 1, Currently violated: 1Protocol Group: SNMP
Packet type: aggregate    System-wide information:      Aggregate bandwidth is being violated!        No. of FPCs currently receiving excess traffic: 0        No. of FPCs that have received excess traffic:  1        Violation first detected at: 2025-08-29 13:58:02 IST        Violation last seen at:      2025-08-29 17:34:24 IST        Duration of violation: 03:36:22 Number of violations: 2      Received:  64606784            Arrival rate:     4999 pps      Dropped:   21664               Max arrival rate: 5005 pps
PFEから：
NGMPC0(lab-mx480-3d-01-PE9 vty)# show ddos policer snmp configuration
DDOS Policer Configuration:
UKERN-Config PFE-Config
idx prot group proto on Pr Q bwidth burst bwidth burst
--- ---- ------------ --------------- -- -- -- ------ ----- ------ -----
98 1900 snmp aggregate Y Lo 0 10000 100 10000 100
NGMPC0(lab-mx480-3d-01-PE9 vty)# show ddos policer snmp stats
DDOS Policer Statistics:
arrival pass # of
idx gid prot group proto pol scfd loc pass drop rate rate flows
--- ------ ------ ----------- ----------- --- ----- ------- -------- -------- ------ ------ -----
98 0x0019 0x1900 snmp aggregate on normal UKERN 386953 0 5005 5005 0
PFE-0:0 386953 0 5005 5005 0
huasu@lab-mx480-3d-01-PE9> show ddos-protection protocols snmp
Packet types: 1, Modified: 1, Received traffic: 1, Currently violated: 1
Currently tracked flows: 0, Total detected flows: 0
* = User configured value
# Reject:aggregate #
Jan 28 02:31:00  xnl-hkgcw1-bo103 jddosd[25708]: %DAEMON-4-DDOS_PROTOCOL_VIOLATION_SET: Warning: Host-bound traffic for protocol/exception  Reject:aggregate exceeded its allowed bandwidth at fpc 0 for 8320 times, started at 2026-01-28 02:30:59 +08
show ddos-protection protocols
show ddos-protection violations fpc 0
show ddos-protection statistics fpc 0
> show ddos-protection protocols reject statistics
Reject: aggregate = 触 发 “ 拒 绝 动 作 ” 或 “ 需 要 
Routing Engine 生 成 ICMP 错 误 ” 的 所 有 异 常 流 量 的 总 
和 。 
包 括 但 不 限 于 : 
● 防 火 墙 reject/deny 的 包 
● 需 要 返 回 ICMP 错 误 的 包 ( 如 PMTUD) 
● TTL 过 期 
● 无 路 由 
● 异 常 头 部 
· malformed packet 
● punt-to-RE 的 各 种 异 常 ...Reject: aggregate = 触 发 “ 拒 绝 动 作 ” 或 “ 需 要 
Routing Engine 生 成 ICMP 错 误 ” 的 所 有 异 常 流 量 的 总 
和 。 
包 括 但 不 限 于 : 
● 防 火 墙 reject/deny 的 包 
● 需 要 返 回 ICMP 错 误 的 包 ( 如 PMTUD) 
● TTL 过 期 
● 无 路 由 
● 异 常 头 部 
· malformed packet 
● punt-to-RE 的 各 种 异 常
