# Option ABC

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

Option ABC
是为了解决当跨AS的时候LDP/RSVP断的问题。因为...igp...不能跨域，所以...lsp...会断。解决只能...ASBR...起ebgp或静态互指。
但是在AS内部的LDP/RSVP却需要依赖于ospf、isisi， 所以无法在两个AS之间直接建立。
Option A: 所有vpn路由都要在ASBR上落地 (unlabeled VPN-IPV4)
Option B: ...ASBR...互传...bgp.l3vpn.0...里的...vpnv4...路由。...ASBR...的...ebgp...开启...inet-vpn unicast
Option C: ASBR之间互传...inet3...的路由。...ASBR...的...ebgp...开启...inet labeled-unicast (BGP-LU)
- ...用...option...，...mpls...只能用...rsvp...不能用...ldp
-option...可以用于...l2vpn...或...l3vpn
# Command #
# run show route table inet.3  <<<看local AS中的对端pe的lo00地址是否在
# run show route table mpls.0 detail <<< 看vpn label的动作是否正确
# run show route table mpls.0 label 299920 detail <<< 看vpn label的动作是否正确
# run show route table VRF.inet.0  <<< 看远端vpn路由是否进入vrf
# run show route advertising-protocol bgp 1.1.1.1 <<看vpn路由是否发给远端peer PE
# run show route receiving-protocol bgp 1.1.1.1 <<看vpn路由是否发给远端peer PE
# Option C #
https://www.juniper.net/documentation/us/en/software/junos/vpn-l3/topics/topic-map/l3-vpns-interprovider.html#d44e186__d46955e3341
PE的标签压栈顺序（由底向上）：
底层 (VPN Label)： 由远端 PE 分配给 VRF 路由。
中层 (BGP-LU Label)： 由远端 AS 的 ASBR 分配，用于到达远端 PE 的 Loopback
外层 (Transport Label)： 本地 AS 的 IGP (LDP/RSVP) 标签，用于到达本地 ASBR
VPNv4 
EBGP 
AS 100 
AS 200 
P 
ASBR1 
ASBR 2 
P 
PE2 
PE1 
BGP-LU 
EBGP 
OSPF/LDP 
CE1 
OSPF/LDP 
CE2 
BGP-LU 
BGP-LU 
IBGP 
IBGP 
INTER-AS VPN OPTION-3 ...VPNv4 
EBGP 
AS 100 
AS 200 
P 
ASBR1 
ASBR 2 
P 
PE2 
PE1 
BGP-LU 
EBGP 
OSPF/LDP 
CE1 
OSPF/LDP 
CE2 
BGP-LU 
BGP-LU 
IBGP 
IBGP 
INTER-AS VPN OPTION-3
# run ping 11.11.11.11 source 10.10.10.10
CE1 10.10.10.10 CE2 11.11.11.11
incorrect behavior on ASBR: on ASBR2 should swap ldp/rsvp label instead of pop
VPNv4 
0C4 
EBGP 
AS 100 
AS 200 
P 
ASBR1 
ASBR 2 
P 
PE2 
PE1 
299792(LDP) + 
299808(LU) + 
299856(LU) + 
16(VPNv4) 
299856(LU) + 
16(VPNv4) 
16(VPNv4) 
16(VPNv4) 
BGP-LU 
EBGP 
OSPF/LDP 
CE1 
OSPF/LDP 
CE2 
BGP-LU 
BGP-LU 
IBGP 
IBGP 
INTER-AS VPN OPTION-3 ...VPNv4 
0C4 
EBGP 
AS 100 
AS 200 
P 
ASBR1 
ASBR 2 
P 
PE2 
PE1 
299792(LDP) + 
299808(LU) + 
299856(LU) + 
16(VPNv4) 
299856(LU) + 
16(VPNv4) 
16(VPNv4) 
16(VPNv4) 
BGP-LU 
EBGP 
OSPF/LDP 
CE1 
OSPF/LDP 
CE2 
BGP-LU 
BGP-LU 
IBGP 
IBGP 
INTER-AS VPN OPTION-3
CE 1 10.49.241.240
CE 2 10.49.231.197
PE1 10.49.241.246 P(AS 100)
P (AS 100)10.49.241.234
ASBR1 10.49.241.251
ASBR2 10.49.241.243
P (AS 200) 10.49.240.182
PE2 10.49.235.49
# mpls-forwarding #
需要加这个命令当中间跑ldp的时候...：
# set protocols mpls traffic-engineering mpls-forwarding
<<<
root@ASBR1# run show route table inet.0
inet.0: 17 destinations, 19 routes (17 active, 0 holddown, 0 hidden)
@ = Routing Use Only, # = Forwarding Use Only
+ = Active Route, - = Last Active, * = Both
1.1.1.1/32         @[OSPF/10] 07:56:33, metric 2
>  to 192.168.2.2 via ge-0/0/2.0
#[LDP/9] 00:00:15, metric 1
>  to 192.168.2.2 via ge-0/0/2.0, Push 299808
<<<<
ospf still will be choosen in inet.0
下面这个命令ldp...，rsvp会出现在inet...0里...，有风险，尽量用第一个
# set protocols mpls traffic-engineering bgp-igp-both-ribs
<<<<
root@ASBR1> show route table inet.0
inet.0: 17 destinations, 19 routes (17 active, 0 holddown, 0 hidden)
@ = Routing Use Only, # = Forwarding Use Only
+ = Active Route, - = Last Active, * = Both
1.1.1.1/32         *[LDP/9] 07:53:14, metric 1
>  to 192.168.2.2 via ge-0/0/2.0, Push 299808
[OSPF/10] 07:53:14, metric 2
>  to 192.168.2.2 via ge-0/0/2.0
<<<<
Juniper 的 VPN Option C 必须 ...use-lsps-for-forwarding...，因为 Option C 本质是“跨 AS 的 MPLS 传输”，而不是 IP 转发。
https://www.juniper.net/documentation/us/en/software/junos/mpls/topics/topic-map/mpls-traffic-engineering-configuration.html
==================================================================
是为了解决当跨...AS...的时候...LDP/RSVP ...断的问题。
*AS100...和...AS200...之间不能跑...ospf...、...isisi...路由协议，因为跨域了。只能起...ebgp...和静态互指。但是在...AS...内部的...LDP/RSVP...却需要依赖于...ospf...、...isisi...，... ...所以无法在两个...AS...之间直接建立。
Option A: ...所有...vpn...路由都要在...ASBR...上落地... (unlabeled VPN-IPV4)
Option B: ...传单个表的...vpnv4...，...PE-PE...对接...(Labeled VPN-IPV4)
Option C: ASBR...之间只传需要互通的...vpn...路由。用一个...LSP...把...2...个...PE...连起来。...PE3...和...PE4...之间的...loopback...起...multi-hop ebgp...。...(Multi-hop MP-eBGP)
##PE4...和...PE3...不是真正的...PE...而是...ASBR...，也就是在他们上面是没有...routing-instance...的。
有...routing-instance...的是...PE1...和...PE2
##...就是在...PE12 PE34...之间导...inet.3...的表
[edit protocols bgp group internal]
regress@R1# set family inet labeled-unicast rib inet.3
======================================
LDP...实现不了...ABC...，因为...LDP...依赖于...IGP...，... IGP...不能跨...AS
============================
Option B
ASBR...之间建立...mp-ebgp...，互传...vpnv4...路由
ASBR...之间建立...mp-ebgp...，互传...vpnv4...路由
Option C
ASBR ...之间建立...MP-eBGP labeled unicast...，... ...相互交换...lo0...路由。...VPNV4...路由不存在...ASBR...上面了。
是为了解决当跨...AS...的时候...LDP/RSVP ...断的问题。
*AS100...和...AS200...之间不能跑...ospf...、...isisi...路由协议，因为跨域了。只能起...ebgp...和静态互指。但是在...AS...内部的...LDP/RSVP...却需要依赖于...ospf...、...isisi...，... ...所以无法在两个...AS...之间直接建立。
Option A: ...所有...vpn...路由都要在...ASBR...上落地... (unlabeled VPN-IPV4)
Option B: ASBR...之间只需要起...mp-ebgp...，通过...policy...把...bgp.l3vpn.0...里面的...vpnv4...的路由互相导
Option C: ASBR...之间只传需要互通的...vpn...路由。用一个...LSP...把...2...个...PE...连起来。...PE3...和...PE4...之间的...loopback...起...multi-hop ebgp...。...(Multi-hop MP-eBGP)
>>>...普通...vpn...是两个...site...中间夹着一个...provider core ...（一个...as...）。而...option...是两个...remote site...中间夹着两个...provider core...（...2...个...as...），所以需要把两个...core...缝在一起。
===============================================================
<Task 1 Option B>
1.在ASBR上开启mpls
2.在ebgp里开启l3vpn family
3.更改policy 让他只针对inet0,并且允许从C2-S4来的bgp路由
4.RR ibgp的family route-target里advertise-default
<<<above is for import from remote site
5.改hub-spoke的community
lab@Canopus# set interfaces ge-0/0/5.302 family mpls
lab@Canopus# set protocols mpls interface ge-0/0/5.302
《《《《...AS border router R3 ...上开启...mpls
3) family inet-vpn unicast  on EBGP session
lab@Canopus# set protocols bgp group p3-1 family inet-vpn unicast
《《《...asbr...的...ebgp session...开启...l3vpn family,...让他有...l3vpn.inet.0...的表存放...vpnv4...路由
>show bgp summary
192.168.0.6      2831679853          6         28       0       0        1:08 Establ
bgp.l3vpn.0: 2/3/2/0  <<<< lost inet.0
lab@Canopus# set protocols bgp group p3-1 family inet unicast <<<< adding this
192.168.0.6      2831679853         91        441       0       0           7 Establ
inet.0: 84/116/84/0  <<<<
bgp.l3vpn.0: 2/3/2/0
<<<<<ASBR...的...CE...方向...ebg session...也开启了...mpls vpn
《《《...optionB...在...neighbor...下面直接是... bgp.l3vpn.0...，而...optionc...下面是...inet3....所以...vpnv4...路由没有在...optionC...的...asbr...上落地，而在...optionB...上落地了
4)Policy  646
????...怎么...existing policy...和...pdf...不一样...    P292
lab@R3# show policy-options policy-statement P3-filter
term 1 {
from {
route-filter 0.0.0.0/0 prefix-length-range /8-/24;
}
then {
local-preference 200;
community set P3;
accept;
}
}
term 2 {
then reject;
}
lab@Canopus# set term 1 from family inet   <<<<
rename term 2 to term 3
rename term 1 to term 2
<<<term 3 add from faimly inet
term 1 {
from {
protocol bgp;
as-path p3-local-routes;
route-filter 0.0.0.0/0 prefix-length-range /32-/32; <<<
}
then accept;
}
lab@Canopus# insert term 1 before term 2 <<< by default according to configuration sequence
---------------------
我认为正确的配置：
[edit policy-options policy-statement P3-filter]
lab@R3# show
term 1 {
from {
protocol bgp;
as-path P3-local-routes;
route-filter 0.0.0.0/0 prefix-length-range /32-/32;
}
then accept;
}
term 2 {
from {
family inet;《《《《
route-filter 0.0.0.0/0 prefix-length-range /8-/24;
}
then {
local-preference 200;
community set P3;
accept;
}
}
term 3 {
from family inet;<<<<...PDF...没有这个，但是没有这个不向...p3-1...发...vpnv4...路由
then reject;
}
lab@Canopus# set policy-options as-path p3-local-routes ...2831679853
---------------------------
虽然不改...import policy...已经... ...能看到...remotePE...的...lo0...地址可以建立...mpebgp...了，但是由于...term3 reject...了所有，而这个...bgp...是有...inet...和...ient-vpn...俩个...families...，...l3vpn...路由是传不过来。
Term1:...允许...inet...和...inet-vpn...的.../32...路由
Term2...：对...family inet...之前...policy...的要求
Term3...：只...reject inet...，放行行其他...inet-vpn...的路由。
5)bgp route target family
lab@R3# run show route advertising-protocol bgp 172.30.5.41 table bgp.l3vpn.0
bgp.l3vpn.0: 33 destinations, 33 routes (33 active, 0 holddown, 0 hidden)
Prefix                  Nexthop              MED     Lclpref    AS path
172.30.5.3:4:0.0.0.0/0
*                         Self                         100        I
172.30.5.3:4:172.30.5.17/32
*                         Self                         100        I
172.30.5.3:4:172.31.48.0/30
《《《《没有来自...64600 ...的
R3:
group ibgp {
type internal;
local-address 172.30.5.3;
family route-target;...  <<<<<
《《《《...L3VPN...专用，...PE...根据自己...local configure...的...vrf...的...RT...，发给...RR...一个...list...，...RR...只接收和反射含有...list...中...RT...的...L3VPN route. ...比如这个...case...中：
R3...的...instance...的...RT ...有...:...54591:501... ,500,300,100...
而...R3-P3...并没有建立...instance...在...r3...上，由于...Route-target...只根据...vrf...配置的...rt...反射，所以来自...P3 64600...的就不会出现。
<<<R3...会把知己收到的都发给...rr ...！！！...Only on R3
lab@Canopus# set protocols bgp group ibgp family route-target advertise-default
lab@R3# run show route advertising-protocol bgp 172.30.5.41 table bgp.l3vpn.0
bgp.l3vpn.0: 69 destinations, 69 routes (69 active, 0 holddown, 0 hidden)
Prefix                  Nexthop              MED     Lclpref    AS path
172.30.5.3:4:0.0.0.0/0
*                         Not advertised               100        I
172.17.47.2:200:172.31.78.0/24
*                         Self                         100        2831679853 64600 I
172.17.47.2:200:172.31.79.0/24
*                         Self                         100        2831679853 64600 I
172.17.47.2:200:192.168.0.100/30
*                         Self                         100        2831679853 I
《《《我的...lab...和...pdf...不一样，此处没有发出去，需要昨晚...normalize...之后才可以
6)received route target
lab@Canopus# run show route receive-protocol bgp 192.168.0.6 table bgp.l3vpn.0 extensive
bgp.l3vpn.0: 70 destinations, 70 routes (70 active, 0 holddown, 0 hidden)
* 172.17.47.2:200:172.31.78.0/24 (1 entry, 1 announced)
Accepted
Route Distinguisher: 172.17.47.2:200
VPN Label: 299792
Nexthop: 192.168.0.6
AS path: 2831679853 64600 I
Communities: target:43208:200...   <<<<<<<...从...P3-1...收进来的带着这个...community
7)Spoke site
<<<...之前...L3VPN C2 sites...是...hub-spoke...，有...S1-S3,...现在把...S4...加进来要求是...spoke site...。
如果不跨...as...的话，...R3...上应该建一个...instance...做...spoke-site...。但是跨...as...，...S4...的...PE...就在...remote as...了，那么...vrf...就建在...remote PE...。... Local R3...成为...ASBR...只负责...policy bgp...路由。
lab@Canopus# set policy-options policy-statement c2-vpn-target-import term 1 from community ce2-remote
lab@Canopus# set policy-options policy-statement c2-vpn-target-import term 1 then community delete ce2-remote
lab@Canopus# set policy-options policy-statement c2-vpn-target-import term 1 then community add ce2-spoke
lab@Canopus# set policy-options policy-statement c2-vpn-target-import term 1 then accept
<<<<...从...S4...收来的...ebgp...路由带着的是... ce2-remote...的...community...，到...R3 delete...这个...community...，...add spoke community...，然后...R3...会发给...hub routers.!!!...因为...R3...没有...vrf,...所以这个...policy...要用在和...P3...的在...mp-ebgp...上。
<<<<P3-1...是和...remote CE...的...ebgp...关系，这个...community...的...polciy...和其他...spoke site RI...的...policy...方向相反。
以上就是通过在...R3...上的...community...的更改让...local AS ...的...hub site...可以识别。
lab@Canopus# set policy-options policy-statement c2-vpn-target-export term 1 from community ce2-hub
lab@Canopus# set policy-options policy-statement c2-vpn-target-export term 1 from community dele ce2-hub
lab@Canopus# set policy-options policy-statement c2-vpn-target-export term 1 then community add ce2-remote
lab@Canopus# set policy-options policy-statement c2-vpn-target-export term 1 then accept
《《《...R3...从...hub routers...收来的路由带着...hub community...，当...R3...发给...S4...的时候，去掉这个...community...，加上...S4...的...ocmmunity
lab@Canopus# set policy-options community ce2-remote members target:43208:200
lab@Canopus# set policy-options community ce2-spoke members target:54591:201
lab@Canopus# set policy-options community ce2-hub members target:54591:200
<<<< hub, spoke ...去看...R1.R7 R5...就能知道。
《《《... ce2-reomote...，...show receiveing protocol bgp exten ...可以知道...S4...发过来的是什么。
lab@Canopus# set protocols bgp group p3-1 import c2-vpn-target-import
lab@Canopus# set protocols bgp group p3-1 export c2-vpn-target-export
《《《...ebgp
lab@Canopus# run show route advertising-protocol bgp 172.30.5.41 table bgp.l3vpn.0 extensive
* 172.17.47.2:200:172.31.78.0/24 (1 entry, 1 announced)
BGP group ibgp type Internal
Route Distinguisher: 172.17.47.2:200
VPN Label: 309952
Nexthop: Self
Flags: Nexthop Change
Localpref: 100
AS path: [54591] 2831679853 64600 I
Communities: target:54591:201 <<<<<
lab@R3# run show route advertising-protocol bgp 192.168.0.6 table bgp.l3vpn.0 extensive
bgp.l3vpn.0: 70 destinations, 70 routes (70 active, 0 holddown, 0 hidden)
* 172.30.5.1:5:172.30.5.9/32 (1 entry, 1 announced)
BGP group P3-1 type External
Route Distinguisher: 172.30.5.1:5
VPN Label: 308224
Nexthop: Self
Flags: Nexthop Change
AS path: [54591] I
Communities: target:43208:200 origin:54591:1... ...《《《《
！！！这个时候才能看到...route...被...advertise...到...RR....顺序和...pdf...不一样啊？？
8) Verify label option
lab@R3# run show route receive-protocol bgp 192.168.0.6 table bgp.l3vpn.0 extensive
bgp.l3vpn.0: 69 destinations, 69 routes (69 active, 0 holddown, 0 hidden)
* 172.17.47.2:200:172.31.78.0/24 (1 entry, 1 announced)
Accepted
Route Distinguisher: 172.17.47.2:200
VPN Label: 299872
Nexthop: 192.168.0.6
AS path: 2831679853 64600 I
Communities: target:43208:200
《《《从...remote site 4 PE...收到的路由。告诉我要到这个地址...nh...是他，需要带着...label ...299872过去。
root@vr-device# run show route label 299872
mpls.0: 5 destinations, 5 routes (5 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both
299872             *[VPN/170] 00:16:43
> to 192.168.0.102 via lt-0/0/0.10, Pop
》》》在...VR...上可以看到当...remote site 4 PE...收到这个包后，把...vpn label pop...掉进去...CE. ...所以这个...label...像显示的是...vpn...的而不是...mpls...的。
lab@R3# run show route advertising-protocol bgp 172.30.5.41 table bgp.l3vpn.0 extensive
* 172.17.47.2:200:172.31.78.0/24 (1 entry, 1 announced)
BGP group ibgp type Internal
Route Distinguisher: 172.17.47.2:200
VPN Label: 310832
Nexthop: Self
Flags: Nexthop Change...<<<ebgp nhs
Localpref: 100
AS path: [54591] 2831679853 64600 I
Communities: target:54591:201
《《《...R3...再把这条传给...RR,...告诉...RR(...其他...PE)...到...remote c2-s4 nh ...是我...R3. ...带着...label ...310832过来
lab@R3# run show route label 310832
mpls.0: 71 destinations, 71 routes (71 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both
310832             *[VPN/170] 00:12:03
> to 192.168.0.6 via ge-0/0/5.302, Swap 299872
《《《可以看出来，当...CORE...里其他...PE...带着310832过来下一跳是...192.168.0.6...，要...swap...他宣告的...label 2...99872过去。
再看...R2:
lab@R2# run show route table bgp.l3vpn.0
172.17.47.2:200:172.31.78.0/24
*[BGP/170] 00:24:00, localpref 100, from 172.30.5.41
AS path: 2831679853 64600 I
> to 172.30.0.14 via ge-0/0/4.123, Push 310832
<<<<...从...R2...看果然去...remote CE...需要...push...这个...label
！！！注意，这场接口的...push ...的...label...在...mpls0...里面并...show...不出来
=======================================
<Task 2 Option C>
《《《《...most scalable...，因为没有...vpnv4...路由存在...ASBR...上。...RR...和...CE...建立...ebgp...关系。
《《《...remote AS PE...和...local AS PEs...彼此有对方的...lo0...，这样要到彼此就可以通过...ASBR...到达。...option c...的...key...是...ASBR...互有对方的...loopback ...在...inet3...和...inet0...里面.......只要带...label...的路由都要在...inet3...里解决...nh...，虽然...inet3...里的路由和...inet0...里面的写法一样，但是不是一回事儿。不像...vpnv4...路由会被存放到...bgp...。...lxvpn...。...0...表里，...mpls label...路由没有自己的表，而只有解决自己...nh...的...inet3...这个表。
《《《《...peer ASBR...应该和...local AS...的所有...vpls PE...建立...mp-ebgp...，但是为了省配置，可以和...RR...建立。
《《《...IZ...要求...remote site...和...vpls...的说有...PE...都互通，所以需要和...RR...建立...ebgp...，这样所有...vpls PE...的...lo0...都能通过...RR...给...remote site...，反之亦然。...BC...只要求...local ...一个...PE...和...remote site...通，所以...ebgp...建立在...local PE...上而不是...rr...上。
SUMMARY:
1.在vpls sites的ibgp和RR上开启labeled-unicast rib inet.3
2. ASBR的ebgp上开启labeled-unicast rib inet.3
3.Change ebgp ...export... policy to ...export locall ... lo0
(confirm if remote site lo0 is now in inet3)
《...rib inet3...传给...peer...》
4.change rib-group policy to import local lo0 from inet0 to inet3
<<<<以上是ASBR之间inet3互有lo0. 这是第一个ebgp session 结束
5.RR和remote site建立multihop no-nh-change EBGP session
6.在RR上，更改ebgp的community import export
<<<<第二个ebgp session 结束
1)Enable label unicast on ibgp ...《《《...C5 bgp vpls PEs:R2345& RR
<<<...加上这个就是给这个包再打一个...label...，同事让这些本来没有...inet3...的...bgp session...有了...inet3....这样当传带着...label...的...vpnv4...路由的时候可以去...inet3...解决...nh...。
<<...这个...family...不是...vpn...的...family...，是...mpls...用来打...label...的
--------------
lab@Canopus# run show bgp summary
192.168.0.6      2831679853         91        422       0       0          33 Establ
inet.0: 84/116/84/0
inet.3: 1/1/1/0<<<...多出来这个表在...bgp session
所以带着...label-unicast...的包会直接进...inet3...，而不是...inet0....因为...family inet laberled-unicast...，如果不配...rib inet3...，...default...路由会收发进...inet0....因为整条路都是...mpls...的，所以不能在...inet0...里收发解决...nh
---------------
[edit protocols bgp group ibgp]
lab@Sirius# set family inet labeled-unicast rib inet.3
<<<labeled-unicast for option c. transport label between 2 ASs
<<< ...因为是...family inet...，所以...default...是...inet0...，为了传...label...，让...label-unicast...的路由里面把...inet3...作为...primary route table...，代替...inet0...。... ...因为两个...asbr...要互有对方...PE...的...lo0...，而这个地址在...inet3...里而不是...inet0. ...因为...vpnv4...的路由的...nh...是在...inet3...里解决的。
《《《这样的话，和...peer ASBR...交换的就是所有...inet3...里的...mpls lsp...路由...,...这个就可以解决跨...AS lsp...断的问题....
lab@R3# run show bgp summary
State|#Active/Received/Accepted/Damped...
172.30.5.41           54591        705        267       0       1        2:20 Establ
inet.3: 0/0/0/0... ...《《《出现了
如果去掉和...rr...的...ibgp...的...labeled-unicast rib inet.3...，和...rr...的...bgp...就不会有这个表。正常和...RR...的...bgp session...里不需要有...inet3.inet3...是...core...里...mpls...路由存放的地方。
2...）...R3: Enable label unicast on ebgp with peer  ...《《《
====================
lab@R3# run show bgp summary
192.168.0.6      2831679853         91        436       0       0           1 Establ
inet.0: 84/116/84/0
bgp.l3vpn.0: 3/3/3/0
<<<<...当...R3-P3...之间的...ebgp...没有开...labele unicast...的时候，这是没有...inet3...的表。意味着没有...lsp...通
====================
lab@Canopus# set protocols bgp group p3-1 family inet labeled-unicast rib inet.3
====================
192.168.0.6      2831679853         93        439       0       0           8 Establ
inet.0: 84/116/84/0
bgp.l3vpn.0: 3/3/3/0
inet.3: 1/1/1/0
《《《《多了...inet3
====================
3...）...R3...上更改...policy...允许收发彼此...lo0...在...inet3&inet0...里
！！！...Lo0...的互传一定要保证是...inet3&inet0...里都允许。因为只有...inet3...，...igp ...没有的话也不可达。
看...import-policy...：
-------------------
lab@Canopus# show policy-options policy-statement p3-import
term 1 {
from {
as-path p3-1;
route-filter 0.0.0.0/0 prefix-length-range /32-/32;
}
then {
local-preference 200;
community add p3;
accept;
}
}
term 2 {
from {
route-filter 0.0.0.0/0 prefix-length-range /8-/24;
}
then {
local-preference 200;
community add p3;
accept;
}
}
term 3 {
then reject;
}
----------------------
lab@Canopus# run show route 172.17.47.3/32
inet.0: 754 destinations, 756 routes (686 active, 0 holddown, 70 hidden)
+ = Active Route, - = Last Active, * = Both
172.17.47.3/32     *[BGP/170] 00:46:14, localpref 200
AS path: 2831679853 I
> to 192.168.0.6 via ge-0/0/5.302
inet.3: 16 destinations, 22 routes (10 active, 0 holddown, 10 hidden)
+ = Active Route, - = Last Active, * = Both
172.17.47.3/32     *[BGP/170] 00:46:14, localpref 200
AS path: 2831679853 I
> to 192.168.0.6 via ge-0/0/5.302
《《《...inet0...和...inet3...都收到...remote lo0...了，多以...import policy...不需要改。
《《《...term 1...允许...inet0...和...inet3...的这些条件的路由。...p3...这个...ebgp...开了...family inet...和...inet3...两个...family...。因此...default...从...bgp...收所有这两个表的路由。如果没有“...rib inet3...“那么...policy default...就只收发...inet0
《《通过...lab...发现...inet0...和...inet3...都属于...family inet...，因此...IZ...上这种用...family inet...过滤...inet0...放行...inet3...并不好用。需要用...rib inet0 inet3
看...export-policy...：
---------------
[edit policy-options policy-statement p3-export]
lab@Canopus# show
term 1 {
from {
protocol aggregate;
route-filter 172.30.0.0/16 exact;
}
then accept;
}
《《《没有...reject inet0...和...inet3...的路由，但是...bgp...自动不产生路由。
-------------------------
问题...1...：
-...看到...inet0...里...lo0...的路由们不是以...bgp...形式存在的
-...看到...inet3...表里的路由都是...mpls...的，只有...remote site...收到的...lo0...是...bgp...。因此...local lo0s...也不会被发出去。
show...一下看到...local lo0...没有被传给...peer bgp...，要作...term export...。
问题...2...：
R3...也是...vpls...的一个...site...，但是因为...inet3...里不会有...local lo0...路由，所以...inet3...里还少自己的...lo0...，这样...remote site...就收不到...r3...的，...vpls session...就有问题。
--------------
解决：
lab@Canopus# show routing-options
rib-groups {
rr-lo0-inet3 {
import-rib [ inet.0 inet.3 ];
import-policy rr-lo0-inet3;
}
《《《把一个表的路由放进另一个表一定用...rib-group...。之前已经有了，只需要改一下...policy...。...
《《《...policy default...都是作用于...inet0...的，如果要作用于另一个表，需要用”...to rib...“来写。
[edit policy-options policy-statement rr-lo0-inet3]
lab@Canopus# set term 1 from route-filter 172.30.5.3 exact
《《《加上这个。
lab@Canopus# run show route table inet.3
172.30.5.3/32      *[Direct/0] 00:00:27
> via lo0.0
《《《进来后还是原协议的形式存在。
PDF...配置：
[edit policy-options policy-statement p3-export]
lab@Canopus# show
term 1 {
from {
protocol aggregate;
route-filter 172.30.0.0/16 exact;
}
then accept;
}
term 2 {
from {
rib inet.3;
route-filter 172.30.5.0/24 prefix-length-range /32-/32;
}
then accept;
《《《加上...term 2...后...inet3...有往外发...lo0s...但是...inet0...没有。
？？？因为...bc...上要求...inet0...和...inet3...都发出去，如果一下这样改...inet0...和...inet3...都能出去。
-------------------
term 2 {
from {
family inet;
route-filter 172.30.5.0/24 prefix-length-range /32-/32;
}
then accept;
}
-------------------
Verification:
lab@Canopus# run show route 172.17.47.3
inet.0: 756 destinations, 758 routes (688 active, 0 holddown, 70 hidden)
+ = Active Route, - = Last Active, * = Both
172.17.47.3/32     *[BGP/170] 02:15:38, localpref 100
AS path: 2831679853 I
> to 192.168.0.6 via ge-0/0/5.302
inet.3: 17 destinations, 23 routes (11 active, 0 holddown, 10 hidden)
+ = Active Route, - = Last Active, * = Both
172.17.47.3/32     *[BGP/170] 02:15:38, localpref 100
AS path: 2831679853 I
> to 192.168.0.6 via ge-0/0/5.302
》》》as-path P3-local-routes "2831679853 64600";这个还是需要改成2831679853，不然这条路由没有。
《《《这条需要在...inet0...和...inet3...上都有。
3)ebgp from RR with P3-1 ...（第二个...ebgp session...，是两个...PE...，这里是...rr-remote pe...的...lo0...建立的）
<<<option C...是两个...b  gp session...来...stitch lsp over as...：
两个...AS...之间的...ebgp...和...RR...与...remote pe...的...ebgp
RR:
group P3-remote-pe {
type external;
multihop {
no-nexthop-change;...<<<<
《《《...RR...和...S7...的...PE...建立...ebgp...，那么当...rr...把从他学来的路由传给其他...PE...时候，会...by defaul...改...nh...。这样就会让所有路由都引到...RR...上，并不是我们希望的...rr...只反射路由，真正路由不经过他，都直接到...remote site 7...。所以要...no-chang...，这样...nh...还是...S7...的...PE...的...lo0...，而这个路由在...inet3...里面可以解析...nh...。... ...这就是为什么我们要让所有...PE...的...lo0...互有。
============================
RR NH:
L2vpn rr:
lab@route-reflector# run show route receive-protocol bgp 172.17.47.3 extensive
bgp.l2vpn.0: 9 destinations, 9 routes (9 active, 0 holddown, 0 hidden)
*  172.17.47.3:500:7:1/96 (1 entry, 1 announced)
Accepted
Route Distinguisher: 172.17.47.3:500
Label-base: 262145, range: 8
Nexthop: 172.17.47.3...<<<<<<
AS path: 2831679853 I
lab@route-reflector# run show route advertising-protocol bgp 172.30.5.2 table bgp.l2vpn.0 extensive
*  172.17.47.3:500:7:1/96 (1 entry, 1 announced)
BGP group cluster-2 type Internal
Route Distinguisher: 172.17.47.3:500
Label-base: 262145, range: 8
Nexthop: Self
Flags: Nexthop Change...<<<<
Localpref: 100
lab@Sirius# run show route receive-protocol bgp 172.30.5.41 table bgp.l2vpn.0 extensive
*  172.17.47.3:500:7:1/96 (1 entry, 0 announced)
Import Accepted
Route Distinguisher: 172.17.47.3:500
Label-base: 262145, range: 8
Nexthop: 172.30.5.41...<<<
Localpref: 100
AS path: 2831679853 I
<<<nh changed
group AS4321 {                          ##RR portion of the config
multihop no-nexthop-change;... ...##Needed so... ...eBGP next-hop self does not occur
##will break VPLS in particular if next-hop self happens
From <https://junipernetworks.sharepoint.com/teams/ps/core-edge/Wiki/JQCE%20-%20Inter-AS%20Option%20C.aspx>
Ibgp rr:
lab@route-reflector# run show route receive-protocol bgp 172.30.5.3 41.22.0.0/15 extensive
inet.0: 668 destinations, 1053 routes (668 active, 0 holddown, 0 hidden)
* 41.22.0.0/15 (1 entry, 1 announced)
Accepted
Nexthop: 172.30.5.3
Localpref: 200
lab@route-reflector# run show route advertising-protocol bgp 172.30.5.2 41.22.0.0/15 extensive
inet.0: 668 destinations, 1053 routes (668 active, 0 holddown, 0 hidden)
* 41.22.0.0/15 (1 entry, 1 announced)
BGP group cluster-2 type Internal
Nexthop: 172.30.5.3...<<<
<<<nh not changed. Normal ibgp RR leave nh unchanged.
https://forums.juniper.net/t5/Routing/How-to-use-quot-next-hop-unchanged-quot-to-IBGP-router-reflector/td-p/23862
The default behavior of route-reflectors is to leave the next-hop unchanged.  The next-hop is only changed if you have an export policy which sets the next-hop.  If you do have an export policy that is doing this, and you are doing it because the reflector is also receiving EBGP routes which must have their next-hop changed, then you need to distinguish between IBGP and EBGP routes within your export policy so that only the EBGP routes change their next-hop.
Default behaviouir of Juniper is to not change the nexthop for iBGP peers, so normally yopu dont need to do anythign to get the "correct" behaviour. Same goes fpr eBGP routes, but when it passes in to your AS it makes a great deal of sense to let the eBGP router apply a nexthop self policy to keep that router as an exit for those particular routes.
From <https://forums.juniper.net/t5/Routing/How-to-use-quot-next-hop-unchanged-quot-to-IBGP-router-reflector/td-p/23862>
============================
lab@route-reflector# set protocols bgp group P3-remote-pe peer-as 23456 ？？？？...BGP active p661
<<<...这样配...bgp...起不来。。。
<<<...如果考试一旦在...rr...上建立...ebgp...，如果正常...4byte...的...as commit...不上，就用...23456
peer-as 43208.365;《《《这样配...ok
lab@route-reflector# run show bgp summary
Groups: 3 Peers: 9 Down peers: 0
Table          Tot Paths  Act Paths Suppressed    History Damp State    Pending
inet.0               986        601          0          0          0          0
inet6.0               48         32          0          0          0          0
bgp.l3vpn.0          101        101          0          0          0          0
bgp.l2vpn.0            9          9          0          0          0          0
inet.3                 1          1          0          0          0          0
Peer                     AS      InPkt     OutPkt    OutQ   Flaps Last Up/Dwn State|#Active/Received/Accepted/Damped...
172.17.47.3      2831679853          3         11       0       0          24 Establ
bgp.l2vpn.0: 1/1/1/0
4) Normalize RT:
现在...vpls site7 ...出现了，...bgp RR-S7...也...ok...了，但是从...S7...收来的路由不会被发给...vpls sites...，因为...RT...不一样。
lab@route-reflector# run show route receive-protocol bgp 172.17.47.3 extensive
bgp.l2vpn.0: 9 destinations, 9 routes (9 active, 0 holddown, 0 hidden)
*  172.17.47.3:500:7:1/96 (1 entry, 1 announced)
Accepted
Route Distinguisher: 172.17.47.3:500
Label-base: 262145, range: 8
Nexthop: 172.17.47.3
AS path: 2831679853 I
Communities: target:43208:500 Layer2-info: encaps:VPLS, control flags:, mtu: 0, site preference: 100
<<<<RR...从...P3-1...收到的...route...用的...target...和...C5 site PEs...用的不一样：
R3:
c5-vpls {
instance-type vpls;
interface ge-0/0/3.600;
interface ge-0/0/3.601;
vrf-target target:54591:501; ... ...《《《《... ...
protocols {
此处要更改，否则进出的...route...因为...target...不一致无法到达。
policy-statement c5-vpn-target-export {
term 1 {
from {
protocol bgp;
community ce5;
}
then {
community delete ce5;
community add ce5-remote;
accept;
}
}
}
policy-statement c5-vpn-target-import {
term 1 {
from {
protocol bgp;
community ce5-remote;
}
then {
community delete ce5-remote;
community add ce5;
accept;
}
}
lab@route-reflector# set protocols bgp group P3-remote-pe import c5-vpn-target-import
lab@route-reflector# set protocols bgp group P3-remote-pe export c5-vpn-target-export
----------------------
!!!...这里如果...community...用...set...，...vpls connection to s7...就建立不起来。
《community add
*  172.17.47.3:500:7:1/96 (1 entry, 0 announced)
Import Accepted
Route Distinguisher: 172.17.47.3:500
Label-base: 262145, range: 8
Nexthop: 172.17.47.3
Localpref: 100
AS path: 2831679853 I
Communities: target:54591:501 Layer2-info: encaps:VPLS, control flags:, mtu: 0, site preference: 100
《community  set
*  172.17.47.3:500:7:1/96 (1 entry, 0 announced)
Import Accepted
Route Distinguisher: 172.17.47.3:500
Label-base: 262145, range: 8, status-vector: 0x0
Nexthop: 172.17.47.3
Localpref: 100
AS path: 2831679853 I
Communities: target:54591:501
<<<一定... ...要注意...community...不仅是...RT...，还包含...vpn...的相关参数，如果用...set...那么其他参数都丢只有...RT. ...如果是单纯的...bgp...环境...set...是...ok...的。所以最好都用...add...。
-----------------------
Verify...：
R2...：
lab@R2# run show vpls connections
7                         rmt   Up     Feb 22 14:00:04 2018           1
Remote PE: 172.17.47.3, Negotiated control-word: No
Incoming label: 262151, Outgoing label: 262148
Local interface: lsi.1048582, Status: Up, Encapsulation: VPLS
Description: Intf - vpls c5-vpls local site 4 remote site 7
《《《...new site 7 ...出现了。
172.17.47.3:500:7:1/96 (1 entry, 0 announced)
*BGP    Preference: 170/-101
Route Distinguisher: 172.17.47.3:500
Next hop type: Indirect
Address: 0x934a218
Next-hop reference count: 5
Source: 172.30.5.41
Protocol next hop: 172.17.47.3
Indirect next hop: 2 no-forward
State: <Active Int Ext>
Local AS: 54591 Peer AS: 54591
Age: 6  Metric2: 1
Task: BGP_54591.172.30.5.41+59166
AS path: 2831679853 I
Communities: target:54591:501 Layer2-info: encaps:VPLS, control flags:, mtu: 0, site preference: 100
Import Accepted
Label-base: 262145, range: 8
Localpref: 100
Router ID: 172.30.5.41
Secondary Tables: c5-vpls.l2vpn.0
Indirect next hops: 1
Protocol next hop: 172.17.47.3 Metric: 10
Indirect next hop: 2 no-forward
Indirect path forwarding next hops: 1
Next hop type: Router
Next hop: 172.30.0.14 via ge-0/0/4.123
172.17.47.3/32 Originating RIB: inet.3
Metric: 10                      Node path count: 1
Indirect nexthops: 1
Protocol Nexthop: 172.30.5.3 Metric: 10 Push 338896
Indirect nexthop: 9554570 -
Indirect path forwarding nexthops: 1
Nexthop: 172.30.0.14 via ge-0/0/4.123
172.30.5.3/32 Originating RIB: inet.3
Metric: 10                              Node path count: 1
Forwarding nexthops: 1
Nexthop: 172.30.0.14 via ge-0/0/4.123
《《《...nh...在...inet3...里解决的。这就是为什么要...label-unicast rib inet3
《《《如果...indirect-nh...是... -0...这个就是...nh...无法解决
lab@R2# run show route table inet.3 172.17.47.3
inet.3: 13 destinations, 18 routes (9 active, 0 holddown, 7 hidden)
+ = Active Route, - = Last Active, * = Both
172.17.47.3/32     *[BGP/170] 00:38:13, localpref 100, from 172.30.5.41
AS path: 2831679853 I
> to 172.30.0.14 via ge-0/0/4.123, Push 313216
R3...：
lab@Canopus# run show route table inet.3 172.17.47.3 extensive
inet.3: 17 destinations, 23 routes (11 active, 0 holddown, 10 hidden)
172.17.47.3/32 (1 entry, 1 announced)
TSI:
Page 0 idx 1 Type 1 val 941f448
Flags: Nexthop Change
Nexthop: Self
Localpref: 200
AS path: [54591] 2831679853 I
Communities: 54591:43208
Path 172.17.47.3 from 192.168.0.6 Vector len 4.  Val: 1
*BGP    Preference: 170/-201
Next hop type: Router
Address: 0x9680b08
Next-hop reference count: 5
Source: 192.168.0.6
Next hop: 192.168.0.6 via ge-0/0/5.302, selected
State: <Active Ext>
Local AS: 54591 Peer AS: 2831679853
Age: 3:36:51
Task: BGP_2831679853.192.168.0.6+179
Announcement bits (3): 0-Resolve tree 4 1-Resolve tree 1 4-BGP_RT_Background
AS path: 2831679853 I
Communities: 54591:43208
Accepted
Route Label: 3
Localpref: 200
Router ID: 172.17.47.1
《《《...r3...从...p3...收进来的路由带着...label 3...说明告诉...r3...你是次末...hop...，也就是...s7...于...p3...是直连的。
lab@R3# run show route advertising-protocol bgp 172.30.5.41 table inet.3 172.17.47.3/32 extensive
inet.3: 17 destinations, 23 routes (11 active, 0 holddown, 10 hidden)
* 172.17.47.3/32 (1 entry, 1 announced)
BGP group ibgp type Internal
Route Label: 311216...  ...《《《《
Nexthop: Self
Flags: Nexthop Change
Localpref: 100
AS path: [54591] 2831679853 I
《《《...as path...是看...advertis...就是从左到右方向
[edit]
lab@R3# run show route table mpls.0 label 311216
mpls.0: 79 destinations, 79 routes (79 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both
311216             *[VPN/170] 00:25:22
> to 192.168.0.6 via ge-0/0/5.302, Pop
311216(S=0)        *[VPN/170] 00:25:22
> to 192.168.0.6 via ge-0/0/5.302, Pop
...<<<<...这个...label...会被通告给其他...PE...，让他们可以到达...s7
