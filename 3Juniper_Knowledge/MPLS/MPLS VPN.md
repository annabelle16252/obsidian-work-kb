# MPLS VPN

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

MPLS VPN
# Commands #
> show mpls lsp name XXX detail
Sep 22 15:51:47
Ingress LSP: 6 sessions <<< 看这条lsp的primary和secondary是否都up
> show route 101.234.63.57
Sep 22 16:43:39
inet.0: 1394 destinations, 1397 routes (1394 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both
101.234.63.57/32   *[OSPF/10] 1w2d 17:15:56, metric 262
to 101.234.61.1 via xe-0/0/0.0
>  to 101.234.61.13 via xe-1/0/0.0
inet.3: 359 destinations, 366 routes (359 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both
101.234.63.57/32   *[RSVP/7/1] 01:53:46, metric 262
>  to 101.234.61.13 via xe-1/0/0.0, label-switched-path BOI-LONHE-ER6-EIG-MUMTT-ER1
to 101.234.61.221 via xe-0/1/0.0, label-switched-path BOI-LONHE-ER6-EIG-MUMTT-ER1
[RSVP/8/1] 01:54:45, metric 262
>  to 101.234.61.1 via xe-0/0/0.0, label-switched-path BOI-LONHE-ER6-EIG-MUMTT-ER1
to 101.234.61.221 via xe-0/1/0.0, label-switched-path BOI-LONHE-ER6-EIG-MUMTT-ER1
[LDP/9] 1w2d 17:15:56, metric 262
to 101.234.61.1 via xe-0/0/0.0, Push 22
>  to 101.234.61.13 via xe-1/0/0.0, Push 22
# set routing-options resolution rib bgp.l3vpn.0 resolution-ribs inet.0 #
当...RSVP lsp...来建立...PE...之间...lsp...的时候，...RR/P routers...上不会配...lsp,...这样就不会有...inet.3 table.
所以当...RR...反射...L3VPN...的时候就会...hidden...，因为...bgp.l3vpn.0...表的...nh...解析...default...是在...inet3...里，而...RR...上有没有。（...L2VPN similar...）
解决方案...1...：
user@host# set routing-options resolution rib bgp.l3vpn.0 resolution-ribs inet.0
<<<RR...上配之个让去...inet0...解析下一跳
https://www.juniper.net/documentation/us/en/software/junos/bgp/topics/topic-map/bgp-rr.html
解决方案...2...：
RR/P...手工配几条...lsp...分别到...PEs...。
***...如果是都配...ldp...就不需要以上操作了，自动...RR...上就会有...inet3...表
