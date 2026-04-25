# OSPF

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

OSPF
# external metric
1. type 1:
Internal cost + external link cost
2. type 2:
User only external cost....就是如果一個...ASBR...发进...area...的...external route...的...metric...是...n...，那么传给其他...router...的这个...external metric...都是...n...，而不需要加上中间的...link cost metric
Default ...是...type 2...，如果有...type1...，那么...type1 ...比...type 2 ...优。
如果只有...type 2...，那么...metric...最小的优。
# Area
-作用：
减小scalability，LSA的泛洪被限制在一个区域内，同一area里的DD必须是一致的。
所有其他area必须都要与area0相连。如果没连，可以用virtual-link来解决。
- types:
NSSA:1237,ABR上7转5。可以到其他area，external路由也可以发给其他area。
Totally NSSA: NSSA + no-summary, 127 ABR上7转5
Totally NSSA + default-metric:127+0/0，ABR上7转5。还是会有一个3类的0/0，如果加lsa-type 7，这个0/0就变成7类。注意3类pref是10，但7类是150.
Stub: 123,没有5类，所以stub不可能连着其他协议
totally stub：stub + no-summary, 12 无法到达本区域之外
totally stub + default metric: 12 + 0/0
- NSSA:
NSSA存在的作用是为了不让external路由进入本area，而不是要阻止本area的external发给其他area，这就是为什么可以在ABR上7转5的原因。
ASBR是负责把external路由引进该区域，而真正把这个external路由传出去给其他area的是ABR。
NSSA area-range把external路由summary成一条在ABR上发给其他area。所以no-summary，default-metric只需要在ABR上配。
# LSA
Information in the link-state database is populated through a Link State Advertisement (LSA).
Each LSA contains routing, metric, and topology information to describe a portion of the OSPF network.
1—Router LSA:
describe interface and neighbors of each OSPF router within  same area
2—Network LSA: send by DR
DR发送同一area内其他router的信息。
只有在同一个网段内的多于2个routers才会选举DR.所有其他router都要和DR家里连接，DR handle所有LSA.
其他routers用组播224.0.0.6这个地址给DR发信息。是在同一area内泛洪的。BDR也和其他所有routers建立连接，他监听224.0.0.6，
DR是谁的ospf先起来，谁当。...同时...的话，ip大的当。DR不抢占。
All routers listen to 224.0.0.5, only DR&BDR listen to 224.0.0.6.
3—Network summary LSA & 4—ASBR summary LSA： by ABR
ABR把内部LSA发给backbone和其他area的ABR。这样area之间就可以互通。谁的router id大谁当ABR.
5—AS external LSA & 6—Group membership LSA： by ASBR
出去external
7—NSSA external LSA
8—External attributes LSA
9—Opaque LSA (link-local scope)
10—Opaque LSA (area scope)
11—Opaque LSA (AS scope)
<<<...如果一个...LSA length...很长，可能是经过的接口多或者有很多...virtual link
<<<...相同的...LSA...可能从若干个邻居收到，但是只会选一个邻居的，从其他邻居收到的...LSA...会被...discard
<<<A...从...B C...都学到...D...的...LSA...，...A...选择从...B...学习。然后...A...会计算到...B...的路由，计算出...A-C-D,...从...C...走。所以...OSPF...的...lsa...这种...host-bond...的包不一定会从路由出口走。
# OSPF Packet Types
-Hello：discover neighbor。default 10s，dead 40s。
-Database Description (DD)：
Summarizes the local database by sending LSA headers to the remote router.
The remote router analyzes these headers to determine whether it lacks any information within its own copy of the link-state database.
DD only used during adjacency formation.
-Link-State Request Packet(LSR):
During the database synchronization process, the local router may find that it is missing information or that its local copy is out of date. The local router acquires the needed database information by sending LSR.
-Link-State Update Packet (LSU):
router advertises LSAs within a LSU packet and it also replies to LSR.
# Status
-Down
-Init:
sent hello
-Attempt:
only for Non-Broadcast Multi-Access (NBMA) networks. a hello packet has not been received from the neighbor and the local router is going to send a Unicast hello packet to that neighbor within the specified hello interval period.
-2-Way:
received hello, bidirectional communication established
-ExStart:
start of database synchronization. elect DR
-Exchange:
LSA exchange
-Loading:
finishing sending, but still in the process of receiving
-Full:
adjancy full and database completely snyced.
#LSDB
装的是LSA。originator sends LSA every 1800s. If LSA has a age time of 3600s,means this LSA has no refresh in the last 3600s and will be deleted.
# OSPF Path Selection
A)
When there are multiple routes available to the same network with different route types, routers use this order of preference (from highest to lowest):
1. Intra-area routes.
2. Inter-area routes.
3.External Type-1 routes.
4. External Type-2 routes.
B)
If there are multiple routes to a network with the same route type, the OSPF metric calculated as cost based on the bandwidth is used for selecting the best route. The route with the lowest value for cost is chosen as the best route.
C)
If there are multiple routes to a network with the same route type and cost, it chooses all the routes to be installed in the routing table, and the router does equal cost load balancing across multiple paths.
https://www.juniper.net/documentation/us/en/software/junos/ospf/topics/topic-map/ospf-overview.html
#VL
非...backbone...的...area...必须有一个...interface...在...area0...里，这样...area0...就可以到达这个区域。如果没有，即使他的...LSA...在...area0 database...里，因为...area0...无法到达他，这些...LSA...还是传不出去给其他...area...。
# Policy #
By default, OSPF export policies reject network-summary LSAs.
By default, OSPF import policies accept network-summary LSAs.
# Install LSA type 3, LSA type 5 and LSA type 7 OSPF routes in the VRF #
https://juniper.lightning.force.com/lightning/r/Knowledge__kav/ka03c0000009TyNAAU/view
How to install LSA type 3, LSA type 5 and LSA type 7 OSPF routes in the VRF routing table
If an OSPF process is performed in a routing instance type VRF, then the router automatically and unconditionally considers itself to be an ABR. The router believes to be connected to a so-called MPLS backbone (even though there may be no BGP/MPLS configured on the router at all). This may pose as problems if such a router is actually a part of a network that uses multiple areas.
In order to suppress the provider edge (PE) specific checks on a router when the OSPF process is associated with the VPN routing and forwarding instance (VRF), it is needed to disable domain-ID under OSPF configuration on PE router.
=============================================
regress@R1# run show ospf interface
Interface           State   Area            DR ID           BDR ID          Nbrs
ae1.0               PtToPt  0.0.0.0         0.0.0.0         0.0.0.0            1
ge-0/0/6.0          PtToPt  0.0.0.0         0.0.0.0         0.0.0.0            1
lo0.0               DRother 0.0.0.0         0.0.0.0         0.0.0.0            0
<<<< ...看该...router...是...ospf...的...DR...或其他
==============================
-----------------------------
STUB: 去掉5类无法到达external，还是有3类到达其他area。
STUB+NO SUMMARY: 去掉3,5类，无法到达其他area和external
NSSA(not so stub)：有3类可以到达其他area，但把5类转化为7类NSSA来到达external
NSSA+NO SUMMARY:去掉3类，剩下7类，所以没办法到达其他area
NSSA+no-summary+default lsa：有7类和一条default 3类到达其他area
NSSA+no-summary+default lsa+lsa type 7：把default 3类变成7类，所以只剩下7类没有3,5
------------------------
STUB:
[edit protocols ospf area 0.0.0.2]
root@R3# show
stub;
interface ge-0/0/3.0 {
interface-type p2p;
}
OSPF database, Area 0.0.0.2
Type       ID               Adv Rtr           Seq      Age  Opt  Cksum  Len
Router  *172.27.255.3     172.27.255.3     0x80000001     7  0x20 0x4f71  36
Summary *172.27.0.0       172.27.255.3     0x80000001     6  0x20 0x8eed  28
Summary *172.27.0.4       172.27.255.3     0x80000001     6  0x20 0x1189  28
Summary *172.27.0.8       172.27.255.3     0x80000001     6  0x20 0x434a  28
Summary *172.27.0.12      172.27.255.3     0x80000001     6  0x20 0x2082  28
Summary *172.27.0.16      172.27.255.3     0x80000001     6  0x20 0xf7a6  28
Summary *172.27.0.20      172.27.255.3     0x80000001     6  0x20 0xcab6  28
Summary *172.27.0.36      172.27.255.3     0x80000001     6  0x20 0xc582  28
Summary *172.27.0.56      172.27.255.3     0x80000001     6  0x20 0x57d3  28
Summary *172.27.255.1     172.27.255.3     0x80000001     6  0x20 0xa00a  28
Summary *172.27.255.2     172.27.255.3     0x80000001     6  0x20 0x3762  28
Summary *172.27.255.3     172.27.255.3     0x80000001     6  0x20 0x8227  28
Summary *172.27.255.4     172.27.255.3     0x80000001     6  0x20 0x8225  28
Summary *172.27.255.5     172.27.255.3     0x80000001     6  0x20 0x731a  28
OSPF AS SCOPE link state database
Type       ID               Adv Rtr           Seq      Age  Opt  Cksum  Len
Extern   10.22.0.0        172.27.255.5     0x80000001   504  0x22 0x4983  36
Extern  *10.22.1.0        172.27.255.3     0x80000002    17  0x22 0x7f44  36
Extern  *10.22.2.0        172.27.255.3     0x80000001   742  0x22 0x764d  36
Extern  *10.22.3.0        172.27.255.3     0x80000001   742  0x22 0x6b57  36
Extern  *10.22.4.0        172.27.255.3     0x80000001   742  0x22 0x6061  36
Extern  *10.22.5.0        172.27.255.3     0x80000001   742  0x22 0x556b  36
Extern  *10.22.6.0        172.27.255.3     0x80000001   742  0x22 0x4a75  36
Extern  *10.22.7.0        172.27.255.3     0x80000001   742  0x22 0x3f7f  36
Extern   172.16.16.0      172.27.255.1     0x80000001  1025  0x22 0xb66d  36
《《《《没有...default...的...external LSA...存在
---------------------
STUB no summary:
[edit protocols ospf area 0.0.0.2]
root@R3# show
stub no-summaries;
interface ge-0/0/3.0 {
interface-type p2p;
}
OSPF database, Area 0.0.0.2
Type       ID               Adv Rtr           Seq      Age  Opt  Cksum  Len
Router  *172.27.255.3     172.27.255.3     0x80000001    25  0x20 0x4f71  36
OSPF AS SCOPE link state database
Type       ID               Adv Rtr           Seq      Age  Opt  Cksum  Len
Extern   10.22.0.0        172.27.255.5     0x80000001   613  0x22 0x4983  36
Extern  *10.22.1.0        172.27.255.3     0x80000002   126  0x22 0x7f44  36
Extern  *10.22.2.0        172.27.255.3     0x80000001   851  0x22 0x764d  36
Extern  *10.22.3.0        172.27.255.3     0x80000001   851  0x22 0x6b57  36
Extern  *10.22.4.0        172.27.255.3     0x80000001   851  0x22 0x6061  36
Extern  *10.22.5.0        172.27.255.3     0x80000001   851  0x22 0x556b  36
Extern  *10.22.6.0        172.27.255.3     0x80000001   851  0x22 0x4a75  36
Extern  *10.22.7.0        172.27.255.3     0x80000001   851  0x22 0x3f7f  36
Extern   172.16.16.0      172.27.255.1     0x80000001  1134  0x22 0xb66d  36
<<<<only type 1
----------------------------------------
NSSA:...（...not so stub area...）
[edit protocols ospf area 0.0.0.2]
root@R3# show
nssa;
interface ge-0/0/3.0 {
interface-type p2p;
}
OSPF database, Area 0.0.0.2
Type       ID               Adv Rtr           Seq      Age  Opt  Cksum  Len
Router  *172.27.255.3     172.27.255.3     0x80000001    18  0x20 0x5569  36
Summary *172.27.0.0       172.27.255.3     0x80000001    18  0x20 0x8eed  28
Summary *172.27.0.4       172.27.255.3     0x80000001    18  0x20 0x1189  28
Summary *172.27.0.8       172.27.255.3     0x80000001    18  0x20 0x434a  28
Summary *172.27.0.12      172.27.255.3     0x80000001    18  0x20 0x2082  28
Summary *172.27.0.16      172.27.255.3     0x80000001    18  0x20 0xf7a6  28
Summary *172.27.0.20      172.27.255.3     0x80000001    18  0x20 0xcab6  28
Summary *172.27.0.36      172.27.255.3     0x80000001    18  0x20 0xc582  28
Summary *172.27.0.56      172.27.255.3     0x80000001    18  0x20 0x57d3  28
Summary *172.27.255.1     172.27.255.3     0x80000001    18  0x20 0xa00a  28
Summary *172.27.255.2     172.27.255.3     0x80000001    18  0x20 0x3762  28
Summary *172.27.255.3     172.27.255.3     0x80000001    18  0x20 0x8227  28
Summary *172.27.255.4     172.27.255.3     0x80000001    18  0x20 0x8225  28
Summary *172.27.255.5     172.27.255.3     0x80000001    18  0x20 0x731a  28
NSSA    *10.22.1.0        172.27.255.3     0x80000001    18  0x20 0x8341  36
NSSA    *10.22.2.0        172.27.255.3     0x80000001    18  0x20 0x784b  36
NSSA    *10.22.3.0        172.27.255.3     0x80000001    18  0x20 0x6d55  36
NSSA    *10.22.4.0        172.27.255.3     0x80000001    18  0x20 0x625f  36
NSSA    *10.22.5.0        172.27.255.3     0x80000001    18  0x20 0x5769  36
NSSA    *10.22.6.0        172.27.255.3     0x80000001    18  0x20 0x4c73  36
NSSA    *10.22.7.0        172.27.255.3     0x80000001    18  0x20 0x417d  36
OSPF AS SCOPE link state database
Type       ID               Adv Rtr           Seq      Age  Opt  Cksum  Len
Extern   10.22.0.0        172.27.255.5     0x80000001   680  0x22 0x4983  36
Extern  *10.22.1.0        172.27.255.3     0x80000002   193  0x22 0x7f44  36
Extern  *10.22.2.0        172.27.255.3     0x80000002    30  0x22 0x744e  36
Extern  *10.22.3.0        172.27.255.3     0x80000001   918  0x22 0x6b57  36
Extern  *10.22.4.0        172.27.255.3     0x80000001   918  0x22 0x6061  36
Extern  *10.22.5.0        172.27.255.3     0x80000001   918  0x22 0x556b  36
Extern  *10.22.6.0        172.27.255.3     0x80000001   918  0x22 0x4a75  36
Extern  *10.22.7.0        172.27.255.3     0x80000001   918  0x22 0x3f7f  36
Extern   172.16.16.0      172.27.255.1     0x80000001  1201  0x22 0xb66d  36
<<<<default external routes z...作为...nssa...出现在...area2...，但是还有...summary
------------------------
NSSA+no-summary
[edit protocols ospf area 0.0.0.2]
root@R3# show
nssa no-summaries;
interface ge-0/0/3.0 {
interface-type p2p;
}
OSPF database, Area 0.0.0.2
Type       ID               Adv Rtr           Seq      Age  Opt  Cksum  Len
Router  *172.27.255.3     172.27.255.3     0x80000001    21  0x20 0x5569  36
NSSA    *10.22.1.0        172.27.255.3     0x80000001    21  0x20 0x8341  36
NSSA    *10.22.2.0        172.27.255.3     0x80000001    21  0x20 0x784b  36
NSSA    *10.22.3.0        172.27.255.3     0x80000001    21  0x20 0x6d55  36
NSSA    *10.22.4.0        172.27.255.3     0x80000001    21  0x20 0x625f  36
NSSA    *10.22.5.0        172.27.255.3     0x80000001    21  0x20 0x5769  36
NSSA    *10.22.6.0        172.27.255.3     0x80000001    21  0x20 0x4c73  36
NSSA    *10.22.7.0        172.27.255.3     0x80000001    21  0x20 0x417d  36
OSPF AS SCOPE link state database
Type       ID               Adv Rtr           Seq      Age  Opt  Cksum  Len
Extern   10.22.0.0        172.27.255.5     0x80000001   789  0x22 0x4983  36
Extern  *10.22.1.0        172.27.255.3     0x80000002   302  0x22 0x7f44  36
Extern  *10.22.2.0        172.27.255.3     0x80000002   139  0x22 0x744e  36
Extern  *10.22.3.0        172.27.255.3     0x80000001  1027  0x22 0x6b57  36
Extern  *10.22.4.0        172.27.255.3     0x80000001  1027  0x22 0x6061  36
Extern  *10.22.5.0        172.27.255.3     0x80000001  1027  0x22 0x556b  36
Extern  *10.22.6.0        172.27.255.3     0x80000001  1027  0x22 0x4a75  36
Extern  *10.22.7.0        172.27.255.3     0x80000001  1027  0x22 0x3f7f  36
Extern   172.16.16.0      172.27.255.1     0x80000001  1310  0x22 0xb66d  36
《《《...summary...没有了，但是没有...0/0...去到其他...area
----------------------
NSSA+no-summary + default lsa
[edit protocols ospf area 0.0.0.2]
root@R3# show
nssa {
default-lsa {
default-metric 10;
metric-type 1;
}
no-summaries;
}
OSPF database, Area 0.0.0.2
Type       ID               Adv Rtr           Seq      Age  Opt  Cksum  Len
Router  *172.27.255.3     172.27.255.3     0x80000001   112  0x20 0x5569  36
Summary *0.0.0.0          172.27.255.3     0x80000001    16  0x20 0xf5b   28
NSSA    *10.22.1.0        172.27.255.3     0x80000001   112  0x20 0x8341  36
NSSA    *10.22.2.0        172.27.255.3     0x80000001   112  0x20 0x784b  36
NSSA    *10.22.3.0        172.27.255.3     0x80000001   112  0x20 0x6d55  36
NSSA    *10.22.4.0        172.27.255.3     0x80000001   112  0x20 0x625f  36
NSSA    *10.22.5.0        172.27.255.3     0x80000001   112  0x20 0x5769  36
NSSA    *10.22.6.0        172.27.255.3     0x80000001   112  0x20 0x4c73  36
NSSA    *10.22.7.0        172.27.255.3     0x80000001   112  0x20 0x417d  36
OSPF AS SCOPE link state database
Type       ID               Adv Rtr           Seq      Age  Opt  Cksum  Len
Extern   10.22.0.0        172.27.255.5     0x80000001   880  0x22 0x4983  36
Extern  *10.22.1.0        172.27.255.3     0x80000002   393  0x22 0x7f44  36
Extern  *10.22.2.0        172.27.255.3     0x80000002   230  0x22 0x744e  36
Extern  *10.22.3.0        172.27.255.3     0x80000002    67  0x22 0x6958  36
Extern  *10.22.4.0        172.27.255.3     0x80000001  1118  0x22 0x6061  36
Extern  *10.22.5.0        172.27.255.3     0x80000001  1118  0x22 0x556b  36
Extern  *10.22.6.0        172.27.255.3     0x80000001  1118  0x22 0x4a75  36
Extern  *10.22.7.0        172.27.255.3     0x80000001  1118  0x22 0x3f7f  36
Extern   172.16.16.0      172.27.255.1     0x80000001  1401  0x22 0xb66d  36
《《《...0/0...出来了，但是是...type 3
---------------------
NSSA+no-summary + default lsa + lsa type 7
[edit protocols ospf area 0.0.0.2]
root@R3# show
nssa {
default-lsa {
default-metric 10;
metric-type 1;
type-7;
}
no-summaries;
}
OSPF database, Area 0.0.0.2
Type       ID               Adv Rtr           Seq      Age  Opt  Cksum  Len
Router  *172.27.255.3     172.27.255.3     0x80000001   187  0x20 0x5569  36
NSSA    *0.0.0.0          172.27.255.3     0x80000001     9  0x20 0xe677  36
NSSA    *10.22.1.0        172.27.255.3     0x80000001   187  0x20 0x8341  36
NSSA    *10.22.2.0        172.27.255.3     0x80000001   187  0x20 0x784b  36
NSSA    *10.22.3.0        172.27.255.3     0x80000001   187  0x20 0x6d55  36
NSSA    *10.22.4.0        172.27.255.3     0x80000001   187  0x20 0x625f  36
NSSA    *10.22.5.0        172.27.255.3     0x80000001   187  0x20 0x5769  36
NSSA    *10.22.6.0        172.27.255.3     0x80000001   187  0x20 0x4c73  36
NSSA    *10.22.7.0        172.27.255.3     0x80000001   187  0x20 0x417d  36
OSPF AS SCOPE link state database
Type       ID               Adv Rtr           Seq      Age  Opt  Cksum  Len
Extern   10.22.0.0        172.27.255.5     0x80000001   955  0x22 0x4983  36
Extern  *10.22.1.0        172.27.255.3     0x80000002   468  0x22 0x7f44  36
Extern  *10.22.2.0        172.27.255.3     0x80000002   305  0x22 0x744e  36
Extern  *10.22.3.0        172.27.255.3     0x80000002   142  0x22 0x6958  36
Extern  *10.22.4.0        172.27.255.3     0x80000001  1193  0x22 0x6061  36
Extern  *10.22.5.0        172.27.255.3     0x80000001  1193  0x22 0x556b  36
Extern  *10.22.6.0        172.27.255.3     0x80000001  1193  0x22 0x4a75  36
Extern  *10.22.7.0        172.27.255.3     0x80000001  1193  0x22 0x3f7f  36
Extern   172.16.16.0      172.27.255.1     0x80000001  1476  0x22 0xb66d  36
《《《...0/0...变成...type 7...了
-------------------------------
=====================================================
Junstine- JIR
-Link State Database learned via  and must match on all routers within an area.
-SFP use the contents of link-state database to calculate the shortest path
-In hello packets, below fields must match:
Network mask/ hello interval / dead interval / options
-Implement OSPF area to shrink the size of link-state database.
-Area 0 serves as backbone and distribute routing information between non-0 area.
- flooding within area, all routers maintain an identical copy of database on a per-area basis.
========================================
JNCIA Study Guide
Link-State Protocol
 1—Router LSA
 2—Network LSA
 3—Network summary LSA
 4—ASBR summary LSA
 5—AS external LSA
 6—Group membership LSA
 7—NSSA external LSA
 8—External attributes LSA
 9—Opaque LSA (link-local scope)
 10—Opaque LSA (area scope)
 11—Opaque LSA (AS scope)
