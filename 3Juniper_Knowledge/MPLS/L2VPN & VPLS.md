# L2VPN & VPLS

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

L2VPN & VPLS
# L2VPN
Kompella VPN (BGP):bgp作信令，通告的是NLRI，可以auto-provision
Mertini VPN (LDP):target-ldp作信令，通告的是FEC - L2Cirucit
# BGP-L2VPN
-控制层面：通过NLRI里的RT找到remote PE
-转发层面：通过NLRI里的信息，算出label到达remote PE
-site id：
两种方式。1.在interface下分别手写site id  2.把所有interface写在site 下，site id按顺序分
-site封装： vlan-ccc
-auto-provision，比如PE1-CE1我写入interface.521 513 514,那现在远端PE2接入的CE2可能只有512 513.
但过后当CE2需要加514的时候，CE1-PE1不用作任何动作，CE2加上514，L2VPN就ok了。
-label计算：
outgoing: remote base + local site-id  -  remote offset
incoming: local base + remote site-id  - local offset
# LDP-L2VPN
-只传virtual cirtuit，不需要site id，RT RD。。。
-lo0.0也需要开ldp，这时是为了target-ldp
# BGP VPLS
-相当于core里是一个swithc，一个vpls上可以跑若干个CE。但L2vpn是一个上面只能跑一对CE。
vpls不需要在instance里指interface，因为会自动学所有interface的MAC
- 和LDP VPLS比的优势：auto-discovery , scalability, can use RR, can inter-AS
- PE上有2个表：
vrf table - routing table
mac table - forwarding table,包含locally learned MAC和remote学到的
-如果没有vt口，就用no-tunnel-service，就会为每个remote site建立一个lsi口来进行二次查找。
- BGP/LDP VPLS都要解决multihome的loop问题
===================================
layer 2 VPN分类：
-CCC
-L2VPN
LDP (l2 circuit)... Martini:manual-provision. Use LDP cannot use optionABC (RFC4447)
BGP (l2vpn)... Kompella:
-auto-provision PE when a new site is added to the PE;use MP-BGP.
- can use optionABC ...（...RFC NONE...）
-l2-vpn-signaling
-VPLS (PMP)
LDP... RFC4762
...BGP RFC4761
-EVPN
***All use Martini encapulation RFC4448 ...《《《...data plane
===================================
juniperbradley@igw21.kmcs-re0> ping mpls l2vpn interface xe-1/0/0.2004
Jun 23 10:20:52
!!!!^C
--- lsping statistics ---
4 packets transmitted, 4 packets received, 0% packet loss
===========================
LDP (l2 circuit)
-...不用配...RD
-...LDP信令分类“：
Direct LDP:mpls path用，物理接口上
Target ...LDP:lo0.0上用来传FEC,就是l2vpn里面的mp-ibgp
Troubleshooting...：（...PDF...）
Recive label:
Local base + remote site - local offset
Send Label:
Remote base + local site -remote offset
BGP (l2vpn)
-Family: l2vpn signaling & inet unicast
FEC 129 BGP Auto-discovering
-SAII
-BGP extended communities:
L2vpn-id(AGI):
RT:
-routes installed into instance.l2vpn.o table
Local generated BGP autodiscovery routes:
Pseudowire routes from remote PE pass the check:
-Then LDP set up targeted session to remote PE and signals FPC 129 pseudowire label.
-family l2vpn auto-discovery-only
-lo0 must enable LDP
VPLS
-BGP Based:
Use NLRI
-A VT interfaceis created for each remote site.
Traffic from core are passed through VT interface so that they can be forwarded based on MAC add.
-LDP Based:
Also has bgp auto-discovering, FEC129
-VPLS pacekt flow
Vpn-name.vpls==> this is MAC table (FIB)
-BUM ...（...bgp mpls only...）
# JNCIS - L2 VPN Lab #
Kompella VPN (BGP)  ...层...2vpn...，...bgp...通告的是...NLRI
Mertini VPN (LDP):...为每个客户新建一个唯一的连接使用...VPN...，... ...层...2...电路...,...通告的是互换...FEC
VCT:VPN connection table
Layer 2 information community:
层二...VPN...需要...PE...和...CE...路由器物理和逻辑接口需要相同的封装。
Kompella VPN (BGP)
**L3VPN instance typ...是...vrf...，...L2...是...l2vpn/l2circuit
Frame Relay:
