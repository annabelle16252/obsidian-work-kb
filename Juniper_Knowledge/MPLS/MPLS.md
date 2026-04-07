# MPLS 

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

MPLS
只有inet3有mpls路由。而只有bgp可以看inet3，bgp会比较把inet0和inet3里面的路由谁优。
而因为...ldp/rsvp preference...更优，所以走...inet3...的...lsp...。
正因此，...PE-PE...之间会走...lsp...而不是...igp...。而...PE-PE...的...issue...要用...mpls ping...来...check...，因为他走...inet3.
# Commands #
show mpls lsp ingress
show mpls lsp transit
show ted database
RSVP
show rsvp interface
show rsvp neighbor
show rsvp session
show rsvp session ingress
show rsvp version
show route table mpls.0
show route table inet.3
show ldp neighbors
show ldp session
show ldp database [session peer]
By default, PHP-...倒数第二跳...pop...，label...=3...。如果改成ultimate... nh...，label...=0...，egree... pop...。
# Traceroute #
1. MPLS...下如果配了...no-propogate-ttl, traceroute...结果会把整个...mpls core hide...起来，只能看到一跳。
2. ...可以在...vrf...里打开...trace: vrf-propagate-ttl...，这样可以从...vrf...里...trace...看到每一跳。
3. ...可以打开...udp 3503,...作...mpls trace
--------------------
edit firewall family inet filter IPv4-PROTECT-RE
set term allow-udp-3503 from protocol udp
set term allow-udp-3503 from port 3503
set term allow-udp-3503 then accept
insert term allow-udp-3503 before term DISCARD-ALL-UNKNOWN
--------------------
> traceroute mpls ldp 101.234.58.51 ...<<< global lo0 address
# 路径选择 #
查路由表到到destination的路由，因为inet...0里的igp比inet3里的mpls路由preference大...，所以会优选inet...3里的rsvp/ldp...，... rsvp7...，ldp...9...，如果选了rsvp，之后就选lsp，lsp看走哪条path。
# mpls.0 route table #
包含所有...label...行为：
lab@R7# run show route table mpls.0
mpls.0: 51 destinations, 51 routes (51 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both
0                  *[MPLS/0] 02:49:23, metric 1
Receive
1                  *[MPLS/0] 02:49:23, metric 1
Receive
2                  *[MPLS/0] 02:49:23, metric 1
Receive
13                 *[MPLS/0] 02:49:23, metric 1
Receive
299776             *[LDP/9] 02:48:03, metric 1
> to 172.30.0.46 via ge-0/0/4.178, Swap 0...  <<< explicit-null
299776(S=0)        *[LDP/9] 02:48:03, metric 1
> to 172.30.0.46 via ge-0/0/4.178, Pop
300000             *[LDP/9] 02:46:30, metric 1
> to 172.30.0.46 via ge-0/0/4.178, label-switched-path Rigel-to-Sirius... <<<< LDP tunnel
to 172.30.0.17 via ge-0/0/4.127, label-switched-path Rigel-to-Sirius
299792             *[RSVP/7/1] 02:47:48, metric 1
> to 172.30.0.46 via ge-0/0/4.178, label-switched-path Sun-to-Procyon
to 172.30.0.17 via ge-0/0/4.127, label-switched-path Bypass->172.30.0.46
299792(S=0)        *[RSVP/7/1] 02:46:54, metric 1
> to 172.30.0.46 via ge-0/0/4.178, label-switched-path Sun-to-Procyon
to 172.30.0.17 via ge-0/0/4.127, label-switched-path Bypass->172.30.0.46
300224             *[VPN/170] 02:47:10
receive table c2-spoke.inet.0, Pop     <...<< forward to vrf
300240             *[VPN/170] 02:47:10
> to 192.168.0.90 via ge-0/0/5.323, Pop
# no-propogate-ttl #
TTL mpls发到re...，... 不带mpls直接ip包不发给re...。发到re后，re给个回包或者超时error
No-propogate-ttl: mpls header里的ttl放成ttl=255...，永远不会timeout除非loop
在core里走看mpls... header里的ttl...，出来才看payload里的ttl
# revert-timer #
https://www.juniper.net/documentation/us/en/software/junos/mpls/topics/ref/statement/revert-timer-edit-protocols-mpls.html
When the software switches from the primary to a secondary path, it continuously attempts to revert to the primary path, switching back to it when it is again reachable but no sooner than the time specified in the revert-timer statement.
Specify the amount of time (in seconds) that an LSP must wait before traffic reverts to a primary path. If during this time the primary path experiences any connectivity problem or stability problem, the timer is restarted.
PathTear
Sent to egress router
ResvTear
Sent to ingress router
PathErr
Sent to ingress router
ResvErr
Sent to egress router
ResvConf
Sent to ingress router
Tspec Object
The Tspec object is not unique to MPLS. In RSVP, the Tspec object carries all the link management information necessary to handle the resource reservations needed. Thus, the Tspec object is essential for MPLS traffic engineering because this is how resource requirements are communicated along the LSP set up with RSVP.
The Tspec allows RSVP to indicate requested bandwidth as well as the minimum and maximum LSP packet size to be used over the LSP. Routers can use this information to determine if they have the resources available to grant to the new LSP. If the resources are not available, the LSP is not set up.
Resource reservations are not service guarantees. In other words, a request for 100 Mbps of bandwidth for an LSP does not subtract 100 Mbps of bandwidth from a link and dedicate this bandwidt to the LSP. Bandwidth reservations are used to determine only if the LSP can be set up and do not imply any performancfe guarantees on the network. Put another way, RSVP is only the messenger, it does not provide any policing functions on the LSPs that it signals.
# Path Refresh Message #
Conventional RSVP uses path and reservation refresh messages to maintain the resource reservations on the LSP.
By default, JUNOS software sends RSVP refresh messages every 45 seconds. You can change this interval as needed; normally you increase the refresh interval when all nodes also support the hello extension because the hello protocols will detect path failures before the RSVP refresh mechanism does anyway. Path maintenance exchanges consist of path and reservation messages that essentially renew the LSP on a segment-by-segment basis.
In contrast to the path and reservation messages used to establish an LSP, refresh messages are exchanged between adjacent nodes only; end-to-end signaling is not necessary for path maintenance.
All interfaces use a single refresh timer, but the timers are independent in each router. You can change the refresh message interval with a set refresh-time seconds command issued at the [edit protocols rsvp] hierarchy. Note that refresh messages are sent based on the configured refresh time multiplied by 1.5. The result is that a default 30-second refresh timer yields refresh messages every 45 seconds.
Valid refresh values range from 1 through 65535.
The formula lifetime = (keep-multiplier + 0.5) x (1.5 x refresh time) is used to determine when an LSP’s state is no longer valid. You can change the keep-multiplier value from its default value of 3 with a set keep-multiplier value command entered at the [edit protocols rsvp] hierarchy.
the default keep-multiplier value of 3 combined with the default 30-second refresh-time results in path failure detection in approximately 157 seconds or 2.6 minutes:
(3 + 0.5) x (1.5 x 30) = 3.05 x 45 = 157.5
label...是...downstream  assign...给...upstream...的，之后...packet...会带着相应...label...从...upstream...到...downstream.
# Label 3 #
is a reserved label that tells the penultimate router to pop the MPLS header from the packet and send the resulting IP packet to the last hop.
Label 3 never appears in an MPLS header or in the mpls.0 table, hence the name implicit null. Put another way, the egress node has signaled a desire for penultimate hop popping (PHP) by virtue of its assigning label 3 to the penultimate LSR. PHP is the default behavior for JUNOS software as it helps to support label stacking and can reduce the processing burden of the egress router.
MPLS instance automatically installs labels 0, 1, and 2 into the mpls.0 switching table
0: pop, forwarding traffic upon  ipv4 route lookup
2:pop, ……. Upon ipv6 route lookup
3:php
Implicit null...：... default. Label 3. ...次末跳弹出
Explicit null: needs to be configured. Label 0. ...最后一跳弹出，保证每台设备都带着...label...知道最后一跳再弹出
======================================
Transit & Egress LSR...只需要...baseline configuration...：
user@r1# set protocols rsvp interface xxx
Egress LSR...除了...baseline configuration...，还有...LSP...具体的配置：
===============================================
Normally, LSPs are built to a router’s loopback address for maximum stability.
A signaled LSP requires configuration on the ingress router only. RSVP takes care of all the details associated with establishing LSP state in transit and egress nodes.
===============================================
Displaying RSVP Interface
user@r1> show rsvp interface
RSVP interface: 3 active
Active Subscr- Static      Available   Reserved    Highwater
Interface   State resv   iption  BW          BW          BW          mark
at-0/1/0.100Up         0   100%  100bps      100bps      0bps        0bps
ge-0/3/0.0  Up         0   100%  1000Mbps    1000Mbps    0bps        0bps
so-0/2/0.0  Up         0   100%  155.52Mbps  155.52Mbps  0bps        0bps
Displaying RSVP Neighbors
user@r1> show rsvp neighbor
RSVP neighbor: 1 learned
Address            Idle Up/Dn LastChange HelloInt HelloTx/Rx MsgRcvd
10.0.1.1              0  1/0          48        9     8/8    5
Displaying RSVP Statistics
user@geras-re0> show rsvp statistics
PacketType              Total                  Last 5 seconds
Sent      Received        Sent      Received
Path              2144          1772           0             0
PathErr             52            36           0             0
PathTear           443           280           0             0
Resv FF              0             0           0             0
Resv WF              0             0           0             0
Resv SE           1032           409           0             0
ResvErr              0             0           0             0
ResvTear             0            12           0             0
ResvConf             0             0           0             0
Ack               2360          1375           0             0
SRefresh        155970        152546           1             1
Hello           129134        126295           2             2
EndtoEnd RSVP        0             0           0             0
Errors                          Total            Last 5 seconds
Rcv pkt bad length                 0                         0
Rcv pkt unknown type               0                         0
Rcv pkt bad version                0                         0
Rcv pkt auth fail                  0                         0
Rcv pkt bad checksum               0                         0
Rcv pkt bad format                 0                         0
Memory allocation fail             0                         0
No path information                7                         0
Resv style conflict                0                         0
Port conflict                      0                         0
Resv no interface                  0                         0
PathErr to client                112                         0
ResvErr to client                  0                         0
Path timeout                      14                         0 <<<<
Resv timeout                      25                         0 <<<<
Message out-of-order               0                         0
Unknown ack msg                  168                         0
Recv nack                        334                         0
Recv duplicated msg-id             0                         0
No TE-link to recv Hop             0                         0
Rcv pkt disabled interface         0                         0
Transmit buffer full               0                         0
Transmit failure                   0                         0
Receive failure                    0                         0
P2MP RESV discarded by appl         0                        0
Rate limit                         0                         0
Err msg loop detected              0                         0
=========================================================
Displaying RSVP Session Status
One of the most useful RSVP-related operational mode commands is the show rsvp session command combined with the detail switch. By default, the command displays information about all RSVP sessions, whether they are ingress, egress, or transit sessions. You can add switches to display information for each type of session, as well as for LSPs that are up or down, as shown in this edited capture:
user@r1> show rsvp session ?
Possible completions:
<[Enter]>            Execute this command
bidirectional        RSVP sessions that are bidirectional
brief                Display brief version of information
bypass               Show RSVP sessions used for protecting other LSPs
detail               Display detailed version of information
down                 Label-switched paths that are in down state
egress               RSVP sessions that end at this router
ingress              RSVP sessions that originate from this router
interface            Show sessions on a particular interface
lsp                  Show RSVP sessions used to set up LSPs
name                 Show a particular RSVP session name
nolsp                Show RSVP sessions not used to set up LSPs
statistics           Display packet statistics
terse                Display terse version of information
transit              RSVP sessions that transit this router
unidirectional       RSVP sessions that are unidirectional
up                   Label-switched paths that are in up state
user@mason-re0# run show rsvp session detail
Ingress RSVP: 14 sessions
12.82.74.7
From: 12.82.74.5, LSPstate: Up, ActiveRoute: 0
LSPname: MASON-2B-PRIORITY, LSPpath: Primary
Suggested label received: -, Suggested label sent: -
Recovery label received: -, Recovery label sent: 0
Resv style: 1 SE, Label in: -, Label out: 0
Time left:    -, Since: Mon Nov  1 20:35:58 2010
Tspec: rate 1000bps size 1000bps peak Infbps m 20 M 1500
Port number: sender 12 receiver 17589 protocol 0
Link protection desired
Type: Link protected LSP
PATH rcvfrom: localclient
Adspec: sent MTU 1500
Path MTU: received 1500
PATH sentto: 12.82.75.13 (xe-0/0/0.0) 2 pkts
RESV rcvfrom: 12.82.75.13 (xe-0/0/0.0) 2 pkts
Explct route: 12.82.75.13
Record route: <self> 12.82.74.7 (node-id) 12.82.75.13
=========================================================
< Troubleshooting LSP Problems >
> show mpls lsp extensive  ...（件...pdf...）
display of primary/secondary status and the historical log of the LSP’s signaling activity.
CSPF-related problems often show up here with a no route to host error
(so RSVP-related commands show no sessions due to the CSPF path calculation failure)
就是...LSP ...问题先查...cspf...是否有问题，如果没有再查...rsvp...。因为...rsvp...是...cspf...算出来的，如果...cspf...有问题，...rsvp...建立不起来。
====================================================
Clearing LSPs
> clear rsvp session
> clear mpls lsp
*default...什么不加只...clear ingress lsp
>clear mpls lsp optimize
the LSP is only cleared if the CSPF algorithm can satisfy the stringent re-optimization rules that are designed to prevent LSP thrashing.
>clear mpls lsp optimize-aggressive
With aggressive reoptimization the LSP is cleared and reestablished if there is a metrically shorter path now available.
=======================================================
RSVP Tracing
Available tracing flags for RSVP include:
all                  Trace everything
error                Trace error conditions
event                Trace RSVP related events
lmp                  Trace RSVP-LMP related interactions
packets              Trace all RSVP packets
path                 Trace RSVP path messages
pathtear             Trace RSVP PathTear messages
resv                 Trace RSVP Resv messages
resvtear             Trace RSVP ResvTear messages
route                Trace routing information
state                Trace state transitions
[edit protocols rsvp]
user@r1# show
traceoptions {
file rsvp-trace;
flag error detail;
flag path detail;
flag resv detail;
flag pathtear detail;
flag resvtear detail;
}
Monitor the resulting rsvp-trace log file:
> monitor start log-file-name
> show log log-file-name commands
===============================================
cvg@LAB-MX240# run show mpls lsp ingress logical-system pe-1 extensive
Ingress LSP: 1 sessions
101.1.1.4
From: 101.1.1.1, State: Up, ActiveRoute: 0, LSPname: pe-1_to_pe-4
ActivePath: to_pe-4_2nd (secondary)
Link protection desired
LSPtype: Static Configured, Penultimate hop popping
LoadBalance: Random
Encoding type: Packet, Switching type: Packet, GPID: IPv4
Revert timer: 300
Time remaining before reverting: 175
Primary   to_pe-4_pri      State: Up
Priorities: 7 0
OptimizeTimer: 10
SmartOptimizeTimer: 180
Reoptimization in 3 second(s).
Computed ERO (S [L] denotes strict [loose] hops): (CSPF metric: 3)
1.1.1.2 S 1.1.1.6 S 1.1.1.10 S
Received RRO (ProtectionFlag 1=Available 2=InUse 4=B/W 8=Node 10=SoftPreempt 20=Node-ID):
101.1.1.2(flag=0x21) 1.1.1.2(flag=1 Label=300432) 101.1.1.3(flag=0x21) 1.1.1.6(flag=1 Label=300416) 101.1.1.4(flag=0x20) 1.1.1.10(Label=3)
49 Jun  7 02:14:20.460 CSPF failed: no route toward 1.1.1.6
48 Jun  7 02:14:10.677 Record Route:  101.1.1.2(flag=0x21) 1.1.1.2(flag=1 Label=300432) 101.1.1.3(flag=0x21) 1.1.1.6(flag=1 Label=300416) 101.1.1.4(flag=0x20) 1.1.1.10(Label=3)
47 Jun  7 02:13:41.789 CSPF failed: no route toward 1.1.1.6
46 Jun  7 02:13:32.341 Record Route:  101.1.1.2(flag=0x23) 1.1.1.2(flag=3 Label=300432) 70.1.1.2(Label=300416) 101.1.1.4(flag=0x20) 1.1.1.10(Label=3)
45 Jun  7 02:13:29.612 CSPF failed: no route toward 1.1.1.6
44 Jun  7 02:13:29.612 CSPF: link down/deleted: 0.0.0.0(1.1.1.6:0)(1.1.1.6)->0.0.0.0(101.1.1.3:0)(101.1.1.3)
43 Jun  7 02:13:29.246 Link-protection Up
42 Jun  7 02:13:29.245 Deselected as active
41 Jun  7 02:13:29.245 Link-protection Down
40 Jun  7 02:13:29.244 CSPF failed: no route toward 1.1.1.6
39 Jun  7 02:13:29.244 CSPF: link down/deleted: 1.1.1.5(101.1.1.2:0)(101.1.1.2)->0.0.0.0(1.1.1.6:0)(1.1.1.6)
=================================================================
smeng@m10i-b-re1# run show ted database logical-system R1
TED database: 0 ISIS nodes 5 INET nodes
ID                            Type Age(s) LnkIn LnkOut Protocol
1.1.1.1                       Rtr    1056     1      1 OSPF(0.0.0.0)
To: 13.1.1.3-1, Local: 13.1.1.1, Remote: 0.0.0.0
Local interface index: 0, Remote interface index: 0
ID                            Type Age(s) LnkIn LnkOut Protocol
2.2.2.2                       Rtr    1021     1      1 OSPF(0.0.0.0)
To: 23.1.1.3-1, Local: 23.1.1.2, Remote: 0.0.0.0
Local interface index: 0, Remote interface index: 0
ID                            Type Age(s) LnkIn LnkOut Protocol
13.1.1.3-1                    Net    1587     2      2 OSPF(0.0.0.0)
To: 1.1.1.1, Local: 0.0.0.0, Remote: 0.0.0.0
Local interface index: 0, Remote interface index: 0
To: 3.3.3.3, Local: 0.0.0.0, Remote: 0.0.0.0
Local interface index: 0, Remote interface index: 0
ID                            Type Age(s) LnkIn LnkOut Protocol
23.1.1.3-1                    Net    1582     2      2 OSPF(0.0.0.0)
To: 3.3.3.3, Local: 0.0.0.0, Remote: 0.0.0.0
Local interface index: 0, Remote interface index: 0
To: 2.2.2.2, Local: 0.0.0.0, Remote: 0.0.0.0
Local interface index: 0, Remote interface index: 0
ID                            Type Age(s) LnkIn LnkOut Protocol
3.3.3.3                       Rtr    1582     2      2 OSPF(0.0.0.0)
To: 13.1.1.3-1, Local: 13.1.1.3, Remote: 0.0.0.0
Local interface index: 0, Remote interface index: 0
To: 23.1.1.3-1, Local: 23.1.1.3, Remote: 0.0.0.0
Local interface index: 0, Remote interface index: 0
==================================================
<RSVP Lab>  CONFIG.: anna104rsvpBYPASS
同一条...LSP...，有...primary path ...和...secondary path...，... primary path...有一条...bypass...（...link protection...）。
当...ge-1/3/0.20 disable...后，...primary path ...虽然走...bypass...路径，但是...LSP ...的...primary path...不会变。
lab@PE1# run show rsvp session brief
Ingress RSVP: 2 sessions
To              From            State   Rt Style Labelin Labelout LSPname
192.168.1.2     192.168.1.1     Up       0  1 SE       -   299840 pe1-to-pe2-1 ...《《《《
192.168.1.2     192.168.1.1     Up       0  1 SE       -   299808 pe1-to-pe2-1 ...《《《《《 两条...session...都...up
lab@PE1# run show mpls lsp brief
Ingress LSP: 1 sessions
To              From            State Rt P     ActivePath       LSPname
192.168.1.2     192.168.1.1     Up     0 *     strict-first-hop pe1-to-pe2-1 ...《《《《... primary path ...没有变
lab@PE1# run show rsvp session extensive
192.168.1.2
From: 192.168.1.1, LSPstate: Up, ActiveRoute: 0
LSPname: pe1-to-pe2-1, LSPpath: Primary
LSPtype: Static Configured
Suggested label received: -, Suggested label sent: -
Recovery label received: -, Recovery label sent: 299840
Resv style: 1 SE, Label in: -, Label out: 299840
Time left:    -, Since: Tue Jul 12 16:39:34 2016
Tspec: rate 0bps size 0bps peak Infbps m 20 M 1500
Port number: sender 4 receiver 5168 protocol 0
Link protection desired
Type: Protection down
1 Jul 12 17:21:44 Link protection disabled on outbound interface[59 times]
PATH rcvfrom: localclient
Adspec: sent MTU 1500
Path MTU: received 1500
PATH sentto: 172.22.211.2 (ge-1/3/0.211) 59 pkts
RESV rcvfrom: 172.22.211.2 (ge-1/3/0.211) 60 pkts, Entropy label: Yes
Explct route: 172.22.211.2 192.168.5.1 192.168.5.2 192.168.5.3
Record route: <self> 192.168.5.4 (node-id) 172.22.211.2 192.168.5.1 (node-id) 172.22.202.1 192.168.5.2 (node-id) 172.22.201.2 192.168.5.3 (node-id) 172.22.206.2
192.168.1.2 (node-id) 172.22.212.1  ...《《《《《走的是...primary path ...（经由...bypass...）
192.168.1.2
From: 192.168.1.1, LSPstate: Up, ActiveRoute: 0
LSPname: pe1-to-pe2-1, LSPpath: Secondary
LSPtype: Static Configured
Suggested label received: -, Suggested label sent: -
Recovery label received: -, Recovery label sent: 299808
Resv style: 1 SE, Label in: -, Label out: 299808
Time left:    -, Since: Tue Jul 12 15:08:47 2016
Tspec: rate 0bps size 0bps peak Infbps m 20 M 1500
Port number: sender 3 receiver 5169 protocol 0
Link protection desired
Type: Protection down
1 Jul 12 17:22:02 Link protection disabled on outbound interface[200 times]
PATH rcvfrom: localclient
Adspec: sent MTU 1500
Path MTU: received 1500
PATH sentto: 172.22.211.2 (ge-1/3/0.211) 200 pkts
RESV rcvfrom: 172.22.211.2 (ge-1/3/0.211) 181 pkts, Entropy label: Yes
Explct route: 172.22.211.2
Record route: <self> 192.168.5.4 (node-id) 172.22.211.2 192.168.5.5 (node-id) 172.22.203.2 192.168.5.6 (node-id) 172.22.204.2 192.168.1.2 (node-id) 172.22.213.1
Total 2 displayed, Up 2, Down 0
lab@PE1# run show mpls lsp extensive
Ingress LSP: 1 sessions
192.168.1.2
From: 192.168.1.1, State: Up, ActiveRoute: 0, LSPname: pe1-to-pe2-1
ActivePath: strict-first-hop (primary) ...《《《《《《《《《《《《《《《
Link protection desired
LSPtype: Static Configured, Penultimate hop popping
LoadBalance: Random
Encoding type: Packet, Switching type: Packet, GPID: IPv4
*Primary   strict-first-hop State: Up
Priorities: 7 0
SmartOptimizeTimer: 180
Received RRO (ProtectionFlag 1=Available 2=InUse 4=B/W 8=Node 10=SoftPreempt 20=Node-ID):
192.168.5.4(flag=0x20) 172.22.211.2(Label=299840) 192.168.5.1(flag=0x20) 172.22.202.1(Label=299840) 192.168.5.2(flag=0x20) 172.22.201.2(Label=299808) 192.168.5.3(flag=0x20) 172.22.206.2(Label=299792) 192.168.1.2(flag=0x20) 172.22.212.1(Label=3)
38 Jul 12 16:40:35.224 Selected as active path: due to 'primary'
37 Jul 12 16:39:34.325 Record Route:  192.168.5.4(flag=0x20) 172.22.211.2(Label=299840) 192.168.5.1(flag=0x20) 172.22.202.1(Label=299840) 192.168.5.2(flag=0x20) 172.22.201.2(Label=299808) 192.168.5.3(flag=0x20) 172.22.206.2(Label=299792) 192.168.1.2(flag=0x20) 172.22.212.1(Label=3)
36 Jul 12 16:39:34.325 Up
35 Jul 12 16:39:34.186 Originate Call
34 Jul 12 16:39:34.185 Clear Call
33 Jul 12 16:39:10.765 Tunnel local repaired[10 times]
32 Jul 12 16:34:32.445 Record Route:  172.22.211.2 192.168.5.5(flag=0x20) 172.22.203.2(Label=299840) 192.168.5.6(flag=0x20) 172.22.204.2(Label=299856) 192.168.1.2(flag=0x20) 172.22.213.1(Label=3)
31 Jul 12 16:34:32.445 Record Route:  172.22.211.2 192.168.5.5(flag=0x20) 172.22.203.2(Label=299840) 192.168.5.6(flag=0x20) 172.22.204.2(Label=299856) 192.168.1.2(flag=0x20) 172.22.213.1(Label=3)
30 Jul 12 16:34:32.343 Deselected as active
29 Jul 12 16:34:32.342 172.22.210.1: Tunnel local repaired
28 Jul 12 16:34:32.342 172.22.210.1: Down
27 Jul 12 16:30:38.352 Link-protection Up
26 Jul 12 16:29:03.896 Selected as active path: due to 'primary'
25 Jul 12 16:28:02.244 Record Route:  192.168.5.1(flag=0x20) 172.22.210.2(Label=299824) 192.168.5.4(flag=0x20) 172.22.202.2(Label=299824) 192.168.5.5(flag=0x20) 172.22.203.2(Label=299840) 192.168.5.6(flag=0x20) 172.22.204.2(Label=299856) 192.168.1.2(flag=0x20) 172.22.213.1(Label=3)
24 Jul 12 16:28:02.244 Up
23 Jul 12 16:28:02.243 Stats related identifier changed
22 Jul 12 16:27:17.159 172.22.210.2: Explicit Route: bad loose route[31 times]
21 Jul 12 16:06:12.089 Session preempted
20 Jul 12 16:06:12.089 172.22.210.1: Down
19 Jul 12 16:06:11.991 Deselected as active
18 Jul 12 16:06:11.988 Record Route:  192.168.5.1(flag=0x20) 172.22.210.2(Label=299808) 192.168.5.2(flag=0x20) 172.22.201.2(Label=299792) 192.168.5.5(flag=0x20) 172.22.205.2(Label=299792) 192.168.5.6(flag=0x20) 172.22.204.2(Label=299808) 192.168.1.2(flag=0x20) 172.22.213.1(Label=3)
17 Jul 12 16:06:11.988 Up
16 Jul 12 16:06:11.988 Stats related identifier changed
15 Jul 12 16:06:11.987 ResvTear received
14 Jul 12 16:06:11.987 172.22.210.1: Down
13 Jul 12 16:06:11.986 172.22.210.2: Session preempted
12 Jul 12 15:19:09.771 Selected as active path: due to 'primary'
11 Jul 12 15:18:08.383 Record Route:  192.168.5.1(flag=0x20) 172.22.210.2(Label=299792) 192.168.5.2(flag=0x20) 172.22.201.2(Label=299792) 192.168.5.5(flag=0x20) 172.22.205.2(Label=299792) 192.168.5.6(flag=0x20) 172.22.204.2(Label=299808) 192.168.1.2(flag=0x20) 172.22.213.1(Label=3)
10 Jul 12 15:18:08.383 Up
9 Jul 12 15:18:08.383 Stats related identifier changed
8 Jul 12 15:17:18.486 172.22.211.2: Explicit Route: wrong delivery[4 times]
7 Jul 12 15:16:21.515 Deselected as active
6 Jul 12 15:16:21.514 No Route toward dest
5 Jul 12 15:16:21.513 172.22.210.1: Down
4 Jul 12 15:02:59.485 Selected as active path
3 Jul 12 15:02:59.484 Record Route:  192.168.5.1(flag=0x20) 172.22.210.2(Label=299792) 192.168.5.2(flag=0x20) 172.22.201.2(Label=299792) 192.168.5.5(flag=0x20) 172.22.205.2(Label=299792) 192.168.5.6(flag=0x20) 172.22.204.2(Label=299808) 192.168.1.2(flag=0x20) 172.22.213.1(Label=3)
2 Jul 12 15:02:59.484 Up
1 Jul 12 15:02:59.272 Originate Call
Standby   any-path         State: Up
Priorities: 7 0
SmartOptimizeTimer: 180
Received RRO (ProtectionFlag 1=Available 2=InUse 4=B/W 8=Node 10=SoftPreempt 20=Node-ID):
192.168.5.4(flag=0x20) 172.22.211.2(Label=299808) 192.168.5.5(flag=0x20) 172.22.203.2(Label=299824) 192.168.5.6(flag=0x20) 172.22.204.2(Label=299840) 192.168.1.2(flag=0x20) 172.22.213.1(Label=3)
13 Jul 12 16:40:35.224 Deselected as active: due to 'primary'
12 Jul 12 16:34:32.344 Selected as active path
11 Jul 12 16:29:03.896 Deselected as active: due to 'primary'
10 Jul 12 16:06:11.991 Selected as active path
9 Jul 12 15:19:09.771 Deselected as active: due to 'primary'
8 Jul 12 15:16:21.521 Selected as active path
7 Jul 12 15:08:48.178 Record Route:  192.168.5.4(flag=0x20) 172.22.211.2(Label=299808) 192.168.5.5(flag=0x20) 172.22.203.2(Label=299824) 192.168.5.6(flag=0x20) 172.22.204.2(Label=299840) 192.168.1.2(flag=0x20) 172.22.213.1(Label=3)
6 Jul 12 15:08:48.178 Up
5 Jul 12 15:08:47.978 Originate Call
4 Jul 12 15:08:47.971 Clear Call
3 Jul 12 15:03:28.197 Record Route:  192.168.5.4(flag=0x20) 172.22.211.2(Label=299792) 192.168.5.5(flag=0x20) 172.22.203.2(Label=299808) 192.168.5.6(flag=0x20) 172.22.204.2(Label=299824) 192.168.1.2(flag=0x20) 172.22.213.1(Label=3)
2 Jul 12 15:03:28.197 Up
1 Jul 12 15:03:28.004 Originate Call
Created: Tue Jul 12 15:02:58 2016
Total 1 displayed, Up 1, Down 0
lab@PE1# run show mpls lsp bypass
Ingress LSP: 1 sessions
To              From            State   Rt Style Labelin Labelout LSPname
192.168.5.1     192.168.1.1     NotInService  0 0  -       -        - anna ...《《《《《《... bypass
Bypass...如果有...TE...是不用配路径的，...ted...自动算。如果...no-cspf...的话，需要手动配
[edit protocols rsvp]
lab@PE1# show
traceoptions {
file rsvp-lab-new;
flag all detail;
}
interface ge-1/3/0.210 {
link-protection {
bypass anna {    ...《《《《《《《《《《《《《《《《《
to 192.168.5.1;
bandwidth 1k;
no-cspf;
path {
172.22.211.2 strict;
}
}
}
}
Bypass lsp...当他保护的...lsp up...的时候，这条...bypass ...是...hidden...的
[edit interfaces ge-1/3/0 unit 210]
lab@PE1# run show route 192.168.5.1 hidden     <<<<<<<<
inet.0: 27 destinations, 29 routes (27 active, 0 holddown, 1 hidden)
+ = Active Route, - = Last Active, * = Both
192.168.5.1/32      [RSVP] 00:01:00, metric 1
> to 172.22.211.2 via ge-1/3/0.211, label-switched-path anna ...《《《《《《《
lab@PE1# run show rsvp session extensive
Ingress RSVP: 4 sessions
172.22.211.2
From: 192.168.1.1, LSPstate: Up, ActiveRoute: 0
LSPname: anna
LSPtype: Static Configured
Suggested label received: -, Suggested label sent: -
Recovery label received: -, Recovery label sent: 3
Resv style: 1 SE, Label in: -, Label out: 3
Time left:    -, Since: Wed Jul 13 08:47:25 2016
Tspec: rate 1000bps size 1000bps peak Infbps m 20 M 1500
Port number: sender 1 receiver 5172 protocol 0
Type: Bypass LSP
Number of data route tunnel through: 1
Number of RSVP session tunnel through: 1   ...《《《《《《 有...1...个...rsvp session...走这条...bypass
ActiveResv 0, PreemptionCnt 0, Update threshold 0%
Subscription 100%,
bc0 = ct0, StaticBW 1000bps
ct0: StaticBW 1000bps, AvailableBW 1000bps
MaxAvailableBW 1000bps = (bc0*subscription)
ReservedBW [0] 0bps[1] 0bps[2] 0bps[3] 0bps[4] 0bps[5] 0bps[6] 0bps[7] 0bps
PATH rcvfrom: localclient
Adspec: sent MTU 1500
Path MTU: received 1500
PATH sentto: 172.22.211.2 (ge-1/3/0.211) 18 pkts
RESV rcvfrom: 172.22.211.2 (ge-1/3/0.211) 18 pkts, Entropy label: Yes
Explct route: 172.22.211.2
Record route: <self> 172.22.211.2
lab@PE1# run show route table inet.0 protocol rsvp active-path   ...《《《《《《《
inet.0: 27 destinations, 29 routes (27 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both
192.168.5.1/32     *[RSVP/7/1] 00:23:42, metric 1
> to 172.22.211.2 via ge-1/3/0.211, label-switched-path anna   ...《《《《《《《
======================================================
ERO
http://www.dummies.com/how-to/content/how-to-configure-an-explicit-route-using-junos.html
躲都躲不过的...node
======================================================
rsvp+ldp
rsvp...在底层选路，然后...ldp...在...rsvp...上面建立...tunnelling
ldp...对标签的分发控制比...rsvp...要简单，排错也容易，配置也容易
rsvp...原来不是用来发表签的，就是用来资源预留的
rsvp+ldp...控制...pe-pe...走那条路。
=========================
L2circuit
Config.:
https://kb.juniper.net/InfoCenter/index?page=content&id=KB22700
控制...ce-ce...走那条路。
=====================
VPLS
http://www.juniper.net/documentation/en_US/junos13.2/topics/example/layer-2-vlans-multiple-for-one-vpls-instance-example-mx-solutions.html
一个...ce...可以对多个...pe, pe-pe...就像一个大的...switch
VPLS:
<BGP Singling>- auto discovery
-VLAN ID must be same for each vpls.(ce-ce must have same vlan id)
-each vpls use 2 tables: VRF & MAC.
-SINGLE VPLS NLRI for each VPLS.
-Automatic label calculation
< LDP>
-Full mesh LDP is required for each VPLS
<LSI interface>
3...层需要手动建立这个口。只要在...routing-instance...下面配上...vrf-table-labe...，就会产生这个...lsi...口了
mpbgp...将包扔进...lsi...口，...lsi...进行...2...次查找，再把包扔到...ce-facing interface.
=========================
2...层，...lsi...口自动产生。
=================================
只要是...PE...和...PE...跨域，就是用...option ABC
Book MPLS 3-3 P19-6
A: pe-pe  ebgp
B:              mpebgp
C:             RR, CE-PE ebgp
========================================================
vrf-table-lable:  mpls laber | vpn label |ip header…….
使用场景：
1. vrf...里有...filter...，要对...ip header...进行二次查找。
2....当多个...ce...通过一个...switch...连到...pe...上时，...packet...是去往哪个...ce...，这个包需要二次查找，因为需要知道相应的...mac...。
当...packet...到了...pe...，去掉...vpn lable...时知道对应的...vrf...是哪个。
如果有...vt interface...就可以二次查，没有的话需要加上...vrf-table-label...时，会自动建一个...lsi int....，这个...lsi...与...vrf...一一对应，这样在...packet...被...forward...到相应...vrf...前，先转到...lsi...上进行...ip lookup...来知道这个...packet...应该通过这个...vrf...的哪条路由去往那个...ce...。
All routes in a VRF routing instance configured with this option are advertised with one label allocated per VRF.
很好的解释
http://forums.juniper.net/t5/Routing/vrf-table-label-why-it-s-for/td-p/82736
Limitation on ce interaface
http://www.juniper.net/techpubs/en_US/junos10.2/topics/usage-guidelines/vpns-filtering-packets-in-layer-3-vpns-based-on-ip-headers.html
https://www.juniper.net/documentation/en_US/junos/topics/reference/configuration-statement/vrf-table-label-edit-routing-instances-vp.html
