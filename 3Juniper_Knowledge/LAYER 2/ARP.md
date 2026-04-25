# ARP

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

ARP
# Commands #
> show arp no-resolve | match ge-0/0/0 --> ping如果不通，先查arp
> clear arp interface ge-0/0/1
> monin traffic interface ge-0/0/1
<<<
15:27:12.438594  In arp who-has 203.125.147.93 tell 203.125.147.94
15:27:12.438631 Out arp reply 203.125.147.93 is-at cc:e1:7f:ab:68:01
15:27:14.620583  In arp who-has 203.125.147.93 tell 203.125.147.94
15:27:14.620621 Out arp reply 203.125.147.93 is-at cc:e1:7f:ab:68:01
15:27:16.639581  In arp who-has 203.125.147.93 tell 203.125.147.94
15:27:16.639633 Out arp reply 203.125.147.93 is-at cc:e1:7f:ab:68:01
15:27:17.711302  In arp who-has 203.125.147.93 tell 203.125.147.94
15:27:17.711352 Out arp reply 203.125.147.93 is-at cc:e1:7f:ab:68:01
像这种一直在询问同样的arp的output...，说明对端没有收到我们的arp... reply...，可能丢在我们设备里了
<<<
p2p interface直连...，arp表象不会自动更新，需要ping触发。非p...2p可以直接update arp表...。
# 原理 #
根据目标设备ip来解析出目标设备的mac地址。作用范围是同一个广播域/局域网，即同一组直接相连的交换机或者统一vlan。二层通讯只要有mac就行，三层需要arp表里有ip和mac的表项。
router的作用就是分割广播域，因为如果全网在一个广播域里，一次泛洪网络就会瘫痪，所以广播域的边界事路由器。广播报文可以在同一广播域里的所有交换机泛洪。不通广播域之间通过路由传输数据。
arp解析过程只需要两个包...，request... & response...：
ARP request 是广播报文...，交换机收到后会泛洪出去：目的mac是全F的包是广播包，这个包里的destination... mac是全0
ARP协议属于TCP/IP协议簇，工作与ipv4网络环境。理论上可以看成是2.5层 但是实际是2层工作的...。
按照OSI的标准,当数据向下传递时,每层会加上自己的信息,各层互不干扰.这样当网络层的IP包进入链路层时,链路层该如何加这个头部的目标信息呢?它要依靠ARP协议来完成.
ARP 老化时间...：
ARP entries in the ARP table are retried after they have expired and are deleted if there is no ARP response within the default timeout value of 20 minutes. The default timeout value can be changed to other values using the aging-timer statement, as explained below.
如果arp不能解析，一条路有就不能在转发表里acitve：
-----
show route forwarding-table | match hold
routes on forwarding table with type "hold", routes next hops not reachable, no arps for next hops
-----
V4...用...arp...，... v6...不用...arp...用另一种
> show system statistics arp >>>show it several times
> show policer __default_arp_policer__  >>>show it several times
> show system queues | match "input protocol|arpintrq"  >>>show it several times
# GARP #
应用场景：
1....检测是否网络中有...ip...地址冲突。当这个...garp...收到...reply...说明有其他...device...用了同样的...ip add
2....当发生...mac move...，发...garp...可以及时通知网络其他设备新的...mac
https://wiki.wireshark.org/Gratuitous_ARP
A gratuitous ARP request is an AddressResolutionProtocol request packet where the source and destination IP are both set to the IP of the machine issuing the packet and the destination MAC is the broadcast address ff:ff:ff:ff:ff:ff. Ordinarily, no reply packet will occur. A gratuitous ARP reply is a reply to which no request has been made.
<<<...这样这个包被其他设备收到时，...local...设备的...mac...和...ip...会被其他设备学到。当然不会有...GARP reply...，因为...destination MAC...是...ffff
# ARP Policer #
如果不配...，每个...ifl...有一个...default arp policer...：
lab> show policer
Policers:
Name                                                Bytes              Packets
__default_arp_policer__                                 0                    0 ...<...<<<
policer...限制...arp...后，还有个...ddos...也有送到...re...的...arp...的限制....
如果特殊配置了：
> show interfaces ae5.601 detail
Policer: Input: POLICER_1M_ARP-ae5.601-log_int-arp
<<<如果ifl配了policer，show interface可以看到
user@router# show interfaces XXXX
unit 0 {
family inet {
policer {
disable-arp-policer;<<<关掉arp policer
}
NPC1(igw21.jrc vty)# show filter index 17000 counters
Filter Counters/Policers:
Index               Packets                 Bytes  Name
--------  --------------------  --------------------  --------
17000            4583247024                        __default_arp_policer__
# How to identify and protect interfaces experiencing ARP storm#
https://juniper.lightning.force.com/lightning/r/Knowledge__kav/ka0Dp000000j9d0IAA/view
NGMPC0(jtac-mx480-r2027-re1 vty)# show filter
Index    Semantic  Properties   Name
--------  ---------- --------  ------
17000  Classic    -         __default_arp_policer__
NGMPC0(jtac-mx480-r2027-re1 vty)# show filter index 17000 counters
Filter Counters/Policers:
Index               Packets                 Bytes  Name
--------  --------------------  --------------------  --------
17000                     0                        __default_arp_policer__
> start shell pfe network afeb0
show filter index 17000 counters
Filter Counters/Policers:
Index    Packets    Bytes Name
-------- -------------------- -------------------- --------
17000    3255603      __default_arp_policer__ <----------- Default ARP policer drops
Default ARP policer has bandwidth-limit of 150000bps and burst size of 15000 bytes:
NGMPC0(jtac-mx480-r2027-re1 vty)# show filter index 17000 program
Filter index = 17000
Optimization flag: 0x0
Filter notify host id = 0
Pfe Mask = 0xFFFFFFFF
jnh inst = 0x0
Filter properties: None
Filter state = CONSISTENT
term default
term priority 0
then
accept
policer template __default_arp_policer__
policer __default_arp_policer__
app_type 0
bandwidth-limit 150000 bits/sec
burst-size-limit 15000 bytes
discard
Find which interface having huge arp traffic:
labroot@jtac-mx480-r2027-re1> show interfaces extensive | match "Broadcast packets"
Broadcast packets                        0                0
Broadcast packets                      176              193
Broadcast packets                        0                0
Broadcast packets                        0                0
labroot@jtac-mx480-r2027-re1> show interfaces extensive | find "176              193"
<snip...>
Logical interface xe-0/0/1.0 (Index 337) (SNMP ifIndex 676) (Generation 146)
KB79410 Multiple BGP/BFD sessions bouncing due to default arp policer drops.
http://juniper.lightning.force.com/lightning/r/Knowledge__kav/ka0Dp000000ihAhIAI/view
This PFE level aggregate default_ARP_policer has a bandwidth of 150Kbps and it is by default applied to all the interfaces ( L3 IFLs ) implicitly within a PFE with an index number of 17000
# Default Policer #
https://junipernetworks.sharepoint.com/sites/nok/technology/routing/SitePages/Discussion%20Details.aspx?RootPostId=105126&ListGuid=undefined#/
For example, I see learning rate ~440 ARPs/sec on one 10G port on MPC2E line card with RE-S-1800x4 with __default_arp_policer__.
As far as I understand, the math is: 440 packets/sec * (28 bytes ARP + 18 bytes Ethernet header) = 440 * 46 bytes ~ 162 kbps,
which correlates with __default_arp_policer__ settings 150 kbps rate/15 KB burst.
If I set up more than 440 ARP/sec from traffic generator, I see drops in __default_arp_policer__ statistics.
As the ARP policer on these router was dropping ARP packets inbound, it was effecting the ARP resolution on both sides which is the reason why some BGP neighbor sessions were destroyed by local router ( Notification SENT by local router ) and some by remote peers ( Notification received from Remote side ) depends the direction in which the BGP keepalives/updates has failed due to ARP resolution problems
The ARP policer will be acting on below two types of ARP packets which is coming from other end which caused the BGP flaps.
# Proxy ARP #
代理ARP是ARP协议的一个变种。 对于没有配置缺省网关的计算机要和其他网络中的计算机实现通信，网关收到源计算机的 ARP 请求会使用自己的 MAC 地址与目标计算机的 IP地址对源计算机进行应答。代理ARP就是将一个主机“作为”另一个主机对收到的ARP请求进行应答。它能使得在不影响路由表的情况下添加一个新的Router，使得子网对该主机来说变得更透明化。同时也会带来巨大的风险，除了ARP欺骗，和某个网段内的ARP增加，最重要的就是无法对网络拓扑进行网络概括。代理ARP的使用一般是使用在没有配置默认网关和路由策略的网络上的。
Host 1 --- R2 --- Host 2
R2 ...从...host 2...学到...arp mac2...：...ip2...，... ...将这个...arp...改成... mac r2...：...ip2 ...发给...host 1. ...这样...host 1...就会认为...ip2...在...R2...身上，就会把包发给...r2.
R2...收到...host 1...来的包，知道是要到...host2...的，就会将这个包送给...host 2.
产生条件：... host...没有...default gateway
#ARP 攻击#
如果发现大量arp response...，肯定就是发生了arp... 攻击...。攻击者会讲destination... mac改成自己的mac...，从而把traffic引流到自己处。
