# Policer

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

Policer
# shared-bandwidth-policer #
[2025-09-24 11:25:07.529]     policer SHARED_BW-POLICER-500M {
[2025-09-24 11:25:07.529]         shared-bandwidth-policer;
[2025-09-24 11:25:07.529]         if-exceeding {
[2025-09-24 11:25:07.529]             bandwidth-limit 500m;
[2025-09-24 11:25:07.529]             burst-size-limit 31250;
[2025-09-24 11:25:07.529]         }
[2025-09-24 11:25:07.529]         then discard;
https://juniper.lightning.force.com/lightning/r/Knowledge__kav/ka0Dp000000jWz8IAE/view
An example of this would be a policer with bandwidth-limit 40mbps and burst-size 40Kbytes configured on an AE interface that has member links ge-0/0/0 and ge-1/0/0. When the policer is applied to the AE interface, this will result in a total bandwidth of 80Mbps as policer is configured for two PFE's.
When shared BW is configured, each member link will bandwidth configured divided by number of member links. So, if 40Mb configured, if there are two member links, each member link gets 20Mb each.
On ae0 where 10m policer is applied,
labroot@jtac-mx10008-r2010-re0> show interfaces ae0 | match rate | refresh 1
---(refreshed at 2024-12-09 15:44:17 IST)---
Input rate     : 0 bps (0 pps)
Output rate    : 9989480 bps (27144 pps)
---(refreshed at 2024-12-09 15:44:18 IST)---
Input rate     : 0 bps (0 pps)
Output rate    : 10017088 bps (27220 pps)
labroot@jtac-mx10008-r2010-re0> show interfaces xe-1/0/0:0 | match rate | refresh 1
---(refreshed at 2024-12-09 15:45:19 IST)---
Input rate     : 0 bps (0 pps)
Output rate    : 4995360 bps (13574 pps)
labroot@jtac-mx480-r2033-re0# run show firewall | find CF_IPv4_CRM12748_LNHB013_OUT-ae0.0-o
Filter: CF_IPv4_CRM12748_LNHB013_OUT-ae0.0-o
Counters:
Name                                                                            Bytes              Packets
burst_test-ae0.0-o                                                       318225470938            233729519
Policers:
Name                                                                            Bytes              Packets
SHARED_BW-POLICER-500M-1-ae0.0-o        5792010378292           4191035006
<<<policer非0一直增长就说明policer起作用了
