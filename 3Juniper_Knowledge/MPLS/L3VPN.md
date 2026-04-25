# L3VPN

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

L3VPN
# Command #
> traceroute 60.60.60.60 routing-instance VRF1...   <<< trace from VRF
# run show route table inet.3  <<<看local AS中的对端pe的lo00地址是否在
# run show route table mpls.0 detail <<< 看vpn label的动作是否正确
# run show route table mpls.0 label 299920 detail <<< 看vpn label的动作是否正确
# run show route table VRF.inet.0  <<< 看远端vpn路由是否进入vrf
# run show route advertising-protocol bgp 1.1.1.1 <<看vpn路由是否发给远端peer PE
# run show route receiving-protocol bgp 1.1.1.1 <<看vpn路由是否发给远端peer PE
# bgp.l3vpn.0 table #
bgp配 family inet-vpn unicast，就把ipv4地址变为vpn v4地址在lsp中传，bgp变成mbgp
这个表里装的是vpnv4路由，nh是mpls路由，只能在inet3里解决. vpnv4地址是ipv4地址加上RD组成的
-RT: ...为了解决要发到...peer...哪个...VRF...的问题
-RD:...如果本端...2...个...vpn...的...3...层地址一样，都进到一个...vrf...的话，就难以区分了。所以要用...RD.
# Secondary table #
user@host> show route table bgp.l3vpn.0 extensive10.255.14.175:3:10.255.14.155/32 (1 entry, 0 announced)        *BGP    Preference: 170/-101                Route Distinguisher: 10.255.14.175:3                Source: 10.255.14.175                Nexthop: 192.168.192.1 via fe-1/1/2.0, selected                label-switched-path vpn07-vpn05                Push 100004, Push 100005(top)                State: <Active Int Ext>                Local AS:    69 Peer AS:    69                Age: 15:27      Metric2: 338                Task: BGP_69.10.255.14.175+179                AS path: 1 I                Communities: target:69:100                BGP next hop: 10.255.14.175                Localpref: 100                Router ID: 10.255.14.175                Secondary tables: VPN-A.inet.0 <<< this route will also be installed in this table
user@host> show route table VPN-A.inet.0 detail10.255.14.155/32 (1 entry, 1 announced)        *BGP    Preference: 170/-101                Route Distinguisher: 10.255.14.175:3                Source: 10.255.14.175                Nexthop: 192.168.192.1 via fe-1/1/2.0, selected                label-switched-path vpn07-vpn05                Push 100004, Push 100005(top)                State: <Secondary Active Int Ext>                Local AS:    69 Peer AS:    69                Age: 1:16:22    Metric2: 338                Task: BGP_69.10.255.14.175+179                Announcement bits (2): 1-KRT 2-VPN-A-RIP                AS path: 1 I                Communities: target:69:100                BGP next hop: 10.255.14.175                Localpref: 100                Router ID: 10.255.14.175                Primary Routing Table bgp.l3vpn.0 <<< this route was imported from this table
# Sham Link... #
比如CE1-CE2，PE1从CE1收进来OSPF 10，从core里收进来的是bgp 170，所以会走backdoor ospf到达CE2.
解决方法就是把PE1-PE2之间的link加上sham-link，这样都是ospf10，sham-link在ospf里优选。但是sham-link的nh是假的，所以nh不可达。因此ospf就不会active，这时只剩BGP了。
#... ...hub-spoke... #
Spoke sites之间通讯，是从一个spoke site -...->... hub site -->另一个spoke site
HUB SITE的router会有hub 和 spoke两个instance。Hub instance只从CE收然后发给remotePE，而spoke instance只从remote PE收然后发给CE.
这样结果就是spoke router 都把自己的路由发给hub site，然后再通过hub site转发给其他spoke router的路由
# rib-group #
instacne之间导表用
# 6VPE #
set protocols mpls ipv6-tunneling
set protocols bgp group ibgp family inet6-vpn unicast
# vrf-table-label #
他的作用就相当于把...PE-CE...的direct路由重分布到vrf里。他不是为了远端，而是如果vrf练了若干个...CE...，这些ce之间的通讯。
这个会建立一个lsi口，这个口和vpls里的no-tunnle-service的口一样，虚拟口
# Provider Edge Link Protections in Layer 3 VPNs #
https://www.juniper.net/documentation/us/en/software/junos/vpn-l3/topics/topic-map/l3-vpns-pe-link-protection.html#id-example-configuring-provider-edge-link-protection-in-layer-3-vpns
Understanding Provider Edge Link Protection for BGP Labeled Unicast Paths
Layer 3 VPNs are used for carrier-of-carriers deployments, the protocol used to link the customer edge (CE) routers in one autonomous system (AS) and a provider edge (PE) router in another AS is BGP labeled-unicast.
Therefore, a precomputed protection path should be configured such that if a link between a CE router and a PE router goes down, the protection path (also known as the backup path) between the other CE router and the alternative PE router can be used.
Understanding Provider Edge Link Protection in Layer 3 VPNs
In an MPLS service provider network, a customer can have dual-homed CE routers that are connected to the service provider through different PE routers.
