# EVPN-Vxlan

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

EVPN-Vxlan
Underlay:...为...overlay...提供...ip reachability...，...spine-leaf...结构
Overlay...：...control...层面...evpn...，...data...层面...vxlan(vtep tunnel)
Junos 22.2R3 release is recommended for EVPN-VXLAN. Junos 22.2R1 and 22.2R2 are not recommended for EVPN-VXLAN for specific QFX5k, EX4650 and EX4400 products.
https://juniper.lightning.force.com/lightning/articles/Knowledge/Junos-22-2R3-release-is-recommended-for-EVPN-VXLAN-Junos-22-2R1-and-22-2R2-are-not-recommended-for-EVPN-VXLAN-for-specific-QFX5k-EX4650-and-EX4400-products
# Route Type #
Type 1
1.1non-DF...置位...split-horizon...，...DF...如果收到包有这置位，就不发给...local CE...，这样防环
1.2: multihome PE...告诉...remote PE...有多个...nh
Type 4...：相同...ESI...的两个...PE...之间互相发现，选举...DF
Type 2
2.1...：发...mac address...给...remote PE
2.2...：发...mac+ip binding...出去
2.3: mac-withdraw, 撤销路由
Type 3...：...bum traffic...，vxlan... vtep之间的通告，建立L2 VNI...，... leaf--leaf vxlan建立
Type 5...：ip... prefix...的通告，vxlan... vtep之间的通告，...建立...L3 VNI...，leaf...--spine vxlan建立
# 常用命令 #
看一个mac/ip是从哪台PE学来的 ：
# run show route table VBASE-21.evpn.0 evpn-mac-address cc:e1:7f:0b:bf:c0
2:101.234.3.134:21::21::cc:e1:7f:0b:bf:c0/304 MAC/IP
*[BGP/170] 00:02:13, localpref 100, from 101.234.3.134
AS path: I, validation-state: unverified
> to 101.234.14.135 via xe-1/0/0.0
2:101.234.3.134:21::21::cc:e1:7f:0b:bf:c0::192.168.1.11/304 MAC/IP
*[BGP/170] 00:00:38, localpref 100, from 101.234.3.134
AS path: I, validation-state: unverified
> to 101.234.14.135 via xe-1/0/0.0
***第二个比第一个多了192.168.1.11，这个地址是source CE的接口地址
看local设备的EVPN MAC表：
annaw@lab-mx240-3d-02-re0# run show evpn mac-table extensive cc:e1:7f:0b:bf:c0
MAC address: cc:e1:7f:0b:bf:c0
Routing instance: VBASE-21
Bridging domain: __VBASE-21__, VLAN : 21
Learning interface: ae10.21      <<<<
Base learning interface: ae10.21
Layer 2 flags: in_hash,in_ifd,in_ifl,in_vlan,in_rtt,kernel,in_ifbd,advt_to_remote,evpn_ri
Epoch: 1                            Sequence number: 1
Learning mask: 0x00000002
查看...local mac learning...：
regress@PE2# run show bridge mac-table vlan-id 100
MAC flags       (S -static MAC, D -dynamic MAC, L -locally learned, C -Control MAC
O -OVSDB MAC, SE -Statistics enabled, NM -Non configured MAC, R -Remote PE MAC, P -Pinned MAC)
Routing instance : evpn
Bridging domain : vlan-100, VLAN : 100
MAC                 MAC      Logical                Active
address             flags    interface              source
2c:6b:f5:41:3c:c0   DL       ae0.100        ...<<<<...  from local interface
regress@PE2# run show evpn database l2-domain-id 100 instance evpn
Instance: evpn
VLAN  DomainId  MAC address        Active source                  Timestamp        IP address
100        00:00:5e:00:01:01  05:00:00:fd:e9:00:00:00:64:00  Sep 22 20:07:44  100.100.100.254
100        2c:6b:f5:41:3c:c0  00:11:11:11:11:11:11:11:11:11  Sep 22 23:32:08  100.100.100.1 <<<
100        2c:6b:f5:48:5a:f0  irb.100                        Sep 22 20:07:44  100.100.100.251
查看...local...是否...advertise MAC/IP route to remote Pes:
regress@PE1# run show route table evpn.evpn.0 match-prefix *2c:6b:f5:f5*    <<< ...一定要是...*...号
evpn.evpn.0: 27 destinations, 27 routes (27 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both
2:1.1.1.1:4001::100::2c:6b:f5:f5:ff:f0/304 MAC/IP
*[EVPN/170] 00:19:09
Indirect
2:1.1.1.1:4001::200::2
c:6b:f5:f5:ff:f0/304 MAC/IP
*[EVPN/170] 00:19:09
Indirect
regress@PE1# run show route advertising-protocol bgp 2.2.2.2 table evpn.evpn.0 match-prefix *2c:6b:f5:f5*
evpn.evpn.0: 27 destinations, 27 routes (27 active, 0 holddown, 0 hidden)
Prefix                  Nexthop              MED     Lclpref    AS path
2:1.1.1.1:4001::100::2c:6b:f5:f5:ff:f0/304 MAC/IP
*                         Self                         100        I
2:1.1.1.1:4001::200::2c:6b:f5:f5:ff:f0/304 MAC/IP
*                         Self                         100        I
2:1.1.1.1:4001::100::2c:6b:f5:f5:ff:f0::100.100.100.251/304 MAC/IP
*                         Self                         100        I
2:1.1.1.1:4001::200::2c:6b:f5:f5:ff:f0::200.200.200.251/304 MAC/IP
*                         Self                         100        I
# Commands to check route flow #
Refer to training: ...EVPN VXLAN FLOW AND TROUBLESHOOTING
<<<<
HW:
# set dcbcm bcmshell "l2 show"
# show shim virtual bridge-domain
# show shim virtual vport
L2ALM: L2
>start shell
%vty fpc0
# show l2 manager mac-table <<< check ifl & mac are correctly bundled
# show l2 manager mac-table mac-address
# show l2 manager mac-ip bd 3 <<< check if mac & ip correctly learned
L2ALD: L3
> show ethernet-switching table 4c:96:14:e6:37:c1 <<< check L2 learning
> show ethernet-switching mac-ip-table 4c:96:14:e6:37:c1 <<< check L3 learning
RPD:
> show evpn database mac-address xxxxxx extensive
> show evpn database l2-domain-id 111 <<< vni id
> show ethernet-switching mac-ip-table kernel
> show route advertising-protocol bgp 10.230.48.1 evpn-mac-address 4c:96:14:e6:37:c1
PEER RPD:
> show route receiving-protocol bgp 10.230.48.7 evpn-mac-address 4c:96:14:e6:37:c1
> show evpn database mac-address 4c:96:14:e6:37:c1 extensive
PEER L2ALD:
> show ethernet-switching table 4c:96:14:e6:37:c1
> show ethernet-switching vxlan-tunnel-end-point esi esi-identifier 00:00:05:06:ae:0b:00:00:00:00
PEER KERNEL:
> show arp no-resolve
> show route forwarding-table destination 10.60.10.10
> show route 10.60.10.10
PEER L2ALM/HW:
# show l2 manager mac-table mac-address
# show bridge-domain
# show l2 manager mac-ip bd 3
# show route ip prefix 10.60.10.10
# show nhdb id 1944 recursive
# show shim jnh vxlan-vlan-gl2d-table
# show shim bridge fdb table entry 4c:96:14:e6:37:c1  262147
<<<<
Type 1:
AD (auto discover) route used in multihome senario.
a. AD per ESI:used for fast convergence and for preventing the looping of BUM traffic. Split-horizon
b. AD per EVI:used for aliasing, or load balancing
-...如何看...type 1 route...：
regress@pe11# run show route table EVPN-1.evpn.0
EVPN-1.evpn.0: 11 destinations, 11 routes (11 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both
1:12.12.12.12:0::111111111111111111::FFFF:FFFF/192 AD/ESI
*[BGP/170] 1d 21:38:54, localpref 100, from 1.1.1.1
AS path: I, validation-state: unverified
>  to 10.11.12.12 via ae1.0, label-switched-path 11-12
1:12.12.12.12:1::111111111111111111::0/192 AD/EVI
*[BGP/170] 1d 21:38:54, localpref 100, from 1.1.1.1
AS path: I, validation-state: unverified
>  to 10.11.12.12 via ae1.0, label-switched-path 11-12
Type 2:
首先它是...advertise mac address...，然后...bind mac+ip...并...advertise...出去
MAC address shall be sent before MAC+IP. MAC withdraw will cause MAC+IP withdraw.
regress@pe11# run show route table EVPN-1.evpn.0 evpn-mac-address 2c:6b:f5:e3:b1:c0
EVPN-1.evpn.0: 15 destinations, 15 routes (15 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both
2:21.21.21.21:1::100::2c:6b:f5:e3:b1:c0/304 MAC/IP
*[BGP/170] 01:41:46, localpref 100, from 1.1.1.1
AS path: I, validation-state: unverified
>  to 10.11.1.1 via ge-0/0/0.0, label-switched-path 11-21
2:21.21.21.21:1::100::2c:6b:f5:e3:b1:c0::100.1.1.13/304 MAC/IP
*[BGP/170] 01:41:46, localpref 100, from 1.1.1.1
AS path: I, validation-state: unverified
>  to 10.11.1.1 via ge-0/0/0.0, label-switched-path 11-21
Type 3:
For forwarding BUM traffic, when a PE receives a BUM packet from a CE device, it makes a copy of the packet corresponding to each of the remote PEs. 用来建立vxlan隧道
regress@pe11# run show route table EVPN-1.evpn.0 | find "^3"
3:11.11.11.11:1::100::11.11.11.11/248 IM
*[EVPN/170] 2d 00:18:21
Indirect
3:12.12.12.12:1::100::12.12.12.12/248 IM
*[BGP/170] 1d 22:31:56, localpref 100, from 1.1.1.1
AS path: I, validation-state: unverified
>  to 10.11.12.12 via ae1.0, label-switched-path 11-12
3:21.21.21.21:1::100::21.21.21.21/248 IM
*[BGP/170] 1d 22:31:56, localpref 100, from 1.1.1.1
AS path: I, validation-state: unverified
>  to 10.11.1.1 via ge-0/0/0.0, label-switched-path 11-21
3:22.22.22.22:1::100::22.22.22.22/248 IM
*[BGP/170] 1d 22:31:56, localpref 100, from 1.1.1.1
AS path: I, validation-state: unverified
>  to 10.11.1.1 via ge-0/0/0.0, label-switched-path 11-22
Type 4:
ES(ethernet segment) route used to discover each other PEs in the same multihome. PEs then select DF to forward BUM traffic.
只有...RR...和...multi-home PEs...上能看到...type4
annaw@lab-mx204-01# run show route table bgp.evpn.0 | find ^4
Dec 01 11:22:21
4:101.234.3.131:0::020000010099000001:101.234.3.131/296 ES
*[EVPN/170] 00:40:30
Indirect
4:101.234.3.131:0::020000010099000002:101.234.3.131/296 ES
*[EVPN/170] 01:53:27
Indirect
4:101.234.3.134:0::020000010099000001:101.234.3.134/296 ES
*[BGP/170] 01:51:39, localpref 100, from 101.234.3.130
AS path: I, validation-state: unverified
>  to 101.234.14.138 via xe-0/1/3.0, Push 401014
4:101.234.3.134:0::020000010099000002:101.234.3.134/296 ES
*[BGP/170] 02:05:32, localpref 100, from 101.234.3.130
AS path: I, validation-state: unverified
>  to 101.234.14.138 via xe-0/1/3.0, Push 401014
annaw@lab-mx204-01# run show route table __default_evpn__.evpn.0
<<< ...这两个命令都能看到
# DF #
The DF is... ...responsible for transmitting broadcast, unknown unicast, and multicast... ...(BUM) traffic from the core to the CE. The non-DF, or Backup... ...Forwarder, PE drops BUM traffic received from the core destined to... ...the CE.
DF value bigger wins.
NDF:
annaw@lab-mx240-3d-02-re0# run show interfaces ae10.21 extensive
Protocol bridge, MTU: 9200, Generation: 761, Route table: 11, Mesh Group: __all_ces__, EVPN multi-homed status: Blocking BUM Traffic to ESI, EVPN multi-homed ESI Split Horizon Label: 808,
Next-hop: 611, vpls-status: up
annaw@lab-mx204-01# run show interfaces ae10.21 extensive
DF:
EVPN multi-homed status: Forwarding, EVPN multi-homed ESI Split Horizon Label: 24, Next-hop: 1135, vpls-status: up
2022-0209-414115... DF elcection method...：
Each PE node builds an ordered list of the IP addresses of all the PE nodes connected to the Ethernet segment (including itself), in increasing numeric order. Every PE router is then given an ordinal indicating its position in the ordered list, starting with 0 as the ordinal for the PE with the numerically lowest IP address. So, each PE will have a ordinal value, starting from 0.
Then, the formula (V mod N), where V is the VLAN ID and N is the number of multi-homed PE nodes. The result i is used to determine which PE in the list is the DF. If there are multiple VLANs, then the lowest value is used.
When (V mod N) = i, then if the PE with ordinal value is i, then this PE is the DF.
Juniper DF election MOD formula is just as per the RFC 7432.
And, from my reading, V is Vlan ID ( the smallest Vlan id is used if there is multiple Vlans)
The juniper doc: https://www.juniper.net/documentation/us/en/software/junos/evpn-vxlan/topics/concept/evpn-bgp-multihoming-overview.html#evpn-multihoming-overview__d3733e861 for more detail.
And the RFC 7432: https://datatracker.ietf.org/doc/html/rfc7432#section-8.5
# ...Split Horizon ...#
The MPLS Split Horizon label, also called the ESI MPLS label, is used to prevent looping of multi-destination traffic amongst multi-homed PE peers, also known as Split Horizon Filtering. In an all-active multi-homing topology, when a non-DF PE forwards a BUM packet to its peer DF PE, it first pushes this received label onto the packet. Then it pushes the Inclusive Multicast label(see the Inclusive Multicast section below) followed by the transport label to reach the loopback of the destination peer PE.
When the DF PE receives and inspects the MPLS labels in the packet, it recognizes the Split Horizon label it previously advertised and does not forward the packet back to the CE.
#Aliasing (per EVI) #
Allows a peer to signal reachability to an ES (Auto Discovery per EVI), even when it has not learned no MAC addresses from that segment, thereby allowing remote peers to load-balance traffic to all peers connected to an ES.
Test case: SN-1 and SN-2 are multihomed to the same ES. SN-1 advertises Type 2 route for Host MAC/IP (H1 = 00:00:1e:63:c8:7c/100.0.10.100 – VNI 5010) to SN-2, SN-3 and SN-4. Even though, SN-3 and SN-4 do not learn the MAC address for H1 from SN-2, traffic for this host on VNI 5010 is load balanced to both SN-1 and SN-2.
https://www.juniper.net/documentation/us/en/software/junos/evpn-vxlan/topics/concept/evpn-bgp-multihoming-overview.html#evpn-multihoming-overview__aa-evpn
就是type 2只由一个...PE...发出去，但...remote PE...发回来的流量能负载到两台multihome pe上。也就是说没有通告路由的那台...PE...也可以帮助分担...remote...来的流量。
Multi-home all active scenario:
Aliasing allows the other multi-homed peer PE, which may not have learned/advertised any MAC addresses, to also receive traffic from remote PEs destined to the common ES.
Multi-home single active scenario:
In single-active multi-homed mode this route is used to implement a... ...similar Backup-path feature. In this case, a remote PE sends traffic to
the multi-homed PE that is the DF and inst
# MAC Mass Withdraw (per ESI) #
When a peer loses connectivity to an ES, it withdraws its Type 1 route (Auto Discovery per ESI) which leads to all remote peers immediately purging from the FIB, all MAC addresses learned from the failed peer on this ESI, instead of waiting for all MAC address routes to be withdrawn thus enabling fast convergence.
Test case: SN-2 is advertising 4 host MAC addresses to SN-4 and SN-3(not shown). SN-4 receives Type 2 host routes and Type 1 routes from SN-2. SN-2 then loses connectivity to its ES. SN-2 withdraws its Type 1 route and SN-4 purges from the forwarding-table, all installed host MAC routes from SN-2.
# MAC Move #
When a remote PE router that is configured with matching route targets, or EVPN instances, receives this advertisement, it has a view of the multi-homing connectivity of the advertising PEs. One benefit here is for fast convergence, also known as MAC Mass Withdraw. In the event a multi-homed PE loses its local link towards the CE, it withdraws this route. This signals to the remote PEs to either invalidate or adjust the next hop of all MAC addresses that correspond to the advertising PE’s failed Ethernet Segment. This is more efficient than requiring the PE to withdraw each individual MAC address in which case the convergence time would be dependent on the scale, or total number, of MAC addresses.
https://www.juniper.net/documentation/us/en/software/junos/vpn-l2/topics/topic-map/example-configuring-loop-prevention-in-vpls-network-due-to-mac-moves.html
when a previously learned media access control (MAC) address appears on a different physical interface, for example, local interfaces (Gigabit Ethernet interfaces) or label switched Interfaces (LSIs), or within a different unit of the same physical interface and if this behavior occurs frequently, then it is considered a MAC move.
You can configure the router to report a MAC address move based on the following parameters:
Number of times a MAC address move occurs
Specified period of time over which the MAC address move occurs
URL  :  https://www.juniper.net/documentation/en_US/junos/topics/task/configuration/configuring-mac-mobility-settings.html
EVPN  MAC move detection feature:
This feature helps in cases of MAC  moves, to see where the mac is being learned if a change happens.
Starting from JUNOS 18.1R2,  for EVPN/MPLS through PR1340723 , IRB interface shall stay up even when local bridge interface is down.
In some cases we still saw IRB going down, got fixed through PR#1436207
# TS #
———————— Basic Checks ———————
show bgp summary
show route advertising protocol bgp <Underlay BGP>
show route receive-protocol bgp <neighbor> extensive
show route advertising-protocol bgp <neighbor> extensive
show vlan
show ethernet-switching table
show ethernet-switching vxlan-tunnel-end-point remote mac-table
show ethernet-switching vxlan-tunnel-end-point remote
show ethernet-switching vxlan-tunnel-end-point source
show interface vtep
show arp no-resolve expiration-timeshow ethernet-switching evpn arp-tableshow ethernet-switching mac-ip-table
show evpn database
show evpn database extensive | no-more
show evpn database mac-address <mac address of hosts> extensive
show ethernet-switching flood  | no-more
show ethernet-switching flood extensive  | no-more
show ethernet-switching flood vlan-name <Vlan-name> extensive | no-more
show route match-prefix <type 2 route> extensive
show route table default-switch.evpn.0 evpn-esi-value <esi> extensive
show route table __default_evpn__.evpn.0 evpn-esi-value <esi> extensive
show evpn instance esi <esi> extensive
show ethernet-switching flood vlan-name <vlan name> extensive
show route forwarding-table destination <mac>			>>> get Nh id
% nhinfo -Ri <nh id>
% cprod -A fpc0 -c "show nhdb id <nh id> extensive"
——— Forwarding issue across QFX 5k —————
Note : These cover preliminary checks and we would need extensive output post analysis
% cprod -A fpc0 -c 'set dcbc bc "l2 show"'     				>>> get gport for mac
% cprod -A fpc0 -c "show shim virtual vtep"
% cprod -A fpc0 -c "show shim virtual vport hw-id <gport>”
% cprod -A fpc0 -c "show shim virtual bridge-domain"		>>> get BD for vlan
% cprod -A fpc0 -c "show shim virtual bridge-domain <“BD id>
% cprod -A fpc0 -c "show dcbcm ifd all"
show evpn mac-ip-table kernel
show evpn mac-ip-table kernel extensive
% cprod -A fpc0 -c "show shim virtual bridge-domain"		>>> get BD for vlan
% cprod -A fpc0 -c "show l2 manager mac-ip bd <BD>”
% cprod -A fpc0 -c "show shim virtual bridge-domain <BD>"		>>> get umc-idx for vlan
% cprod -A fpc0 -c 'set dcb bc "d chg ipmc <umc-idx>”’		>>> get L3_BITMAP
% cprod -A fpc0 -c 'set dcb bc "pbmp <L3_BITMAP>“‘
% cprod -A fpc0 -c "show dcbcm ifd all"
————Forwarding issue across QFX10k ————
% cprod -A fpc0 -c "show bridge-domain"				>>> get BD for vlan
% cprod -A fpc0 -c "show shim bridge gl2d-to-bd"			>>> get Gl2D id for specific BD
% cprod -A fpc0 -c "show shim jnh vxlan-vlan-gl2d-table"	>>> get Gl2D id for specific vlan
% cprod -A fpc0 -c "show shim bridge fdb table entry <mac> <GL2D>”
% cprod -A fpc0 -c "show shim jnh vtep-nh-table"
% cprod -A fpc0 -c "show shim jfdb ht by_key gl2d_dmac 1 <Gl2D> <mac>”		>>> get next-addr
% cprod -A fpc0 -c "show shim jnh descriptor dev 0 nh-mm-index <vext-addr> 1 “	>>>	get flabel
% cprod -A fpc0 -c "show jencap show_flabel 0 <flabel> regular”
% cprod -A fpc0 -c "show shim jnh vni-table”
% cprod -A fpc0 -c "show shim jfdb ht by_key gl2d 1 0 <Gl2D>”					>>> get nhid
% cprod -A fpc0 -c "show nhdb id <nhid> recursive”
Tracing L2 daemon
set protocols l2-learning traceoptions level all
set protocols l2-learning traceoptions l2-trce
set protocols l2-learning traceoptions size 10m
set protocols l2-learning traceoptions files 5
set protocols l2-learning traceoptions flag all
Tracing Kernel
root@Leaf1:RE:0% rtsockmon –t
Tracing EVPN
set protocols evpn traceoptions file evpn-trce
set protocols evpn traceoptions file size 10m
set protocols evpn traceoptions file files 5
set protocols evpn traceoptions flag all
Tracing BGP
set protocols bgp traceoptions file bgp-trace
set protocols bgp traceoptions file size 10m
set protocols bgp traceoptions file files 5
set protocols bgp traceoptions flag all
vlan=28673 converted to hex HW-id (0x7001)
FPC0(jtac-qfx5110-48s-4c-r2601 vty)# show shim virtual bridge-domain
idx(0x7001): bd-id(3) sw-id(1) hw-id(0x7001) vnid(10) vlan(10) mc-idx(2)
idx(0x7002): bd-id(4) sw-id(2) hw-id(0x7002) vnid(20) vlan(20) mc-idx(3)
GPORT = VPORT
GPORT is the virtual port in BCM
EVPN-Vxlan
Underlay:...为...overlay...提供...ip reachability...，...spine-leaf...结构
Overlay...：...control...层面...evpn...，...data...层面...vxlan(vtep tunnel)
Junos 22.2R3 release is recommended for EVPN-VXLAN. Junos 22.2R1 and 22.2R2 are not recommended for EVPN-VXLAN for specific QFX5k, EX4650 and EX4400 products.
https://juniper.lightning.force.com/lightning/articles/Knowledge/Junos-22-2R3-release-is-recommended-for-EVPN-VXLAN-Junos-22-2R1-and-22-2R2-are-not-recommended-for-EVPN-VXLAN-for-specific-QFX5k-EX4650-and-EX4400-products
# Route Type #
Type 1
1.1non-DF...置位...split-horizon...，...DF...如果收到包有这置位，就不发给...local CE...，这样防环
1.2: multihome PE...告诉...remote PE...有多个...nh
Type 4...：相同...ESI...的两个...PE...之间互相发现，选举...DF
Type 2
2.1...：发...mac address...给...remote PE
2.2...：发...mac+ip binding...出去
2.3: mac-withdraw, 撤销路由
Type 3...：...bum traffic...，vxlan... vtep之间的通告，建立L2 VNI...，... leaf--leaf vxlan建立
Type 5...：ip... prefix...的通告，vxlan... vtep之间的通告，...建立...L3 VNI...，leaf...--spine vxlan建立
# 常用命令 #
看一个mac/ip是从哪台PE学来的 ：
# run show route table VBASE-21.evpn.0 evpn-mac-address cc:e1:7f:0b:bf:c0
2:101.234.3.134:21::21::cc:e1:7f:0b:bf:c0/304 MAC/IP
*[BGP/170] 00:02:13, localpref 100, from 101.234.3.134
AS path: I, validation-state: unverified
> to 101.234.14.135 via xe-1/0/0.0
2:101.234.3.134:21::21::cc:e1:7f:0b:bf:c0::192.168.1.11/304 MAC/IP
*[BGP/170] 00:00:38, localpref 100, from 101.234.3.134
AS path: I, validation-state: unverified
> to 101.234.14.135 via xe-1/0/0.0
***第二个比第一个多了192.168.1.11，这个地址是source CE的接口地址
看local设备的EVPN MAC表：
annaw@lab-mx240-3d-02-re0# run show evpn mac-table extensive cc:e1:7f:0b:bf:c0
MAC address: cc:e1:7f:0b:bf:c0
Routing instance: VBASE-21
Bridging domain: __VBASE-21__, VLAN : 21
Learning interface: ae10.21      <<<<
Base learning interface: ae10.21
Layer 2 flags: in_hash,in_ifd,in_ifl,in_vlan,in_rtt,kernel,in_ifbd,advt_to_remote,evpn_ri
Epoch: 1                            Sequence number: 1
Learning mask: 0x00000002
查看...local mac learning...：
regress@PE2# run show bridge mac-table vlan-id 100
MAC flags       (S -static MAC, D -dynamic MAC, L -locally learned, C -Control MAC
O -OVSDB MAC, SE -Statistics enabled, NM -Non configured MAC, R -Remote PE MAC, P -Pinned MAC)
Routing instance : evpn
Bridging domain : vlan-100, VLAN : 100
MAC                 MAC      Logical                Active
address             flags    interface              source
2c:6b:f5:41:3c:c0   DL       ae0.100        ...<<<<...  from local interface
regress@PE2# run show evpn database l2-domain-id 100 instance evpn
Instance: evpn
VLAN  DomainId  MAC address        Active source                  Timestamp        IP address
100        00:00:5e:00:01:01  05:00:00:fd:e9:00:00:00:64:00  Sep 22 20:07:44  100.100.100.254
100        2c:6b:f5:41:3c:c0  00:11:11:11:11:11:11:11:11:11  Sep 22 23:32:08  100.100.100.1 <<<
100        2c:6b:f5:48:5a:f0  irb.100                        Sep 22 20:07:44  100.100.100.251
查看...local...是否...advertise MAC/IP route to remote Pes:
regress@PE1# run show route table evpn.evpn.0 match-prefix *2c:6b:f5:f5*    <<< ...一定要是...*...号
evpn.evpn.0: 27 destinations, 27 routes (27 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both
2:1.1.1.1:4001::100::2c:6b:f5:f5:ff:f0/304 MAC/IP
*[EVPN/170] 00:19:09
Indirect
2:1.1.1.1:4001::200::2
c:6b:f5:f5:ff:f0/304 MAC/IP
*[EVPN/170] 00:19:09
Indirect
regress@PE1# run show route advertising-protocol bgp 2.2.2.2 table evpn.evpn.0 match-prefix *2c:6b:f5:f5*
evpn.evpn.0: 27 destinations, 27 routes (27 active, 0 holddown, 0 hidden)
Prefix                  Nexthop              MED     Lclpref    AS path
2:1.1.1.1:4001::100::2c:6b:f5:f5:ff:f0/304 MAC/IP
*                         Self                         100        I
2:1.1.1.1:4001::200::2c:6b:f5:f5:ff:f0/304 MAC/IP
*                         Self                         100        I
2:1.1.1.1:4001::100::2c:6b:f5:f5:ff:f0::100.100.100.251/304 MAC/IP
*                         Self                         100        I
2:1.1.1.1:4001::200::2c:6b:f5:f5:ff:f0::200.200.200.251/304 MAC/IP
*                         Self                         100        I
spine上查看arp是否resolve
> show arp no-resolve |match irb.12027
spine上查看
> show evpn database instance EVPN-C80319-1080319000 l2-domain-id 3190997
> show tunnel database
# Commands to check route flow #
Refer to training: ...EVPN VXLAN FLOW AND TROUBLESHOOTING
<<<<
HW:
# set dcbcm bcmshell "l2 show"
# show shim virtual bridge-domain
# show shim virtual vport
L2ALM: L2
>start shell
%vty fpc0
# show l2 manager mac-table <<< check ifl & mac are correctly bundled
# show l2 manager mac-table mac-address
# show l2 manager mac-ip bd 3 <<< check if mac & ip correctly learned
L2ALD: L3
> show ethernet-switching table 4c:96:14:e6:37:c1 <<< check L2 learning
> show ethernet-switching mac-ip-table 4c:96:14:e6:37:c1 <<< check L3 learning
RPD:
> show evpn database mac-address xxxxxx extensive
> show evpn database l2-domain-id 111 <<< vni id
> show ethernet-switching mac-ip-table kernel
> show route advertising-protocol bgp 10.230.48.1 evpn-mac-address 4c:96:14:e6:37:c1
PEER RPD:
> show route receiving-protocol bgp 10.230.48.7 evpn-mac-address 4c:96:14:e6:37:c1
> show evpn database mac-address 4c:96:14:e6:37:c1 extensive
PEER L2ALD:
> show ethernet-switching table 4c:96:14:e6:37:c1
> show ethernet-switching vxlan-tunnel-end-point esi esi-identifier 00:00:05:06:ae:0b:00:00:00:00
PEER KERNEL:
> show arp no-resolve
> show route forwarding-table destination 10.60.10.10
> show route 10.60.10.10
PEER L2ALM/HW:
# show l2 manager mac-table mac-address
# show bridge-domain
# show l2 manager mac-ip bd 3
# show route ip prefix 10.60.10.10
# show nhdb id 1944 recursive
# show shim jnh vxlan-vlan-gl2d-table
# show shim bridge fdb table entry 4c:96:14:e6:37:c1  262147
<<<<
Type 1:
AD (auto discover) route used in multihome senario.
a. AD per ESI:used for fast convergence and for preventing the looping of BUM traffic. Split-horizon
b. AD per EVI:used for aliasing, or load balancing
-...如何看...type 1 route...：
regress@pe11# run show route table EVPN-1.evpn.0
EVPN-1.evpn.0: 11 destinations, 11 routes (11 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both
1:12.12.12.12:0::111111111111111111::FFFF:FFFF/192 AD/ESI
*[BGP/170] 1d 21:38:54, localpref 100, from 1.1.1.1
AS path: I, validation-state: unverified
>  to 10.11.12.12 via ae1.0, label-switched-path 11-12
1:12.12.12.12:1::111111111111111111::0/192 AD/EVI
*[BGP/170] 1d 21:38:54, localpref 100, from 1.1.1.1
AS path: I, validation-state: unverified
>  to 10.11.12.12 via ae1.0, label-switched-path 11-12
Type 2:
首先它是...advertise mac address...，然后...bind mac+ip...并...advertise...出去
MAC address shall be sent before MAC+IP. MAC withdraw will cause MAC+IP withdraw.
regress@pe11# run show route table EVPN-1.evpn.0 evpn-mac-address 2c:6b:f5:e3:b1:c0
EVPN-1.evpn.0: 15 destinations, 15 routes (15 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both
2:21.21.21.21:1::100::2c:6b:f5:e3:b1:c0/304 MAC/IP
*[BGP/170] 01:41:46, localpref 100, from 1.1.1.1
AS path: I, validation-state: unverified
>  to 10.11.1.1 via ge-0/0/0.0, label-switched-path 11-21
2:21.21.21.21:1::100::2c:6b:f5:e3:b1:c0::100.1.1.13/304 MAC/IP
*[BGP/170] 01:41:46, localpref 100, from 1.1.1.1
AS path: I, validation-state: unverified
>  to 10.11.1.1 via ge-0/0/0.0, label-switched-path 11-21
Type 3:
For forwarding BUM traffic, when a PE receives a BUM packet from a CE device, it makes a copy of the packet corresponding to each of the remote PEs. 用来建立vxlan隧道
regress@pe11# run show route table EVPN-1.evpn.0 | find "^3"
3:11.11.11.11:1::100::11.11.11.11/248 IM
*[EVPN/170] 2d 00:18:21
Indirect
3:12.12.12.12:1::100::12.12.12.12/248 IM
*[BGP/170] 1d 22:31:56, localpref 100, from 1.1.1.1
AS path: I, validation-state: unverified
>  to 10.11.12.12 via ae1.0, label-switched-path 11-12
3:21.21.21.21:1::100::21.21.21.21/248 IM
*[BGP/170] 1d 22:31:56, localpref 100, from 1.1.1.1
AS path: I, validation-state: unverified
>  to 10.11.1.1 via ge-0/0/0.0, label-switched-path 11-21
3:22.22.22.22:1::100::22.22.22.22/248 IM
*[BGP/170] 1d 22:31:56, localpref 100, from 1.1.1.1
AS path: I, validation-state: unverified
>  to 10.11.1.1 via ge-0/0/0.0, label-switched-path 11-22
Type 4:
ES(ethernet segment) route used to discover each other PEs in the same multihome. PEs then select DF to forward BUM traffic.
只有...RR...和...multi-home PEs...上能看到...type4
annaw@lab-mx204-01# run show route table bgp.evpn.0 | find ^4
Dec 01 11:22:21
4:101.234.3.131:0::020000010099000001:101.234.3.131/296 ES
*[EVPN/170] 00:40:30
Indirect
4:101.234.3.131:0::020000010099000002:101.234.3.131/296 ES
*[EVPN/170] 01:53:27
Indirect
4:101.234.3.134:0::020000010099000001:101.234.3.134/296 ES
*[BGP/170] 01:51:39, localpref 100, from 101.234.3.130
AS path: I, validation-state: unverified
>  to 101.234.14.138 via xe-0/1/3.0, Push 401014
4:101.234.3.134:0::020000010099000002:101.234.3.134/296 ES
*[BGP/170] 02:05:32, localpref 100, from 101.234.3.130
AS path: I, validation-state: unverified
>  to 101.234.14.138 via xe-0/1/3.0, Push 401014
annaw@lab-mx204-01# run show route table __default_evpn__.evpn.0
<<< ...这两个命令都能看到
# DF #
The DF is... ...responsible for transmitting broadcast, unknown unicast, and multicast... ...(BUM) traffic from the core to the CE. The non-DF, or Backup... ...Forwarder, PE drops BUM traffic received from the core destined to... ...the CE.
DF value bigger wins.
NDF:
annaw@lab-mx240-3d-02-re0# run show interfaces ae10.21 extensive
Protocol bridge, MTU: 9200, Generation: 761, Route table: 11, Mesh Group: __all_ces__, EVPN multi-homed status: Blocking BUM Traffic to ESI, EVPN multi-homed ESI Split Horizon Label: 808,
Next-hop: 611, vpls-status: up
annaw@lab-mx204-01# run show interfaces ae10.21 extensive
DF:
EVPN multi-homed status: Forwarding, EVPN multi-homed ESI Split Horizon Label: 24, Next-hop: 1135, vpls-status: up
2022-0209-414115... DF elcection method...：
Each PE node builds an ordered list of the IP addresses of all the PE nodes connected to the Ethernet segment (including itself), in increasing numeric order. Every PE router is then given an ordinal indicating its position in the ordered list, starting with 0 as the ordinal for the PE with the numerically lowest IP address. So, each PE will have a ordinal value, starting from 0.
Then, the formula (V mod N), where V is the VLAN ID and N is the number of multi-homed PE nodes. The result i is used to determine which PE in the list is the DF. If there are multiple VLANs, then the lowest value is used.
When (V mod N) = i, then if the PE with ordinal value is i, then this PE is the DF.
Juniper DF election MOD formula is just as per the RFC 7432.
And, from my reading, V is Vlan ID ( the smallest Vlan id is used if there is multiple Vlans)
The juniper doc: https://www.juniper.net/documentation/us/en/software/junos/evpn-vxlan/topics/concept/evpn-bgp-multihoming-overview.html#evpn-multihoming-overview__d3733e861 for more detail.
And the RFC 7432: https://datatracker.ietf.org/doc/html/rfc7432#section-8.5
# ...Split Horizon ...#
The MPLS Split Horizon label, also called the ESI MPLS label, is used to prevent looping of multi-destination traffic amongst multi-homed PE peers, also known as Split Horizon Filtering. In an all-active multi-homing topology, when a non-DF PE forwards a BUM packet to its peer DF PE, it first pushes this received label onto the packet. Then it pushes the Inclusive Multicast label(see the Inclusive Multicast section below) followed by the transport label to reach the loopback of the destination peer PE.
When the DF PE receives and inspects the MPLS labels in the packet, it recognizes the Split Horizon label it previously advertised and does not forward the packet back to the CE.
#Aliasing (per EVI) #
Allows a peer to signal reachability to an ES (Auto Discovery per EVI), even when it has not learned no MAC addresses from that segment, thereby allowing remote peers to load-balance traffic to all peers connected to an ES.
Test case: SN-1 and SN-2 are multihomed to the same ES. SN-1 advertises Type 2 route for Host MAC/IP (H1 = 00:00:1e:63:c8:7c/100.0.10.100 – VNI 5010) to SN-2, SN-3 and SN-4. Even though, SN-3 and SN-4 do not learn the MAC address for H1 from SN-2, traffic for this host on VNI 5010 is load balanced to both SN-1 and SN-2.
https://www.juniper.net/documentation/us/en/software/junos/evpn-vxlan/topics/concept/evpn-bgp-multihoming-overview.html#evpn-multihoming-overview__aa-evpn
就是type 2只由一个...PE...发出去，但...remote PE...发回来的流量能负载到两台multihome pe上。也就是说没有通告路由的那台...PE...也可以帮助分担...remote...来的流量。
Multi-home all active scenario:
Aliasing allows the other multi-homed peer PE, which may not have learned/advertised any MAC addresses, to also receive traffic from remote PEs destined to the common ES.
Multi-home single active scenario:
In single-active multi-homed mode this route is used to implement a... ...similar Backup-path feature. In this case, a remote PE sends traffic to
the multi-homed PE that is the DF and inst
# MAC Mass Withdraw (per ESI) #
When a peer loses connectivity to an ES, it withdraws its Type 1 route (Auto Discovery per ESI) which leads to all remote peers immediately purging from the FIB, all MAC addresses learned from the failed peer on this ESI, instead of waiting for all MAC address routes to be withdrawn thus enabling fast convergence.
Test case: SN-2 is advertising 4 host MAC addresses to SN-4 and SN-3(not shown). SN-4 receives Type 2 host routes and Type 1 routes from SN-2. SN-2 then loses connectivity to its ES. SN-2 withdraws its Type 1 route and SN-4 purges from the forwarding-table, all installed host MAC routes from SN-2.
# MAC Move #
When a remote PE router that is configured with matching route targets, or EVPN instances, receives this advertisement, it has a view of the multi-homing connectivity of the advertising PEs. One benefit here is for fast convergence, also known as MAC Mass Withdraw. In the event a multi-homed PE loses its local link towards the CE, it withdraws this route. This signals to the remote PEs to either invalidate or adjust the next hop of all MAC addresses that correspond to the advertising PE’s failed Ethernet Segment. This is more efficient than requiring the PE to withdraw each individual MAC address in which case the convergence time would be dependent on the scale, or total number, of MAC addresses.
https://www.juniper.net/documentation/us/en/software/junos/vpn-l2/topics/topic-map/example-configuring-loop-prevention-in-vpls-network-due-to-mac-moves.html
when a previously learned media access control (MAC) address appears on a different physical interface, for example, local interfaces (Gigabit Ethernet interfaces) or label switched Interfaces (LSIs), or within a different unit of the same physical interface and if this behavior occurs frequently, then it is considered a MAC move.
You can configure the router to report a MAC address move based on the following parameters:
Number of times a MAC address move occurs
Specified period of time over which the MAC address move occurs
URL  :  https://www.juniper.net/documentation/en_US/junos/topics/task/configuration/configuring-mac-mobility-settings.html
EVPN  MAC move detection feature:
This feature helps in cases of MAC  moves, to see where the mac is being learned if a change happens.
Starting from JUNOS 18.1R2,  for EVPN/MPLS through PR1340723 , IRB interface shall stay up even when local bridge interface is down.
In some cases we still saw IRB going down, got fixed through PR#1436207
# TS #
———————— Basic Checks ———————
show bgp summary
show route advertising protocol bgp <Underlay BGP>
show route receive-protocol bgp <neighbor> extensive
show route advertising-protocol bgp <neighbor> extensive
show vlan
show ethernet-switching table
show ethernet-switching vxlan-tunnel-end-point remote mac-table
show ethernet-switching vxlan-tunnel-end-point remote
show ethernet-switching vxlan-tunnel-end-point source
show interface vtep
show arp no-resolve expiration-timeshow ethernet-switching evpn arp-tableshow ethernet-switching mac-ip-table
show evpn database
show evpn database extensive | no-more
show evpn database mac-address <mac address of hosts> extensive
show ethernet-switching flood  | no-more
show ethernet-switching flood extensive  | no-more
show ethernet-switching flood vlan-name <Vlan-name> extensive | no-more
show route match-prefix <type 2 route> extensive
show route table default-switch.evpn.0 evpn-esi-value <esi> extensive
show route table __default_evpn__.evpn.0 evpn-esi-value <esi> extensive
show evpn instance esi <esi> extensive
show ethernet-switching flood vlan-name <vlan name> extensive
show route forwarding-table destination <mac>			>>> get Nh id
% nhinfo -Ri <nh id>
% cprod -A fpc0 -c "show nhdb id <nh id> extensive"
——— Forwarding issue across QFX 5k —————
Note : These cover preliminary checks and we would need extensive output post analysis
% cprod -A fpc0 -c 'set dcbc bc "l2 show"'     				>>> get gport for mac
% cprod -A fpc0 -c "show shim virtual vtep"
% cprod -A fpc0 -c "show shim virtual vport hw-id <gport>”
% cprod -A fpc0 -c "show shim virtual bridge-domain"		>>> get BD for vlan
% cprod -A fpc0 -c "show shim virtual bridge-domain <“BD id>
% cprod -A fpc0 -c "show dcbcm ifd all"
show evpn mac-ip-table kernel
show evpn mac-ip-table kernel extensive
% cprod -A fpc0 -c "show shim virtual bridge-domain"		>>> get BD for vlan
% cprod -A fpc0 -c "show l2 manager mac-ip bd <BD>”
% cprod -A fpc0 -c "show shim virtual bridge-domain <BD>"		>>> get umc-idx for vlan
% cprod -A fpc0 -c 'set dcb bc "d chg ipmc <umc-idx>”’		>>> get L3_BITMAP
% cprod -A fpc0 -c 'set dcb bc "pbmp <L3_BITMAP>“‘
% cprod -A fpc0 -c "show dcbcm ifd all"
————Forwarding issue across QFX10k ————
% cprod -A fpc0 -c "show bridge-domain"				>>> get BD for vlan
% cprod -A fpc0 -c "show shim bridge gl2d-to-bd"			>>> get Gl2D id for specific BD
% cprod -A fpc0 -c "show shim jnh vxlan-vlan-gl2d-table"	>>> get Gl2D id for specific vlan
% cprod -A fpc0 -c "show shim bridge fdb table entry <mac> <GL2D>”
% cprod -A fpc0 -c "show shim jnh vtep-nh-table"
% cprod -A fpc0 -c "show shim jfdb ht by_key gl2d_dmac 1 <Gl2D> <mac>”		>>> get next-addr
% cprod -A fpc0 -c "show shim jnh descriptor dev 0 nh-mm-index <vext-addr> 1 “	>>>	get flabel
% cprod -A fpc0 -c "show jencap show_flabel 0 <flabel> regular”
% cprod -A fpc0 -c "show shim jnh vni-table”
% cprod -A fpc0 -c "show shim jfdb ht by_key gl2d 1 0 <Gl2D>”					>>> get nhid
% cprod -A fpc0 -c "show nhdb id <nhid> recursive”
Tracing L2 daemon
set protocols l2-learning traceoptions level all
set protocols l2-learning traceoptions l2-trce
set protocols l2-learning traceoptions size 10m
set protocols l2-learning traceoptions files 5
set protocols l2-learning traceoptions flag all
Tracing Kernel
root@Leaf1:RE:0% rtsockmon –t
Tracing EVPN
set protocols evpn traceoptions file evpn-trce
set protocols evpn traceoptions file size 10m
set protocols evpn traceoptions file files 5
set protocols evpn traceoptions flag all
Tracing BGP
set protocols bgp traceoptions file bgp-trace
set protocols bgp traceoptions file size 10m
set protocols bgp traceoptions file files 5
set protocols bgp traceoptions flag all
vlan=28673 converted to hex HW-id (0x7001)
FPC0(jtac-qfx5110-48s-4c-r2601 vty)# show shim virtual bridge-domain
idx(0x7001): bd-id(3) sw-id(1) hw-id(0x7001) vnid(10) vlan(10) mc-idx(2)
idx(0x7002): bd-id(4) sw-id(2) hw-id(0x7002) vnid(20) vlan(20) mc-idx(3)
GPORT = VPORT
GPORT is the virtual port in BCM
