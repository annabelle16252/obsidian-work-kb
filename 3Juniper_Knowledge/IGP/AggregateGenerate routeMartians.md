# Aggregate/Generate route/Martians

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

Aggregate/Generate route/Martians
labroot@mxvc-2026-2027# run show route 211.211/16
inet.0: 869753 destinations, 869961 routes (869636 active, 0 holddown, 119 hidden)
+ = Active Route, - = Last Active, * = Both
211.211.0.0/16 *[Aggregate/130] 00:00:06
Discard
211.211.211.1/32 *[Static/5] 00:00:06
Discard
set routing-options aggregate route 211.211.0.0/16 discard
aggregate存在于rib需要1.掩码短于明细 2.至少有一条明细存在
==================================
如果把aggregate换成generate route，100.100.100/24就不能当pnh来用
30/8的pnh变成0/0
==================================
Martian addresses are host or network addresses about which all routing information is ignored. When received by the routing device, these routes are ignored. They commonly are sent by improperly configured systems on the network and have destination addresses that are obviously invalid.
{master}[edit]
ipcore@imse22.pujlab# show | compare rollback 0
[edit routing-options martians]
192.0.0.1/32 exact { ... }
+ 0.0.0.0/0 exact allow;
==================================
