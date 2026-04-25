# ISIS

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

ISIS
# 常用命令... #
<ISIS Sample Configuration>
-...所有...interface...起...iso
groups {
iso {
interfaces {
<*> {
unit <*> {
family iso;
}
}
}
}
}
apply-groups iso;
-lo0...或任意接口设一个...ENT address
[edit interfaces lo0 unit 0]
root# show
family inet {
address 1.1.1.1/32;
}
family iso {
address 49.0001.1000.1000.1001.00; <<<
}
# Level:
- ISIS 用LSP （类似于ospf的LSA）来描述自己的直连链路。LSP分类：
L1：在区域内邻居间传递
L2：区域间
-默认开启isis的接口，都是L1&L2,可以用命令disable
- L1的external是160，当这条被leak进L2 router的时候，就变成165
L1 internal 15  L2 internal 18
L1 external 160 L2 external 165
1、 运行level-1的路由器类似于非骨干路由器
2、 运行level-2的路由器类似于骨干路由器
3、 Level 1的路由默认都会通告进level 2
4、 默认情况下，level 2的路由是不会注入到level 1的（如果要注入，需要做路由泄露）
5、 外部L1的路由默认也不会被注入到level 2（如果要注入，要么做路由泄露，要么做wide-metric,metric可以大于dafaut的63限制）
6、 外部路由条目可以被通告进Level 1
7、 Level 1的路由器不可以穿越区域传输。而Level 2的路由器可以。
8、 和level 1相连的路由器，他们之间的area ID要一样，和level 2相连的路由器，他们之间的area ID可以不一样。
9、 L1-L2 Att位置位的含义：L1路由器会在本地产生一条默认路由指向离自己最近的L2路由器。
L1：
1. L1 会把自己的isis路由发给 other L1和L12 routers
2. L1不会吧自己的external protocol路由发给 L12 & L2 routers， 需要用policy实现.L12就会有L1的external路由,然后他传给L2。
L12：
1.会把L1的internal路由发给L2
2.不会把L1 level的external路由发给L2 （有一种例外）
3.不会把L2的路由发给L1，但会自动生成一条0/0路由让L1指向自己从而L1可以到达L2和以外区域
L2：
1.只会发给...L12 router...的...L2
# Address:
NSAP:49.0001(区域号)+ 系统ID
#ISIS 状态机：
1.new：开始isis邻接进程
2.one-way: 发hello包
3.Initializing： 收到对方的hello
4.up： 邻接关系建立，开始交换LSP
5.reject: 认证失败，接下来进入down的状态
6.down： area，hold-time，认证失败等
-up之后交换LSP:
1.所有routers发送CSNP(包含LSDB,LSP的序列号和生存时间)
2.比较CSNP，本地没有的发送PSNP去请求
3.对方router会回link-state PDU（LSP）
4.local router收到后会回一个LSP确认收到
-CSNP:完整lsdb，用于广播链路。PSNP：点对点请求lsp。用于点对点
-LSP:Link-state PDU。PDU:协议数据单元。OSI每层都用PDU传输信息:
物理层：bit    数据链路层：frame 网络层：packet   传输层：segment   其他更高层：message
#DIS
- 作用是减小...LSP ...database的大小，他是虚拟出来的。DIS会代表伪结点，给相连的isis routers 发LSP with metric 0，而相连isis router给DIS发LSP metric 10.... ...这样就形成了一个以DIS为中心的star topology。
-选举：优先级大优，相同的话比较MAC地址大优。DIS会抢占。
-L1和L2都会选举DIS，但有可能是同一台router。
# ISIS和OSPF的区别：
1.isis选DIS没有备份，看优先级和mac，而ospf有DR/BDR，看router-id
2.isis使用LSP，而ospf用LSA。ospf的LSA泛洪是在一个area里的，而isis是在一个level里的
3.isis更新用TLV，扩展性好，如果另一个协议也用TLV，那么和isis共通的。ospf只能用LSA，其他协议不可能共通。
4.isis不依赖IP，也就是即使不在一个网段isis也可以up。但ospf必须依赖ip。isis carry ip inside TLV，所以即使两端ping不通，也不影响isis起来
5.isis不像ospf有v2 v3，它的v4和v6共用一个database算路径，容易造成v6 ping不通，所以要配这个。
set protocols isis topologies ipv6-unicast
#TS
1.检查物理层和数据层
2.查area mismatch
3.mtu
4.net family, lo0.0没划进isis
- traceoption： flag error， hello， lsp detail
# Area
-OSPF的LSA泛洪是控制在一个area里的，然而ISIS的泛洪是控制在一个level里的。
-和L1 router相连的区域ID要一样，和L2相连的他们的area id可以不一样。
-level是由interface决定的，而area是由lo0的net地址决定的
# Wide-metric #
use TLV 135, does not contain internal/external bit.把整个isis router看成一个L1或L2。
# ...改...metric #
# set protocols isis interface ge-0/0/1 level 2 metric 2000
<<< isis default metric is 10, ...用这个改
# ...算...isis metric #
labroot@jtac-mx480-r2016-re0# run show isis database logical-system r2 detail
jtac-mx480-r2016-re0-r2.00-00 Sequence: 0x8, Checksum: 0x69f, Lifetime: 982 secs
IS neighbor: jtac-mx480-r2016-re0-r2.02    Metric:       10
IS neighbor: jtac-mx480-r2016-re0-r3.02    Metric:       63 <<<...因为不是...wide-metric...只能到...63
IP prefix: 10.10.10.0/24                   Metric:       10 Internal Up
IP prefix: 40.40.40.0/24                   Metric:       63 Internal Up
<< ...要找...r2...的，因为...r1...到...r3...经过...r2...，... metric...和是...73
[edit logical-systems r2]
labroot@jtac-mx480-r2016-re0# set protocols isis level 2 wide-metrics-only
jtac-mx480-r2016-re0-r2.00-00 Sequence: 0x9, Checksum: 0xde34, Lifetime: 1165 secs
IS neighbor: jtac-mx480-r2016-re0-r2.02    Metric:       10
IS neighbor: jtac-mx480-r2016-re0-r3.02    Metric:      300
IP prefix: 10.10.10.0/24                   Metric:       10 Internal Up
IP prefix: 40.40.40.0/24                   Metric:      300 Internal Up
<<< metric...是...310
# ISIS Family #
2022-0801-520577
如果...localinterface...开启...inet6...对端没开启，...isis...会...down
<<<
<<<
ISIS packet from our device to peer COREZT41.LAB does support IPV6:
<<<
# ...抓包... #
=================================================================
<Command>
regress@R1# run show interfaces terse lo0.0
Interface               Admin Link Proto    Local                 Remote
lo0.0                   up    up   inet    172.27.255.1        --> 0/0
iso      49.0001.0172.0027.2551
regress@R1# run show isis adjacency
Interface             System         L State        Hold (secs) SNPA
ae1.0                 R4             1  Up                   26  0:5:86:1e:d:c1
ge-0/0/3.0            R2             1  Up                    8  0:5:86:7e:cc:1
ge-0/0/6.0            R3             1  Up                    7  0:5:86:6e:ed:1
[edit protocols isis]
lab@R1# run show route protocol isis
# passive (Protocols IS-IS) #
https://www.juniper.net/documentation/us/en/software/junos/is-is/topics/ref/statement/passive-edit-protocols-isis.html
passive {    remote-node-id address;    remote-node-iso iso-id;}
Advertise the direct interface addresses on an interface or into a level on the interface without actually running IS-IS on that interface or level.
# Overload #
Configure the local routing device so that it appears to be overloaded. This statement causes the routing device to continue participating in IS-IS routing, but prevents it from being used for transit traffic. Traffic destined to immediately attached subnets continues to transit the routing device.
https://www.juniper.net/documentation/us/en/software/junos/is-is/topics/ref/statement/overload-edit-protocols-isis.html
<<< isis 不断...，但穿越流量不从这台走了
[edit routing-instances routing-instance-name protocols isis]
=================================================================
<JNCIA>
Router - IS
Host - ES
**ISIS...既支持...CLNP...也支持...IP. Junos...只支持...IP
- ...在一同一个...level...的...routers...的...LSDB...是相同的。
- Level...表示一个任意的边界或者路由器组。... ISIS ...用...LSP ...（类似于...ospf...的...LSA...）来描述自己的直连链路。...LSP...分类：
L1...：在区域内邻居间传递
L2...：区域间
-Address:
ISIS ...用...CLNS...地址唯一标识一台路由器。路由器使用的...clns...地址称为...NSAP
-...状态机：
New...：开始...isis...邻接进程
regress@MX1_re0# show interfaces ge-0/0/1
unit 0 {
family inet {
address 10.10.10.1/24;
}
family iso;  <<< interfac...需要... ...这个建立...isis
}
命令：
>show isis adjacency
>show isis interface
>show isis spf log
>show isis statistics
>show isis route
>show isis database
-...和...isis...不同点：
##ISIS ...不像...ospf...，他不依赖...IP...。他是...carry ip...信息在...TLV...里面。所以...isis...仍可以...up...，及时两端...interface...的...ip...不在一个网段。
<JIR - ISIS>
-ISIS ...路由器互换...link-state PDUs
-PDU:...协议数据单元。...OSI...每层都用...PDU...传输信息。包含来自上层的和本层的信息。
物理层：...bit    ...数据链路层：...frame
网络层：...packet   ...传输层：...segment   ...其他更高层：...message
-ISIS use TLV...，扩展性号，支持更多协议。而...ospf...多支持一个协议就要加一种类型的...LSA.
-ISIS PDUs:
1.Hello
Discover neighbor and keepalives
LAN & P-to-P
DIS:3s  non-DIS:9s
2.Link State:
Include adjance to neighbor ISIS system.
Used to bulild link-state database. Same as LSA in OSPF.
3.CSNP:
Includes complete description of all link-state ISIS system. (similar as LSDB)
Broadcast all link-state PDUs.
ISIS support TE:
https://kb.juniper.net/InfoCenter/index?page=content&id=KB25145
https://www.juniper.net/documentation/en_US/junos/topics/reference/configuration-statement/wide-metrics-only-edit-protocols-isis.html
•By default, traffic engineering is enabled in Junos for the ISIS protocols.
•By default, Junos generates and accepts both wide and narrow style TLV`s.
•When ISIS wide-metrics-only is configured for a level or interface, TLV`s 2 and 128 are suppressed from being sent by the local router.
•When ISIS Traffic Engineering is disabled, TLV`s 22, 134, and 135 are suppressed from being sent.
====================================
<Justin -  JIR ISIS>
- Use CSPF
-Change link-state PDUs, the PDU is layer 2  protocol, not IP
-ES(End-system): host…
IS(Intermediate system): routers
ISIS Area
*area divided by link not router. One router will be purely belong to one area.
*Level1 router within one area, Level 2 router has links connecting to other area.
<JNCIE>
By default, Level 1 routes (internal) are advertised to any Level 2 router. You must restrict this default behavior by employing some form of restrictive route leaking. This restrictive route leaking must occur on the border routers R3 and R4. An export policy must be configured that stops the advertisement of R1’s and R2’s loopback addresses into Level 2. Then, on R3 and R4, you must create and inject an aggregate route into Level 2 that represents those loopback addresses. Although the task does not specify the route leaking direction, it is recommended to create a policy that uses the to level option. This option directs which level the policy leaks routes to. This helps clarify the policy and reduces unnecessary LSP flooding that can occur.
By default, L2 routes cannot inject to L1 automatically.
By default, the Junos OS does not flood Level 1 external routes to Level 2 routers. R5 does not receive these routes and no action is required to accomplish this part of the task. However, you must create aggregate routes on R3 and R4, which represents these routes, and flood these aggregate routes into Level 2, which then allows R5 to reach these destinations.
By default, Level 2 external routes do not flood to Level 1 routers. You must adjust the route leaking policy on R4 to allow the flooding of this route from R4 to the Level 1 routers; R1 and R2.
=============================================
IS-IS的level
1、 运行level-1的路由器类似于非骨干路由器
2、 运行level-2的路由器类似于骨干路由器
3、 Level 1的路由默认都会通告进level 2
4、 默认情况下，level 2的路由是不会注入到level 1的（如果要注入，需要做路由泄露）
5、 外部L1的路由默认也不会被注入到level 2（如果要注入，要么做路由泄露，要么做宽度量）
6、 外部路由条目可以被通告进Level 1
7、 Level 1的路由器不可以穿越区域传输。而Level 2的路由器可以。
8、 和level 1相连的路由器，他们之间的区域ID要一样，和level 2相连的路由器，他们之间的区域ID可以不一样。
9、 L1-L2 Att位置位的含义：L1路由器会在本地产生一条默认路由指向离自己最近的L2路由器。
=======================================
Area...：
--OSPF...的...LSA...泛洪是控制在一个...area...里的，然而...ISIS...的泛洪是控制在一个...level...里的。
--...一台...router...最多可以配...3...个区域。
--...和...L1 router...相连的区域...ID...要一样，和...L2...相连的他们的...area id...可以不一样。
--...一般...lo0...放进骨干区域，...L2 area...。
--ISIS...没有...import...都是...export...。
--Local L1...路由会被通告给...L2...。
--...只有...L2...的路由会被转发到区域以外。
--L12router...不会将...L2...路由转化成...L1...路由，... ...而是...L1 router...会生自动成一条...0/0...路由指向...L1/L2 router...。这样...L1 router...就可以通过到达...L12router...来到达...L2...了。
>>>>...这个...0/0...路由的产生，是所有...L1 router...都会自动...att bit set...。
>>>>...然而，如果...L1 router...需要...L2...的细节路由而不是...0/0...，那么需要...L12router...上作路由泄露！
"set policy to level 1...“
--L1...的...external...路由（在...L1L2router...上...show...是...160...）是不会被注入...L12...的。...solution...：
1....需要路由泄露。
2.wide-metric...：相当于他的...L1area...扩张到把整个...ISIS area...，那么...L1 external...就可以进...L12 router...了。
...在...L1router...上作。
Route Summarize:
--...用一个...aggregate route...在...L12...路由器上汇总从...L1...来的路由，再发给...L2
===========================================
[edit protocols isis]
regress@R1# run show log isis-adj-issue.log | match ge-0/0/3
Nov 22 04:58:32.837309 ISIS L1 periodic xmit to 01:80:c2:00:00:14 interface ge-0/0/3.0
Nov 22 04:58:34.266660 Received L1 LAN IIH, source id 0172.0027.2554 on ge-0/0/3.0
Nov 22 04:58:34.267183 ERROR: IIH from 0172.0027.2554 with no matching areas, interface ge-0/0/3.0
Nov 22 04:58:34.716431 ISIS L1 periodic xmit to 01:80:c2:00:00:14 interface ge-0/0/3.0
Nov 22 04:58:36.136957 Received L1 LAN IIH, source id 0172.0027.2554 on ge-0/0/3.0
Nov 22 04:58:36.137087 ERROR: IIH from 0172.0027.2554 with no matching areas, interface ge-0/0/3.0
Nov 22 04:58:36.291420 ISIS L1 periodic xmit to 01:80:c2:00:00:14 interface ge-0/0/3.0
Nov 22 04:58:37.906440 ISIS L1 periodic xmit to 01:80:c2:00:00:14 interface ge-0/0/3.0
Nov 22 04:58:37.911134 Received L1 LAN IIH, source id 0172.0027.2554 on ge-0/0/3.0
Nov 22 04:58:37.911199 ERROR: IIH from 0172.0027.2554 with no matching areas, interface ge-0/0/3.0
>>> IIH is hello packet
============================================
Pseudo-Node:
On LAN networks the DIS is used to minimize the link-state database size by using the concept of Pseudo-Node, and reduce the number of acknowledgment messages “PSNPs”.
Since the IS-IS communication between routers on the broadcast networks happening through multicast MAC addressed all routers will form a full-mesh of adjacencies with each others, so if we have 3 routers on the same LAN each router will reports that he has adjacency with 2 routers which ultimately makes the link-state topology  appears like as 6 adjacency which complex the SPF task, as a solution for such scenario the DIS creates a logical router called Pseudo-node who makes the network appears like a star topology instead of full-mesh, then on behave the Pseudo-Node, the DIS sends Level-1 and Level-2 Pseudo-Node LSPs, each to the appropriate Level  destination MAC address which instructs the the IS-IS routers that the Pseudo-Node has adjacency to each router on the LAN including the DIS itself with metric 0, on other hand each router send in its LSPs that he has adjacency to the Pseudo-Node only with metric 10, so now the network is appeared like centralized router which is the Pseudo-Node and all routers are connected to this router which is ultimately reduce the link-state database size because visually instead of 3 routers and 6 adjacencies we have 3 routers with 3 adjacencies, this benefit become significant in a LAN network with over 20 routers running IS-IS, Calculate it :)
=================================================
