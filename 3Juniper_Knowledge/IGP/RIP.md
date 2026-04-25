# RIP

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

RIP
- by default, RIP auto accept all routes from neighbors but does not send any route. 所以rip一般会配一个export policy。
- 当show advertising rip的时候，peer地址要指local interface address，因为rip的neighbor是一个multicast address
- 距离矢量协议，Hop used as the metric for path selection
======================================================
[edit protocols rip]
lab# show
group rip-group {
export to-rip-routes; <<< ...貌似本身也不产生路由
neighbor ge-0/0/0.0;<<< local ifl...，和...peer...相连的口
}
[edit policy-options]
lab# show
policy-statement to-rip-routes {
from protocol direct;
then accept;
}
=======================================
lab@R4# run show route advertising-protocol rip 172.30.0.49   <<<<
和...bgp...不同，...rip...后面跟的地址是连着...peer...的出接口
run show route receive-protocol rip 172.30.0.50
rip...后面跟的是...nh ...地址
