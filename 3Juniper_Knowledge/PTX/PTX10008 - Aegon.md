
ptx10008+lc1301 <<< NGTV4
# Chassis
![[Pasted image 20260420110203.png]]
![[Pasted image 20260429095606.png]]
# Commands 
==FPC AFT login==
```
start shell pfe network fpc0
@r1-RE0-fpc0:pfe>  
```

==FPC linux login==
```
start shell user root
chvrf iri ssh fpc0 
```

==RE shell==
```
> start shell 
[vrf:none] regress@r1-RE0-re0:~$ 
```

==Kernel sysfs nodes for new FRUs==
```
[vrf:none] regress@r1-RE0-re0:~$ cd /sys/devices/platform/jnx/card

[vrf:none] root@r1-RE0-re0:/sys/devices/platform/jnx/card# cat fpc0/assembly_id 
0x0dce

[vrf:none] root@aegon-platsw-04-re0:/sys/devices/platform/jnx/card/fpc0# cat state
Active

```

01:58


# RCB 
![[Pasted image 20260418123214.png]]
# FPC 
与MX不同，PTX接口直接集成在fpc上，而非使用独立的FPC+PIC架构，所以PIC的位置永远是0.

Express 4 (Scapa) — 上一代线卡（LC1201/LC1202）BT chip
Express 5 (Aegon) — 新一代线卡（LC1301）BX chip

Express5 引入了新的 CCL 协议版本和 cell 编码方式。在 default 模式下，Express5 必须降级兼容 Express4 的格式（比如 fabric 链路速率从 106.25G 降到 53.125G）。而 express5-enhanced 模式下可以用原生的高性能格式，发挥全部带宽和新特性。混用会影响Aegon性能。
![[Pasted image 20260501122149.png]]

```
> show chassis interoperability 
Chassis Interoperability Mode: default

# set chassis interoperability express5-enhanced 
reboot

> show chassis interoperability 
Chassis Interoperability Mode: express5-enhanced

regress@r2-RE0-re0> show chassis fpc detail 

> show chassis fpc detail 
Slot 2 information:
  State                               Offline   
  Reason                              FPC incompatible with interop config
  PFE Type                            Express-4
```


![[Pasted image 20260429101548.png]]
Main board = 线卡主板，承载高速数据面器件与主互连
Mezzanine board = 插在主板上的子板形态，用于扩展特定功能
PMB = 控制面子板（线卡控制“大脑”），其上是 AMD Snowy Owl 8-core CPU + 64GB DDR4（2x32GB）。PMB 负责控制管理，数据面高速转发由 BXF/BF ASIC 完成。



PTX10K-LC1301-36DD	PTX10K 28.8T Line-card, 36x800G/72x400G/288x100G QSFP-DD

LC1301 is BXF based =BX+BF, the BF chipset is used to connect with Fabric -BF chip
![[Pasted image 20260420110450.png]]

BF ASIC on LC1301, its function mainly fabric-link signal retiming/bridging, not main forwarding.
LC1301 BF = fabric-interface retimer/bridge.
SF5 BF = fabric switching ASIC role.

What a retimer does:
Receives a degraded electrical signal from one side.
Recovers clean timing (clock/data recovery).
Re-equalizes and cleans jitter/noise.
Transmits a fresh, clean signal out the other side.
So it is not forwarding packets at Layer 2/3, and it is not making routing decisions. It is a physical-layer helper.

# Fabric
6 X SIB8 (JNP10008-SF5)
SF5 SIB 板卡 (一块物理PCB)
├── 3x BF ASIC ─── 做实际的 fabric 交换（数据包跨 LC 转发）
├── 6x MT3775 Retimer ─── 做信号中继（解决信号完整性问题）
├── 1x FPGA ─── 板级管理（电源时序、复位、中断）
└── 1x PCIe Switch ─── 控制通路连接 RE
![[Pasted image 20260429103702.png]]
Granular failure handling：
Failure of one BF ASIC does not impact the entire SIB board – the failed BF ASIC can be taken out of fabric map, ==leaving the other two BFs on the SIB fully functional==. This feature reduces the switching fabric bandwidth by ONE BF ASIC instead of one SIB (SW support in future Evo release)

