# IGP

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

IGP
Static Route
[edit routing-options]
lab@mx480-re0# show
static {
route 192.168.0.0/28 next-hop 192.168.1.1;
*****这个必须带掩码，否则默认/32这种明细路由找不到
**STATIC ...路由...nh...：
1....直连
2....非直连，需要加...resolve...。这样可以在本地路由表里递归查找。
#set routing-option static 0/0 resolve
3.reject ...匹配的话，包被丢弃
4.qualified-next-hop...，... ...允许到同一个目的地址有多个吓一跳，并且给每个...nh...一个优先值
5. PASSIVE:...允许...nh...即使不可达也可写入路由表
====================================================
104 （1/2/0）----- （1/3/0）80 （1/2/0）----（0/2/0）480
从104 PING 80，能ping通，是直连。
从104 PING 480，PING不通，因为没有路由。两头写static route就通了。
104：[edit routing-options]
lab@mx104-re0# show
static {
route 192.168.1.0/28 next-hop 192.168.0.1;
}
480：[edit routing-options]
lab@mx480-re0# show
static {
route 192.168.0.0/28 next-hop 192.168.1.1;
=============================================
计算是否在一个网段：
172.30.32.0/28
1111 /  1111<<<...剩下的是...4...位，那么每一段网络就是...2...的...4...次方共...16...位。
172.30.32.0 -- 172.30.32.15
172.30.32.16 -- 172.30.32.31
….
….
=======================================================
<Aggregate Route>
# run show route protocol aggregate detail
preference...：...130
Understanding Route Aggregation
http://www.juniper.net/documentation/en_US/junos15.1/topics/concept/policy-aggregate-routes.html
*****An aggregate route is a route you define but which is not used for forwarding traffic (next-hop is discard or reject).Typically, the aggregate route would be advertised into BGP.
aggregate...路由的默认...nh...是...reject
不需要至少有一条路由是在路由表里活的
计算：
2...的...6...次方是...64...，...2...的...7...次方是...128.  00000000...的...8...位是...2...的...0...次方开始到...2...的...7...次方。
172.18.129.0/24... -- 10000001
172.18.130.0/24... -- 10000010
172.18.132.0/24... -- 10000100
172.18.133.0/24... -- 10000101
》》》前面...10000...是相同的。... ...前...5...位相同，再加上...172.18...这个...8+8=16...位，一共...21....所以是.../21....而汇聚路由的地址都是这个网络块的第一个值。把...100000...用...0...补全...8...位...10000000-...》...128.
172.18.128/21
#...当在...R1...上建立...aggregate route...，这个必须在协议里...export...给...neigbor with nh reject...。...Neighbor routerR2...接到后会变成该协议路由下一跳指向...R1...。所以当从...R2...上...ping...明细路由地址...ping...不同，因为是...aggregate...路由。但是...traceroute...可以到达目的地址。
=======================================
Generate ...路由（生成路由）
至少有一条路由是在路由表里活的
======================================
Martian routes ...（火星路由）
https://www.juniper.net/documentation/en_US/junos/topics/concept/martian-addresses-understanding.html
Martian addresses are host or network addresses about which all routing information is ignored. When received by the routing device, these routes are ignored. They commonly are sent by improperly configured systems on the network and have destination addresses that are obviously invalid.
user@host> show route martians table inet.
inet.0:             0.0.0.0/0 exact -- allowed             0.0.0.0/8 orlonger -- disallowed
在公网中，一些路由是不可路由的
Protocol Preference
Default route
他的作用是当本台设备上没有到目的网段的明细路由的时候，通过这条...default0/0...的路由，走到下一他设备，如果下台设备有到这个目的网段的路由那么就可以到达了。
**...有到下台设备接口的路由，并不代表可以用下台设备的路由表。因为目的地址并不算是本台路由表的条目。...0/0...路由就是起这个作用，把包引到下台设备，从而可以使用下台路由表的路由条目。
============================================================
Aggregate Route
[edit routing-options]
aggregate {
route 172.20.64.0/18;
}
root# run show route
inet.0: 12 destinations, 12 routes (12 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both
172.20.64.0/18     *[Aggregate/130] 00:05:01
Reject <<<<
root# run show route protocol aggregate detail
inet.0: 12 destinations, 12 routes (12 active, 0 holddown, 0 hidden)
172.20.64.0/18 (1 entry, 1 announced)
*Aggregate Preference: 130
Next hop type: Reject
Address: 0x9292a44
Next-hop reference count: 2
State: <Active Int Ext>
Age: 5:21
Validation State: unverified
Task: Aggregate
Announcement bits (1): 0-KRT
AS path: I (LocalAgg)
Flags:                  Depth: 0        Active
AS path list:
AS path: I Refcount: 3
Contributing Routes (3):
172.20.66.0/30 proto Direct <<<<<
172.20.77.0/30 proto Direct <<<<<
172.20.111.0/24 proto Direct<<<<<
root# run show route 172.20.64.1  <<< ...当路由表里没有更匹配的明细路由时候，就走...aggregate...。这时的...default next-ho...是...reject...。
inet.0: 12 destinations, 12 routes (12 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both
172.20.64.0/18     *[Aggregate/130] 00:07:56
Reject
==============================================
RIB Groups
用来把...master inet.0...和...routing-instance...的路由表...instance.inet.0...互相导的。
<...把...inet0...的导入...instance>
[edit routing-options]
root# show
rib-groups {
inet.0-to-instance-a {
import-rib [ inet.0 instance-a.inet.0 ];...《《把...inet0...的导入...instance
}
把路由表里的...interface...和静态路由导入对方表里面。
interface-routes {
rib-group inet inet.0-to-instance-a;
}
static {
rib-group inet.0-to-instance-a;
<...把...instance...的导入... inet0>
建立一个...rib group
instance-a-to-inet.0 {
import-rib [ instance-a.inet.0 inet.0 ];...《《把...instance...的导入...inet0
}
把路由表里的...interface...和静态路由导入对方表里面。
routing-options {
interface-routes {
rib-group inet instance-a-to-inet.0;
}
static {
rib-group instance-a-to-inet.0;
****...在...master...下面设...import...，是将...master...的给...routing-instance...。
在...routing-instance...下面设...import...，反之。
======================================================
Forced Rejection of Passive Route Traffic
Generally, only active routes are included in the routing and forwarding tables. If a static route's next-hop address is unreachable, the route is marked passive, and it is not included in the routing or forwarding tables. To force a route to be included in the routing tables regardless of next-hop reachability, you can flag the route as passive. If a route is flagged passive and its next-hop address is unreachable, the route is included in the routing table, and all traffic destined for the route is rejected.
https://www.juniper.net/documentation/en_US/junos12.3/topics/topic-map/policy-static-route-control.html
当路由吓一跳不可达，这个路由会消失在路由表里，如果还想要他在路由表，加上...passive...，但是下一跳会变成...reject...。
[edit routing-options static]
lab# set route 3.3.3.3 next-hop 192.168.12.2 passive
Disable interface
3.3.3.3/32         *[Static/5] 00:00:08
Reject
==========================================
无论是路由表还是转发表，他的条目都是从...peer...学到的。是对方告诉我，我到对方怎么走。数据是和表里的条目方向一致。我们说的路由和数据方向相反，指的是从哪里学习来路由的方向是和数据走的方向相反。
分...label...的行为是数据方向一致，因此和路由相反。
===========================================
<passive interface>
https://www.juniper.net/documentation/en_US/junose16.1/topics/reference/command-summary/passive-interface.html
这个口只是宣告自己的...ip address...到这个...protocol...里面，并不接受...to...发送这个的协议包。
=================================================
Nexthop: receive ...可以...ping...，... reject...可以...traceroute
===============================================================
