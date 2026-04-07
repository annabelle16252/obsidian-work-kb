# BGP

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

BGP
# BGP Packet #
<sync>
14:50:27.915197 In IP 10.242.58.46.54786 > 10.242.58.45.179: S 2158280544:2158280544(0) win 16384 <mss 1460>
<ack>
14:50:13.915821 Out IP 10.242.58.45.179 > 10.242.58.46.54786: S 1486669129:1486669129(0) ack 2158280545 win 16384 <mss 9134>
# Route Selection Process #
1. The Next Hop attribute value for each route must be reachable in the local routing table
2. highest Local Preference attribute value.(ibgp)
3. shortest AS Path length.(ebgp)
4. smallest Origin attribute value.
5. smallest MED attribute value. （for routes from same AS）
6. learned from an EBGP peer over routes learned from an IBGP peer.
---------
<IBGP Only>
7. smallest IGP metric to the advertised BGP Next Hop.
8. for RR,shortest Cluster-List length.
---------
<<<如果加multipath，比到这就停了，2条路都会被使用
9. peer with the smallest numerical Router ID.
10.peer with the smallest numerical Peer Address.
# Configuration #
-RR
Client:
set protocols bgp group r4 type internal
set protocols bgp group r4 local-address 192.168.0.2
set protocols bgp group r4 neighbor 192.168.0.4  <<< rr
RR:
set protocols bgp group r4 type internal
set protocols bgp group r4 local-address 192.168.0.4
set protocols bgp group r4 cluster 4.4.4.4 <<< rr
set protocols bgp group r4 neighbor 192.168.0.1
set protocols bgp group r4 neighbor 192.168.0.2
set protocols bgp group r4 neighbor 192.168.0.3
# Indirect nh #
ipcore@rr21.pujlab-re1> show route 1/24 extensive
Path 1.0.0.0
from 10.233.254.30
Vector len 4.  Val: 0 1 2 4
*BGP    Preference: 170/-141
Next hop type: Indirect, Next hop index: 0
Address: 0x59d5375c
Next-hop reference count: 11340
Source: 10.233.254.30
Next hop type: Router, Next hop index: 633
Next hop: 10.64.0.111 via xe-1/0/0.0, selected
Label operation: Push 486819
Label TTL action: no-prop-ttl
Load balance label: Label 486819: None;
Label element ptr: 0xcfdba70
Label parent element ptr: 0x0
Label element references: 2
Label element child references: 1
Label element lsp id: 0
Session Id: 0x140
Protocol next hop: 10.233.97.24
Indirect next hop: 0x7819008 1178665 INH Session ID: 0x7d2
State: <Active Int Ext>
Local AS:  4788 Peer AS:  4788
Age: 2d 16:19:14        Metric2: 2120
Validation State: unverified
Task: BGP_4788.10.233.254.30
Announcement bits (4): 0-KRT 6-BGP_RT_Background 7-Resolve tree 5 9-Resolve tree 9
AS path: 13335 I  (Originator)
Aggregator: 13335 172.69.19.1
Cluster list:  58.27.127.60 58.27.127.7 10.233.32.17 10.233.33.20
Originator ID: 10.233.97.24
Communities: 4788:400 4788:440 4788:441
Accepted
Localpref: 140
Router ID: 10.233.254.30
Thread: junos-main
Indirect next hops: 1
Protocol next hop: 10.233.97.24 Metric: 2120
Indirect next hop: 0x7819008 1178665 INH Session ID: 0x7d2
Indirect path forwarding next hops: 1
Next hop type: Router
Next hop: 10.64.0.111 via xe-1/0/0.0
Session Id: 0x140
0.0.0.0/0 Originating RIB: inet.0
Metric: 2120 Node path count: 1
Indirect next hops: 1
Protocol next hop: 10.233.254.30 Metric: 2120
Inode flags: 0x204 path flags: 0x0
Path fnh link: 0xe4441c0 path inh link: 0x0
Indirect next hop: 0x7813e08 1048580 INH Session ID: 0x7bd
Indirect path forwarding next hops: 1
Next hop type: Router
Next hop: 10.64.0.111 via xe-1/0/0.0
Session Id: 0x140
10.233.254.30/32 Originating RIB: inet.3
Metric: 2120 Node path count: 1
Forwarding nexthops: 1
Next hop type: Router
Next hop: 10.64.0.111 via xe-1/0/0.0
Session Id: 0x140
----------------------
# route target #
The family route-target statement is useful for filtering VPN routes before they are sent. Provider edge (PE) routers inform the route reflector (RR) which routes to send, using family route-target to provide the route-target-interest information. The RR then sends to the PE router only the advertisements containing the specified route target.
https://www.juniper.net/documentation/us/en/software/junos/vpn-l2/topics/ref/statement/family-edit-protocols-bgp-route-target-vp.html
labroot@jtac-mx480-r2605-re0# run show route table bgp.rtarget.0 logical-system rr2
65001:100:100/96
*[BGP/170] 19:21:18, localpref 100, from 2.2.2.2
AS path: I, validation-state: unverified
> to 13.13.13.2 via ge-0/1/4.0
[BGP/170] 20:52:48, localpref 100, from 3.3.3.3
AS path: I, validation-state: unverified
> to 14.14.14.2 via ge-0/1/6.0
<<<rr2...如果收到带着...65001:100:100 RT...的路由，发给...PE2...，... ...从...output...可以看出优选...PE2 than PE3
labroot@jtac-mx480-r2605-re0# run show route receive-protocol bgp 2.2.2.2 logical-system rr2 table bgp.rtarget.0
bgp.rtarget.0: 2 destinations, 4 routes (2 active, 0 holddown, 0 hidden)
Prefix                  Nexthop              MED     Lclpref    AS path
65001:100:100/96
*                         2.2.2.2                      100        I
<<<rr2 received this rt route from pe2,...这个...RT...的值由...pe2 vrf...的...vrf-target...决定...,PE2...上有几个...vrf...，就会给...rr2...发几条...rt...路由，分别带着这几个...vrf...的...rt
# advertise-default #
The advertise-default statement causes the router to advertise the default route target route (0:0:0/0) and suppress all routes that are more specific. This can be used by a route reflector on BGP groups consisting of neighbors that act as PE routers only. PE routers often need to advertise all routes to the route reflector.
Suppressing all route target advertisements other than the default route reduces the amount of information exchanged between the route reflector and the PE routers.
labroot@jtac-mx480-r2605-re0# run show route table bgp.rtarget.0 logical-system pe2
0:0:0/0
*[BGP/170] 18:00:31, localpref 100, from 22.22.22.22
AS path: I, validation-state: unverified
> to 13.13.13.1 via ge-0/1/5.0
65001:100:100/96
*[RTarget/5] 18:59:37
Type Default
Local
<<< pe2 received default route-target from rr2, rr2 has below configuration:
--------------
family route-target {
advertise-default; <<<
--------------
If delete advertise-default:
labroot@jtac-mx480-r2605-re0# run show route table bgp.rtarget.0 logical-system pe2
bgp.rtarget.0: 1 destinations, 2 routes (1 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both
65001:100:100/96
*[RTarget/5] 19:00:26
Type Default
Local
[BGP/170] 00:00:05, localpref 100, from 22.22.22.22  <<<<specific route appear
AS path: I, validation-state: unverified
> to 13.13.13.1 via ge-0/1/5.0
<<< 0:0:0/0  disappeared, but specific route appear
# next-hop self #
改变的是路由的...protocol nh
Protocol nh: ...是这条路由始发...router...的出接口的地址
regress@122# run show route 111.111.111.111
111.111.111.111/32 *[BGP/170] 02:22:30, localpref 100, from 165.21.12.237
AS path: 7473 I, validation-state: unverified
>  to 25.25.25.1 via ge-0/0/1.0
# accept-remote-nexthop #
https://www.juniper.net/documentation/us/en/software/junos/bgp/topics/ref/statement/accept-remote-nexthop-edit-protocols-bgp.html
Specify that a single-hop EBGP peer accepts a remote next hop with which it does not share a common subnet. Configure a separate import policy on the EBGP peer to specify the remote next hop.
In some situations, it is necessary to configure a single-hop... ...EBGP peer to accept a remote next hop with which it does not share a common subnet.... The default behavior is for any next-hop address received from a single-hop EBGP peer that is not recognized as sharing a common subnet to be discarded. The ability to have a single-hop EBGP peer accept a remote next hop to which it is not directly connected also prevents you from having to configure the single-hop EBGP neighbor as a multihop session. When you configure a multihop session in this situation, all next-hop routes learned through this EBGP peer are labeled indirect even when they do share a common subnet. This situation breaks multipath functionality for routes that are recursively resolved over routes that include these next-hop addresses. Configuring the accept-remote-nexthop ...statement allows a single-hop EBGP peer to accept a remote next hop, which restores multipath functionality for routes that are resolved over these next-hop addresses.
From <https://community.juniper.net/communities/community-home/digestviewer/viewthread?MID=66802>
EBGP ...收进来的路由如果...nh...不是...direct ...同网段地址，就会直接被...discard...。即使你...apply...了...policy...去改...nh...，也不会成功。因为...discard...会发生在...policy...之前。
# Attribut #
- Aggregator=0
set routing-instances HSBA_MXSVOB-s_DOWNSTREAM routing-options aggregate route 10.65.32.0/19 as-path atomic-aggregate
* 10.65.32.0/19 (1 entry, 1 announced)
BGP group IBGP-3 type Internal
Route Distinguisher: 10.233.65.32:10223
VPN Label: 201
Nexthop: Self
Flags: Nexthop Change
Localpref: 100
AS path: [4788] I
Communities: target:4788:10216
AttrSet AS: 65503
Localpref: 100
AS path: I (Atomic)
Adding As and origin address manually.
set routing-instances HSBA_MXSVOB-s_DOWNSTREAM routing-options aggregate route 10.65.32.0/19 as-path aggregator 65503 10.65.32.1
Limit/Threshold: 5000/4500 destinations
* 10.65.32.0/19 (1 entry, 1 announced)
BGP group IBGP-3 type Internal
Route Distinguisher: 10.233.65.32:10223
VPN Label: 201
Nexthop: Self
Flags: Nexthop Change
Localpref: 100
AS path: [4788] I
Communities: target:4788:10216
AttrSet AS: 65503
Localpref: 100
AS path: I
Aggregator: 65503 10.65.32.1
# export policy #
只set from neighbor term 不生效，所有routes都会被放行：
XL211150@offramp21.klg-re0> show configuration policy-options policy-statement NSATMS_ADS_NEXT_HOPS
term V4_REDIRECT_PREFIXES {
from {
neighbor 10.55.224.250;
route-filter 0.0.0.0/0 prefix-length-range /32-/32;
}
then {
next-hop 10.55.224.249;
accept;
}
}
< - From the following document, for BGP export policies, specifying the neighbor match condition has no effect and is ignored. That’s why all /32 routes are advertised to both RRs.
https://www.juniper.net/documentation/us/en/software/junos/routing-policy/topics/concept/policy-configuring-match-conditions-in-routing-policy-terms.html
加上比如from protocol bgp或者community xxx之类的...，这个term才能生效
# BGP Refresh Timer #
Starting with Junos OS 16.1R1, the BGP route age will be refreshed whenever any route attributed(includes overload bit) will change an active IGP path/route.
>>> This problem was fixed along with changes for PR#1124202. This change in BGP behavior is applicable from junos:15.1F5-S3 junos:15.1F6 junos:16.1R1
https://juniper.lightning.force.com/lightning/r/Knowledge__kav/ka03c0000009UA4AAM/view
-优势：不仅是可以跨AS，而且可以大的scale traffic，通过policy来具体control路由，可靠传输
-用TCP port 179，TCP的三次握手，所有bgp消息都通过单播，因此不需要组播包。
需要通过有IGP的路由可以承载tcp包。TCP建立成功后，传输的是bgp的包。
-Bgp本身不产生路由，需要export，否则即使peer建立起来了，show route也显示不出来这条路由
#BGP Message：
Open，tcp session建立后发open... ...msg，协商的是bgp的参数
Update，增加或撤销路由,里面包含...NLRI...信息
Notification，bgp出错，通知peer要断开bgp connection
keepalive，保活bgp，Keepalive= hold time/3=30s by default
#BGP States:
1.Idle:初始状态
2.Connect: waitting for TCP session to be completed. 如果tcp建立成功发open
3.Active：如果Failed变成active and retry.
4.OpenSent:收到对方的open。如果没收到变成active。
5.OpenConfirm: sendt keepalive msg to peer.wait for keepalive from remote
6.Established:received keepalive from peer. Begin to exchange bgp route info.
# RIB:
BGP router establishes memory locations in which to store routing knowledge, this is RIB.
Rib-in --> *import policy*  -->  rib -->  *export policy* --> rib-out
RIB-In: 从bgp收进来的所有路由，注意import policy是在rib-in表之后对路由作动作，他可以对rib-in里的bgp路由作动作，也可以再把一些igp路由引入bgp，之后这些路由会被放进RIB
RIB: forwarding-table, only one best entry is added to here
RIB-Out: 对rib表里的路由或其他IGP路由作动作，决定export给peer哪些路由
# BGP 和... IGP ...关系... #
BGP...负责送...source...到...destination...，算出...source...出站口...(nh)...和目的地的入站口...(protocol nh)
IGP...负责算出将包从...source...出站口...(nh)...运送到目的地的入站口...(protocol nh)...的最优路径，全程都需要使用的一种...IGP...协议通
# BGP Flap troubleshooting #
1.If only one session flap <<<< check peer
if multiple session flap <<<<<< check the router
2. 看是否是bfd引起的
3.看pfe stream 1151是否有drop, 也会显示在hardware input drop里，看是否有涨.
4. 1151有8个output queue到RE，show mqchip 0 dstat stats 0 1016-1023 看是否有drop
5.查filter里的policer
6. 如果有链路mtu小，那么上游来的大包就可能被分片。分片可能造成重组乱序。导致bgp falp。
解决方法：1. path-mtu-discovery，定期检测链路mtu状况 2. tcp-mss，可以及时调整发包mtu的大小
7.Authendication导致的
traceoption is helpful
# Attribute: 可以用来控制bgp选路
-Origin:
I  -  igp
E -  egp
? -  Incomplete: for those do not fall into either I or E
- AS-Path:
改这个属性方法：1. remove-private 2. xxx private 3. as-prepend 4.local-as
- Nxet-hop：
必须可达，这条路由才会active.
从ebgp学到的路由，发给ibgp邻居的时候，nh默认不变还是ebgp邻居。因此需要在自己身上加nhs或把ebgp邻居路由写进igp里。
从ibgp学到的路由，发给其ebgp邻居的时候，nh默认改成自己。
- MED:
用于出去的路由（进来的traffic）的比较
always-compare-med: by default只有来自同一AS的才比较MED，加这个可以让来自不同AS的也比较
- local-preference：
用于进来的路由（出去的traffic）
- Community：
no-export:比如local A把带有no-export的路由export给peer B，那么B不可以把这条路由发给其他ebgp邻居（也就是其他AS的）。
no-advertise：比如local A把带有no-advertise的路由发给neighbor B，那么B不可以把这条路由发给他的任何邻居。
- Originator-ID：
RR loop prevention
- Cluster-id:
RR loop prevention
# IBGP
- Scaling Methods: 为了ibgp防环从一个ibgp peer学来的路由不会传给另一个peer，从而要求full mesh，产生scalablility issue。
solution：route reflection OR confederations
# RR:
-打破从一个ibgp peer学到的路由不能传给另一个ibgp peer的规定
-转发机制：
1. 从ebgp peer收到的路由：
rr把路由发给所有non-client和client routers。但发给client的时候会给路由加上cluster list 和originate id这两个属性。
2.从 client ibgp收到：
rr把路由发给其他client和non-lient以及ebgp peers。client和non-client收到的路由都有originater-id和modified cluster list。...nh...还是...client...自己，不是改成...rr...。
3.从non-client ibgp收到：
发给所有clients和ebgp peer。不发给其他non-clients。
- RR 冗余：可以避免RR的单点故障
- Hierachiy RR: 可以减小RR之间的full mesh
-RR...转发路由不会改变...nh
-...只经过...local...会加上...cluster id...但不会有...cluster-list...，只有经过其他...RR...才会加上这个
RR...从...client...学到的路由发给另一个...RR...：
labroot@jtac-mx480-r2016-re0# run show route advertising-protocol bgp 3.3.3.3 logical-system rr2 extensive
inet.0: 12 destinations, 12 routes (12 active, 0 holddown, 0 hidden)
* 22.22.22.22/32 (1 entry, 1 announced)
BGP group peer type Internal
Nexthop: 5.5.5.5 <<< nh ...不变还是...client
Localpref: 100
AS path: [65001] I
Cluster ID: 2.2.2.2 <<< ...自己的
Originator ID: 5.5.5.5 <<< client
RR...从...ebgp...邻居学到的路由发给...client...：
labroot@jtac-mx480-r2016-re0# run show route advertising-protocol bgp 2.2.2.2 logical-system rr1 33.33.33.33 extensive
inet.0: 16 destinations, 17 routes (16 active, 0 holddown, 0 hidden)
* 33.33.33.33/32 (2 entries, 1 announced)
BGP group client type Internal
Nexthop: 40.40.40.2...  <<< ...发给...client...的时候...nh...还是...ebgp...邻居
Localpref: 100
AS path: [65001] 65002 I
<<< RR...不改变...nh...，需要配...nhs...在...rr...身上，这样发给...client/non-client...的时候...nh...才可达
<<<...不加...cluster id...这些了
RR...从另一个...RR...（...non-client...）学到的路由发给自己的...client...：
labroot@jtac-mx480-r2016-re0# run show route advertising-protocol bgp 2.2.2.2 logical-system rr1 22.22.22.22 extensive
inet.0: 12 destinations, 12 routes (12 active, 0 holddown, 0 hidden)
* 22.22.22.22/32 (1 entry, 1 announced)
BGP group client type Internal
Nexthop: 5.5.5.5... <<< ...始发...router
Localpref: 100
AS path: [65001] I  (Originator)
Cluster list:  2.2.2.2... <<< ...另一个...RR
Originator ID: 5.5.5.5... <<< ...始发...router
Cluster ID: 1.1.1.1... <<< ...自己
!!!cluster-list:...包含出了自己之外，所经过的...RR
RR...从...client...学到的路由发给...ebgp...：
labroot@jtac-mx480-r2016-re0# run show route advertising-protocol bgp 40.40.40.2 logical-system rr1 extensive
inet.0: 15 destinations, 15 routes (15 active, 0 holddown, 0 hidden)
* 11.11.11.11/32 (1 entry, 1 announced)
BGP group ebgp type External
Nexthop: Self
AS path: [65001] I
* 22.22.22.22/32 (1 entry, 1 announced)
BGP group ebgp type External
Nexthop: Self
AS path: [65001] I
[edit]
labroot@jtac-mx480-r2016-re0# run show route advertising-protocol bgp 50.50.50.2 logical-system rr2 extensive
inet.0: 15 destinations, 15 routes (15 active, 0 holddown, 0 hidden)
* 11.11.11.11/32 (1 entry, 1 announced)
BGP group rr2 type External
Nexthop: Self
AS path: [65001] I
* 22.22.22.22/32 (1 entry, 1 announced)
BGP group rr2 type External
Nexthop: Self
AS path: [65001] I
labroot@jtac-mx480-r2016-re0# run show route logical-system r3
inet.0: 7 destinations, 9 routes (7 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both
6.6.6.6/32         *[Direct/0] 00:05:46
> via lo0.5
11.11.11.11/32     *[BGP/170] 00:00:29, localpref 100
AS path: 65001 I, validation-state: unverified
> to 40.40.40.1 via xe-4/1/3.0
[BGP/170] 00:00:29, localpref 100
AS path: 65001 I, validation-state: unverified
> to 50.50.50.1 via xe-4/2/1.0
22.22.22.22/32     *[BGP/170] 00:00:29, localpref 100
AS path: 65001 I, validation-state: unverified
> to 40.40.40.1 via xe-4/1/3.0
[BGP/170] 00:00:29, localpref 100
AS path: 65001 I, validation-state: unverified
> to 50.50.50.1 via xe-4/2/1.0
RR...把从一个...client...学到的路由发给另一个...client...：
labroot@jtac-mx480-r2016-re0# run show route advertising-protocol bgp 7.7.7.7 logical-system rr1 extensive
inet.0: 19 destinations, 20 routes (19 active, 0 holddown, 0 hidden)
* 11.11.11.11/32 (1 entry, 1 announced)
BGP group client type Internal
Nexthop: 2.2.2.2...  << ...始发...router
Localpref: 100
AS path: [65001] I
Cluster ID: 1.1.1.1... << local RR
Originator ID: 2.2.2.2... << ...始发...router
# Confederations
把global的AS划分成几个小的sub-AS
建立两个bgp session，一个是global的ibgp，一个是用private-AS的cbgp
#BGP Load balance
lab@R5# run show route protocol bgp aspath-regex "64514 *"
172.31.32.0/24     *[BGP/170] 00:07:02, localpref 100, from 192.168.0.10
AS path: 64514 I
to 192.168.0.10 via ge-0/0/5.303
> to 192.168.0.14 via ge-0/0/5.304
<<<从这个可以看出，multipath可以安装2个nh到active的route里面。这样可以随机走任何一条nh，一个nh断了也不会造成路由层面断。
root@R5# run show route forwarding-table destination 172.31.32.0/24
Routing table: default.inet
Internet:
Destination        Type RtRef Next hop           Type Index NhRef Netif
172.31.32.0/24     user     0                    ulst 262148    17
192.168.0.10       ucst   625     4 ge-0/0/5.303 <<<<
192.168.0.14       ucst   627     3 ge-0/0/5.304 <<<<
<<<在forwarding-option里面配LB，就可以看到forwarding table里有两个nh
#RT - route target #
普通vpnv4路由是通过在本地vrf-import vrf-export来控制收发...。...rt这个family是我告诉bgp peer你通告哪些路由过来...，这样在local就不需要先收进来在通过vrf...-import来过滤路由了...。
这个rt也有一个nh，这个nh在发rt路由的设备上，而收到的vpnv...4的...nh是在bgp... peer身上...。
当rt的nh在bgp peer上不可达...，bgp... peer会把rt路由hidden...，然后withdraw往外通过的vpnv...4路由...。
RT nh is igp:
当学到的rt的nh是bgp...，inet...3是通过lsp学到的
在发rt的设备上...，当rt... ...nh变化，会撤销rt路由通告给bgp... peer
2023-0804-744335
# BGP default policy #
Each BGP policy has a default term at the very end. It isn’t configurable, but follows the default rules of eBGP: advertise all eBGP and iBGP prefixes to the neighbor; otherwise, deny all other prefixes. This simply means that other BGP prefixes in the routing table will be advertised to other peers. You can stop this behavior by installing an explicit reject action at the end.
==================================
# BGP packet default DF set #
Default DF set, 如果...path interface mtu...太小，会导致...bgp ...丢包
# ...BGP Link-State distribution routes (table lsdist.0)... #
XL211147@rr01.klg-re0> show route table lsdist.0 | match localpref | except w | except d
Oct 24 09:31:43
*[BGP/170] 15:50:40, localpref 100, from 10.233.160.16
[BGP/170] 15:50:43, localpref 100, from 10.233.160.17
*[BGP/170] 22:34:43, localpref 100, from 10.233.45.43
*[BGP/170] 15:50:40, localpref 100, from 10.233.160.16
# unilst #
https://kb.juniper.net/InfoCenter/index?page=content&id=KB34832&act=login
[Internal Only] How to efficiently find the prefixes associated with unilist index ID on MX platforms
-------------------
>start shell pfe network fpc
#show topology nhdb id <unilist id>
< -- Pick RTNH ID from above output and use below:
#show route hw nhs <RTNH id>
-------------------
# Dumping #
BGP flap damping should in principle be applied only in the Global Internet Routing Table. Some recent research can be found here:
https://labs.ripe.net/author/clemens_mosig/should-you-update-your-route-flap-damping-parameters/
Summary - the table in the article suggests using suppress threshold of at least 6000, while all other parameters should be kept at their defaults:
RFD parameter
Cisco Default
Juniper Default
Recommendations: BCP 194 / RIPE-580
Suppress-threshold
2000
3000
6000 (needs adjustment)
Re-advertisement penalty
0
1000
0/1000
Attributes change penalty
500
500
500
Withdrawal Penalty
1000
1000
1000
Half-life (min)
15
15
15
Reuse-threshold
750
750
750
Max suppress time (min)
60
60
60
# ...抓包... #
RST: reset bgp session
# Sequence Number #
# Local-as #
如果一个...router...上的一个...instance...学到的...bgp...路由包含...as-path...是这台...router...上任何一个其他...instance...的...local-as...，那个这条路由就会在这个...instance...上...hidden...。... ...2021-0413-0012...-bradley
{master}
XL211145@imse21.brf-re0> show route table WSMGT-h.inet.0 hidden detail
AS path: 65503 4818 I  (Looped: 65503) ... <<<
Workaround...：... ...在重复...as-path...的那个其他...instance...里配...as loops 2...：
set routing-instances vpn-b protocols bgp group ebgp local-as loops 2
那个这条路由就会不...hidden...在原本的...instance...里了
# Limit AS-path length #
https://junipernetworks.sharepoint.com/teams/CSS/sirt/hot_page/Wiki%20Pages/DOC-169409.aspx
Workaround
One way to limit AS_PATH length is to create an import policy with an associated as-path filter that will drop updates with AS_PATH longer than a specified number of ASes (eg. 40). The AS_PATH filter can be any length. In the following example, AS_PATHs with more than 40 ASes, which would take 80 bytes with 2-byte AS and 160 bytes with 4-byte AS, are rejected:
policy-statement LIMIT-AS_PATH {
term 10 {
from {
protocol bgp;
as-path MAX-AS_PATH;
}
then reject;
}
}
as-path MAX-AS_PATH ".{41,}"
# BGP unumbered routers #
允许 BGP 在点对点链路上不配置接口 IP 地址，而是直接通过 接口的链路本地地址 (IPv6 link-local address) 或 接口 来建立邻居关系。
protocols {
bgp {
group fabric {
type external;
neighbor-interface xe-0/0/0;
family inet-vpn;
family evpn;
}
> show route protocol bgp
10.0.0.0/24   *[BGP/170] 00:01:23, localpref 100
to fe80::250:56ff:feaa:bbcc via xe-0/0/0.0
广泛应用：数据中心 EVPN fabric、Clos 网络、SR（Segment Routing）架构中常用。
# remove private #
2021-0503-0724
Please confirm if you wanted to remove all the private AS number ? if yes please use "remove-private all" command. remove-private will remove only the immediate private neighbor.
# independent-domain #
https://www.juniper.net/documentation/us/en/software/junos/static-routing/topics/ref/statement/independent-domain-edit-routing-options.html
The independent domain uses transitive path attribute 128 (attribute set) messages to tunnel the independent domain’s BGP attributes through the internal BGP (IBGP) core.
This improves the transparency of Layer 3 VPN services for customer networks by preventing the IBGP routes that originate within an autonomous system (AS) in the customer network from being sent to a service provider’s AS. Similarly, IBGP routes that originate within an AS in the service provider’s network are prevented from being sent to a customer AS.
If the independent AS domain (It is enabled with independent-domain knob, and attribute set messages are enabled by default) is configured for the virtual routing and forwarding (VRF) instance, the global autonomous system (AS) number in the master routing instance should be prepended to the AS path when the route prefix is imported into the VRF instance. And with no-attrset configured (which disable the attribute set messages), the global AS number in the master routing instance should not be prepended to the AS path.
PR1679646 aggregator with AS 0 under attribute-set when indipendent-domain is configured
set routing-instances HSBA_MXSVOB-s_DOWNSTREAM routing-options router-id 10.65.32.1set routing-instances HSBA_MXSVOB-s_DOWNSTREAM routing-options autonomous-system 65503set routing-instances HSBA_MXSVOB-s_DOWNSTREAM routing-options autonomous-system independent-domainset routing-instances HSBA_MXSVOB-s_DOWNSTREAM routing-options static route 10.65.33.0/24 discardset routing-instances HSBA_MXSVOB-s_DOWNSTREAM routing-options aggregate route 10.65.32.0/19 discardset routing-instances HSBA_MXSVOB-s_DOWNSTREAM routing-options maximum-prefixes 5000
> show route receive-protocol bgp 8.8.8.8 10.65.32.0/19 extensive
bgp.l3vpn.0: 623257 destinations, 922795 routes (623257 active, 0 holddown, 0 hidden)* 8.8.8.8:10223:10.65.32.0/19 (1 entry, 1 announced)     Accepted     Route Distinguisher: 8.8.8.8:10223     VPN Label: 58     Nexthop: 8.8.8.8     Localpref: 100     AS path: I      Unrecognized Attributes: 32 bytes     Attr flags e0 code 80: 00 00 ff df 40 01 01 00 40 02 00 40 05 04 00 00 00 64 c0 07 08 00 00 00 00 00 00 00 00     Communities: target:4788:10216
RFC7607
An UPDATE message that contains the AS number of zero in the AS_PATH or AGGREGATOR attribute MUST be considered as malformed and be discarded.
<JNCIS>
# Modify BGP Attributes #
Origin - mandatory
user@Zinfandel>
show route protocol isis
inet.0: 20 destinations, 20 routes (20 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both
10.222.1.0/24 *[IS-IS/18] 15:56:32, metric 20
to 10.222.2.1 via at-0/1/0.0
> to 10.222.3.2 via at-0/1/1.0
192.168.20.1/32 *[IS-IS/18] 15:56:32, metric 10
> to 10.222.2.1 via at-0/1/0.0
192.168.40.1/32 *[IS-IS/18] 15:57:02, metric 10
> to 10.222.3.2 via at-0/1/1.0
user@Zinfandel>
show route advertising-protocol bgp 192.168.40.1
inet.0: 20 destinations, 20 routes (20 active, 0 holddown, 0 hidden)
Prefix Nexthop MED Lclpref AS path
* 10.222.1.0/24 10.222.3.2 20 100 I
* 172.16.2.4/30 Self 0 100 I
* 172.16.2.8/30 Self 0 100 I
* 192.168.20.1/32 10.222.2.1 10 100 I
* 192.168.40.1/32 10.222.3.2 10 100 I
Origin: display on "as path" column.
I  -  igp
E -  ...e...gp
? -  Incomplete...: for those do not fall into either I or E
The JUNOS software uses the existing next-hop value for routes redistributed from an IGP:
Modify Origin:
term statics {
from protocol static;
then accept;
}
term isis {
from protocol isis;
then {
origin incomplete;
accept;
}
}
user@Zinfandel>
show route advertising-protocol bgp 192.168.40.1
inet.0: 20 destinations, 20 routes (20 active, 0 holddown, 0 hidden)
Prefix Nexthop MED Lclpref AS path
* 10.222.1.0/24 10.222.3.2 20 100 ?
* 172.16.2.4/30 Self 0 100 I
* 172.16.2.8/30 Self 0 100 I
* 192.168.20.1/32 10.222.2.1 10 100 ?<<<<
* 192.168.40.1/32 10.222.3.2 10 100 ?
AS-Path...  - mandator
1. Removing Private AS Values
Prevent private AS# to be exposed to the internet
remove-private configuration<<<before add local AS, the router will remove all the private AS from AS-Path list.
2....Migrating to a New Global AS Number
[edit protocols bgp]
user@Chardonnay# show group external-peers
type external;
peer-as 4444;
local-as 1111 private; <<not advertise this as# to peer
neighbor 10.222.44.1;
user@Merlot> show route protocol bgp terse
inet.0: 16 destinations, 16 routes (16 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both
A Destination P Prf Metric 1 Metric 2 Next hop AS path
* 172.16.1.0/24 B 170 100 100 >10.222.45.2 4444 I
3.Providing Backbone Service for BGP Peers
4.Routing Policy
如果只是...prepend as...号，由于选路...as list...长的不优，容易造成这条路不给选。
--a...s-path-prepend：自己在...bgp group...下加这个命令，那么自己从这个...group...的...peer...收到的路由就有...peer as...号两遍，从而加长...AS-list
[edit policy-options]
user@Chardonnay# show policy-statement prepend-to-aggregate
term prepend {
from protocol aggregate;
then {
as-path-prepend "64888 64888";
accept;
}
user@Chardonnay# show protocols bgp
group external-peers {
type external;
export adv-routes;
neighbor 10.222.45.1 {
export prepend-to-aggregate;
peer-as 65020;
user@Chardonnay> show route advertising-protocol bgp 10.222.45.1
inet.0: 11 destinations, 11 routes (11 active, 0 holddown, 0 hidden)
Prefix Nexthop MED Lclpref AS path
* 172.16.0.0/16 Self 64888 64888 [64888] I... ...《《《《《
--Prepend customer's AS#
the as-path-expand last-as count value policy
user@Merlot# show policy-options
policy-statement prepend-customer-as {
term prepend {
from as-path AS64888;
then {
as-path-expand last-as count 3;
}
}
}
as-path AS64888 ".* 64888 .*";
user@Merlot# show protocols bgp group external-peers
type external;
neighbor 10.222.3.1 {
peer-as 65030;
}
neighbor 10.222.1.1 {
export prepend-customer-as;
peer-as 65020;
}
MED
By default, only compare MED for routes from same AS.
Always Comparing MED Values KNOB
path-selection cisco-non-deterministic command.
Associating the MED to the IGP Metric
To apply the MED values only to specific routes, use a routing policy to locate those
routes and then set your desired value.
user@Sherry> show configuration policy-options | find set-med
policy-statement set-med {
term only-AS65010-routes {
from as-path local-to-AS65010;
then {
metric 10;
}
}
}
as-path local-to-AS65010 "()"
user@Sherry> show configuration policy-options | find set-med
policy-statement set-med {
term only-AS65010-routes {
from as-path local-to-AS65010;
then {
metric 10;
}
}
}
as-path local-to-AS65010 "()"
Local Preference
user@Shiraz> show configuration policy-options | find set-local-preference
policy-statement set-local-preference {
term only-AS65030-routes {
from {
route-filter 172.31.0.0/16 exact;
}
then {
local-preference 150;
accept;
}
}
}
as-path local-to-AS65010 "()";
type internal;
local-address 192.168.36.1;
export nhs;
neighbor 192.168.16.1;
neighbor 192.168.24.1 {
export [ nhs set-local-preference ];
}
IBGP Scaling Methods
产生原因：...ibgp...要求...full mesh...，从而产生...scalablility issue...。
solution...：route reflection .../ c...onfederations
Route Reflection
The approach taken in a route reflection network to solving the full-mesh problem is allowing... ...an IBGP-learned route to be readvertised, or reflected, to an IBGP peer. This is allowed to occur... ...only on special routers called route reflectors (RR), which utilize some BGP attributes defined... ...specifically for a route reflection network.
- th...e route reflector and its clients are considered a... ...cluster in the network
To prevent routing loops in the network, two new BGP attributes and a new identifier value... ...are defined by the route reflection specification. These items are:
Cluster ID The cluster ID is similar in nature to the AS number defined for each router. A
unique 32-bit value is assigned to each cluster in the network that identifies and separates it
from other network clusters.
Cluster List The Cluster List is a BGP attribute that operates like the AS Path attribute. It contains... ...a list of sequential cluster IDs for each cluster a particular route has transited. It is used as... ...the main loop avoidance mechanism and is never transmitted outside the local AS.
Originator ID The Originator ID is also a BGP attribute defined for use by route reflectors.... ...It identifies the router that first advertised the route to the route reflector in the network. Route... ...reflectors use the Originator ID as a second check against routing loops within the AS. Like the... ...Cluster List attribute, the Originator ID is local to the AS and is never transmitted across an... ...EBGP peering session.
The route... ...reflector forwards active BGP routes based on how they were received using the following rules:
From an EBGP peer：
When an RR receives an active BGP route from an EBGP peer, it forwards
the route to all clients in its cluster as well as to all other IBGP nonclient peers.
From an IBGP client peer：
When an RR receives an active BGP route from an IBGP peer that
belongs to its cluster, it forwards the route to all other clients in its cluster as well as all other... ...IBGP nonclient peers.... ...These IBGP advertisements contain the Originator ID attribute and a... ...modified Cluster List.The route reflector also advertises the route to all of its EBGP peers
From an IBGP nonclient peer：
When an RR receives an active BGP route from an IBGP peer
that is not within a cluster, it forwards the route to all clients in its cluster with the appropriate... ...attributes attached. The route reflector also advertises the routes to its EBGP peers without the... ...route reflection attributes
Hierarchical Route Reflection
RR ...Redundancy
user@Zinfandel> show configuration protocols bgp
group internal-peers {
type internal;
local-address 192.168.56.1;
neighbor 192.168.20.1;
}
group cluster-1 {
type internal;
local-address 192.168.56.1;
cluster 1.1.1.1;...  <<<this is RR
Confederations
<<<breaking the network up into smaller pieces.
AS Confederation The AS confederation is technically the collection of the sub-AS networks
you create. Generally speaking, it is your globally assigned AS number, and it is how other systems in the Internet view you.
AS Confederation ID Your AS confederation ID is the value that identifies your AS confederation
as a whole to the Internet. In other words, it is your globally unique AS number.
Member AS Member AS is the formal name for each sub-AS you create in your network. In
essence, each small network is a member of the larger confederation that makes up your global AS.
Member AS Number Each member AS in your confederation receives its own member AS
number. This unique value is placed into the AS Path attribute and is used for loop prevention.
========================================================
-BGP...好处：
不仅是可以跨...AS
而且可以大的...scale traffic
通过...policy...来具体...control...路由。
可靠传输
-...Reliable Transport
BGP accomplishes... ...this goal by utilizing the Transmission Control Protocol (TCP) as its underlying transport... ...mechanism.
TCP... ...port 179
TCP...的三次握手，需要通过有...IGP...的路由可以承载...tcp...包。...TCP...建立成功后，传输的是...bgp...的包。
Bgp...：
When two BGP peers establish a connection, they initially advertise their entire routing... ...knowledge to each other. ... ...Updates are sent only when new information is learned or existing information is no longer... ...valid. This change-based system provides the best possible scalability for the Internet’s routing... ...protocol.
OSPF:
DB...有一个老化时间...3600s...，到时间及时没有路由变化也要...refresh...自己的...DB...给...peer...。...peer...对比自己的...DB...有不一样的，就发...LSR. ...如果没到老化时间，只有当自己的路由变化时，才发送自己的新的...DB...给...peer...。
==========================================
BGP ...报文格式
http://bbs.51cto.com/thread-931196-1.html
BGP在建立对等连接之前，两个邻居必须先执行标准的TCP三次握手进程，并在端口179打开TCP连接。TCP为一条可靠连接提供了分段，重传，确认以及排序等功能，从而将BGP从这些功能中解放出来。所有的BGP消息均采用单播方式经TCP连接传递给邻居。
BGP keepalive...包：
由于KEEPALIVE纯粹是一个通信知会，不需要携带什么信息，因此KEEPALIVE报文实际上是不带数据的BGP报文头。...TCP ...报文
Type 1——Open（打开）消息
Type 3——Keepalive（保持激活）消息
Type 2——Update（更新）消息：增加或撤销路由
Type 4——Notification（通告）消息：断开...bgp connection
https://zhidao.baidu.com/question/488249256.html
数据发送时，由上层向下层封装。
四层，协议层传输的是数据报文，主要是协议格式；
三层，网络层传输的是数据包，包含数据报文，并且增加传输使用的IP地址等三层信息；
二层，数据链路层传输的是数据帧，包含数据包，并且增加相应MAC地址与二层信息。
数据接收的时候，下层向上层解封装。
一个基本TCP/IP报文格式，如楼上所说，ip报文包住tcp报文
所以：
mac地址是在Ethenet帧头里面
源ip和目的ip在IP头部（图中显示为源地址和目的地址）
端口在TCP头部
IP包里面包住TCP包，TCP包里面再包住BGP。
**...与...ospf...包的区别：...ospf...没有...tcp...，...ip...层后直接包着四层协议...ospf
高级...BGP
AS Path...：
路由器提供防环功能，丢弃包含自己...AS...的包。
改变...AS PATH...苏醒：
1.remove private as#...：... remove-private
2.Migrating a new as#...：...local-as
3.backbone service...：... as loop #
4.Policy...：...as-path-prepend
MED:
只有当多条路由来自同一个...peer AS...的时候，才比较...MED...，... ...小优。
1.always-compare-med
Local Preference...：
RR
-...属于...RR...的...BGP...属性：
Cluster ID
Cluster List
Originator ID:...当收到一条带有...RR...属性的...bgp...路由是，如果...originator ID...是本台路由器，那么就会被丢掉。
-RR...需要全互联，...client...不用
Hierachi RR
-...允许一个...cluster...的...RR...是另一个...cluster...的...client...，解决...RR...之间需要全互联的问题。
RR...的冗余
Confederation
把...AS ...分成若干小的...sub-as...，跑...CBGP.
MBGP
# RPKI #
培训视频和PPT已经上传到 CFTS APAC : Shared Documents>Training Material>RPKI_Training下面
https://junipernetworks-my.sharepoint.com/:v:/g/personal/jinjiew_juniper_net/EUSiVR-IP89MoXRPceAE99sBVA7SjXYKxTs7i0fXYb2Jrg
给...bgp...边界作安全保证。在公网一般大于.../24...的太明细了，容易吧路由表撑大，会被...filter...掉。
《...JNCIA...》
-p332... ...为什么有从...ibg peer...学到的路由不传给其他ibgp... ...peer的规定？
A--B
|    |
C--D
因为...ibgp...没有as-...path...的属性防环，所以...A...从...B...学到的路由会通过...C...和...D...的传递到达...B...，...B...会把这个由...A...始发的路由再传回给...A. ...造成loop
BGP States:
Idle
Connect: waitting for TCP session to be completed.
Success->send open msg and change state to OpenSent
Failed->state ->active and retry. Then state will change to connect again
Active: failed
OpenSent: tcp success.
Send open msg and wait for open from peer.
Upon receiving the open msg from peer, it will send a keepalive msg.
If bgp negotiation success, then state-> open confirm
Failed state->active
OpenConfirm: wait for keepalive from remote
Established:receive keepalive from peer. Begin to exchange bgp route info.
BGP Message:
Open Message
The Open message is the first packet BGP sends to a peer after the TCP connection has been established.
Hold Time: junos default 90s.  the two peers negotiate this value to the lower of the two proposals. Keepalive= hold time/3=30s by default
Update Message
Routes update
Path attributes
Notification Message
When a BGP peer detects an error within the session, it sends a Notification message to the remote router and immediately closes both the BGP and TCP sessions.
-Error Code
1 for a Message Header Error
 2 for an Open Message Error
 3 for an Update Message Error
 4 for Hold Time Expired
 5 for a Finite State Machine Error
 6 for a Cease
When you manually
clear a BGP connection, you generate a Cease (Error Code 6) Notification message on the
local router. The BGP and TCP sessions are torn down and reestablished. In addition, a configuration change that alters the parameters of an existing session will generate a Cease message by the local router.
Keepalive Message
-The advertisement of an Update message within the keepalive period resets the timer to 0. In short, a Keepalive is sent only in the absence of other messages for a particular session.Should the local router not receive a Keepalive or Update message within the hold-time period,
-19-octet message header and no other data.
RIB
Each BGP router establishes memory locations in which to store routing knowledge. These are collectively known as a Routing Information Base (RIB). A BGP peer maintains three categories of RIBs: the Adjacency-RIB-In, the Local-RIB, and the Adjacency-RIB-Out.
RIB-In: import policy...，... best route put in RIB table
RIB: forwarding-table, only one best entry is added to here
RIB-Out: export policy...，... export routes only in RIB table
Route Selection Process
1. The Next Hop attribute value for each route must be reachable in the local routing table;... ...otherwise, the local router discards the route.
2. The router selects the route with the highest Local Preference attribute value.(ibgp)
3. The router selects the route with the shortest AS Path length.(ebgp)
4. The router selects the route with the smallest Origin attribute value.
5. The router selects the route with the smallest Multiple Exit Discriminator attribute value.This step is executed, by default, only for routes from the same neighboring AS.
6. The router selects routes learned from an EBGP peer over routes learned from an IBGP... ...peer. If the remaining routes are all EBGP-learned routes, the router skips to step 9.
7. The router selects the route with the smallest IGP metric to the advertised BGP Next Hop.... ...《《只有...ibgp compare...这个
8. If Route Reflection is used for IBGP peering, the router selects the route with the shortest... ...Cluster-List length.
9. The router selects the route from the peer with the smallest numerical Router ID.
10. The router selects the route from the peer with the smallest numerical Peer Address.
BGP Attributes
Next Hop
-...从...ebgp...学到的路由...next-hop...改成...peer address...，从...ibgp...学到的路由默认不改下一跳。
The Shiraz and Cabernet routers are EBGP peers in AS 10 and AS 20, respectively. As we
discussed in the “Peers” section earlier in this chapter, they are directly connected and the
BGP session is established between the 172.16.1.1 and 172.16.1.2 addresses. As Shiraz advertises... ...the 10.200.0.0 /16 and 10.201.0.0 /16 routes to Cabernet, it changes the Next Hop
attribute to the IP address on its side of the session—172.16.1.2 in our example. Cabernet is
directly connected to the 172.16.1.0 /24 subnet, so reachability to the Next Hop is achieved.
Figure 8.11 shows an IBGP peer session between Cabernet and Riesling. When Cabernet
advertises those same routes across that peer session, the Next Hop attribute is not changed—
it is still 172.16.1.2. Chances are that Riesling does not have a route to the 172.16.1.0 /24 subnet
because it is outside the AS boundary. This lack of reachability causes the 10.200.0.0 /16
and 10.201.0.0 /16 routes to become unusable to Riesling.
To resolve this issue, the administrators of AS 10 must alter their current environment. There
are five viable methods for providing reachability to the BGP Next Hop:
Setting the Next Hop attribute
Using an IGP passive interface
Configuring an export policy
Establishing an IGP adjacency
Using static routes
MED...（...optional...）
By default, the MED attribute is only compared for routes received from the same ...AS
AS.
Community（...optional...）
The community-ids are either a single community value or multiple community values. When more than one value is assigned to a community name, the routing device interprets this as a logical AND of the community values. In other words, a route must have all of the configured values before being assigned the community name
show bgp summary
show bgp group
show bgp neighbor
show route protocol bgp
show route receive-protocol bgp 192.168.7.7...  ...《《...rib in
show route advertising-protocol bgp 192.168.5.5... ...《《...rib out
A route can appear in the routing table as a BGP route only if it... ...is received from a BGP peer, but a BGP peer can only advertise a route if it’s already in the routing... ...table as a BGP route.
Nexthop-Self (for ibgp learning ebgp routes sends to ibgp peer. Bye default, the nh not change)
user@Shiraz# show policy-options
policy-statement next-hop-self {
term change-the-attribute {
from protocol bgp;
then {
next-hop self;
}
[edit protocols bgp]
user@Shiraz# show
group ebgp-peers {
type external;
neighbor 172.16.1.1 {
peer-as 10;
}
neighbor 172.16.2.1 {
peer-as 30;
}
}
group ibgp-peers {
type internal;
local-address 192.168.5.5;
export next-hop-self;
user@Shiraz> show route advertising-protocol bgp 192.168.7.7
inet.0: 26 destinations, 27 routes (26 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both
10.10.1.0/24
Self 0 100 10 I
10.10.2.0/24
Self 0 100 10 I
10.10.3.0/24
Self 0 100 10 I
===================================
BGP ...综合实验
***...当在...ibgp group...里面...set export...的时候，就是我这台...router...向我的...ibgp ...邻居发路由。
[edit protocols bgp]
lab# show
group ibgp-peers {
type internal;
local-address 192.168.4.1;
log-updown;
export next-hop-self;  <<<<<<<
neighbor 192.168.1.1;
neighbor 192.168.2.1;
neighbor 192.168.3.1;
***bgp...不产生路由，当...bgp up...但没...export...路由的时候，...inet0...里面只有...igp...的路由。
******export ospf routes to ebgp peer
policy-statement to-bgp-routes {
term term1 {
from {
route-filter 202.103.0.0/16 longer;
}
then reject;
}
term term2 {
from protocol direct;
then reject;
}
term term3 {
from protocol ospf;
then accept;
}
group ebgp-peer {
type external;
log-updown;
export to-bgp-routes;  <<<<<<
neighbor 202.103.45.5 {
peer-as 200;
}
neighbor 202.103.46.6 {
peer-as 200;
}
On ebgp peer
lab# run show route
inet.0: 33 destinations, 36 routes (33 active, 0 holddown, 3 hidden)
+ = Active Route, - = Last Active, * = Both
192.168.1.1/32     *[BGP/170] 00:00:32, MED 2, localpref 100
AS path: 100 I, validation-state: unverified
> to 202.103.45.4 via ge-0/0/2.0
192.168.2.1/32     *[BGP/170] 00:00:31, MED 1, localpref 100
AS path: 100 I, validation-state: unverified
> to 202.103.45.4 via ge-0/0/2.0
192.168.3.1/32     *[BGP/170] 00:00:31, MED 1, localpref 100
AS path: 100 I, validation-state: unverified
> to 202.103.45.4 via ge-0/0/2.0
***...关于...nhs...详解
R15...上...export 15.1...路由进...ebgp group...：
[edit protocols bgp]
lab# show
group ebgp-peers {
type external;
log-updown;
export to-bgp-routes;...  ...《《《《《《《《
neighbor 202.103.145.14 {
peer-as 200;
}
}
R14...有了这条...bgp...路由：
192.168.15.1/32    *[BGP/170] 00:00:02, localpref 100
AS path: 300 I, validation-state: unverified
R5...上这条...bgp...路由被...hidden...：...indirect nh
nhs...是为了解决...nh...不可达问题的。如果...ebgp...的路由传给...ibgp...邻居时，路由可达就不需要...nhs...了。
但是...R14...上没有到192.168.15.1的路由所以需要...nhs...。
###export nhs:...从...ebgp...邻居学来的路由，下一跳是...ebgp...邻居，本台路由器有到...ebgp...邻居路由所以...ok...。但是当本台...router...将该条路由传给...ibgp...邻居的时候，下一跳是...ebgp...邻居对于...ibgp...邻居就是不可达了。所以这条路由会被...hidden...。所以需要加...nhs...，让下一跳变成本台路由器的地址。
***...注意当...ebgp...将从...ibgp...邻居学来的路由传给...ebgp...邻居的时候，下一跳自动改成本台路由器。
lab# run show route hidden extensive
inet.0: 31 destinations, 31 routes (30 active, 0 holddown, 1 hidden)
192.168.15.1/32 (1 entry, 0 announced)
BGP    Preference: 170/-101
Next hop type: Unusable
Address: 0x9293e84
Next-hop reference count: 1
State: <Hidden Int Ext>
Local AS:   200 Peer AS:   200
Age: 1:24
Validation State: unverified
Task: BGP_200.192.168.14.1+50491
AS path: 300 I
Accepted
Localpref: 100
Router ID: 192.168.14.1
Indirect next hops: 1...   ...《《《《《《《
Protocol next hop: 202.103.145.15
Indirect next hop: 0x0 - INH Session ID: 0x0
R14...上给...ibgp...加...nhs
group ibgp-peers {
type internal;
local-address 192.168.14.1;
log-updown;
export next-hop-self;  <<<<
neighbor 192.168.5.1;
neighbor 192.168.6.1;
neighbor 192.168.7.1;
neighbor 192.168.8.1;
neighbor 192.168.9.1;
neighbor 192.168.10.1;
neighbor 192.168.11.1;
neighbor 192.168.12.1;
neighbor 192.168.13.1;
}
>>...这时经过的每个...ibgp ...邻居都把该条路由的...nh...改成自己到吓一跳...router...的出口...address...，再传给吓一跳路由器。
R5...这时就有了这条路由。
192.168.15.1/32    *[BGP/170] 00:00:15, localpref 100, from 192.168.14.1
AS path: 300 I, validation-state: unverified
> to 202.103.57.7 via ge-0/0/1.0  <<<<<< 57.7...是...R7
to 202.103.58.8 via ge-0/0/3.0
当...R5...把这条路由传给...ebgp...邻居...R4...的时候，默认从...ibgp...学到的路由传给...ebgp...邻居，...nh...改成...R5...自己。所以...R4...出道这条路由：
192.168.15.1/32    *[BGP/170] 00:05:09, localpref 100
AS path: 200 300 I, validation-state: unverified
> to 202.103.46.6 via ge-0/0/3.0...
[BGP/170] 00:05:07, localpref 100
AS path: 200 300 I, validation-state: unverified
> to 202.103.45.5 via ge-0/0/2.0《《《...45.5...是...R5
R3
192.168.15.1/32    *[BGP/170] 00:02:12, localpref 100, from 192.168.4.1
AS path: 200 300 I, validation-state: unverified
> to 202.103.34.4 via ge-0/0/0.0
R1
192.168.15.1/32    *[BGP/170] 00:00:07, localpref 100, from 192.168.4.1
AS path: 200 300 I, validation-state: unverified
> to 202.103.12.2 via ge-0/0/0.0
to 202.103.13.3 via ge-0/0/1.0
可以发现即使...R3/R2/R1...上没有配...nhs...，也能收到这条...bgp...路由，原因是他们有到202.103.45....0...的路由。所以当...R4...给他们传这条...ebgp...路由时，...nh...可达。因为...1.45...和...1.46...在...ospf are0...里面，所以这个与...ebgp...的吓一跳可达。
R1
202.103.45.0/24    *[OSPF/10] 01:53:54, metric 3
to 202.103.12.2 via ge-0/0/0.0
> to 202.103.13.3 via ge-0/0/1.0
如果将这两个口从...ospf...里...disable...，那么这个...bgp...路由就消失了。或者配上...nhs...。
****...如果不...export...一条路由进...bgp...，...bgp...是不会和...bgp peer...发送路由的。当...export...路由进...bgp group...后，...local inet0...表里就出现了...bgp...路由。从而，这条...bgp...路由会传给所有...bgp...邻居。
当...bgp...邻居再将这条...bgp...路由传给他的邻居的时候，就出现了...nh...的问题。
****bgp...改...nh...规律：
1....从...ibgp...邻居学到的路由传给...ebgp...：...nh...自动改成自己
2....从...ibgp...学到的路不传给...ibgp...邻居
3....从...ebgp...学到的路由（...ibgp...不通）传给...ibgp...邻居：...nh...不改，需要配静态或者...nhs
****...把从...ebgp...学到的路由传给...ibgp...邻居，用的是...indirect nh...。...ibgp...用...lb...口建立。
lab# run show route 192.168.15.1 extensive
inet.0: 36 destinations, 36 routes (36 active, 0 holddown, 0 hidden)
192.168.15.1/32 (1 entry, 1 announced)
TSI:
KRT in-kernel 192.168.15.1/32 -> {indirect(1048574)}
*BGP    Preference: 170/-101
Next hop type: Indirect
Address: 0x940e818
Next-hop reference count: 75
Source: 192.168.4.1
Next hop type: Router, Next hop index: 562
Next hop: 202.103.24.4 via ge-0/0/1.0, selected
Session Id: 0x1
Protocol next hop: 192.168.4.1
Indirect next hop: 0x96fc000 1048574 INH Session ID: 0x13
State: <Active Int Ext>
Local AS:   100 Peer AS:   100
Age: 20:11      Metric2: 1
Validation State: unverified
Task: BGP_100.192.168.4.1+61893
Announcement bits (2): 0-KRT 4-Resolve tree 1
AS path: 200 300 I
Accepted
Localpref: 100
Router ID: 192.168.4.1
Indirect next hops: 1
Protocol next hop: 192.168.4.1 Metric: 1
Indirect next hop: 0x96fc000 1048574 INH Session ID: 0x13
Indirect path forwarding next hops: 1
Next hop type: Router
Next hop: 202.103.24.4 via ge-0/0/1.0
Session Id: 0x1
192.168.4.1/32 Originating RIB: inet.0
Metric: 1                       Node path count: 1
Forwarding nexthops: 1
Nexthop: 202.103.24.4 via ge-0/0/1.0
****BGP
Export:...把。。。从...bgp session...，...group...，...neighbor...中...export...出去《《《效果做到...neibor...路由器上
import...：把。。。在该...bgp session...，...group...，...neighbor...中...import...进去《《效果做到...local...路由器上
***...改...origin
policy-statement set-r5-origin {
term term1 {
from {
route-filter 172.16.2.0/24 exact;
route-filter 172.16.3.0/24 exact;
}
then origin egp;
}
}
policy-statement set-r6-origin {
from {
route-filter 172.16.1.0/24 exact;
}
then origin incomplete;
group ebgp-peer {
type external;
log-updown;
export to-bgp-routes;
neighbor 202.103.45.5 {
import set-r5-origin;
peer-as 200;
}
neighbor 202.103.46.6 {
import set-r6-origin;
peer-as 200;
}
***remove private AS#
group ebgp-peers {
type external;
log-updown;
export to-bgp-routes;
neighbor 202.103.45.4 {
remove-private;  <<<<<
peer-as 100;
}
如果私有...as...号在两个公有...as...号之间，就不会被移除
***AS ...迁移：对某个...bgp peer...使用特殊的私有...AS#
[edit protocols bgp]
lab# show
group ebgp-peers {
type external;
log-updown;
export to-bgp-routes;
neighbor 202.103.145.14 {
peer-as 200;
local-as 65300; <<<<
}
}
[edit routing-options]
lab# show
autonomous-system 300; <<<<
lab# run show route receive-protocol bgp 202.103.145.15
inet.0: 36 destinations, 40 routes (36 active, 0 holddown, 0 hidden)
Prefix                  Nexthop              MED     Lclpref    AS path
* 172.16.1.0/24           202.103.145.15                          65300 I
* 172.16.2.0/24           202.103.145.15                          65300 I
* 172.16.3.0/24           202.103.145.15                          65300 I
* 192.168.15.1/32         202.103.145.15                          65300 I
*****AS-Override
group ebgp-peers {
type external;
log-updown;
export to-bgp-routes;
neighbor 202.103.45.4 {
peer-as 100;
as-override;
}
lab# run show route
inet.0: 43 destinations, 58 routes (43 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both
172.16.1.0/24      *[BGP/170] 00:00:43, localpref 100
AS path: 200 200 I, validation-state: unverified
> to 202.103.45.5 via ge-0/0/2.0
AS100--AS200--AS100
当从...as100...通过...as200...发给...as100...路由时，...as200...会把...as100...用自己的...200...覆盖住。这样路由到了另一个...as100...就不会因为...as...号重复被丢。
****...或者允许接受...as loops...：
[edit routing-options]
lab# show
autonomous-system 100 loops 2; <<<
lab# run clear bgp neighbor soft-inbound
****AS-Prepend
policy-statement set-as-path {
from {
route-filter 172.16.2.0/24 exact;
route-filter 172.16.3.0/24 exact;
}
then as-path-prepend "100 100 100"; <<<...收到路由后加的，不受...AS...重复限制
group ebgp-peer {
type external;
log-updown;
export to-bgp-routes;
neighbor 202.103.45.5 {
import [ set-r5-origin set-as-path ];<<<<
peer-as 200;
172.16.2.0/24      *[BGP/170] 00:03:07, localpref 100
AS path: 100 100 100 200 300 I, validation-state: unverified
> to 202.103.46.6 via ge-0/0/3.0
MED  875-->...需要加一根线... ...略
Local preference...：... ...略
RR
***所谓ibgp全互联，指的是ibgp session全互联，就是一一互指对方为peer。router之间不需都直连，igp可达就行。解决方法：rr 或 联邦
[edit protocols bgp]
lab# show
group ibgp-peers {
type internal;
local-address 192.168.9.1; <<<<
log-updown;
neighbor 192.168.5.1;
neighbor 192.168.6.1;
neighbor 192.168.7.1;
neighbor 192.168.8.1;
neighbor 192.168.12.1;
}
group cluster-peers {
type internal;
local-address 192.168.9.1; <<<<
log-updown;
cluster 9.9.9.9; <<<<<RR
neighbor 192.168.10.1; <<<<client
neighbor 192.168.11.1;  <<<<client
}
* 192.168.15.1/32 (1 entry, 1 announced)
Accepted
Nexthop: 192.168.14.1
Localpref: 100
AS path: 300 I (Originator)
Cluster list:  9.9.9.9 12.12.12.12 <<<< add cluster ID within AS
Originator ID: 192.168.14.1
* 192.168.12.1/32 (1 entry, 1 announced)
BGP group ebgp-peers type External
Nexthop: Self
MED: 2
AS path: [200] I
<<<<...当通告给...ebgp peer...的时候，...remove...掉...cluster id
分层...RR:
联邦：
group cbgp-peers {
type external;
multihop;
local-address 192.168.7.1;
log-updown;
neighbor 192.168.9.1 {
peer-as 65002; <<<<<< member AS
}
}
[edit routing-options]
lab# show
autonomous-system 65001;
confederation 200 members [ 65001 65002 ];
# RTBH #
2篇有关RTBH的RFC文档
https://datatracker.ietf.org/doc/rfc3882/
https://datatracker.ietf.org/doc/rfc5635/
This document describes an operational technique that uses BGP communities to remotely trigger black-holing of a particular destination network to block denial-of-service attacks.
[10:21 AM] Thomas Yang
Dmitry Shokarev
Limited to source match
You can not be as selective as with filters (RPF check can be selectively enabled on certain interfaces though)
Traffic to offending destinations will be dropped as well.
Hi Matt, SRTBH is way better. You can have millions of rules. For SRTBH we re-use same forwarding structures (routes). And flow spec uses filters. The SRTBH downsides are:
Thanks,Dmitry.
[10:23 AM] Thomas Yang
Jeff HaasThe main question is what they want to do about the traffic.
If it's drop it, use the rtbh features.  This, however, completes the attack.
If it's do fancier things like rate limit, or redirect for analysis/mitigation, you want flowspec.
-- Jeff
[10:25 AM] Thomas Yang
Dmitry Shokarev
Hi Takehiko,
I guess, setting protocol next-hop which is resolved through the route with discard next-hop should be enough?
Why do you need dsc interface?
See some examples below, this is actually coming from the business edge 1.0 design guide, which is about to be published next week (the final document draft is here :  https://matrix.juniper.net/docs/DOC-168921).
Thanks,
Dmitry.
Discard route:
routing-options {
rib inet6.0 {
static {
/* IPv6 blackhole routes should be set up with the ::ffff:192.0.2.1 next-hop
The 192.0.2.0/24 network is only used for documentation and testing
(same applies to IPv6 mapped prefix), therefore it is safe to configure
addresses from this network as blackhole destination.
There is a guarantee that there will be no valid destinationwith this
prefix assigned */
route ::ffff:192.0.2.1/128 {
discard;
no-readvertise;
}
}
}
}
BGP Import Policy:
policy-options {
/*  This policy is customer specific. It allows only certain routes from the customer
It also allows routes with a blackhole community */
policy-statement pol-prefix-allow-${peer_as}-bh {
term allow-v6-blackhole {
from {
family inet6;
/* Accept longer prefixes with the blackhole community
All allowed prefixes are listed here.*/
prefix-list-filter pl-allowed-${peer_as}-v6 orlonger;
community comm-remote-triggered-black-hole;
}
then {
/* Set the next-hop address that is resolved through the discard route */
next-hop ::ffff:192.0.2.1;
/* Attach no-export community */
community add no-export;
next policy;
}
}
}
}
From: Takehiko Akizuki
Sent: Tuesday, January 28, 2014 5:16 PM
To: mx-interest
Subject: RTBH for IPv6
Hello experts.
Can someone please tell me how to configure RTBH for IPv6?
it seems dsc interface isn’t available for IPv6 traffic,so I’m wondering how RTBH filter IPv6 traffic.
Many thanks.
--------------------
Takehiko Akizuki
Resident engineer
JNCIP-SP
==================
from {
protocol tcp;
port 179;
}
then accept;
<<<bgp...的...fw...要这样写，不能写成...destination-port...或...source-port 179...，否则...bgp...起不来
=============================
veto msgs due to slow peer
==================================
Bgp...本身不产生路由，需要...export...，否则即使...peer...建立起来了，...show route...也显示不出来这条路由。
Import...是让这条路由在本地能...show route ...看到
Export...是让自己的这条路由在...peer...的...show route ...能看到。
{master}[edit policy-options]
lab@mx480-re0# show
policy-statement 1 {
term 1 {
from {
route-filter 1.1.1.1/32 exact; ...《《《《《《
}
then accept;
}
}
{master}[edit protocols bgp group ebgp]
lab@mx480-re0# show
type external;
traceoptions {
file ebgp.log size 1000000 files 10;
flag keepalive;...《《《
}
multihop {
ttl 2;
}
family inet {
unicast {
prefix-limit {
maximum 500;
teardown 80;
}
}
}
export 1; ...《《《《《 调用
remove-private;
peer-as 65420;
neighbor 124.1.1.3 {
local-address 123.1.1.2;
}
lab@MX104-re1# set protocols bgp group 11 traceoptions flag ?
Possible completions:
4byte-as             Trace 4 byte AS events
add-path             Trace add-path events
all                  Trace everything
bfd                  Trace BFD events
DAing              Trace BGP damping information
general              Trace general events
graceful-restart     Trace Graceful Restart events
keepalive            Trace BGP keepalive packets
normal               Trace normal events
nsr-synchronization  Trace NSR synchronization events
open                 Trace BGP open packets
packets              Trace all BGP protocol packets
policy               Trace policy processing
refresh              Trace BGP refresh packets
route                Trace routing information
state                Trace state transitions
task                 Trace routing protocol task processing
timer                Trace routing protocol timer processing
update               Trace BGP update packets
===========================================================
<BGP PIC>
http://www.juniper.net/documentation/en_US/junos15.1/topics/task/configuration/bgp-configuring-bgp-pic-for-inet.html
On a BGP Prefix Independent Convergence (PIC) enabled router, Junos OS installs the backup path for the indirect next hop on the Routing Engine and also provides this route to the Packet Forwarding Engine and IGP. When an IGP loses reachability to a prefix with one or more routes, it signals to the Routing Engine with a single message prior to updating the routing tables. The Routing Engine signals to the Packet Forwarding Engine that an indirect next hop has failed, and traffic must be rerouted using the backup path. Routing to the impacted destination prefix continues using the backup path even before BGP starts recalculating the new next hops for the BGP prefixes. The router uses this backup path to reduce traffic loss until the global convergence through the BGP is resolved.
The BGP PIC edge feature is supported only on MX Series 3D Universal Edge routers with MPC interfaces.
Stars from 15.1
http://www.juniper.net/documentation/en_US/junos15.1/topics/task/configuration/layer3-vpn-bgp-pic-edge-configuring.html
Enable BGP PIC Edge:
[edit routing-instances routing-instance-name routing-options]
user@host# set protect core
lab@re0# set routing-options protect ?
Possible completions:
core                 Protect against unreachability to service-edge router
Configure per-packet load balancing:
[edit policy-options]
user@host# set policy-statement policy-name then load-balance per-packet
Apply the per-packet load balancing policy to routes exported from the routing table to the forwarding table:
[edit routing-options forwarding-table]
user@host# set export policy-statement-name
Indirect next hop: 944c000 1048574 INH Session ID: 0x3 Indirect next hop: weight 0x1<<<<active
Protocol next hop: 1.1.1.5 Push 299824 Indirect next hop: 944c1d8 1048577 INH Session ID: 0x4 Indirect next hop: weight 0x4000 <<<<passive
================================================================
<Lab Practice>
Please find below the list of labs to practice
BGP
1
eBGP directly connected interface
2
eBGP based on loopback interface (multihop)
3
iBGP directly connected interface
4
iBGP based on loopback interface
5
BGP authentication
6
Next-Hop-Self
7
Route-reflector (Cluster)
8
BGP Path Manipulation - Local Preference
9
BGP Path Manipulation - AS-PATH
10
BGP Path Manipulation - MED
11
BGP Multipath & Load Balancing
12
BGP As-override
13
BGP Loops & Advertise-peer-as
14
BGP Confederation
==================================================
http://www.juniper.net/techpubs/en_US/junos10.4/topics/reference/configuration-statement/multihop-edit-protocols-bgp.html
ebgp default ttl is 1
set multihop: defualt is 64
set multihop ttl= n  >>> the real ttl=1+n
====================================================
<RR>
bgp {
local-address 26.33.0.5;
hold-time 600;
log-updown;
inactive: group VPLS-RR {
type internal;
family l2vpn {
signaling;
}
local-as 65033;
neighbor 26.33.0.51 {
description ndojima-mx480-t;
}
}
group VPLS-RC {
type internal;
family l2vpn {
signaling;
}
cluster 26.33.0.5;
local-as 65033;
neighbor 26.33.0.30 {
description kote21-kenshomx80-1;
}
}
}
Juniper RR does not need to set rr, but only set cluster under RR, all the neighbor will be client, cluster's ip add will be RR server.
=======================================================
<BGP Flap>
http://www.juniper.net/techpubs/en_US/junos11.4/topics/example/bgp-vpn-session-flap-prevention.html
workaround for a known issue in which BGP sessions sometimes go down and then come back up (in other words, flap) when virtual private network (VPN) families are configured.
=======================================================
# Color #
2021-1022-346460
So for customer's query (1), based on our tests with independent-domain active "color" can work as another "tag" which can be used to as a means to match/select routes with policy in vrf. It can't affect BGP path selection and route selection between static/BGP or other protocols. "Color" can only work as a tiebreaker for static, aggregate or generated routes. ( Please help to confirm and correct me if I'm wrong).
============================================
<Anna>This command re-apply export policies or import policies, respectively, and send refresh updates to one or more BGP neighbors without changing their state. So it does not affect sessions.
> Clear bgp neghibor soft-inbound
http://www.juniper.net/techpubs/en_US/junos10.2/topics/reference/command-summary/clear-bgp-neighbor.html
===============================================
<Chat with harry>  2015-0109-9001
trinity: 0-1023 fabri   1024-1143 wan  1151 host
NPC1(mx480-a-re0 vty)#  show mqchip 0 stream 1151
Input Stream 1151
-----------------
attached    : 1
enabled     : 1
pic slot    : 0
mac mode    : 0
port        : 0
conn        : 19
tclass      : 255 N/A
prio        : 1
weight      : 2047
Output Stream 1151
------------------
attached    : 1
enabled     : 1
pic slot    : 0
mac mode    : 0
wan if      : 0
port        : 0
conn        : 19
weight      : 1
sched       : 0 MQ0
l1 node     : 127
queues      : 1016..1023   <<<<<<<<<<<<<<< NHQ's 8 queues  (wan...方向一个...stream8...个...q)
qxif quota  : 0
qxif start  : 0
qxif size   : 0
10:18 AM
output...方向就是到...RE...的方向
*********
#show mqchip 0 dstat stats 0 1016-1023 ...可以查看这...8...个...queue...的状态...and if any drop on any stream Q
MX104-ABB-0(mx104-re1 vty)# show mqchip 0 dstat stats 0 1016
QSYS 0 QUEUE 1016 colormap 2 stats index 0:
Counter          Packets     Pkt Rate            Bytes    Byte Rate
------------------------ ---------------- ------------ ---------------- ------------
Forwarded (NoRule)                    0            0                0            0
Forwarded (Rule)                      0            0                0            0
Color 0 Dropped (WRED)                0            0                0            0
Color 0 Dropped (TAIL)                0            0                0            0
Color 1 Dropped (WRED)                0            0                0            0
Color 1 Dropped (TAIL)                0            0                0            0
Color 2 Dropped (WRED)                0            0                0            0
Color 2 Dropped (TAIL)                0            0                0            0
Color 3 Dropped (WRED)                0            0                0            0
Color 3 Dropped (TAIL)                0            0                0            0
Dropped (Force)                       0            0                0            0    <<<<<<<<
Dropped (Error)                       0            0                0            0    <<<<<<<<
Queue inst depth     : 0
Queue avg len (taql): 0
Harry Song          stats 0 ...代表...queue system, ...都是...0
*********
Annabelle Wang ...想问下，去...RE...的...traffic...都一定经过...loopback0...吗， 对吗，要过...filter...，可...loopback0...配置里没有...Q
...不是物理上经过...lo0
10:21 AM
...是要经过...lo0...上的...filter
10:21 AM
Annabelle Wang ...哦哦，明白了
10:21 AM
Harry Song          lo0 filter...是在...hnq...之前
*************
Harry Song          show ddos asic punt-proto-maps  ...这个可以看不同的...host bound traffic...会走哪个...hnq
show ddos asic punt-proto-maps
10:42 AM
...有显示针对不同流量有独立的限速
---- -------------
2 PUNT_OPTIONS     |
4 PUNT_CONTROL     |
6 PUNT_HOST_COPY   |
32 PUNT_PROTOCOL    |---------------+
34 PUNT_RECEIVE     |               |
54 PUNT_SEND_TO_HOST_FW |               |
|
------------------------------------------------------------------
type   subtype            group proto         idx q# bwidth  burst
------ ----------    ---------- ----------   ---- -- ------ ------
contrl LACP                lacp aggregate    2c00  3  20000  20000
contrl STP                  stp aggregate    2d00  3  20000  20000
contrl ESMC              t  esmc aggregate    2e00  3  20000  20000
contrl OAM_LFM          oam-lfm aggregate    2f00  3  20000  20000
contrl EOAM                eoam aggregate    3000  3  20000  20000
contrl LLDP                lldp aggregate    3100  3  20000  20000
contrl MVRP                mvrp aggregate    3200  3  20000  20000
contrl PMVRP              pmvrp aggregate    3300  3  20000  20000
contrl ARP                  arp aggregate    3400  2  20000  20000
contrl PVSTP              pvstp aggregate    3500  3  20000  20000
contrl ISIS                isis aggregate    3600  1  20000  20000
contrl POS                  pos aggregate    3700  3  20000  20000
contrl MLP                  mlp unclass..    3801  2   1024   1024
contrl JFM                  jfm aggregate    3900  3  20000  20000
contrl ATM                  atm aggregate    3a00  3  20000  20000
contrl PFE_ALIVE      pfe-alive aggregate    3b00  3  20000  20000
filter ipv4              dhcpv4 aggregate     600  0   5000   5000
filter ipv6              dhcpv6 aggregate     700  0   5000   5000
filter ipv4                icmp aggregate     900  0  20000  20000
filter ipv4                igmp aggregate     a00  1  20000  20000
filter ipv4                ospf aggregate     b00  1  20000  20000
filter ipv4                rsvp aggregate     c00  1  20000  20000
filter ipv4                 pim aggregate     d00  1   8000  16000
filter ipv4                 rip aggregate     e00  1  20000  20000
filter ipv4                 ptp aggregate     f00  1  20000  20000
filter ipv4                 bfd aggregate    1000  1  20000  20000
filter ipv4                 lmp aggregate    1100  1  20000  20000
filter ipv4                 ldp aggregate    1200  1  20000  20000
filter ipv4                msdp aggregate    1300  1  20000  20000
filter ipv4                 bgp aggregate    1400  0  20000  20000    <<<<<<<<<<<  bgp go through Q0
filter ipv4                vrrp aggregate    1500  1  20000  20000
filter ipv4              telnet aggregate    1600  0  20000  20000
filter ipv4                 ftp aggregate    1700  0  20000  20000
filter ipv4                 ssh aggregate    1800  0  20000  20000
filter ipv4                snmp aggregate    1900  0  20000  20000
filter ipv4                ancp aggregate    1a00  1  20000  20000
filter ipv6              igmpv6 aggregate    1b00  1  20000  20000
filter ipv6               egpv6 aggregate    1c00  1  20000  20000
filter ipv6              rsvpv6 aggregate    1d00  1  20000  20000
filter ipv6            igmpv4v6 aggregate    1e00  1  20000  20000
filter ipv6               ripv6 aggregate    1f00  1  20000  20000
filter ipv6               bfdv6 aggregate    2000  1  20000  20000
filter ipv6               lmpv6 aggregate    2100  1  20000  20000
filter ipv6               ldpv6 aggregate    2200  1  20000  20000
filter ipv6              msdpv6 aggregate    2300  1  20000  20000
filter ipv6               bgpv6 aggregate    2400  0  20000  20000
filter ipv6              vrrpv6 aggregate    2500  1  20000  20000
filter ipv6            telnetv6 aggregate    2600  0  20000  20000
filter ipv6               ftpv6 aggregate    2700  0  20000  20000
filter ipv6               sshv6 aggregate    2800  0  20000  20000
filter ipv6              snmpv6 aggregate    2900  0  20000  20000
filter ipv6              ancpv6 aggregate    2a00  1  20000  20000
filter ipv6            ospfv3v6 aggregate    2b00  1  20000  20000
filter ipv4           tcp-flags unclass..    4801  0  20000  20000
filter ipv4           tcp-flags initial      4802  0  20000  20000
filter ipv4           tcp-flags establish    4803  0  20000  20000
filter ipv4                dtcp aggregate    4900  0  20000  20000
filter ipv4              radius server       4a02  0  20000  20000
filter ipv4              radius account..    4a03  0  20000  20000
filter ipv4              radius auth..       4a04  0  20000  20000
filter ipv4                 ntp aggregate    4b00  0  20000  20000
filter ipv4              tacacs aggregate    4c00  0  20000  20000
filter ipv4                 dns aggregate    4d00  0  20000  20000
filter ipv4            diameter aggregate    4e00  0  20000  20000
filter ipv4             ip-frag first-frag   4f02  0  20000  20000
filter ipv4             ip-frag trail-frag   4f03  0  20000  20000
filter ipv4                l2tp aggregate    5000  0  20000  20000
filter ipv4                 gre aggregate    5100  0  20000  20000
filter ipv4               ipsec aggregate    5200  0  20000  20000
filter ipv6               pimv6 aggregate    5300  1   8000  16000
filter ipv6              icmpv6 aggregate    5400  0  20000  20000
filter ipv6               ndpv6 aggregate    5500  0  20000  20000
filter ipv4               amtv4 aggregate    5f00  0  20000  20000
filter ipv6               amtv6 aggregate    6000  0  20000  20000
filter ipv4              syslog aggregate    6100  0   2000  10000
option rt-alert          ip-opt rt-alert     3d02  1  20000  20000
option unclass           ip-opt unclass..    3d01  4  10000  10000
*********************************
Annabelle Wang ...我的问题是，如果...nhq...的...q0...，有丢包那在这...8...个...q...里有没有优先级
10:45 AM
...好的，
10:45 AM
Harry Song          show mqchip 0 sched 0 q 1016
10:53 AM
show mqchip 0 sched 0 q 1016-1023
10:53 AM
...这么来看
10:53 AM
...对于...guarantee prio...，...8...个...q...都是...low
10:53 AM
...对于...excess prio, ...只有两个...q...是...high
10:54 AM
NPC1(mx480-a-re0 vty)# show mqchip 0 sched 0 q 1016
Q node 1016:
allocated           : true
parent node         : 254
guarantee prio      : 3 GL
excess prio         : 2 EL
10:54 AM
gl ...就是...guarantee low , el...就是...excess low
10:54 AM
NPC1(mx480-a-re0 vty)# show mqchip 0 sched 0 q 1017
Q node 1017:
allocated           : true
parent node         : 254
guarantee prio      : 3 GL
excess prio         : 1 EH
10:54 AM
q1...就是...excess high
10:54 AM
---------------------------------------------------------------------------
Harry Song          guarantee ...就是小于...bandwidth...的流量，... excess...就是超过...bandwidth...但是小于...burst...的流量
10:56 AM
code PUNT name                     group proto         idx q# bwidth  burst
---- --------------------      --------- ------       ---- -- ------ ------
1 PUNT_TTL                        ttl aggregate    3c00  5   2000  10000  <<<<<<<< ...看是那种...punt traffic...，...punt...就是去...re...的流量
10:56 AM
...对应这里的...2000...和...10000
10:56 AM
Annabelle Wang ...稍等，我想一下
我的问题是，如果...nhq...的...q0...，有丢包那在这...8...个...q...里有没有优先级
Troubleshooting information is available online, including best practices for using Lync.
好的，
...怎么看出来“...q1...就是...excess high...”
11:01 AM
...哦哦
11:01 AM
...是引文...1017...是去
11:01 AM
Harry Song          excess prio         : 1 EH
11:01 AM
Annabelle Wang ...是...q1
11:01 AM
Harry Song          ...对
11:01 AM
************************************
# show mqchip 0 sched 0 q 1016  ...《《《《《《这边...sched ...后面的...0...， 就是...hnq...的...q0...？
11:03 AM
sorry
11:03 AM
...错了
11:03 AM
sched...后面的...0 ...到底是什么？
11:04 AM
Harry Song          ...我不知道。。
11:05 AM
Harry Song          ...问了下...jack
11:07 AM
NPC1(mx480-a-re0 vty)#  show mqchip 0 stream 1151
Input Stream 1151
-----------------
attached    : 1
enabled     : 1
pic slot    : 0
mac mode    : 0
port        : 0
conn        : 19
tclass      : 255 N/A
prio        : 1
weight      : 2047
Output Stream 1151
------------------
attached    : 1
enabled     : 1
pic slot    : 0
mac mode    : 0
wan if      : 0
port        : 0
conn        : 19
weight      : 1
sched       : 0 MQ0
l1 node     : 127
queues      : 1016..1023
qxif quota  : 0
qxif start  : 0
qxif size   : 0
11:07 AM
---------------------------------------------------------------------------
Harry Song          sched       : 0 MQ0
11:07 AM
...这里是...0
11:07 AM
Annabelle Wang ...哦哦哦，在这看得
11:08 AM
Harry Song          ...所以如果对比下别的...stream...来看的话
11:08 AM
11:11 AM
Harry Song          sched 0 ...都是...wan...方向的
11:11 AM
Harry Song          sched 1 ...都是...fabric...方向
11:11 AM
Annabelle Wang Output Stream 1151...这个...host...的也是...sched 0
11:11 AM
Harry Song          ...可以看到这些...q...的当前速率
11:12 AM
...往...re...送应该也算...wan
11:12 AM
************************************
11:13 AM
...其实我想知道...bgp...被...drop...了多少包，能用什么命令知道吗？
Harry Song          ...得去他经过的所有...filter ...或... queue...上去找
************************************
< BGP Flap troubleshooting >
1.If only one session flap <<<< check peer
if multiple session flap <<<<<< check the router
2. ...看是否是...bfd...引起的
3....看...pfe stream 1151...是否有...drop, ...也会显示在...hardware input drop...里，看是否有涨....
4. 1151...有...8...个...output queue...到...RE...，...show mqchip 0 dstat stats 0 1016-1023 ...看是否有...drop
5....查...filter...里的...policer
6. ...看是否是沿路...mtu...的问题造成的
==============================
bgp_hold_timeout:4436: NOTIFICATION sent to 20.6.198.3 (External AS 65500): code 4 (Hold Timer Expired Error), Reason: holdtime expired for 20.6.198.3 (External
AS 65500), socket buffer sndcc: 0 rcvcc: 114 TCP state: 4, snd_una: 1524441726 snd_nxt: 1524441726 snd_wnd: 22592 rcv_nxt: 2120450819 rcv_adv: 2120516240, hold timer out 90s, hold timer remain 0s
Please note that all the hold time expiry message in this NSR switchover is because of the same reason"NOTIFICATION sent to 20.6.198.*" ...."sndcc: 0 rcvcc: 114". The expiring BGP session has 114 bytes of data in the receive buffer and is still not able to read it resulting in hold time expiry. BGP team should investigate more into this issue.
===========================================
2016-0520-0099  ipv6 learn route from ebgp peer, nh not peer address
默认行为：
A-ibgp--B-ebgp--C
B...从...A...学到的路由传给...c...的时候会把...nh...自动改为自己
**...从...ibgp...邻居学到的路由不会传给...ibgp...邻居，除非是全互联，这是要加...next hop self...把...nh...改为自己
==============================================
Community
root# set policy-options policy-statement 1 term 1 then community ?
Possible completions:
<community_name>     Name to identify a BGP community
+                    Add BGP communities to the route
-                    Remove BGP communities from the route
=                    Set the BGP communities in the route
add                  Add BGP communities to the route
delete               Remove BGP communities from the route
set                  Set the BGP communities in the route
Community
<unexpectedly has MD5 digest>
Aug 12 03:45:20   /kernel: tcp_auth_ok: Packet from 118.143.96.75:36222 unexpectedly has MD5 digest
可能有两种原因造成：
1.peer...有一端没有配...MD5
2.t...opology:  R1------------R2
When R1 doesn't configure R2 as its BGP neighbor, but R2 configures R1 as its BGP neighbor with md5 authentication configured,
after receiving BGP related TCP packets from R2, R1 will generate following logs to indicate an error
2016-0812-0035...  ...KB31143
======================================
advertise-inactive  2017-0215-1045
https://www.juniper.net/documentation/en_US/junos/topics/example/bgp-advertise-inactive.html
In Junos OS, BGP advertises BGP routes that are installed or active, which are routes selected as the best based on the BGP path selection rules. The advertise-inactive statement allows nonactive BGP routes to be advertised to other peers.
Note: If the routing table has two BGP routes where one is active and the other is inactive, the advertise-inactive statement does not advertise the inactive BGP prefix. This statement does not advertise an inactive BGP route in the presence of another active BGP route. However, if the active route is a static route, the advertise-inactive statement advertises the inactive BGP route.
============================================
RFC3345 med+RR/confederation  may cause BGP flap
https://tools.ietf.org/html/rfc3345
2017-0215-1107
==============================================
解决...BGP...链路
，如果有链路...mtu...小，那么上游来的打包就可能被分片。分片可能造成重组乱序。导致...bgp down...。有...2...钟解决方案：
1. path-mtu-discovery
2017-0216-1014
https://www.juniper.net/documentation/en_US/junos/topics/reference/configuration-statement/path-mtu-discovery-edit-system.html
https://kb.juniper.net/InfoCenter/index?page=content&id=KB7049
如果链路上下游某台设备的...interface MTU...比较小，那么他收到的大包就会被分片。有些包被分片可能造成乱序。所以应用这个...knob...，会定期发...mtu discovery...包，来试探链路...mtu...大小。如果没有...ICMP unreachable...的回包就说明...MTU ok...。如果有回包，包里面可能带有...mtu ...大小，上游设备会调整发包...mtu...。
Normally hosts send packets whose size is based on the MTU of the interface that the host is communicating on, for example 1,518 bytes for Ethernet. However, if the hosts are communicating over a non-local network such as the Internet, it is possible that there may be a link in-between whose MTU is less than 1518 bytes. In this case, the router connected to this link would fragment the packet to several smaller packets before forwarding the data over that link.
To avoid this packet fragmentation, the initiating host sends a regular packet whose size is based on its interface MTU, with the Don't Fragment (DF) flag set. This tells any intermediate router that it must not fragment this packet. If a router receiving this packet must fragment it in order to transmit it over a low MTU link, it drops the packet and sends an ICMP Destination Unreachable message to the originator. This warning message may also contain information specifying the MTU that the host must use in order for the packet to be forwarded over the low MTU link. The host now reduces its Path MTU estimate to the value specified in this warning; or, if no value was given, the host can iteratively try smaller MTU sizes. The host then resends the packet.
The packet may get through this time, or there may be an even smaller MTU link further downstream, in which case the router connected to it will send another warning, so the above process is repeated. Eventually, the host will find the smallest MTU in its path to the remote host, and use this value to avoid fragmentation.
Path MTUs can change over time, meaning that the packets may be re-routed over different links. Therefore the DF bit is always set, and the host will be notified if a new smaller MTU link is suddenly introduced into the path. Of course, the path MTU might increase as well - in this case the RFC allows for the host to periodically send a packet larger than the current path MTU  to see if it gets through.
y default, path MTU discovery on outgoing TCP connections is enabled. To disable path MTU discovery, include the no-path-mtu-discovery statement at the [edit system internet-options] hierarchy level:
[edit system internet-options]
no-path-mtu-discovery;
2.
2018-0131-0028
TCP-MSS
https://www.juniper.net/documentation/en_US/junos/topics/example/bgp-tcp-mss.html
If the router receives a TCP packet with the SYN bit and the MSS option set, and the MSS option specified in the packet is larger than the MSS value specified by the tcp-mss statement, the router replaces the MSS value in the packet with the lower value specified by the tcp-mss statement.
The assumption is that the TCP MSS value used by the sender to communicate with the BGP neighbor is the same as the TCP MSS value that the sender can accept from the BGP neighbor.
[willee@wftac-tools 2018-0131-0028]$ less MI32WR06_20180201.txt | grep "TCP (6), length: 15[0-9][0-9]" | grep 62.115.156.150
15:36:31.689921  In IP (tos 0xc0, ttl   1, id 16893, offset 0, flags [none], proto: TCP (6), length: 1500) 62.115.156.150.bgp > 62.115.156.151.55764: . 1:1449(1448) ack 19 win 16384 <nop,nop,timestamp 1734072282  1244829874>: BGP, length: 1448 [|BGP]
> show system connections extensive
<<<...这个能看出来...TCP MSS ...大小
Inet0 BGP
#BGP Rule:
1. ibgp...会把从...ebgp...学到的路由传给...ibgp...邻居。（需要加...nhs...）
2.ebgp...会把学到的无论...ebgp...或...ibgp...路由传给...ebgp...邻居。（...default...会把...nh...改成自己）
3.ibgp...不把从...ibg...学到的路由传给他的...ibgp...邻居。（所以需要...ibgp full-mesh...或...rr/confederation...来解决互联问题）《《《《这条是为了...ibgp...在...AS...内防环，因为它不像...ebgp...可以通过...AS...号来放环。
#Attribute
--Origin...：...I-igp E-egp ...？...-incomplete...。... Junos by default use IGP....这个是...routes...如何被...inject...进...bgp...的。
--Local preference: define way out of the AS. ...唯一一个大优先...attribute...。...Ibgp only...。...Default value...：...100
--MED: control come into the AS. ...用于...AS...间的，小优先。...Ebgp only...。
bgp...选路... ...Route Selection Process
1. The Next Hop attribute value for each route must be reachable in the local routing table;... ...otherwise, the local router discards the route.
2. The router selects the route with the highest Local Preference attribute value.(ibgp)
3. The router selects the route with the shortest AS Path length.(ebgp)
4. The router selects the route with the smallest Origin attribute value.
5. The router selects the route with the smallest Multiple Exit Discriminator attribute value.This step is executed, by default, only for routes from the same neighboring AS.
6. The router selects routes learned from an EBGP peer over routes learned from an IBGP... ...peer. If the remaining routes are all EBGP-learned routes, the router skips to step 9.
7. The router selects the route with the smallest IGP metric to the advertised BGP Next Hop.... ...《《只有...ibgp compare...这个
8. If Route Reflection is used for IBGP peering, the router selects the route with the shortest... ...Cluster-List length.
9. The router selects the route from the peer with the smallest numerical Router ID.
10. The router selects the route from the peer with the smallest numerical Peer Address.
#AS Number...分两类：
16bit - 2Byte type...：...scope between ...色特... for private...。这个范围可以被用作...sub-AS (confederation AS) Others are for public...。
32bit - 4bytes type...：又分两种
#AS Number...分两类：
16bit - 2Byte type...：...scope between ...色特... for private...。这个范围可以被用作...sub-AS (confederation AS) Others are for public...。
32bit - 4bytes type...：又分两种