**SF5 New Feature: Selective Power shutdown**
在上一代 fabric （如 SF3）中，任何一个板上组件故障都会导致整块 SIB 下线，直接造成 16.6% 的全系统交换容量损失（因为 PTX10008 有 6 块 SIB，1/6 ≈ 16.6%）。这个粒度太粗了。

![[Pasted image 20260501104944.png]]
当某个功能组内发生 电源故障、过温、或其他硬件故障 时，只关闭该功能组，其余 4 个组继续正常工作。故障可以等到维护窗口再处理。

带宽损失影响表
![[Pasted image 20260501110116.png]]

==Mixed SIB types are not supported==
With Aegon SIB, PTX10008 chassis supports 2 different Fabric ASIC based SIBs – SF3 and SF5. Fabric configuration differs depending on SIB type.

系统用 System-SIB-Type 决定最终 fabric 配置，这个类型的判定规则是：
看“已插入的最低编号 SIB 槽位”里是什么类型的 SIB。
例如：
如果 slot 0 插的是 Scapa SIB，那么系统就按 Scapa/SF3 fabric 方式起来
如果 slot 0 没有卡，而 slot 1 是 Aegon SIB，那系统就按 Aegon/SF5 fabric 方式起来

Incompatible SIB will remain in offline state with below offline reason：
```
> show chassis sibs detail 
Slot 5 information:
  State                                 Offline   
  Reason                              Incompatible with other SIBs

```

==Recommended steps to be followed in a maintenance window for SIB upgrade==
Power down the system.
Remove the Fan trays
Replace all the SIBs.
Plug back the Fan trays.
Power up the system.

==DP - data path==
每个 BX ASIC 里有两个 DP，也就是 DP0 和 DP1,每个 DP 支撑大约 7.2T 的 WAN 带宽
为了支撑这 7.2T，它会对应使用一组 fabric links 往 fabric 侧喷流量:
72 private links
18 shared links
shared link 会被两个 DP 共享
```
LC1301
  |- BXF0
  |   |- BX0
  |       |- DP0
  |       |- DP1
  |
  |- BXF1
      |- BX1
          |- DP0
          |- DP1
```
每个 DP 72 private links & 18 shared links /2= 81 links
其中shared links一半是自己DP,另一半是给另一个 DP 用


==fabric plane==
不在 FPC或SIB 上，它是整个 chassis fabric 的逻辑平面，跨的是 FPC 侧 + SIB/fabric 侧 这整条通路，是把一组这样的 links 逻辑归成一个 plane：
DP 在 FPC 上，位于 BX ASIC 内部
BF ASIC 在 SIB/fabric card 上
fabric link 是 FPC 上的 DP 连到 SIB 上的 BF 的链路

Aegon has 18-plane system, each plane will have 2 links from Scapa PFE and 4.5 links from BX PFE. The 0.5 is possible because each shared link can carry 50% traffic from itself and 50% from sibling PFE



# Software Architecture
Aegon 复用 Scapa 的 EVO uLC 架构，把新的 Aegon SIB 和 LC1301 接进来。
Master RE 上的 EVO 软件负责系统 bring-up 和管理。SIB/FPC 各自有本地进程负责 ASIC、retimer、fabric link 和端口管理；同时延续 GRES/应用 HA 的高可用能力。
![[Pasted image 20260503095838.png]]

==Salient RE Applications==
hwdre 管 FRU 和平台框架
fabricHub 做全局 fabric 编排
fabspoked-fchip 在每张 SIB 上具体把 fabric 口和 BF/retimer 管起来。

==Salient FPC Applications==
hwdfpc:
它是 FPC 侧的平台管理进程。
职责包括：
配合 hwdre 完成 FPC online,创建 logical PIC,监控 FPC 本地 sensors,处理相关 config/CLI
在 Aegon 上，FPC local sensors 由 hwdfpc 管；而在 Scapa 上，这部分更多是 hwdre 管。

fabspoked-pfe
这是 FPC 侧 fabric 管理的核心进程。
职责包括：
管 BX 芯片里的 fabric blocks：fi、fo、fabrord,管理 BXF 里的 BF 部分和 Mediatek retimer,做 fabric link training / detraining / link fault monitoring,周期性拉统计、counter、interrupt，必要时抛 cmerror
它就是运行在 PFE 角色上的 fabspoked，负责 line card 上 fabric 这一半。
你可以把它理解成：SIB 上有 fabspoked-fchip，FPC 上就有 fabspoked-pfe，两边配合把 fabric 链路拉起来并持续监控。

