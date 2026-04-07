# Training

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

Training
P4
-NAT网络地址转换，主要作用是把 私有网络的 IP 地址 转换成 公网 IP 地址，或者在不同网段之间进行地址映射。
-为什么要有nat...，...私有网络里可以重复使用私有地址段（如 192.168.x.x、10.x.x.x）...，但去公网我们可以经过翻译避免地址冲突。同时从安全方面考虑，可以...隐藏内部真实地址，外部网络只能看到转换后的地址。
p...5
一般家庭用户上网会经过2个NAT...：
把内部私网地址 → 转换成运营商分配的私网 IP...（路由器的WAN口地址）
现在 IPv4 地址紧缺，大多数 ISP（尤其是移动/光纤接入）不会给你真实公网地址。（CGNAT），把成千上万个用户再映射到少量公网地址
p...6
source...，destination，
twice... nat...：比如...A--B公司通过vpn连接...，双方都不想暴露地址，或者私有网段冲突。
Static NAT...：...一个私有地址 ↔ 一个公网地址固定绑定
Dynamic NAT...：...内部地址从公网地址池中动态分配一个，可能会变化。
PAT...：...多个私有地址共享一个公网地址，通过端口号来区分会话。
p7
Stateless NAT → 静态映射，不存状态...，简单粗暴
Stateful NAT → 动态分配地址/端口...，...存状态
p9
NAT：只改网络层和传输层（IP、端口）。
ALG：补充 NAT，帮忙改应用层协议里的地址/端口。
关系：ALG 是 NAT 的“助手”，解决 NAT 带来的应用协议失效问题。
p11
> show chassis fpc pic-status
p18
每个SPC3上有两个SPU...，每个...SPU都有两个CPU和若干组件构成...，接下来让我们看一下他的硬件架构。
p19
Talus FPGA transits packets from I2C to HSL2 which connecting to Fabric interfaces.
P21
Core 0 of CPU complex 0 is used to bring the whole SPC3 card in service.
SPCD as ukernel runs on Linux of core0 of Complex 0.
我们知道ukernel就是把一些简单的任务外包到pfe上来进行...。
Each Linux on SPC3 has one flowd. Each flowd represents as one PIC. the main task is packet processing.
p22
简而言之...，...USF就是可以支持一个architecture来兼容这些不同地方
-Adaptive service cards里面...，...mspmand来负责进行packet processing...，...SPC3变成flowd了
p28
NAT配置基本分这四块...：
nat-rule:规定用什么方式配置nat
service-set作用就是把我要应用的nat rule set到service interface上
p31
dynamic-nat44
映射是动态建立的：当会话结束或超时，映射关系会被释放。
它对应的是静态nat44...，需要手动绑定mapping
p32
interface type...：整个interface的流量都进一个service...-set中
nh type type...：根据nh可以进不同的service...-set...，也就是可以根据nh进行vrf隔离
p62
labroot@jtac-mx480-r2033-re0# run show chassis fpc pic-status
Slot 3   Online       SPC3
PIC 0  Online       SPC3-PIC
PIC 1  Online       SPC3-PIC
=================
RFC 1918 defined the following private address space
Private Address Space
10.0.0.0 - 10.255.255.255 (10/8 prefix)
172.16.0.0 - 172.31.255.255 (172.16/12 prefix)
192.168.0.0 - 192.168.255.255 (192.168/16 prefix)
RFC 6598 allocates the following shared space to be used by service providers for CGNAT deployment
Shared Address Space
100.64.0.0/10
-分类
-硬件...：ms...-mpc...，...ms-mic, ...spc...3...，inline... nat(not for CGNAT, but for small scale NAT)
-接口:ams,vms,mams,ms-
-配置:nat44
-command verify
> show services sessions source-prefix 100.86.200.1
> show services nat source mappings address-pooling-paired private 100.86.200.1/32
> ...show services sessions destination-prefix 220.239.219.33
> ...show services session source-prefix <subscriber-ip>...
这个source... ...session... landing到哪些service pic上
-LB-ams,mams
# run show interfaces load-balancing detail
vty)# show interfaces services-lb
# run show services nat pool basic_AMS
vty)# show nhdb id <index> detail
==============================
Carrier-Grade NAT
CGNAT 是运营商用来节省公网 IPv4 地址的技术，把很多用户的私网地址通过双重 NAT 转换到少量公网 IP 上。
运营商在你的家庭路由器 NAT 之后，再做一次 NAT（第二层 NAT），把成千上万个用户的内网地址（通常是 100.64.0.0/10 这个保留地址段）再转换成少量公网 IP。
这就是 ...双重 NAT...（Double NAT）。
CGNAT 会通过端口号来区分不同用户的连接，比如同一个公网 IP，但端口范围不同分配给不同用户。
JUNOS supports two forms of NAT configuration on MX series routers:
Stateful NAT, which requires dedicated hardware such as MS-DPC (EOL), MS-MPC or SPC3
Stateless NAT, which can be configured as inline service on any TRIO based MPC card
分类
Based on Translation Direction
Symmetric NAT
映射：内网源 IP+端口 + 外网目标 IP+端口
S...tatic... NAT.../one-to-one ...NAT.../...Full-cone NAT
(Address)-restricted-cone NAT
只看外部主机的 IP 是否在内部之前访问过，不检查端口号
Port-restricted cone NAT
必须是内部访问过的外部... ...IP+端口... ...才能回连
Address polling pair
Non-AP... address allocation
第一个池子不用完，不会用第二个
pool pool1 {
>...show services nat
address-range low 31.0.0.1 high 31.0.0.32;
port automatic random-allocation;
address-allocation round-robin;
ECMP & FBF:
Only Next-Hop style service-sets can use the above methods
