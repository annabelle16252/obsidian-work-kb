# RSVP/RSVP-TE

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

RSVP/RSVP-TE
RSVP follows IGP routes.
RSVP-TE uses a combination of CSPF and Explicit Route Objects (EROs) to determine how traffic is routed through the network.
Secondary path 按照path up顺序，先up的就被优选。
Secondary path会不停尝试弹回p...rimary... path...，但secondary... path之间切换后不会弹回...。
ldp走IGP的路径不支持TE...，但可以通过rsvp... tunnel 走TE路径...。...TE是通过RSVP实现的...。
因为CSPF使用TLV...，ISIS可以直接支持，OSPF需要手动开启...TE且TED...不能跨area
> show ted database extensive...去看...TED...内容
# Priority #
setup... ...hold，...Default 7 0...，7 (that is, it cannot preempt any other LSPs) and a reservation priority of 0 (that is, other LSPs cannot preempt it).... ...同一条... lsp...的...hold...一定比...setup...优。0是最优的，7是最不优的。当没有...new lsp...，资源紧张，这些...lsp...比较的是谁的...hold...大，谁被...tear...。...new lsp...用...setup...和...existing lsp...的...hold...比。
# auto-bw #
make-before-break,...在...bw...出现紧张前重新...signal lsp
# soft-preempt #
当...bw...出现紧张，一条...lsp...要被...preempt...时，...ingress router...提前找一个新的...path...。这也是种...make-before-break...，模式还是...FF...。
# optimize-timer #
default lsp...建立好了就不会再计算直到这条...lsp down...。配这个可以...periodic...的让...CSPF...去算最优...lsp path...。
# adaptive #
比如一条...lsp...有两个...path...，都经过同一根...link...，那么...bw...会被...reserve...两次。这个是...default style FF...。配了...adatpive...变成...SE...，... ...就是只...reserve...一次。
# install address <prefix> #
https://www.juniper.net/documentation/us/en/software/junos/cli-reference/topics/ref/statement/install-edit-protocols-mpls.html
Associate one or more prefixes with an LSP. When the LSP is up, all the prefixes are installed as entries into the inet.3.
# set protocols mpls label-switched-path a-centauri-to-sirius install 192.168.1.0/24  <<<
Install knob: add to inet3
install active knob: add this lsp route into inet0 as well
inet.3: 359 destinations, 366 routes (359 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both
101.234.63.57/32   *[RSVP/7/1] 01:53:46, metric 262
>  to 101.234.61.13 via xe-1/0/0.0, label-switched-path BOI-LONHE-ER6-EIG-MUMTT-ER1
to 101.234.61.221 via xe-0/1/0.0, label-switched-path BOI-LONHE-ER6-EIG-MUMTT-ER1
[RSVP/8/1] 01:54:45, metric 262
>  to 101.234.61.1 via xe-0/0/0.0, label-switched-path BOI-LONHE-ER6-EIG-MUMTT-ER1
to 101.234.61.221 via xe-0/1/0.0, label-switched-path BOI-LONHE-ER6-EIG-MUMTT-ER1
[LDP/9] 1w2d 17:15:56, metric 262
to 101.234.61.1 via xe-0/0/0.0, Push 22
>  to 101.234.61.13 via xe-1/0/0.0, Push 22
# ldp tunneling #
整个是...ldp...，中间插入...rsvp
172.30.5.7/32      *[LDP/9] 00:00:44, metric 20
> to 172.30.0.22 via ge-0/0/4.134, label-switched-path Canopus-to-Vega
正常...ldp...路由不可能...nh...是...lsp...，但...ldp-tunnling...就会出现这种情况
# 常用命令 #
> traceroute mpls rsvp LSP-to-R6
> show ted database extensive | no-more
> show isis adjacency  | no-more
> show mpls lsp ingress name 10.242.211.10:10.233.254.82:4040:mvpn:TM_IPTV_HSBB extensive | no-more
> show mpls lsp p2mp extensive | no-more
> show isis database extensive | no-more
> show isis hostname | no-more
> show rsvp neighbor | no-more
> traceroute 10.242.211.10 source 10.233.254.82
Enable RSVP traceoption...：
set protocols rsvp traceoptions file rsvp-trace
set protocols rsvp traceoptions file size 500m
set protocols rsvp traceoptions file files 10
set protocols rsvp traceoptions flag all
Monitor traffic interface:
> monitor traffic interface xe-2/2/0 layer2-headers size 1500 matching "ip[9] = 46" write-file RSVP-packet_2-2-0.pcap
# Troubleshooting RSVP hop by hop #
XL211154@igw21.mecs-re0> show mpls lsp extensive name LSP:00.igw21.mecs--igw22.mecs
Standby   PATH-SEC:00.igw21.mecs--igw22.mecs State: Up
Priorities: 7 0
Hoplimit: 5
Computed ERO (S [L] denotes strict [loose] hops): (CSPF metric: 420)
10.55.99.175 S 10.55.100.126 S
Received RRO (ProtectionFlag 1=Available 2=InUse 4=B/W 8=Node 10=SoftPreempt 20=Node-ID):
10.55.99.175 10.55.100.126
1489 May 19 07:24:10.350 Record Route:  10.55.99.175 10.55.100.126
1488 May 19 07:24:10.350 Up
1487 May 19 07:24:10.350 Self-ping started
1486 May 19 07:24:10.350 Self-ping enqueued
1485 May 19 07:24:09.736 Originate Call
1484 May 19 07:24:09.736 CSPF: computation result accepted  10.55.99.175 10.55.100.126 << ...路径
1483 May 19 07:23:41.072 CSPF failed: no route toward 10.55.99.175[9 times]
1482 May 19 07:19:49.655 Clear Call: CSPF computation failed
1481 May 19 07:19:49.651 ResvTear received
1480 May 19 07:19:49.651 Self-ping ended due to LSP tear-down
1479 May 19 07:19:49.651 10.55.99.174: Down
1478 May 19 07:19:49.651 10.55.99.175: No Route toward dest <<<...这个只代表路径上有一跳出问题，不代表一定是...local...设备出接口问题
Troubleshooting ...要一跳一跳的去查：
{master XL211154@igw21.mecs-re0> show route 10.55.99.174
10.55.99.174/32    *[Local/0] 4w3d 12:35:53
Local via et-11/1/0.0  <<<
set interfaces et-11/1/0 unit 0 description "BACKHAUL: et-11-1-0.igw21.mecs--et-7-0-2.hgw21.klg - 100G - WL1024095942 - (AUG 2017)"
<<<...找到下一跳设备和接口
XL211154@hgw21.klg-re0> show route 10.55.100.126
inet.0: 950892 destinations, 1896873 routes (950885 active, 0 holddown, 7 hidden)
+ = Active Route, - = Last Active, * = Both
10.55.100.126/31   *[Direct/0] 2w0d 00:20:18
>  via et-4/0/2.0   <<< ...在下一跳设备上...show...到下下一跳设备
set interfaces et-4/0/2 unit 0 description "MONITOR BACKHAUL: et-4-0-2.hgw21.klg--et-4-1-0.igw22.mecs - 100G - WL1029965802 - (MAR 2018)"
<<<...下一跳设备和入接口
# RSVP Message #
ingress router通过cspf算出一条route...，然后往egress... 逐跳发path msg including ERO list...，让它reserve路径。egress往ingress逐跳发...resv msg...。
ERO msg:
RRO msg:
RSVP-TE ...is a soft-state protocol, 需要通过...periodical exchanging of signaling messages between two peering routers to keep the state up。
once the LSP-PATH has been established, it must be constantly refreshed to keep the operational state up and is achieved by sending a constant PATH and RESV messages.... ...the content of these PATH and RESV messages (which are sent to refresh the RSVP sessions) has the same content as the original PATH and RESV messages.
-Bundle message: from junos 15 by default use bundle message, as long as from same interface, can bundle all types of rsvp messages together into one bundle.
-Path message:...上游发给下游要求建立...lsp
-Resv message:...下游发个上游...reseve bw
-PathTear message: remove path
-ResvTear message: remove reservation
-PathErr message:...上游发给下游
-ResvErr message:...上游发给下游
label...是...resv msg...来...allocate...的。他把...label...放入...resv msg...的...label object...里放给上游。
Path Msg...里有...ERO RRO...，但...Resv msg...里只有...RRO
Object ...就是...rsvp msg...里含有的，比如...ERO, RRO, SESSION...
RSVP Extention:
Hello protocol: speed up failure detection, 9 second
Label distribution
# MBB #
RSVP-TE...可以实现...MBB. ...通过以下两个参数控制...MBB:
https://www.juniper.net/documentation/en_US/junos/topics/topic-map/basic-lsp-configurtion.html#id-57537
optimize-switchover-delay—Amount of time to wait before switching to the new LSP instance.
optimize-hold-dead-delay—Amount of time to wait after switchover and before deletion of the old LSP instance.
Adaptive LSPs
Automatic bandwidth allocation
BFD for LSPs
Graceful Routing Engine switchover
Link and node protection
Nonstop active routing
Optimized LSPs
Point-to-multipoint (P2MP) LSPs
Soft preemption
Standby secondary paths
Both the... optimize-switchover-delay ...and... optimize-hold-dead-delay ...statements apply to all LSPs that use the make-before-break behavior for LSP setup and teardown, not just for LSPs for which the... optimize-timer ...statement has also been configured. The following MPLS features cause LSPs to be set up and torn down using make-before-break behavior:
LSP Protection
#Secondary path...：...
lsp...只有当...primary path fail...的时候，才会用到...secondary path...。而...primary path recover...后，又会切回...primary...。用...standby knob...，可以让...secondary path...一直保持...UP...的状态。这样...primary down...就可以直接切到...secondary...。在...lsp...上用...adaptive...，就可以让...primary...和...secondary ...用...SE...，...share...同一个...bw...，因为他们不会同时跑流量，所以...share...也没关系。
# ...然而，...2nd path...并没办法解决掉...primary fail...造成的...traffic loss...。因为这个...path error...的信息传到...ingress router...是需要时间的。因此我们需要用到...FF...或...bypass...在这个时间段临时走...traffic...。
#Fast Reroute
当...lsp...上某一节点发生问题，可以立即切换到另一个...path...并通过...path error msg...通知...ingress...去调用...2nd path...或者...re-signal...一条新的...lsp...。这个另一条...path...就是...detour path...，他是...temporty...的解决方案。但如果没有...2nd path...或...re-signal ...一条新的...lsp...一直不成功，那么就会一直用...detour...。
Check detour...路径，需要在...transit router...上看。因为...detoure...是为在这条...lsp...上的每一台...transit router...上为防止到下一跳...router...那根...link...出问题，绕过这根...link...建立一条...detoure...。...ingress...上是没有...detoure...的。
# Link-Protection
FR...是...1:1...保护，他保护的是这一条...LSP. ...而...link-protection...是...1...：...n...保护，他保护的是...interface...，也就是穿过这个...interface...的所有...lsp...。配置在两个层级：...1.rsvp interface 2. lsp
# Link-node-Protection
FR...路径可能就不会经过...nh router...了。但...link-protection...一定会经过...nh router...只是走不同...link...。但是问题是如果整台...nh router down...怎么办。所以就出现了...link-node-protection...。
# Self-ping #
New feature and is enabled by default in JUNOS 16.1 or later
LSP self-ping is on by default starting with JUNOS 16.1R1
LSP self-ping relies on sending UDP datagram with destination port 8503 (IPv4 and IPv6 as needed). If there is a loopback filter, you need to allow UDP destination port 8503 to get through your loopback filter
no-self-ping
Since Circuit Cross-Connect (CCC) LSPs is neither IPv4 nor IPv6, LSP self-ping is not supported on such LSPs. Hence, you need to turn off LSP self-ping for these LSPs using the following configuration:
https://supportportal.juniper.net/s/article/JUNOS-16-1-or-later-MPLS-LSP-related-configuration-changes?language=en_US
作用是当...lsp path...切换之前，...ingress router...发起...ping...，看看最优路径沿途每个...router...是否都准备好，可以走了。...Self-ping...只在...optimize...的时候起作用，如果是...fail over...不会触发（...FRR…...）。
有...self-ping...场景：
1....新建...lsp/clear mpls lsp...，会发起一个...self-ping...，但是否成功不影响...lsp...能否建立成功
2.cspf...计算出新的...path...要求发起...MBB...的时候才会用...self-ping...。...Self-ping...和...cspf...，...link-protection...都没有关系。
<<...所以...self-ping...有意义，只有...MBB...的场景。
link-protection不是MBB，是local repair，他的次优路径会提前放入转发表
MBB是先把更优的放入转发表，再删去次优。
# 6PE #
在...v4...的...core...里跑...v6 traffic...，
set protocols mpls ipv6-tunneling  <<< copy inet.3 ...路由去...inet6.3...里变成...compatible v46...路由
set protocols bgp group ibgp family inet6 labeled-unicast explicit-null<<<...多加一个...label
#  Link Protection  #
保护一条...lsp...某个...LSR...的一个...interface...到...nh LSR...接口的...link...。如果这个...link fail...，马上切到...bypass...上。
Bypass LSP established around protected interface to adjacent node
Uses CSPF to calculate bypass LSP
Can add ERO to influence CSPF routing of bypass LSP
Link Protection Configuration
1.
[edit protocols mpls]
lab@r6# show
label-switched-path r6-r4 {
to 10.0.3.4;
link-protection; <<<<<< LSP ...下面配
primary r6-r4;
}
path r6-r4 {
10.0.3.3 loose;
}
interface all;
2.
[edit protocols rsvp]
lab@r3# show
interface fe-0/0/0.0;
interface fe-0/0/1.0;
interface fe-0/0/3.0;
interface at-0/1/0.0;
interface so-0/2/0.100 {
link-protection; <<<<<< ...口下面配
}
Monitor Link Protection
Note that link protection is an RSVP feature, and as a result, the resulting bypass LSPs are not listed in the output of show mpls lsp commands.
> show rsvp session ingress detail
Ingress RSVP: 6 sessions
12.82.74.5
From: 12.82.74.4, LSPstate: Up, ActiveRoute: 0
LSPname: GERAS-MASON-BYPASS
Suggested label received: -, Suggested label sent: -
Recovery label received: -, Recovery label sent: 300464
Resv style: 1 SE, Label in: -, Label out: 300464
Time left:    -, Since: Mon Oct 25 05:07:19 2010
Tspec: rate 2Mbps size 2Mbps peak Infbps m 20 M 1500
Port number: sender 1 receiver 34471 protocol 0
Type: Bypass LSP   <<<<<<<<<<
Number of data route tunnel through: 2
Number of RSVP session tunnel through: 0
PATH rcvfrom: localclient
Adspec: sent MTU 1500
Path MTU: received 1500
PATH sentto: 12.82.75.17 (ae1.0) 1 pkts
RESV rcvfrom: 12.82.75.17 (ae1.0) 1 pkts
Explct route: 12.82.75.17 12.82.75.14  <<<<<< cspf ...算出来的...bypass...路径
Record route: <self> 12.82.75.17 12.82.75.14
