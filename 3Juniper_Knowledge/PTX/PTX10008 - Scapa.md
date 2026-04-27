
ptx10008+lc1301 <<< NGTV4
# Chassis
![[Pasted image 20260420110203.png]]
# Commands #
FPC AFT login:
start shell pfe network fpc0

FPC linux login:
start shell user root
chvrf iri ssh fpc0 
# RCB 
![[Pasted image 20260418123214.png]]
# FPC 
与MX不同，PTX接口直接集成在fpc上，而非使用独立的FPC+PIC架构，所以PIC的位置永远是0.
"Aegon – LC1301" - Express5 BX
"Scapa - LC1201" - Scapa BT

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

"BF-as-RETIMER" 的含义
![[Pasted image 20260421121238.png]]
BX 芯片 Fabric 侧用的是 XSR SERDES（省电 70%），但 XSR 只能走短距离（MCM 内部），无法直接驱动背板上的长距离 trace。
解决方案：把一颗 BF die 和 BX die 封装在同一个 MCM 里。BF 在这里不做交换，只做 XSR→LR 的信号转换（retimer 角色）。封装里同时有 HBM 贴在 BX die 旁边。


# SIB 
SF5 -BF chipset
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


LC1201 & LC1202 overview
[https://junipernetworks.sharepoint.com/:p:/r/sites/VerizonCFTS-USTeam/_layouts/15/Doc.aspx?sourcedoc=%7BF1131071-C31A-470C-9CD3-317672EB3133%7D&file=LC1201%20LC1202%20overview1_678186508.pptx&action=edit&mobileredirect=true&DefaultItemOpen=1](https://junipernetworks.sharepoint.com/:p:/r/teams/RBU/software/PFE/trio_pfe/projects/aft/_layouts/15/Doc.aspx?sourcedoc=%7B7AD87734-49AC-4D64-B910-8BE9A11AE1D9%7D&file=LC1201%20OVERVIEW.pptx&action=edit&mobileredirect=true&DefaultItemOpen=1)