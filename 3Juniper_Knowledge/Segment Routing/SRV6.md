# SRV6

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

SRV6
-SRv6和SR的区别是它完全脱离mpls。用SRv6来取代MPLS协议分发label...。... 总结就是各种协议over ipv6来实现宿求。
(1) L3VPN over SRv6
传统 L3VPN 是：MP-BGP 分发 VPNv4/v6 路由 + MPLS Label。
SRv6 L3VPN 是：MP-BGP 分发 VPNv4/v6 路由 + SRv6 SID（代替 MPLS Label）。
数据包到达出口 PE 时，通过 SRv6 SID 指定的函数来把流量送入 VRF。
(2) EVPN over SRv6
underlay不再需要vxlan或者mpls...，...而是 SRv6 SID。
BGP EVPN 分发 MAC/IP 路由时，附带 SRv6 信息（例如 End.DT2U、End.DT4、End.DT6 函数）。
>show route table bgp.l3vpn.0 extensive
10.0.1.0/24 (1 entry, 1 announced)
*BGP    Preference: 170/-101
Next hop type: Router, Next hop index: 552
Next-hop reference count: 4
Localpref: 100
Label operation: Push SRv6 SID
SRv6 SID: 2001:db8:100:1::100