evo-cda-bx 和 evo-aftmand-bx
这两个是 BX ASIC 的转发面应用。
evo-cda-bx：做 BX ASIC 初始化和编程
evo-aftmand-bx：把 forwarding state 编程到硬件里

packetIO
负责 host path setup 和 packet processing support。
这块 PPT 提得不多，知道它是主机通路/报文上送相关即可。

picd
这是端口管理核心进程，职责很集中：
发布 port state DDS 对象
管 transceiver / optics
管 port LED
管 WAN side link training
管 MAC / PCS / Benes / WANIO programming
管 IFD 创建/删除和链路状态
如果现场有人问：
“端口起不来是谁管？”
“光模块是谁认？”
“IFD 谁创建？”
大概率都先想到 picd。

marvd / marvell-cpss-app / clockd
marvd / marvell-cpss-app：line card 上的 Marvell switch 初始化与端口状态轮询
clockd：PLL/时钟初始化与监控，给 ASIC、retimer 等组件提供时钟
这几个属于“底座型配套进程”，通常在平台 bring-up 或硬件异常排查时会碰到。

==HA==
Aegon 继承 modular 系统的 GRES/HA 思路.
RE 上参与 HA 的关键应用是 fabspoked-fchipX 和 fabricHub. PPT 里说这两个支持 hitless application restart。

SIB 上的 PCI endpoint 需要在 master RE 和 backup RE 之间平滑接管. SIB 上的 PCI endpoints 会从 master RE hand over 到 backup RE，并且要做到让下游 BF/SuperCon 尽量感知不到。
PCI endpoint = SIB 板上某个能通过 PCIe 被系统看到、管理、配置的硬件功能点。
SIB 上有一个 PCI switch，它下面挂着多个 endpoint 比如：
一个是 SuperCon SIB FPGA
三个是 BF ASIC
上游连到 master RE / backup RE
当发生 RE switchover 时，这些 endpoint 的控制权要从 master RE 平滑切到 backup RE

Apps can be manually restarted by using CLI command
```
> request system application restart node re0 app <app-name>
 	fabricHub – For FabricHub
	fabspoked-fchip – For  Fchip Fabspoke
	fabspoked-pfe - For  Pfe Fabspoke

```

# BXF
```
> show chassis fpc 0 pfe-instance all    
FPC 0
PFE-Instance    PFE          PFE-State
0               0            ONLINE               
0               1            ONLINE               
1               2            ONLINE               
1               3            ONLINE               
```
![[Pasted image 20260421093859.png|701]]

Each slice is one datapath:
![[Pasted image 20260421094308.png]]
BX Memory:
```
BX ASIC
│
├── 1. Central Buffer SRAM (154MB 片内)
│      → 存包体本身（正常转发路径的缓冲队列）
│      → Ingress DBB write buffer + Egress OQ
│
├── 2. HBM (16GB 片外，封装内)
│      → 也是存包体（突发溢出缓冲）
│      → External DBB，拥塞时 SRAM 装不下才用
│
└── 3. Shared Memory Table
       → 存转发表，不存包！
       → FIB（路由/转发表）
       → FIB Cache
       → IRP/EPP lookup tables（策略查找表）
       → 两个 PP complex 共享这张表
       
```

普通 DDR 的总线宽度是 64-bit，而HBM 每个 stack 是 1024-bit，所以带宽是 DDR 的十几倍——这才跟得上 14.4T 转发速率下的缓冲读写需求。
![[Pasted image 20260421095156.png]]


![[Pasted image 20260421095641.png]]
![[Pasted image 20260421123401.png]]
Datapath（IG）：负责收包、缓存整个包体（存 IBUF）、管理 buffer
PP Complex：负责查表逻辑（FIB/LPM/ACL），运算密集，面积大

BX die 内部：
  PG（MAC/SERDES）
  + IG（ingress buffer）
  + PP Complex（查表/转发逻辑）  ← Trio 的 LU
  + CBUF + VOQ/CM              ← Trio 的 MQ
  + Fabric Interface（CCL）    ← Trio 的 XL
全在一颗 die 上

LU chip（查表） + MQ chip（队列管理）+ XL chip（fabric）
= 多颗独立芯片焊在一起，通过芯片间总线通信
每个芯片专职一件事，芯片间通信开销大，但每颗芯片可以独立优化。