OSPF Packet Types:
***LSA using LSR,LSRU,LSA to flood
1.Hello Packet - discover neighbor
To establish and maintain a neighbor relationship, an OSPF-speaking router determines
whether any directly connected routers also speak OSPF. The router sends an
OSPF hello packet out all configured interfaces and awaits a response. The hello packet, type code 1, is addressed to the AllSPFRouters multicast address of 224.0.0.5 for broadcast and point-to-point connections. Other connection types unicast the hello packet to their neighbor.
2. Database Description Packet
After discovering its neighbors, the local router begins to form an adjacency with each neighbor
(as discussed in the “Forming Adjacencies” section later in this chapter). This adjacency process
requires that each router advertise its local database information. An OSPF router uses the
Database Description (DD) packet for this purpose.
The DD packet, type code 2, summarizes the local database by sending LSA headers to the
remote router. The remote router analyzes these headers to determine whether it lacks any information within its own copy of the link-state database.
-DD only used during adjacency formation. 2 purposes: who is in charge of sync and who transfer the LSA headers(higher RID)
3. Link-State Request Packet (LSR)
During the database synchronization process, the local router may find that it is missing information or that its local copy is out of date. The local router acquires the needed database information by sending a link-state request packet to its neighboring router. This packet contains identifiers that uniquely describe the requested LSA.
Link-State Type (4 octets)
LSA Headers (Variable)
This field carries the LSA headers describing the local router’s database
information. Each header is 20 octets in length and uniquely identifies each LSA in the
database. Each DD packet may contain multiple LSA headers.
This field displays the type of LSA being requested. The possible type codes include:
 1—Router LSA
 2—Network LSA
 3—Network summary LSA
 4—ASBR summary LSA
 5—AS external LSA
 6—Group membership LSA
 7—NSSA external LSA
 8—External attributes LSA
 9—Opaque LSA (link-local scope)
 10—Opaque LSA (area scope)
 11—Opaque LSA (AS scope)
