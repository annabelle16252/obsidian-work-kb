# Segment routing 

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

Segment routing
# 常用命令 #
> show spring-traffic-engineering lsp  <<< 看SR—TE lsp
To              State     LSPname
10.233.254.64-1<c> Up     LSP-SRTE:01.tdf01.lab--hgw21.lab
10.233.254.133-1<c> Up    LSP-SRTE:01.tdf01.lab--imse03_04.lab
# run show route 101.234.3.132   <<< inet.3里有给接下来各个跳分的label
inet.0: 23 destinations, 24 routes (23 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both
101.234.3.132/32   *[IS-IS/18] 2d 20:06:03, metric 400
>  to 101.234.16.18 via xe-1/0/5:0.0
inet.3: 4 destinations, 5 routes (4 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both
101.234.3.132/32   *[SPRING-TE/8] 2d 00:07:22, metric 1, metric2 400
>  to 101.234.16.18 via xe-1/0/5:0.0, Push 32, Push 201101(top)<<<ingress push all labels
[L-ISIS/14] 2d 20:01:57, metric 400
>  to 101.234.16.18 via xe-1/0/5:0.0, Push 401012
to 101.234.16.20 via xe-1/0/5:1.0, Push 401012
nh to 101.234.16.18上check label 201101: <<< 根据上面label可以按hop来看
annaw@lab-mx480-3d-02-re0# run show route table mpls.0 label 201101
201101             *[L-ISIS/14] 2d 20:25:55, metric 0
>  to 101.234.16.7 via xe-1/1/1.0, Pop
to 101.234.16.9 via xe-1/1/17.0, Swap 401011
201101(S=0)        *[L-ISIS/14] 2d 20:25:55, metric 0
>  to 101.234.16.7 via xe-1/1/1.0, Pop
to 101.234.16.9 via xe-1/1/17.0, Swap 401011
nh to 101.234.16.7上check label 32:
annaw@lab-mx204-01> show route table mpls.0 label 32
32                 *[L-ISIS/14] 2d 20:31:44, metric 0
>  to 101.234.16.1 via et-0/0/0.0, Pop
to 101.234.16.6 via xe-0/1/3.0, Swap 401012
32(S=0)            *[L-ISIS/14] 2d 20:31:44, metric 0
>  to 101.234.16.1 via et-0/0/0.0, Pop
to 101.234.16.6 via xe-0/1/3.0, Swap 401012
> show isis adjacency extensive  <<< 看这台router作为ingress到直连设备的adjance label
lab-mx480-3d-02-re0
Interface: xe-1/0/5:0.0, Level: 2, State: Up, Expires in 25 secs
Priority: 0, Up/Down transitions: 1, Last transition: 2d 19:20:34 ago
Circuit type: 2, Speaks: IP, IPv6
Topologies: Unicast
Restart capable: Yes, Adjacency advertisement: Advertise
IP addresses: 101.234.16.18
Level 2 IPv4   protected Adj-SID:  201601, Flags: -BVL-P
Level 2 IPv4 unprotected Adj-SID:  200601, Flags: --VL-P
Transition log:
When                  State        Event           Down reason
Thu Oct 30 14:36:04   Up           Seenself
lab-mx240-3d-01-re0
Interface: xe-1/0/5:1.0, Level: 2, State: Up, Expires in 18 secs
Priority: 0, Up/Down transitions: 1, Last transition: 3d 18:10:57 ago
Circuit type: 2, Speaks: IP, IPv6
Topologies: Unicast
Restart capable: Yes, Adjacency advertisement: Advertise
IP addresses: 101.234.16.20
Level 2 IPv4   protected Adj-SID:  201602, Flags: -BVL-P
Level 2 IPv4 unprotected Adj-SID:  200602, Flags: --VL-P
Transition log:
When                  State        Event           Down reason
Wed Oct 29 15:45:41   Up           Seenself
# SR+MPLS & Legcy MPLS #
SR替代了传统的RSVP/LDP进行lmpls的label分发...。
传统的...mpls ...是从...ingress...到...egress...都用一个...label...，这个...label...是在...ingress push...的代表的是整条...lsp path...。
而...SR path...是在...ingress push...若干个...label...给这个...packet...，每个...label...代表...path...上的每一跳。最外面的...label...代表最近的...nh...。
One of the key properties of SR is that MPLS labels are distributed via the Interior Gateway Protocol (IGP), ISIS or OSPF, rather than a dedicated label distribution protocol.
普通SR不需要BGP...，因为SID靠IGP的扩展TLV来传播。
Segment Routing over BGP / BGP-LS / SR-TE 需要BGP + SR
# SR-TE #
路径并不是像传统 RSVP-TE 那样由每一跳建立 LSP，而是由ingress直接决定完整路径。
SR-TE 的路径由一个 SID 列表定义： Segment List = [SID1, SID2, SID3, ...]
数据包在进入网络时就被打上这一串 MPLS Label（每个 SID 对应一个 label）。沿途的路由器只根据最上层 label 转发，不再需要为该 LSP 建立任何 RSVP 状态。
→ Stateless (无状态)，是 SR-TE 的最大优势。
路径建立方式：
1.static
<<<<
protocols {
isis {
interface xe-1/0/5:0.0 {
level 2 {
ipv4-adjacency-segment {
protected label 201601;
unprotected label 200601;
}
}
}
interface xe-1/0/5:1.0 {
level 2 {
ipv4-adjacency-segment {
protected label 201602;
unprotected label 200602;
}
}
}
traffic-engineering l3-unicast-topology;
}
mpls {
label-range {
static-label-range 200000 299999;
}
}
source-packet-routing {
segment-list SL-via-mx480-S-S {
auto-translate;
HOP1 ip-address 101.234.16.18; <<<to RR
HOP2 ip-address 101.234.16.7; <<< to mx204-3d-01
HOP3 ip-address 101.234.16.1;<<< to mx204-01
}
source-routing-path PATH-SL-via-mx480-S-S {
to 101.234.3.132; <<< to mx204-01
primary {
SL-via-mx480-S-S;
}
}
<<<<
# run show spring-traffic-engineering lsp
To                        State        LSPname
101.234.3.132             Up           PATH-SL-via-mx480-S-S
2.控制器下放：控制器计算路径，通过 PCEP协议下发到设备
PCE 收集 IGP 拓扑（通过 BGP-LS）
工 作 原 理 
1. 在 网 络 设 备 上 ( 通 常 是 路 由 器 或 RR) 启 用 BGP-LS 。 
2. 设 备 会 把 从 IGP 收 到 的 链 路 状 态 ( 例 如 节 点 、 链 路 、 SRGB 、 SR label 、 带 宽 等 ) 封 装 为 BGP-LS NLRI 
(Network Layer Reachability Information) . 
3. 这 些 NLRI 会 通 过 BGP 分 发 给 BGP-LS 客 户 端 ( 通 常 是 SDN Controller 、 PCE 、 路 径 计 算 器 等 ) 。 
4. 控 制 器 接 收 后 , 就 能 构 建 出 整 个 网 络 拓 扑 。 ...工 作 原 理 
1. 在 网 络 设 备 上 ( 通 常 是 路 由 器 或 RR) 启 用 BGP-LS 。 
2. 设 备 会 把 从 IGP 收 到 的 链 路 状 态 ( 例 如 节 点 、 链 路 、 SRGB 、 SR label 、 带 宽 等 ) 封 装 为 BGP-LS NLRI 
(Network Layer Reachability Information) . 
3. 这 些 NLRI 会 通 过 BGP 分 发 给 BGP-LS 客 户 端 ( 通 常 是 SDN Controller 、 PCE 、 路 径 计 算 器 等 ) 。 
4. 控 制 器 接 收 后 , 就 能 构 建 出 整 个 网 络 拓 扑 。
PCE 计算最优路径（考虑延迟、带宽）
PCE 通过 PCEP 下发 SR Policy 到 headend
路由器根据下发的 segment list 自动装载 SR Policy
set protocols pcep
set protocols pcep local-address 10.0.0.1
set protocols pcep pce controller address 10.0.0.100
set protocols segment-routing traffic-engineering pcep
3.BGP SR Policy...：...通过 BGP（Color + Endpoint）分发 SR Policy
# SRGB #
SRGB 是整个 SR 域（SR domain）内所有设备共同约定的一段 Label 范围。
每个 Node 的 SID（Segment ID）都是从这个区间里派生出来的。default的值是16000–23999.
liang@jmxa# commit
[edit protocols isis source-packet-routing]
‘srgb’
can’t config SRGB
error: configuration check-out failed -- I can’t modify SRGB value
After adding network-services enhanced-ip, we were able to configure the SRGB value.
# Adj-SID Flag #
The F-flag is the Address Family flag. As you can see, it is set for IPv6 Adj-SIDs and clear for IPv4 Adj-SIDs.
The B-flag is the Backup flag. It is set if the SID is eligible for protection, and clear if it is not. You can see that it is not set for the unprotected label values (73 and 103).
The V-flag is set if the Adj-SID carries a Value, which is the case in all the examples above.
The L-flag is set if the label has Local significance, which is also the case in all the examples above.
The final two flags are not set in any of the examples above - later you will see an example in which they are set.
The S-Flag means that the SID refers to a set (S-) of adjacencies.
The P-Flag... ...means that the Adj-SID is persistently (P-) allocated (because it was statically configured)... ...and therefore remains consistent across router restarts and/or interface... ...flaps.
# Adjacency Segment #
set protocols isis interface-group group1 interface ge-0/0/2.0 weight 1 <<< 1/4
set protocols isis interface-group group1 interface ge-0/0/4.0 weight 3 <<< 3/4
set protocols isis interface-group group1 level 2 ipv4-adjacency-segment protected label 600001 <<<...到这两个口都用这个...label
ge-0/0/4 in a 1:3 ratio.
# set protocols isis source-packet-routing <<< ...这一条命令就会自动为...neighbor...建立...ADJ-SID
lab@R2> show isis adjacency extensive <<< ...查...label
R1
Interface: ge-0/0/0.0, Level: 2, State: Up, Expires in 26 secs
Priority: 0, Up/Down transitions: 1, Last transition: 15:13:56 ago
Circuit type: 2, Speaks: IP, IPv6
Topologies: Unicast
Restart capable: Yes, Adjacency advertisement: Advertise
IP addresses: 10.0.0.0
IPv6 addresses: fe80::5668:a3ff:fe17:3281
Level 2 IPv4 Adj-SID: 30
Level 2 IPv6 Adj-SID: 47
State: Up
# Static Label Range #
set protocols mpls label-range static-label-range 700000 799999
# run show route table mpls.0 logical-system r2 label 66  <<<...这个命令...看label...去哪里的
mpls.0: 35 destinations, 35 routes (35 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both
66                 *[L-ISIS/14] 3d 05:44:24, metric 0
>  to 10.0.0.0 via ge-0/1/3.0, Pop
66(S=0)            *[L-ISIS/14] 3d 05:44:24, metric 0
>  to 10.0.0.0 via ge-0/1/3.0, Pop
# ...Unprotected and Protected Adjacency Segments... #
lab@R1...# set protocols isis interface ge-0/0/1.0 link-protection ... <<< this is a must
lab@R1# set protocols isis interface ge-0/0/1.0 level 2 ipv4-adjacency-segment protected dynamic
lab@R1# set protocols isis interface ge-0/0/1.0 level 2 ipv4-adjacency-segment unprotected dynamic
lab@R1# set protocols isis interface ge-0/0/1.0 level 2 ipv6-adjacency-segment protected dynamic
lab@R1# set protocols isis interface ge-0/0/1.0 level 2 ipv6-adjacency-segment unprotected dynamic
# Node Segment #
R1:
set protocols isis source-packet-routing srgb start-label 1000
set protocols isis source-packet-routing srgb index-range 9000
set protocols isis source-packet-routing node-segment ipv4-index 401
set protocols isis source-packet-routing node-segment ipv6-index 601
lab@R2> show isis overview | no-more
Source Packet Routing (SPRING): Enabled
SRGB Config Range:
SRGB Start-Label : 1000, SRGB Index-Range : 9000
SRGB Block Allocation: Success
SRGB Start Index : 1000, SRGB Size : 9000, Label-Range: [ 1000, 9999 ]
Node Segments: Enabled
Ipv4 Index : 402, Ipv6 Index : 602
# segment list #
When configured, the segment-list of a segment routing traffic engineering (SR-TE) LSP accepts IP addresses for all the hops along the path. These IP addresses can be either the loopback address of a node, or the IP address of a link, as identified by the node-type.
# Prefix-SID/Anycast-SID #
R3&R4:
set interfaces lo0 unit 0 family inet address 192.168.0.99/32 <<...给所有属于一个...prefix-sid...的...node...配相同的...2nd lo0...地址
set policy-options policy-statement ISIS-EXPORT term ANYCAST from protocol direct
set policy-options policy-statement ISIS-EXPORT term ANYCAST from interface lo0.0
set policy-options policy-statement ISIS-EXPORT term ANYCAST from route-filter 192.168.0.99/32 exact
set policy-options policy-statement ISIS-EXPORT term ANYCAST then prefix-segment index 499
set policy-options policy-statement ISIS-EXPORT term ANYCAST then accept
set protocols isis export ISIS-EXPORT
# LDP&SR Interworking #
LDP >>> SR:
lab@R6# set protocols ldp sr-mapping-client
**...要配...router-id ...否则...ldp session...起不来，除非手动配指向...physical interface address
This triggers R6 to allocate LDP labels corresponding to the node SIDs of R1, R2, R3, and R4. It advertises them to its LDP neighbors (R8 in the example network). For example, R6 allocates a label of value 29 corresponding to the loopback address of R2, 192.168.0.2. (you’ll also notice that an LDP label has been allocated for the 192.168.0.99 anycast address that was configured in Chapter 3):
*** r2...路由进入...r9...的...inet3...条件是，...inet0...里...r2...这个路由...nh...也要是从...r8...学来的
SR >>>>LDP:
The mapping... ...server can be any SR speaker in the domain. To emphasize this, let’s use R1 as the... ...mapping server, as it is nowhere near R6, R8, or R9.
<<< ...想知道在...local...设备上到...destination router push...什么标签，看...inet3.
...想知道某个...label...在...local router...到...nh router...上要做设么样的...label...行为，看...mpls.0
user@R6# set protocols isis source-packet-routing ldp-stitching
<<<...没有这个，在...r6...上...show mpls table label 1409...是没有的。加了这个之后：
-----------------
regress@R6_re0# run show route table mpls.0 label 1409
mpls.0: 64 destinations, 64 routes (64 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both
1409               *[L-ISIS/14] 00:00:04, metric 20
>  to 10.0.0.17 via ge-0/0/1.0, Swap 21... <<<<
---------------
user@R6> show route table mpls.0
<snip>
1408 *[L-ISIS/14] 00:00:05, metric 1000
> to 10.0.0.17 via ge-0/0/1.0, Pop
1408(S=0) *[L-ISIS/14] 00:00:05, metric 1000
> to 10.0.0.17 via ge-0/0/1.0, Pop
1409 *[L-ISIS/14] 00:00:05, metric 2000
> to 10.0.0.17 via ge-0/0/1.0, Swap 54
This triggers R6 to install entries in the mpls.0 table, to stitch incoming SR labels... ...corresponding to R8 and R9 to the corresponding LDP label values. R8 advertised... ...label 3 to R9 (penultimate hop-popping), hence the “pop” action for label 1408.... ...R8 advertised label 54 for the loopback address of R9, hence the incoming label... ...value of 1409 is swapped for label value 54:
# TI-LFA #
In both cases (RSVP 1:1 and SPRING) the shortest... ...post-convergence backup path can be created... ...(enforced) in any arbitrary topology– it is topology independent. Therefore, in SPRING terminology,... ...this kind of protection... ...is called Topology Independent Loop Free Alternate (TI-LFA).
How can the post-convergence path be calculated? Very simply: just use the for the... ...SPF calculation modified link state database (LSDB), where the network resource... ...(for example, link or node) being protected is removed from the LSDB.
legacy LFA (RLFA):  (node-link-protection)
r1...有两个...nh...可选，...r3...和...r2...，它会计算从这个...nh router(r2/r3)...到...destination router...的...cost...，...r3-r9...是...3000...，而...r2-r9...是...3500...，所以他会选从...R3...走
use-source-packet-routing...:... configuration knob enables SPRING-based... ...LFA protection for prefixes in inet.0 (plain IPv4 traffic) and inet6.0 (plain IPv6
traffic) RIBs. Without this knob being configured, only SPRING prefixes in inet.3... ...(ingress labeled IPv4 traffic), inet6.3 (ingress labeled IPv6 traffic), and mpls.0... ...(transit labeled traffic) RIBs are subject for SPRING-based LFA protection.
<<< legcy LFA...是从...r3/r2(...就是...r4...如果...fail...可以改走的...router...开始计算到...egress router...的...metric...，这个计算和...ingress router...无关了...)
TI-LFA:
...他是算从...source router(r1)...到...destination...经过两个...nh...的总...cost...，...r1-r3-r9...是...5000...，而...r1-r2-r9...是...4500...，所以选从...r2...走
TI-LFA, by default, uses link-protection backup next hops.
To use node protection with TI-LFA, you must enable it on the IGP interface level:
# ...inherit-label-nexthops ...#
inherit- 
Inherit label next hops for first hop in this segment list that have both IP address and label configured in the first 
label- 
hop. 
nexthops 
You can configure the inherit-label-nexthops statement globally or individually for each segment list. 
The inherit-label-nexthops statement takes effect only when the segment list first hop has both IP address and 
SID label present. 
If the inherit-label-nexthops is not configured at the [edit protocols source-packet-routing segment-list] 
hierarchy, and the first hop in the segment list has both IP address and label specified, the default behavior is to 
use the IP address. ...inherit- 
Inherit label next hops for first hop in this segment list that have both IP address and label configured in the first 
label- 
hop. 
nexthops 
You can configure the inherit-label-nexthops statement globally or individually for each segment list. 
The inherit-label-nexthops statement takes effect only when the segment list first hop has both IP address and 
SID label present. 
If the inherit-label-nexthops is not configured at the [edit protocols source-packet-routing segment-list] 
hierarchy, and the first hop in the segment list has both IP address and label specified, the default behavior is to 
use the IP address.
inherit-label-nexthops  Inherit label nexthops for first hop in segment lists
# l3-unicast-topology #
# set protocols isis traffic-engineering l3-unicast-topology
-- Download IGP topology into TED
https://juniper.lightning.force.com/lightning/r/Knowledge__kav/ka0Dp000000jRl3IAE/view...
For ipv4 prefix (from ISIS TLV135), ISIS will install only prefixes having SIDs attached into TED. You may see 
the list by "show ted ipv4-prefix" cli command. 
For ipv6 prefix (from ISIS TLV236), ISIS will install all prefixes into TED. You may see the list by "show ted 
ipv6-prefix" cli command. ...For ipv4 prefix (from ISIS TLV135), ISIS will install only prefixes having SIDs attached into TED. You may see 
the list by "show ted ipv4-prefix" cli command. 
For ipv6 prefix (from ISIS TLV236), ISIS will install all prefixes into TED. You may see the list by "show ted 
ipv6-prefix" cli command.
annaw@lab-singtel-mx10003-re0> show ted ipv4-prefix
Prefix                Node
100.2.1.1/32           lab-mx240-3d-02-RR-re0.00(101.234.3.128)
100.2.2.1/32           lab-mx240-3d-02-RR-re0.00(101.234.3.128)
101.234.3.128/32       lab-mx240-3d-02-RR-re0.00(101.234.3.128)
101.234.3.130/32       lab-mx480-3d-02-re0.00(101.234.3.130)
101.234.3.131/32       lab-mx204-01.00(101.234.3.131)
101.234.3.132/32       lab-mx204-02.00(101.234.3.132)
101.234.3.135/32       lab-mx240-3d-01-re0.00(101.234.3.135)
101.234.3.136/32       lab-singtel-mx10003-re0.00(101.234.3.136)
By enabling the knob of 'set protocols isis traffic-engineering l3-unicast-topology', it downloads isis IGP topology into TED including nodes, links and prefixes.
# set protocols mpls traffic-engineering database import igp-topology bgp-link-state
-- Copy IGP topology from TED into lsdist.0