正常情况（不拥塞）包存在片上 CBUF（154MB SRAM），拥塞时（队列积压）新来的包 spill 到 HBM（16GB）需要发这个包时，从 HBM 读回到 CBUF再发出去。


![[Pasted image 20260421125705.png]]

# Port Group
800Gbps per PG，total 18 PG instances on BX
![[Pasted image 20260421130246.png]]
# RETIMER 
BXF 这个例子里
BX die 和 BF die = 两颗独立制造的裸硅片
BXF package = 把这两颗 die + HBM die 一起封进一个 MCM package
焊到线卡 PCB 上，外面看到的是一颗"芯片"，里面其实是 3 颗 die

MCM 是一种封装技术：把多颗独立的裸 die（芯片）放在同一个基板上，封装成一个看起来像单颗芯片的 package，焊到 PCB 上。

LC 上的 BX ASIC ──→ [backplane] ──→ MT3775 Retimer ──→ BF ASIC
                     信号衰减            ↑ 在这里重新整形信号

BX 芯片 Fabric 侧用的是 XSR SERDES（省电 70%），但 XSR 只能走短距离（MCM 内部），无法直接驱动背板上的长距离 trace。
解决方案：把一颗 BF die 和 BX die 封装在同一个 MCM 里。BF 在这里不做交换，只做 XSR→LR 的信号转换（retimer 角色）。封装里同时有 HBM 贴在 BX die 旁边。


# VOQ
VOQ 可以理解为“按目的出口拆分的虚拟队列”。它不是单纯一个 FIFO，而是很多个逻辑队列。
在 BX 里，每个 7.2T datapath 有一个 VOQ Manager，维护 48K 个 VOQ。
Ingress 处理完包头后，会得到转发结果（包括 VOQ 编号），然后把包写入 CBUF，并把包描述符入对应 VOQ。
CM（Congestion Manager）按 VOQ 做拥塞判断、丢弃/标记决策，并统计每个 VOQ 占用。
VOQ 调度是 fabric 的 request/grant 调度核心；你在 day1/day3 字幕里提到的“VOQ scheduling across fabric”就是这个。
当某个 VOQ 拥塞时，才会把对应流量页下沉到外部 HBM（off-chip DBB），不是所有包都下沉。

VOQM 管的是 descriptor/page，不是 payload 本体。

ingress fabric interface包含：
VOQ Manager：维护和选择待发队列状态
Request Scheduler：做 request/grant 仲裁，决定谁先发
Fabric Out：把获批流量真正送到 fabric 链路

![[Pasted image 20260428141152.png]]
1.
IG x9 + IG_MISC -> enq pkt_descr x10
来自 ingress group 的入队请求。
不是整包数据进 VOQM，而是“包描述符”进 VOQM（真正包数据在 CBUF/HBM）。
2.
CM <-> enq/deq updts
CM（Congestion Manager）和 VOQM 交换队列占用变化。
VOQM 入队/出队会更新拥塞状态，CM 再用于丢弃/标记/阈值控制。
3.
RS enq / RS deq
RS（Request Scheduler）和 VOQM 的队列级交互。
可以理解为：RS 负责调度，VOQM 提供“谁有包、该出谁”的队列状态与出队执行。
4.
FI (xN) -> fabric Gnt
来自 Fabric In 的 grant（授权）反馈。
VOQM 拿到 grant 后，才会把对应 page descriptor 发给后续发送通路。
右侧输出到存储与发送路径
5.
Wr-page descr -> MemOut(x2)，Ack 回来
当队列判定要下沉到外部 DBB（HBM）时，VOQM 发写页描述符给 MemOut。
MemOut 处理后回 Ack。
6.
Rd-page descr -> MemIn(x2)，Ack 回来
出队时如果数据在外部 HBM，VOQM 发读页描述符给 MemIn，把页搬回 CBUF/发送链路。
完成后回 Ack。
7.
fabric Req 和 page_descr -> FO
VOQM 向 FO（Fabric Out）发 fabric 请求和对应页描述符，驱动真正上 fabric 发包。
8.
LS Req / LS Gnt / LS page descr <-> VIQM(x2)
这是本地交换（Local Switch）路径的请求/授权/描述符交互。
不走远端 fabric 的流量可在本地路径完成。
一句话理解这张图
VOQ Manager 是“队列状态中枢 + 描述符调度中枢”：
左边收 ingress 入队和调度/拥塞反馈，右边按 grant 决定是走 fabric 发送、走 local-switch，还是和外部 HBM 做页级 spill/fill。

