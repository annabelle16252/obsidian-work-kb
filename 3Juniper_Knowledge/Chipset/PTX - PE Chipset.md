# PTX - PE Chipset

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

PTX - PE Chipset
FULL PTX PORTFOLIO (2020-2023) 
100G/400G/800G GENERATION 
NG Triton, BX; 
Fixed 
Modular 
Line Cards 
800G Gen 
with EVO. 
Triton, BT 
Triton, BT 
Coming in 
2022/23 
Express+, ZX 
Triton, BT 
PTX10003 
PTX10001- 
36MR 
PTX NG FIXED 
PTX10004 
PTX10008 
PTX10016 
LC1201- 
LC1202- 
NG Line 
36CD 
36MR 
Card 
3RU, Fixed 
1RU, Fixed 
Single ASIC 
7RU, 4-slots 
13RU, 8-slots 
21RU, 16-slots 
36 × 400GE 
4 × 400G + 32 x 
100G 
Interoperable 
8T & 16T 
9.6T 
28.8T 
57.6T 
115.2T 
230.4T 
14.4T 
4.8T 
28.8T 
16-32 × 400 GE 
24 x 400 GE 
100/400/800GE 
144 × 400GE 
288 × 400GE 
576 × 400GE 
36 × 400GE 
4 × 400GE 
400/800GE 
80-160 × 100GE 
108 × 100GE 
Interfaces 
576 × 100GE 
1152 × 100GE 
Ships Q32021 
144 x 100GE 
48 x 100GE 
Interfaces 
HIGH PACKET PERFORMANCE 
COUNTERS AND STATISTICS 
SECURE CONNECTIVITY 
% rate 
V 
8M 
VISIBILITY COUNTERS 
400GE 
Byte 
50% MORE 
DENSITY 
NO TRADE OFF 
MACSEC ...FULL PTX PORTFOLIO (2020-2023) 
100G/400G/800G GENERATION 
NG Triton, BX; 
Fixed 
Modular 
Line Cards 
800G Gen 
with EVO. 
Triton, BT 
Triton, BT 
Coming in 
2022/23 
Express+, ZX 
Triton, BT 
PTX10003 
PTX10001- 
36MR 
PTX NG FIXED 
PTX10004 
PTX10008 
PTX10016 
LC1201- 
LC1202- 
NG Line 
36CD 
36MR 
Card 
3RU, Fixed 
1RU, Fixed 
Single ASIC 
7RU, 4-slots 
13RU, 8-slots 
21RU, 16-slots 
36 × 400GE 
4 × 400G + 32 x 
100G 
Interoperable 
8T & 16T 
9.6T 
28.8T 
57.6T 
115.2T 
230.4T 
14.4T 
4.8T 
28.8T 
16-32 × 400 GE 
24 x 400 GE 
100/400/800GE 
144 × 400GE 
288 × 400GE 
576 × 400GE 
36 × 400GE 
4 × 400GE 
400/800GE 
80-160 × 100GE 
108 × 100GE 
Interfaces 
576 × 100GE 
1152 × 100GE 
Ships Q32021 
144 x 100GE 
48 x 100GE 
Interfaces 
HIGH PACKET PERFORMANCE 
COUNTERS AND STATISTICS 
SECURE CONNECTIVITY 
% rate 
V 
8M 
VISIBILITY COUNTERS 
400GE 
Byte 
50% MORE 
DENSITY 
NO TRADE OFF 
MACSEC
PTX 专注于核心IP传输和大规模数据转发，设计用于运营商骨干网、超大规模数据中心互联（DCI）及云服务提供商的流量汇聚场景。核心路由器包括PTX,MX304,MX10008等
MX作为通用边缘路由器，适用于运营商边缘（如5G回传）、企业广域网（WAN）及多业务聚合场景。边缘路由器包括MX legcy...，...ACX等
Express 2 (PE)、Express 3 (ZX)， Express 4 (BT)
# Commands #
# PTX FPC #
PTX FPC = Legacy FPC + PIC
传统路由器：
FPC：负责数据包转发（Packet Forwarding Engine, PFE）。
Line Card：提供物理接口（如100G/400G端口），需通过背板与FPC连接
与MX不同，PTX接口直接集成在fpc上，而非使用独立的FPC+PIC架构所以PIC的位置永远是0
PTX的线卡（FPC）直接内置物理接口（如100G/400G端口），不再需要外接PIC模块。
若接口或转发引擎故障，需更换整个FPC（线卡），而非单独PIC
所以出问题，不是QSFP就是FPC坏了
# RCB #
RCB是一个整体...，支持热插拔，如果...RE或CB坏了...，...都...需要更换整个RCB。
User@ptx10008-re0> request node ?Possible completions:  halt                 Halt the node  offline              Offline the node  online               Online the node  power-off            Power-off the node  power-on             Power-on the node  reboot               Reboot the node
# SIB #
与MX960这些platform的fabric在SCB上不同，PTX的fabric是单独的SIB。
# PE Chip #
PTX...10K PFE
> show chassis fpc errors
> request pfe execute command "show pechip 0" target fpc2
> request pfe execute command "show pechip 1" target fpc2
> request pfe execute command "show pechip 2" target fpc2
> request pfe execute command "show pechip 3" target fpc2
> request pfe execute command "show show cmerror module brief" target fpc2
KB75826... ...PTX10008 LC1101 PECHIP_CMERROR_RT_CORRECT0_FSET_REG_CORRECTED_LMEM (0x2100ba)
# fabric degradation #
> show chassis fabric degradation
> show chassis fabric reachability
labroot@router-re0# run show chassis fabric degradation 
Reqd/Curr Configured 
Current 
Time Last 
FPC 
State 
Planes 
Degrad(%) , action 
Degrad(%) 
Action Initiated 
0 
Online 
21/15 
n/a, none 
28 
1 
Online 
21/15 
n/a, none 
28 
2 
Online 
21/15 
n/a, none 
28 
3 
Online 
21/15 
n/a, none 
28 
4 
Empty ...labroot@router-re0# run show chassis fabric degradation 
Reqd/Curr Configured 
Current 
Time Last 
FPC 
State 
Planes 
Degrad(%) , action 
Degrad(%) 
Action Initiated 
0 
Online 
21/15 
n/a, none 
28 
1 
Online 
21/15 
n/a, none 
28 
2 
Online 
21/15 
n/a, none 
28 
3 
Online 
21/15 
n/a, none 
28 
4 
Empty
labroot@router-re0# run show chassis fabric reachability 
Fabric reachability status: Fabric degradation detected, action in progress 
Detected on 
: 2021-04-08 20:58:07 PDT 
Reason 
: Fabric Degradation due to grant timeouts seen by 
Fabric reachability action: 
Fabric reachability action 
: Plane action 
Current phase 
: Plane Restart Phase is in progress 
Action started 
: 2021-04-08 20:58:17 PDT ...labroot@router-re0# run show chassis fabric reachability 
Fabric reachability status: Fabric degradation detected, action in progress 
Detected on 
: 2021-04-08 20:58:07 PDT 
Reason 
: Fabric Degradation due to grant timeouts seen by 
Fabric reachability action: 
Fabric reachability action 
: Plane action 
Current phase 
: Plane Restart Phase is in progress 
Action started 
: 2021-04-08 20:58:17 PDT
KB33743... [MX] Preventing chassis fabric degradation by triggering early fabric healing
<<<<
labroot@router-re0# show chassis fabricdegraded {    action-on-non-blackhole-degradation 20;   <<<<< Here we used 20%, but you can use any required value.}
<<<<
为避免fabric blackhole，设置degration percentage，提前trigger fabric healing。不配置默认到最后一块fabric plane offline了才开始healing
[edit chassis fabric event reachability-fault degraded error-threshold percentage]
