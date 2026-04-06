
/volume/case_2026/2026-0331-658701
/volume/CSdata/annaw/case/2026-0331-658701


![[Pasted image 20260401083902.png]]






MX-BD-04: ae35.1318, ae35.1450
MX-TP-04: ae35.1280, ae35.1218, ae35.1351 and ae35.1477
Link 1 (MX-BD-04) is the active link towards SATs customer while Link 2 (MX-TP-04) is the back up link.
The SATs impacted links are under ae35. There appear to be no service impact for other ae35 unit links.
- When there is user traffic, rapid ping test of 10,000 packets will have random packet drops (without MTU1472 DF)
- When there is no user traffic, Singtel did a rapid ping test of 10,000 packets with MTU1472 DF and no packet drop

ae35 interfaces output drops:
Drops: 1760504592

Matching in total of Queue drops:
Queue counters: Queued packets Transmitted packets Dropped packets
0 6753735274397 6751979638040 1755636357
1 8223660711 8218798184 4862527
2 769391933641 769391929183 4458
3 44939887447 44939886197 1250

From juniper side when pinged to CISCO side, Juniper only received 98 back.

We have concluded the call and determine that there are two issues causing the packet drops which is L2 issue in between PE and CE and the CE COS buffer issue.