# VIQ
Egress Fabric Interface: DATA Flow
![[Pasted image 20260429091155.png]]
![[Pasted image 20260429093431.png]]
OQ就是给port group interfaces的queue在BX/BXF上进行缓存的。


# DBB
Datapath Buffering, 是 BX 的“深缓冲”机制，用片上+片外协同，专门扛拥塞和突发。
它分两层：
片上 Internal DBB（在 ==CBUF== 里，低时延）
片外 External DBB（==HBM==，大容量但带宽超分）
正常先走片上缓冲；只有队列拥塞时，才把流量页下沉到片外 HBM。
这个 on-chip/off-chip 的选择是在 enqueue 时由 CM/VOQ 相关逻辑决定的。

# CBF
Central memory Subsystem
![[Pasted image 20260428134541.png]]

# HBM
![[Pasted image 20260428140216.png]]
# PWR
3 power supplies can be sufficient for powering the system. However all the 6 power supplies are requested to be physically inserted into the chassis even though some of them are not connected to power source. 
1. Electrical load may need only 3 PSUs. This is about watts for RE/FPC/SIB.
2. Thermal design expects 6 PSU FRUs physically present. Even a PSU slot that is not cabled is still expected to be occupied, because PSU bays are part of the airflow architecture (ducting/sealing/pressure path).

# Document
PTX10008 Packet Transport Router Hardware Guide
https://www.juniper.net/documentation/us/en/hardware/ptx10008/topics/topic-map/ptx10008-system-overview.html

BX chipset TOI:
https://junipernetworks.sharepoint.com/:p:/r/sites/JuniperTOI/_layouts/15/Doc.aspx?sourcedoc=%7BB8A0C100-25C0-4E22-A5CD-6CCDDB1F18CE%7D&file=JTAC_BX_PFE_TOI.pptx&action=edit&mobileredirect=true&DefaultItemOpen=1

AGEON:LC1301
https://junipernetworks.sharepoint.com/:p:/r/teams/RBU/test/RBU-SP/_layouts/15/Doc.aspx?sourcedoc=%7B7911FE06-F32D-48A1-9010-607A795DFB57%7D&file=Aegon_SnP_v2.pptx&action=edit&mobileredirect=true&DefaultItemOpen=1

PTX10008 LC1301 Knowledge Hub
https://junipernetworks.sharepoint.com/sites/gs-knowledge-hub/NPI/Forms/AllItems.aspx?FolderCTID=0x012000A84CDFE1A930FB449DB698E0B039C7F7&id=%2Fsites%2Fgs%2Dknowledge%2Dhub%2FNPI%2FPTX10008%2DLC1301%2D36DD%28Aegon%29
Day1（PLM & ASIC）
偏架构总览：平台定位、ASIC/芯片模块、fabric 与 buffer 体系。
明确提到 Port Group（18 个 PG、每个 800G）和 clock gating 的功耗思路。
Day2（HW, SW, Test）
偏硬件实现与 bring-up：板级连接、fabric links、lane/retimer、电源与热设计、测试讨论。
lane/fabric/power 相关讨论密度很高。
Day3（SW Contd）
偏软件与转发流水：ingress/egress 处理、VLAN 操作、端口到 ASIC datapath/PG 的映射。
有比较多 datapath 与 egress pipeline 的实操解释。

LC1201 & LC1202 overview
[https://junipernetworks.sharepoint.com/:p:/r/sites/VerizonCFTS-USTeam/_layouts/15/Doc.aspx?sourcedoc=%7BF1131071-C31A-470C-9CD3-317672EB3133%7D&file=LC1201%20LC1202%20overview1_678186508.pptx&action=edit&mobileredirect=true&DefaultItemOpen=1](https://junipernetworks.sharepoint.com/:p:/r/teams/RBU/software/PFE/trio_pfe/projects/aft/_layouts/15/Doc.aspx?sourcedoc=%7B7AD87734-49AC-4D64-B910-8BE9A11AE1D9%7D&file=LC1201%20OVERVIEW.pptx&action=edit&mobileredirect=true&DefaultItemOpen=1)