4. Link-State Update Packet (LSU)
Information in the link-state database is populated through a Link State Advertisement (LSA).
Each LSA contains routing, metric, and topology information to describe a portion of the OSPF
network. The local router advertises LSAs within a link-state update packet to its neighboring
routers. This packet is reliably flooded throughout the network until each router has a copy. In
addition, the local router advertises a link-state update packet in response to a link-state request
5.Link-State Acknowledgment Packet (not LSA)
Forming Adjance:
Down Down is the starting state for all OSPF routers. A start event, such as configuring the
protocol, transitions the router to the Init state. The local router may list a neighbor in this
state when no hello packets have been received within the specified router dead interval for that interface.
Init The Init state is reached when an OSPF router receives a hello packet but the local router
ID is not listed in the received Neighbor field. This means that bidirectional communication has
not been established between the peers.
Attempt The Attempt state is valid only for Non-Broadcast Multi-Access (NBMA) networks.
It means that a hello packet has not been received from the neighbor and the local router is going to send a Unicast hello packet to that neighbor within the specified hello interval period.
2-Way The 2-Way state indicates that the local router has received a hello packet with its own
router ID in the Neighbor field. Thus, bidirectional communication has been established and
the peers are now OSPF neighbors.
ExStart In the ExStart state, the local router and its neighbor establish which router is in
charge of the database synchronization process. The higher router ID of the two neighbors controls which router becomes the master.
Exchange In the Exchange state, the local router and its neighbor exchange DD packets that
describe their local databases.
Loading Should the local router require complete LSA information from its neighbor, it transitions to the Loading state and begins to send link-state request packets.
Full The Full state represents a fully functional OSPF adjacency, with the local router having
received a complete link-state database from its peer. Both neighboring routers in this state add the adjacency to their local database and advertise the relationship in a link-state update packet.
Full State: finish LSU
--------------------
Link-State Request <<< LSA Header
Link-State Update <<< LSA
----------------------
The Router LSA  1
type code 1, which displays data about the local router.
This includes all links connected to the router, the metrics of those interfaces,
and the OSPF capabilities of the router.
Network LSA 2
Designated Routers (DR)
只有在同一个网段内的多于...2...个...routers...才会选举...DR BDR...，被选为...DR...的，会把广播泛红给所有其他...routers...。所以在...p2p...的...link...，一定不会有...DR...，因为一对一的不需要。
Use DR to avoid unnecessary repeat LSAs in a broadcast network.
Each broadcast segment in an OSPF network elects a designated router to act as the main point
of contact for the network segment. Each router on the link must become adjacent with the DR,
which handles all LSAs for the network. Each router sends the DR information using a new
multicast destination address of 224.0.0.6, AllDRRouters. The designated router generates a
network LSA, type code 2, to represent the broadcast segment to the rest of the network. Like
the router LSA, the Type 2 LSA has an area-flooding scope ensuring that each router in the
area receives a copy for the link-state database.
BDR
Avoiding this potential pitfall
requires the election of another router on the segment, the backup designated router (BDR).
The BDR also listens to the 224.0.0.6 multicast address and monitors the operations of the DR.
Additionally, the BDR forms a Full adjacency relationship with all other routers on the segment.
Should a problem arise with the designated router, the BDR immediately assumes the role
of the DR for the segment. This mechanism provides for stability in the network.
DR Elections
First became OSPF router, selected as DR
Check Priority: higher selected
If same, then check router ID(loopback address by default):higher choosen
==>Select DR & BDR
If DR failed, then BDR becomes DR and one from Drothers becomes BDR
==>when previous
DR is back, it only resume the DR position if  the current DR failed and re-selection is needed. DR...不抢占
Generally, DR election is not preempt.
##set interface P2P, ...会让...ospf...区域里没有...dr bdr
=======================================================
OSPF Areas
-T...he primary purpose of an OSPF area is scalability of the protocol. Boundaries are defined in
the network to limit the flooding of specific LSA types.
-the backbone area, forms the core of the network. All other... ...OSPF areas must connect to the backbone area. The backbone connects all areas and redistributes... ...all non-backbone routing information between the areas.... ...（...area 0...）
-Within OSPF, the link-state database must be identical
on all routers within an area.
======================================================
Internal router A router that maintains all operational interfaces within a single area is known
as an internal router. An internal router may belong to any OSPF area.
Backbone router A router that has at least one interface in area 0 is known as a backbone
router.
Area border router The area border router (ABR) connects one or more OSPF areas to the
backbone. This means that at least one interface is within area 0 while another interface is in
another area. The ABR plays a very important role in an OSPF network. We’ll see its responsibilities grow as we scale and expand our routing domain.
Autonomous System boundary router (ASBR) injects external routing knowledge into an OSPF network.
======================================================
Design Considerations
-The use of OSPF areas is an effective tool in minimizing the flooding scope of LSAs.
-it is generally a good idea to have more than one ABR connecting an area to the
backbone.
-ABRs must support a larger link-state database than the area routers.
======================================================
LSA Types:
1 - router-LSAs.
They describe interface and neighbors of each OSPF router within  same area
2 - network-LSAs.
They describe the set of routers attached to the network.
They describe inter-area routes, and enable the condensation of routing information at area borders.
Originated by DR.
3, 4 - Network Summary LSA
Originated by area border routers (ABR),
Type 3 network summary-LSAs:
1)It contains the subnet info in type 1 LSA (non-backbone), ABR floods this L3 LSA to backbone and all the routers in backbone will receive it.
2)ABR TO ABR (one to one base), so that ABR can flood LSA from other ABR to its own non-backbone routers
====>With the above 3 LSAs, all OSPF routers have reachability to each other. Internal router only floods LSA within it's own area. Only ABR can flood non-area LSAs to backbone.
Type 5 - AS-external-LSAs.
Originated by AS boundary routers, they describe routes to destinations external to the Autonomous System.
A default route for the Autonomous System can also be described by an AS-external-LSA.
Type 4 ASBR summary- ABR generate
====>with type 4,5, non-backbone routers can know external LSAs.
6 - Multicast
7 - ...存在于...NSSA : ABR injects default route into NSSA
====================================
Stub Areas...：... 123 (reject 5)
##stub area因为没有external routes，所以需要在ABR上有一条default route。JUNOS需要手动config这条default route。不会自动生成。
Under normal circumstances,the ABR re-floods the Type 5 LSAs into the area.When configured as a stub area,however, the ABR simply does not flood the AS external LSAs into the area. To provide the required IP reachability, the ABR should instead generate a summary LSA for the default route and inject that into the stub area.
[edit protocols ospf area 0.0.0.1]
regress@R4# set stub no-su?
Possible completions:
no-summaries         Don't flood summary LSAs into this stub area... <<<<
[edit protocols ospf area 0.0.0.1]
regress@R4# set stub no-summaries default-met?
Possible completions:
default-metric       Metric for the default route in this stub area (1..16777215)
[edit protocols]
lab@R2# set ospf area 1 stub... <<<<<
Totally Stub: 12 (reject 3,5)
Only receive default route from area 0
##stub...还是可以从...abr...那接受其他...area ...路由，可是...toally stub...只能接受...default route from ABR...，其他路由都接不了。
Nssa 1237: ...允许...ASBR...通告外部路由进来，并在...ABR...上...7...转...5...，从而通告...external...路由进...ospf...里
##...允许...ASBR...存在在该区域，并且通过...type 7LSA...把...external route...在...ABR...转成...type 5...发送给别的...area...。但是其他...area...过来的...external route cannot enter thisarea...。
https://www.juniper.net/documentation/en_US/junos/topics/example/ospf-not-so-stubby-area-configuring.html
Examples: Configuring OSPF Stub and Not-So-Stubby Areas
https://www.juniper.net/documentation/en_US/junos/topics/topic-map/ospf-stub-and-not-so-stubby-areas.html
default-metric—Configures the ABR to generate a default route with a specified metric into the stub area. This default route enables packet forwarding from the stub area to external destinations. You configure this option only on the ABR. The ABR does not automatically generate a default route when attached to a stub. You must explicitly configure this option to generate a default route.
=============================================
<Summary routes/Interarea routes>
Routes generated from other area
[edit protocols ospf]
regress@R2# run show ospf database
OSPF database, Area 0.0.0.1
Type       ID               Adv Rtr           Seq      Age  Opt  Cksum  Len
Router  *128.102.184.212  128.102.184.212  0x80000043  1726  0x22 0xbb6c  84
Router   128.102.184.247  128.102.184.247  0x80000042   320  0x22 0x87c3  48
Summary  128.102.183.6    128.102.184.247  0x8000001d   712  0x22 0x3e74  28
Summary  128.102.183.70   128.102.184.247  0x8000003a    60  0x22 0x86e7  28
Summary  128.102.184.106  128.102.184.247  0x8000001b  2146  0x22 0x4b04  28
Summary  128.102.184.247  128.102.184.247  0x8000003f  1103  0x22 0x854a  28
Summary  172.27.0.8       128.102.184.247  0x80000041  1494  0x22 0x37c   28
Summary  172.27.0.12      128.102.184.247  0x8000003f  1364  0x22 0xd98a  28
Summary  172.27.0.16      128.102.184.247  0x8000003a  2668  0x22 0xb695  28
Summary  172.27.0.20      128.102.184.247  0x8000003a  1233  0x22 0x93cd  28
Summary  172.27.0.24      128.102.184.247  0x8000003f  2016  0x22 0x57ce  28
Summary  172.27.0.56      128.102.184.247  0x8000001d   581  0x22 0x5acd  28
Summary  172.27.255.1     128.102.184.247  0x8000003f   972  0x22 0x643a  28
Summary  172.27.255.3     128.102.184.247  0x8000001b  1885  0x22 0x8eff  28
Summary  172.27.255.4     128.102.184.247  0x80000039  2407  0x22 0x4d3b  28
Summary  172.27.255.5     128.102.184.247  0x8000001d   451  0x22 0x7614  28
ASBRSum  128.102.183.70   128.102.184.247  0x8000001d   842  0x22 0xb2d7  28
ASBRSum  128.102.184.106  128.102.184.247  0x8000001b  1755  0x22 0x3d11  28
OSPF AS SCOPE link state database
Type       ID               Adv Rtr           Seq      Age  Opt  Cksum  Len
Extern   10.22.0.0        128.102.184.106  0x8000001a  1896  0x22 0xd3d5  36
Extern   10.22.1.0        128.102.184.106  0x80000038  1244  0x22 0xcc81  36
Extern   10.22.2.0        128.102.184.106  0x80000038  1114  0x22 0xc18b  36
Extern   10.22.3.0        128.102.184.106  0x80000038   983  0x22 0xb695  36
Extern   10.22.4.0        128.102.184.106  0x80000038   853  0x22 0xab9f  36
Extern   10.22.5.0        128.102.184.106  0x80000038   723  0x22 0xa0a9  36
Extern   10.22.6.0        128.102.184.106  0x80000038   592  0x22 0x95b3  36
Extern   10.22.7.0        128.102.184.106  0x80000037   462  0x22 0x8cbc  36
Extern   172.16.16.0      128.102.184.247  0x80000037   190  0x22 0xa678  36
Extern   172.27.0.96      128.102.184.106  0x80000001   308  0x22 0x8c36  36
========================================
<External Routes>
Routes originate from other routing protocols, or different ospf process and that are injected into OSPF via redistribution
========================================
OSPF...是化接口进...ospf...的。
ospf...要去其他区域就要有...3...类，要想去别的...protocol...必须有...5...类。
3...类是...1,2...类的汇总，到其他区域...1,2...类会变成...3...类，
5...类是从别的...protocol...的域充分发进来的，要想去别的...protocol...必须有...5...类。
stub...区域定义拒绝...5...类，允许...3...类。
TOTAOLY STUB...区域：拒绝...3...，...5...类，只有一条自动产生的默认路由。
NSSA...区域：只有...7...类，...1,2...类，... 0.0.0.0...（只有这种...3...类，... juniper...可以将这个转成...7...类）
...可以发出去，不可以收进来充分发的
======================================================================
2017-0209-0928... OSPF v3 down during GRES
Without RID, mgmt ip will be selected as RID. No Ipv4 address is configured under interfaces except mgmt..... (RE0 and RE1 has different ip address of fxp0)
And RID of ospf3 will be changed after GRES which triggers ospf flap.
{master}
labroot@mercury-re1> show ospf3 overview
Instance: master
Router ID: 172.22.147.20           <<<<
Route table index: 0
LSA refresh time: 50 minutes
Area: 0.0.0.0
Stub type: Not Stub
Area border routers: 0, AS boundary routers: 0
Neighbors
Up (in full state): 1
Topology: default (ID 0)
Prefix export count: 0
Full SPF runs: 12
SPF delay: 0.200000 sec, SPF holddown: 5 sec, SPF rapid runs: 3
Backup SPF: Not Needed
{master}
labroot@mercury-re1> edit
Entering configuration mode
{master}[edit]
labroot@mercury-re1# set routing-options router-id 1.1.1.1
{master}[edit]
labroot@mercury-re1# commit
{master}[edit]
labroot@mercury-re1# run show ospf3 overview
Instance: master
Router ID: 1.1.1.1
Route table index: 0
LSA refresh time: 50 minutes
Area: 0.0.0.0
Stub type: Not Stub
Area border routers: 0, AS boundary routers: 0
Neighbors
Up (in full state): 0
Topology: default (ID 0)
Prefix export count: 0
Full SPF runs: 13
SPF delay: 0.200000 sec, SPF holddown: 5 sec, SPF rapid runs: 3
Backup SPF: Not Needed
Regards,
Young
=======================================
OSPF network Point-to-Point is default option for point-to-point interfaces such as HDLC, PPP or point-to-point NBMA subinterface. It uses multicast hellos OSPF messages and sent it on multicast address 224.0.0.5 and there is no Designated Router (DR) and Backup DR (BDR) election.
From <https://supportforums.cisco.com/t5/network-infrastructure-documents/how-to-configure-ospf-over-a-point-to-point-link/ta-p/3132649>
========================================
Ospf hello by default 10s send 1, missed 4 hellos in 40s, then peer declare down
===================================
<Virtual Link>
In rare condition, if a new area is introduced that cannot have direct physical access to the area0, then we can use virtual link.
Stub area will not have virtual link, because no ABR is allowed.
=================================================
<OSPF Overload>
https://www.juniper.net/documentation/en_US/junos/topics/concept/ospf-overload-function-overview.html
An overloaded routing device determines it is unable to handle any more OSPF transit traffic, which results in sending OSPF transit traffic to other routing devices. OSPF traffic to directly attached interfaces continues to reach the routing device.
==========================================
<sham link>
You can create an intra-area link or sham link between two provider edge (PE) routing devices so that the VPN backbone is preferred over the back-door link.
https://www.juniper.net/documentation/en_US/junos/topics/topic-map/ospfv2-sham-links.html
==========================================
#Task 1.OSPF Troubleshooting  #
一个...area...里面的...router...他们的...LSA database...相同。通过水平分割来防止...loop.
LSA Type:
1: within area
2: DR
3:ABR
4:ASBR
5:external
7:NSSA
10:TE
Area:
-- Used to limit the flooding of LSA within the area.
-- non-backbone area must be connected to backbone area except NSSA & Stub....如果...non-backbone...没有和...area0...连，还不是特殊区域，那么用...virtual-link...。
All routers listen to 224.0.0.5, only DR&BDR listen to 224.0.0.6.
#OSPF Neighbor#
-show ospf neighbor
-show ospf interface  ...《《《可以看哪个...area
- show ospf database
--trace...只要...flag error detail...就...ok
#DR
>>>> DR...是谁的...ospf...先起来，谁当。同事的话，...ip...大的当。...DR...不抢占。
#ABR
<<<...谁的...router id...大就选谁
#3.area mismatch
Traceoption
[edit protocols ospf]
lab@R2# show
traceoptions {
file ospf.log;
flag error detail;
run show log ospf.log
#3.b mtu mismatch
>>> exstart state
#3.c ip address/subnet mismatch
#3.d
--area type mismatch
Jan 28 13:28:25.468440 OSPF packet ignored: area nssaness mismatch from 172.30.0.22 on intf ge-0/0/4.134 area 0.0.0.4
#3.e
--authentication
Jan 28 13:28:34.602134 OSPF packet ignored: authentication failure (bad cksum).
Jan 28 13:28:34.602312 OSPF packet ignored: authentication failure from 172.30.0.26
>>>md5show...出来的...pw...是密文
interface ge-0/0/4.136 {
authentication {
md5 1 key "$9$z6dnn9peK87NbIElM";《《《《
直接...copy...这个密文到...peer...的...authentication...下面：
[edit protocols ospf area 0.0.0.0 interface ge-0/0/4.136 authentication]
lab@R6# set md5 1 key "$9$L3KNs4f5F6CuHqPQnCB1LxNbYo"
#LSDB#
4.Verify LSDB
>>>LSDB...里面装的是...LSA...，路由更新和同步。
>>>originator sends LSA every 1800s. If LSA has a age time of 3600s, means this LSA has no refresh in the last 3600s and will be deleted.
（外挂带有其他协议的...sites...，因为有...7...，在...ABR...上可以...7...转...5...到达其他...area...。但是其他...area...的...5...类进不来）
NSSA...区域：只有...1 2 3 7
...可以通过...ASBR...收进来其他协议的路由，并通过在...ABR...上...7...转...5...发给其他...area...。但是不可以从...ABR...收进来其他协议的路由。
Totally NSSA...区域...: ...有...1 2 7 & 0/0...。... nssa...加上...no-summary...变成...totally nssa...。这时会自动给其他非...ASBR routers...发一条...0/0 type 3 lsa...。这样可以让他们通过...ABR ...到达其他...area...。如果再加上lsa...-...type... ...7，就可以把这条default的3类变成7类。
（不能外挂带有其他协议的...sites...，因为拒绝...5...）
STUB...区域...: ...定义拒绝...5...类，允许...3...类.... ...只有...123
TOTAOLY STUB...区域：...stub+ no-summary knob,...拒绝...3...，...5...类，不自动产生一条的默认路由...0/0...。需要加...default-metric...发给同一...area...里的其他非...ABR router...发...0/0...，然后其他...router...才能想到达其他...area...都走...ABR...。...!!ABR...的路由表里并没有...0/0,...只有...1 2. ...而...stub...区域里有...tyep 1 2 & 0/0
<<<no-summary knob: local rouer...不发...type3 lsa into...这个...area...。只有一条他自动产生的...0/0...会发给这个...area...其他非...ABR router...。这样其他...router...可以通过这条...0/0...到达其他...area...。但是如果...ABR...连接的是...stub...区域，那么不会自动向其他...stub area ...非...ABR router ...发...0/0...，需要...set default-mteric
《《《...default-metric knob...：...only on ABRs...，...generate default-lsa into stub area...。
https://www.juniper.net/documentation/en_US/junos/topics/topic-map/ospf-stub-and-not-so-stubby-areas.html
#R3
area 0.0.0.4 {
nssa {
area-range 172.30.32.0/20;
}
<<<要求...area4...把一条...rip...的...summary...路由传给...area0...，这样...area0...到...rip ...这些...ip...都走...summary...。
其实就是用...nssa area-range...来实现把...rip routes aggregate...发给...area 0.
《《《《如果存在至少一条更明细的路由，一条...aggregate route ...172.30.32.0/20会被...generate...并发给其他...area...。明细路由不会再发给其他...area...。...ABR...会自动...summary...路由，但是...ASBR...不会....
<<<...一定要配在...nssa...下面。
《《《要求...are0...到...rip ...这些...destination address...只有一条...summary route...，也就是...are4...只能有一条...summary route external lsa...，而不是把所有...rip...都传。在...Area4...的...rip...是...external lsa...。
没...fix...前，...area 4 ...的...rip external lsa...是这么些。
OSPF AS SCOPE link state database
Type       ID               Adv Rtr           Seq      Age  Opt  Cksum  Len
Extern  *172.30.32.0      172.30.5.6       0x80000005   165  0x22 0x8f87  36
Extern  *172.30.33.0      172.30.5.6       0x80000001   165  0x22 0x8c8d  36
Extern  *172.30.34.0      172.30.5.6       0x80000001   165  0x22 0x8197  36
Extern  *172.30.35.0      172.30.5.6       0x80000001   165  0x22 0x76a1  36
Extern  *172.30.36.0      172.30.5.6       0x80000001   165  0x22 0x6bab  36
Extern  *172.30.37.0      172.30.5.6       0x80000001   165  0x22 0x60b5  36
Extern  *172.30.38.0      172.30.5.6       0x80000001   165  0x22 0x55bf  36
Extern  *172.30.39.0      172.30.5.6       0x80000001   165  0x22 0x4ac9  36
Extern  *172.30.40.0      172.30.5.6       0x80000001   165  0x22 0x3fd3  36
Extern  *172.30.41.0      172.30.5.6       0x80000001   165  0x22 0x34dd  36
Extern  *172.30.42.0      172.30.5.6       0x80000001   165  0x22 0x29e7  36
Extern  *172.30.43.0      172.30.5.6       0x80000001   165  0x22 0x1ef1  36
Extern  *172.30.44.0      172.30.5.6       0x80000001   165  0x22 0x13fb  36
Extern  *172.30.45.0      172.30.5.6       0x80000001   165  0x22 0x806   36
Extern  *172.30.46.0      172.30.5.6       0x80000001   165  0x22 0xfc10  36
Extern  *172.30.47.0      172.30.5.6       0x80000001   165  0x22 0xf11a  36
Fix ...后：
Type       ID               Adv Rtr           Seq      Age  Opt  Cksum  Len
Extern  *172.30.32.0      172.30.5.6       0x80000005   165  0x22 0x8f87  36
#R4
《《《... NSSA...区域的... 0/0
《《《... ASBR...是负责把...external...路由引进该区域，而真正把这个...external...路由传出去给其他...area...的是...ABR
#On R6
lab@R6# run show ospf database
Summary *172.30.0.0       172.30.5.2       0x800003ae  3600  0x22 0xbc35  28
Summary *172.30.0.20      172.30.5.2       0x8000031e     4  0x22 0x2943  28
>>>...带...*...的...LSA...的...advertise router...都是...5.2...，... ...可是这个是...R2...的...lo0...。
查看...R2...发现，...R6...和...R2...的...router-id...重复！！...<<<...这个还会影响其他...router...从...R2...学...lo0...，所以考试的时候每台...router...都先...check...一下...router-id...配的对不对！！
5) Area 4 LSA Type
As required, area 4 only allows 1 & 7
# NSSA has type 2. delete type 2:  (on all are 4 routers)
lab@R3# set protocols ospf area 4 interface ge-0/0/4.134 interface-type p2p  <<<<
lab@R4# run show ospf interface
Interface           State   Area            DR ID           BDR ID          Nbrs
ge-0/0/4.134        PtToPt  0.0.0.4         0.0.0.0         0.0.0.0            1
ge-0/0/4.145        PtToPt  0.0.0.4         0.0.0.0         0.0.0.0            1
<<<below only on ABR R36
#Remove Type 3:
>>> NSSA has normal  type 3
先加...no-summary...：
lab@R4# set protocols ospf area 4 nssa no-summaries <<<...产生一条...default lsa type 3
Summary  0.0.0.0          172.30.5.6       0x80000007  1941  0x20 0xb5a3  28
<<< type 3...不允许，所以要...Change NSSA to Totoally NSSA...，把这条...0/0 3...转成...7.only set on ABR 's nssa...（...R3 R6...）
lab@R3# set protocols ospf area 4 nssa default-lsa default-metric 10 type-7
NSSA    *0.0.0.0          172.30.5.3       0x80000001   806  0x20 0xabaa  36
NSSA     0.0.0.0          172.30.5.6       0x80000001    68  0x20 0x99b9  36
<<<changed from summary to nssa
<<<!!! LSA Summary ...的时候... ospf...发给...rip...是... 10...，而改成...LSA nssa...后，发给...rip...这个路由是...150
Internal area preference 10, external 150.(lsa type 5 & 7).
<<<...为什么要加...default-metric 10...？
Type 7: ...允许...ASBR...通告外部路由进来...nssa...区域，在这个区域是...type 7....当通过在...ABR...要发给其他区域的时候，在...ABR...上...7...转...5...，从而通告...external...路由进...ospf...其他...area...里。
##...允许...ASBR...存在在该区域，并且通过...type 7LSA...把...external route...在...ABR...转成...type 5...发送给别的...area...。但是其他...area...过来的...external route cannot enter this area...。《《《这就是...nssa...为什么要用...type7...，是为了拒绝其他...area...进来的...type 5...，... ...但允许在即...area...往其他...area...发...type 7.
《《《《...ASBR R3&R6...会有...type5...，因为需要在他们上面...7...转...5. R4R5...不能有。...NSSA...可以吧...external ...路由...7...转...5...发给其他...area...，但是从其他...area...来的...type5...是进不来的，因为...NSSA...不识别...type 5.
NSSA ...区域的配置：
R36...上要配...no-summary...，...type-7 ...。。。
R45...只需要...nssa...。如果在...R45...上配...type-7...会导致从...r36...收到的...0/0...变成...summary...路由
6) RIP & OSPF-RIP distribution
>>>by default, RIP auto accept all routes from neighbors but does not send any route. ...所以...rip...一般会配一个...export policy...。
<<<...当...show advertising rip...的时候，...peer...地址要指...local interface address...，因为...rip...的...neighbor...是一个...multicast address
lab@R4# run show rip neighbor
Local  Source          Destination     Send   Receive   In
Neighbor          State  Address         Address         Mode   Mode     Met
--------          -----  -------         -----------     ----   -------  ---
ge-0/0/4.202         Up 172.30.0.49     224.0.0.9       mcast  both       1
<<<rip's destination...都是到这个...multicast...地址的。
#RIP...只收不发，所以在...DC-2...上配了...export policy...，把静态...export...给...R4&R5
----------------------
policy-statement DC2-static-to-rip {
term 1 {
from protocol static;
then accept;
}
term 2 {
from protocol rip;
then accept;
--------------------
lab@R4# run show route receive-protocol rip 172.30.0.50  ..., 58...可以看到
172.30.32.0/24... -- ...172.30....47....0/24
而在...r4 5...上...show ospf database ...也可以看到... NSSA for...这些...prefix...。说明从...rip...收...route ok...！
看发...route...：
lab@R4# run show route advertising-protocol rip 172.30.0.49
inet.0: 38 destinations, 54 routes (38 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both
0.0.0.0/0          *[OSPF/150] 02:41:47, metric 11, tag 0
> to 172.30.0.21 via ge-0/0/4.134
《《《...R4...把...NSSA...的...0/0...发给...DC-2...了
root@vr-device# run show route 0/0 table DC2.inet.0
DC2.inet.0: 23 destinations, 23 routes (23 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both
0.0.0.0/0          *[RIP/100] 02:44:08, metric 2, tag 0
> to 172.30.0.49 via ge-0/0/8.202
root@vr-device# run show route receive-protocol rip 172.30.0.49
DC2.inet.0: 23 destinations, 23 routes (23 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both
0.0.0.0/0          *[RIP/100] 02:46:02, metric 2, tag 0
> to 172.30.0.49 via ge-0/0/8.202
《《《...DC2...收到
但是...R5...并没有：
[edit]
lab@R5# run show route advertising-protocol rip 172.30.0.57
[edit]
lab@R5# ... ...《《《《...  no route advertised
原因：
lab@R5# run show route 0/0 exact
inet.0: 34 destinations, 51 routes (34 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both
0.0.0.0/0          *[RIP/100] 02:48:15, metric 3, tag 0
> to 172.30.0.58 via ge-0/0/4.204
[OSPF/150] 02:48:14, metric 11, tag 0
> to 172.30.0.34 via ae0.0
《《这条已经从...rip...收到了，并且它的...pref ...更优。因为...R4...已经通告给...DC2...这条，并被作为...rip...写入...DC2...的表里面。通过...export policy...，...rip...会被通告给...peer...。
lab@R5# run show route receive-protocol rip 172.30.0.58
inet.0: 34 destinations, 51 routes (34 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both
0.0.0.0/0          *[RIP/100] 02:54:55, metric 3, tag 0
> to 172.30.0.58 via ge-0/0/4.204
《《《从...dc2...收到
root@vr-device# run show route receive-protocol rip 172.30.0.57
《《《...dc2...不从...R5...收...0/0
***...这样造成一个问题，如果...R4 failed...，会...traffic down...。因为...R4...，...R5...都有一条指向...ABR...的...default route...。他们上面都有...rip policy export...给...dc2. ...但是因为...R4...已经...export...了，这条路由变成...RIP...后，...dc2...会自动发给...R5,...造成在...R5...上...rip...被优于...ospf external...。所以...R5...无法通告这条...0/0...给...dc2....当...R4down...，这条路由没了，就会出现一段时间...dc2...无法到...core...了。
7) Suboptimal routes
#Regarding Policy:
1. Policy only can handle "active" routes in routing-table!!
2.
-- RIP & BGP: import&export policy can be applied.
-- OSPF: import policy cannot control external area routes flooding. Only can be used for internal area and     external area change routes in distance fashion.-->meaning to say, seldom use. Export policy can be used to inter area routes flooding to ABR.
-- ISIS: only export policy.
-------------
“...to...”...statement: match routes which are sent to one level and to specify RIB
-------------
3. RIP by default receive everyting in import direction. Means as long as peer advertise, RIP will accept. But it will not export anything to peer by default.
4.OSPF will reject everything peer export. If wants to redistribute routes into ospf, then needs to use export policy.
Apply router filter on both R4 & R5:
lab@R5# set policy-options policy-statement rip-filter term 1 from protocol rip
lab@R5# set policy-options policy-statement rip-filter term 1 from route-filter 0/0 exact
lab@R5# set policy-options policy-statement rip-filter term 1 then reject
lab@R5# set protocols rip group rip import rip-filter
<<<<rip default policy...，...receive all...。所以不需要加...term 2 then accept...，当然加了也不影响。
lab@R5# run show route 0/0 exact
inet.0: 34 destinations, 50 routes (34 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both
0.0.0.0/0          *[OSPF/150] 00:46:36, metric 11, tag 0
> to 172.30.0.34 via ae0.0
<<<<<rip disappear
<<<...注意当...nssa default-lsa type 7 ...的时候...0/0 preference...是...150
当不加...typ 7...，就是...default...的...summary type3...，...0/0 preference...是...10
8) ospf export to rip with metric to control route seletction:
《《《...rip...老化时间很长，如果...R4...出问题，那么很长时间这条路由还在表里，但是已经不可大了。所以这个违背题里面要求任何...ASBR down...都不会导致...default ...路由消失在...rip...里。......《《《...rip...的...metric...是用...hop count...来...measure...的。所以...default...是...1 hop...。
lab@R5# show policy-options policy-statement default-to-rip
term 1 {
from {
protocol ospf;
route-filter 0.0.0.0/0 exact;
}
then {
metric 5;《《《...r5...设为...5...，比...r1...的...default 1...大，...prefer r4
accept;
}
9) Area Range:
-R3 R6...都是...ABR...候选，... R6...的...router-id...大，所以他是...ABR...。他向外宣告路由。
-by default, ABR does not summarize routes being sent from one area to another.
If requested, then use area-range
<<<only needs to confiugre under ABR,...因为是要从...nssa...把这个...external ...路由发给...ABR, ABR...再发给...area 0.
lab@Vega# run show ospf database area 0
OSPF AS SCOPE link state database
Type       ID               Adv Rtr           Seq      Age  Opt  Cksum  Len
Extern  *172.30.32.0      172.30.5.6       0x80000004     5  0x22 0x9186  36
Extern  *172.30.33.0      172.30.5.6       0x80000001     5  0x22 0x8c8d  36
Extern  *172.30.34.0      172.30.5.6       0x80000001     5  0x22 0x8197  36
Extern  *172.30.35.0      172.30.5.6       0x80000001     5  0x22 0x76a1  36
Extern  *172.30.36.0      172.30.5.6       0x80000001     5  0x22 0x6bab  36
Extern  *172.30.37.0      172.30.5.6       0x80000001     5  0x22 0x60b5  36
Extern  *172.30.38.0      172.30.5.6       0x80000001     5  0x22 0x55bf  36
Extern  *172.30.39.0      172.30.5.6       0x80000001     5  0x22 0x4ac9  36
Extern  *172.30.40.0      172.30.5.6       0x80000001     5  0x22 0x3fd3  36
Extern  *172.30.41.0      172.30.5.6       0x80000001     5  0x22 0x34dd  36
Extern  *172.30.42.0      172.30.5.6       0x80000001     5  0x22 0x29e7  36
Extern  *172.30.43.0      172.30.5.6       0x80000001     5  0x22 0x1ef1  36
Extern  *172.30.44.0      172.30.5.6       0x80000001     5  0x22 0x13fb  36
Extern  *172.30.45.0      172.30.5.6       0x80000001     5  0x22 0x806   36
Extern  *172.30.46.0      172.30.5.6       0x80000001     5  0x22 0xfc10  36
Extern  *172.30.47.0      172.30.5.6       0x80000001     5  0x22 0xf11a  36
>>> here R6 has higher IP than R3, so it advertise the routes.
>>>>however the ASBR R4 R5 did not summarize the routes. if request summarize, it has to use area-range command.
area 0.0.0.4 {
nssa {
default-lsa {
default-metric 10;
type-7;
}
no-summaries;
}
area-range 172.30.32.0/20; <<<<< wrong location, needs to be under nssa
<<<Change area-range under nssa
lab@Vega# run show ospf database area 0
OSPF AS SCOPE link state database
Type       ID               Adv Rtr           Seq      Age  Opt  Cksum  Len
Extern  *172.30.32.0      172.30.5.6       0x80000005    12  0x22 0xf108  36
<<<<summarized as 172.30.32.0/20
计算汇聚路由方法：
172.30.32.0/24
172.30.33.0/24
….
….
172.30.47.0/24
>>> ...用...47-32+1=16...，... ...然后...2...的...4...次方，就用...24-4=20
汇聚成...172.30.32.0/20
lab@R3# run show ospf database external
OSPF AS SCOPE link state database
Type       ID               Adv Rtr           Seq      Age  Opt  Cksum  Len
Extern   172.30.32.0      172.30.5.6       0x80000001    13  0x22 0xf904  36
<<< now only see this summary route...。之前...area0...的...external ...是那些明细。
11 12)Virtual-Link
--Verify lo0 reacability
lab@R1# run show route 172.30.5/24 terse
R1&R8 -- OK
R2 R3 R6 R7 -- no R1 R8
R4 -- only 4 5 -- OK
-non 0-area without an interface included in 0-area, will have no access to 0-area. Therefore, non 0-area's summary LSA cannot be used by 0-area. Even it appears as summary LSA in 0-area.
《《《一个...router...没有任何...interface...在...area0...里，即使...area 0 ...收到了他的...LSA3...也没办法使用。因为到不了。
-Use a virtual link to connect the non 0-area with 0 area virtually.
-All non-0 area should have at least one interface in area 0, but if it does not,then needs to have VL.
>>> r1...和...r8...组成...area 2...，但是没有任何...interface...在...area0...里面。
》》》从...R2...上看在...area 3...里面有...r1...的...lo0...，但是不是自己产生的...summary...的...lsa...也不是...router lsa...。
lab@R2# run show ospf database netsummary lsa-id 172.30.5.1
OSPF database, Area 0.0.0.3
Type       ID               Adv Rtr           Seq      Age  Opt  Cksum  Len
Summary  172.30.5.1       172.30.5.1       0x80000005  2153  0x22 0xe5b1  28
show ospf database 在...R2...上可以发现，...area3...里面带...*...的...router ...和...summary lsa...会被放进...area 0...，并且除了自己产生的...summary...路由其他的放进...area 0...里面都不带...*...了
》》》》所以...R1 R8...的...lo0...的路由没有哦被放进...area0 ...里面。所以...ospf...的...non-backbone...必须和...backbone...有...interface...，否则路由就进不去...area 0. ...例外：...NSSA & STUB
解决方法，...R1-R2  r7-r8 ...建立两根...virtual links
lab@R1# set protocols ospf area 0 virtual-link neighbor-id 172.30.5.2 transit-area 3
lab@R2# set protocols ospf area 0 virtual-link neighbor-id 172.30.5.1 transit-area 3
lab@R1# run show ospf neighbor
Address          Interface              State     ID               Pri  Dead
172.30.0.10      ge-0/0/4.118           Full      172.30.5.8       128    33
172.30.0.2       ae0.0                  Full      172.30.5.2       128    36
172.30.0.2       vl-172.30.5.2          Full      172.30.5.2         0    39 <<<<<...与...area0 ...建成...neigbhor
》》》...P103
R5 metric 5 ...发给...rip 0/0...路由，保证...ce router...选...R4.
13) test redundancy
<<<...为了验证任何...abr failure...都不会造成...area isolation
lab@R8# run traceroute 172.30.32.1 no-resolve
traceroute to 172.30.32.1 (172.30.32.1), 30 hops max, 40 byte packets
1  172.30.0.45  16.319 ms  25.883 ms  26.922 ms
2  172.30.0.41  32.171 ms  38.806 ms  32.680 ms
3  172.30.0.33  39.213 ms  54.231 ms  39.751 ms
4  172.30.0.58  44.919 ms !N  32.710 ms !N  52.480 ms !N
r7-r6-R5-VR
<<< ping NG, but traceroute works???
Now on r6 deactivate ospf
lab@R8# run traceroute 172.30.32.1 no-resolve
traceroute to 172.30.32.1 (172.30.32.1), 30 hops max, 40 byte packets
1  172.30.0.45  35.634 ms  25.401 ms  26.344 ms
2  172.30.0.17  38.036 ms  32.914 ms  32.341 ms
3  172.30.0.14  46.096 ms  32.110 ms  39.446 ms
4  172.30.0.22  52.811 ms  59.093 ms  59.450 ms
5  172.30.0.50  73.498 ms !N  64.565 ms !N  74.078 ms !N
R8-r7-r2-r3-r4-vr
