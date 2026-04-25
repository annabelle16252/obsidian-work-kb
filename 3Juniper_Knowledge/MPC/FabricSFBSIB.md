# Fabric/SFB/SIB

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

Fabric/SFB/SIB
SFB
用在MX2020、老的 T 系列等机型
提供交换矩阵 (Switch Fabric) 功能，把流量从 ingress FPC 转发到 egress FPC；
SIB
用于 PTX、QFX10000 等基于 QFabric/Express/Sib 架构的设备
Line card (FPC/MPC) ↔ Fabric 芯片 的接口，和中央的 fabric 芯片/板卡配合，组成完整的交换矩阵
# Fabric degradation #
user@root> show chassis fabric degradation                 Reqd/Curr  Configured          Current     Time LastFPC    State     Planes     Degrad(%),action    Degrad(%)   Action Initiated0      Online    21/15      n/a,none            281      Online    21/15      n/a,none            282      Online    21/15      n/a,none            283      Online    21/15      n/a,none            284      Empty
user@host> show chassis fabric degraded-fabric-reachabilityDegraded Fabric reachability Information:FPC #0    PFE #0      SIB0_Plane 0           Link errors  FPC/PFEs    2/0 5/0 5/1 5/2 5/3      SIB0_Plane 1           Link errors  FPC/PFEs    2/0 5/0    PFE #1      SIB0_Plane 0           Link errors  FPC/PFEs    2/0 5/0 5/1 5/2 5/3      SIB0_Plane 1           Link errors  FPC/PFEs    2/0 5/0
degraded {    action-fpc-restart-disable;     degraded-fabric-detection-enable;     degraded-fpc-bad-plane-threshold number-of-bad-planes;}
When a Packet Forwarding Engine is affected by a degraded fabric condition, the software disables the interfaces associated with that Packet Forwarding Engine.
# Fabric Healing #
fabric在degradation后，如果条件恢复，...FH会自动尝试修复/重建路径...。
自 动 fabric healing 会 触 发 的 场 景 主 要 有 : 
● 模 块 恢 复 (SFB/SIB/FPC fabric link) 
● 链 路 错 误 消 失 (CRC/Parity error 消 除 ) 
● 环 境 恢 复 ( 电 源 / 温 度 正 常 后 FRU 自 动 回 归 ) 
● 进 程 自 愈 (chassisd/fabricd 重 新 bring-up fabric plane) 
√ 换 句 话 说 : 只 要 之 前 导 致 fabric degradation 的 条 件 消 除 了 , 系 统 会 自 动 触 发 fabric 
healing, 让 路 由 器 回 到 full fabric capacity 。 ...自 动 fabric healing 会 触 发 的 场 景 主 要 有 : 
● 模 块 恢 复 (SFB/SIB/FPC fabric link) 
● 链 路 错 误 消 失 (CRC/Parity error 消 除 ) 
● 环 境 恢 复 ( 电 源 / 温 度 正 常 后 FRU 自 动 回 归 ) 
● 进 程 自 愈 (chassisd/fabricd 重 新 bring-up fabric plane) 
√ 换 句 话 说 : 只 要 之 前 导 致 fabric degradation 的 条 件 消 除 了 , 系 统 会 自 动 触 发 fabric 
healing, 让 路 由 器 回 到 full fabric capacity 。
default FH只有在最后一块fabric plane也offline后才会被trigger来检测之前degrade的plane是否恢复...，这样的话会产生流量黑洞。使用以下命令可以提前进行FH：
labroot@router-re0# show chassis fabricdegraded {    action-on-non-blackhole-degradation 20;   <<<<< Here we used 20%, but you can use any required value.}
labroot@router-re0# run show chassis fabric reachability
Fabric reachability status: Fabric degradation detected, action in progress        Detected on                         : 2021-04-08 20:58:07 PDT        Reason                              : Fabric Degradation due to grant timeouts seen by DPCs, MPCs, or FPCs
Fabric reachability action:    Fabric reachability action              : Plane action    Current phase                           : Plane Restart Phase is in progress    Action started                          : 2021-04-08 20:58:17 PDT
Apr  8 20:58:07.393  router-re0 chassisd[7277]: CHASSISD_FM_FABRIC_DEGRADED: DPCs are seeing grant timeouts; System is blackholing   Need to attempt fabric healing.  Action will be taken after 10 seconds, to address the fabric down condition. Apr  8 20:58:17.402  router-re0 chassisd[7277]: CHASSISD_FM_ACTION_FPC_OFFLINE: FPC 0 offline initiated  to attempt healing of the fabric down condition.
# RDR Error #
When a Switch Fabric Board (SFB) or fabric plane is in transition (offlined/onlined/check), leading to the loss of in-flight fabric cells. When such cells are lost, the receiving PFE cannot reassemble the full packet at the fabric input interface, resulting in these RDR error logs.
Another scenario is when there is a complete PFE oversubscription in such cases, RDR errors may also be seen.
<<<
Aug 13 08:38:09.914  iad4-j2-vpn7 fpc0 MQSS(3): RDR: Packet errors detected in PSV block - Stream 0, Count 4
Aug 13 08:38:10.092  iad4-j2-vpn7 fpc0 MQSS(2): RDR: Packet errors detected in PSV block - Stream 1, Count 65
<<<
如果SIB error都指向一个FPC，那reseat FPC. 如果发生在不同FPC上，找到问题...SIB reseat
# ...Minor FPC 0 SIB Link Error... #
Get the RSI, var/log and these commands:
show chassis fabric errors fpc <slot>FPC slot number (0..7) ----> for all FPCs please
show chassis fabric errors sib <sib-slot> SIB slot number (0..5) ----> for all SIBs please
show chassis fabric fpcs
show chassis fabric plane-location
show chassis fabric reachability detail
show chassis fabric sibs
show chassis fabric summary
show chassis fabric topology
show chassis environment sib
show chassis sibs detail
show chassis sibs errors
Feb 29 05:21:24.668 re0 fpc0 cmqfx: Link errors detected for PFE 4 SIB 0 Plane 1
Feb 29 05:21:25.090 re0 chassisd[22469]: CHASSISD_FM_ERROR: fm_qfx10_ev_fpc_state_handler: Packet Forwarding Engine got link errors (SIB#0, Packet Forwarding Engine 4 on FPC 0)
# ...SIB 2 FPC Link Error... #
QFX10008> show chassis alarms
7 alarms currently active
Alarm time Class Description
2024-05-10 17:32:03 UTC Minor SIB 2 FPC Link Error
2024-05-10 17:31:44 UTC Minor FPC 0 SIB Link Error
2024-05-10 17:31:43 UTC Minor FPC 3 SIB Link Error
2024-05-10 17:31:42 UTC Minor FPC 1 SIB Link Error
2024-05-10 17:31:41 UTC Minor FPC 4 SIB Link Error
2024-05-10 17:31:40 UTC Minor FPC 5 SIB Link Error
2024-05-10 17:31:39 UTC Minor FPC 2 SIB Link Error
