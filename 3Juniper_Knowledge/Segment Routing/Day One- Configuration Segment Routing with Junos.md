# Day One- Configuration Segment Routing with Junos

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

Day One- Configuration Segment Routing with Junos
？...C4 CE3 ping CE6 ok...，... traceroute core...里...label...行为不显示：
labroot@jtac-mx480-r2605-re0# run traceroute 69.69.69.2 routing-instance ce3-r2
traceroute to 69.69.69.2 (69.69.69.2), 30 hops max, 52 byte packets
1  33.33.33.1 (33.33.33.1)  0.630 ms  0.585 ms  0.567 ms
2  * * *
3  * * *
4  * * *
5  * * *
6  10.0.0.21 (10.0.0.21)  1.264 ms  0.967 ms  0.953 ms
MPLS Label=29 CoS=0 TTL=1 S=1
7  nj-69-69-69-2.sta.embarqhsd.net (69.69.69.2)  1.102 ms  1.214 ms  1.102 ms
======================================
# Day One- Segment Routing with Junos #
<<<R4,R7:
set resolution rib bgp.l3vpn.0 resolution-ribs inet.0 <<< rr...传...l3vpn...需要加这个
C2 Adjacency Segments
The minimum configuration to invoke Adjacency Segments:
lab@R2# set protocols isis source-packet-routing
<<< on all routers
This triggers each router to dynamically allocate two labels for each adjacency, one for IPv4 and one for IPv6. In order to check this, use the following command:
on R2:
lab@R2> show isis adjacency extensive
R1
Interface: ge-0/0/0.0, Level: 2, State: Up, Expires in 26 secs
Priority: 0, Up/Down transitions: 1, Last transition: 15:13:56 ago
Circuit type: 2, Speaks: IP, IPv6
Topologies: Unicast
Restart capable: Yes, Adjacency advertisement: Advertise
IP addresses: 10.0.0.0
IPv6 addresses: fe80::5668:a3ff:fe17:3281
Level 2 IPv4 Adj-SID: 30
Level 2 IPv6 Adj-SID: 47
State: Up
R5
Interface: ge-0/0/1.0, Level: 2, State: Up, Expires in 21 secs
Priority: 0, Up/Down transitions: 1, Last transition: 15:13:43 ago
Circuit type: 2, Speaks: IP, IPv6
Topologies: Unicast
Restart capable: Yes, Adjacency advertisement: Advertise
IP addresses: 10.0.0.5
IPv6 addresses: fe80::5668:a3ff:fe17:312a
Level 2 IPv4 Adj-SID: 29 <<< label value for R2-R5 adjance
Level 2 IPv6 Adj-SID: 48
State: Up
For example, when a data-plane packet... ...arrives at R2 with top label 29, the label is popped and the packet is forwarded on
ge-0/0/1 (R2’s interface facing R5)：
lab@R2> show route table mpls.0
<snip>
29 *[L-ISIS/14] 15:13:44, metric 0
> to 10.0.0.5 via ge-0/0/1.0, Pop  <<<
29(S=0) *[L-ISIS/14] 00:05:01, metric 0
> to 10.0.0.5 via ge-0/0/1.0, Pop
30 *[L-ISIS/14] 15:13:57, metric 0
> to 10.0.0.0 via ge-0/0/0.0, Pop
30(S=0) *[L-ISIS/14] 00:05:01, metric 0
> to 10.0.0.0 via ge-0/0/0.0, Pop
R2 --> R5...：
MPLS...里：分...label...是下游给上游分...(r5...分给...r2)...，而数据包要带着...label...从上游到下游是数据层面的...(...包带着这个...label...从...r2...到...r5)...。
SR...里：...router...自己给自身连着的...link...分...label...，... ...当数据包带着这个...label...到达本台...router...，...pop...这个...label...将包转发到这个...label...代表的那个...link...的对端...router...。
<<<...一定是数据包才能带着...label...走，..."> show isis adjacency extensive"...可以看到...SR...分...label...的行为，而...mpls.0...是一个转发表，在这里可以看到...local router...对...label...的处理行为，也就是数据层面的处理。
之前需要...igp...外，再加一个...mpls...协议来分...label...。...SR...只需要...igp...，...label...带在...igp...里面。
Unprotected and Protected Adjacency Segments
By default, an adjacency segment can benefit from fast protection mechanisms, if... TI-LFA... are configured.
In some cases, you may have some traffic that you want to benefit... ...from fast protection, but other traffic does not need such protection：
On interface between r1-r4:
lab@R1...# set protocols isis interface ge-0/0/1.0 link-protection ... <<< this is a must
lab@R1# set protocols isis interface ge-0/0/1.0 level 2 ipv4-adjacency-segment protected dynamic
lab@R1# set protocols isis interface ge-0/0/1.0 level 2 ipv4-adjacency-segment unprotected dynamic
lab@R1# set protocols isis interface ge-0/0/1.0 level 2 ipv6-adjacency-segment protected dynamic
lab@R1# set protocols isis interface ge-0/0/1.0 level 2 ipv6-adjacency-segment unprotected dynamic
The ISIS extensions for segment routing include an Adj-SID sub-TLV to enable a... ...router to advertise its Adj-SIDs throughout the IGP area. In this way all routers... ...from R1 to R9 in the topology are aware of all the Adj-SIDs on all of the links in... ...the topology. To illustrate this, look at the ISIS database. You can do this on any of... ...the routers, as they all have identical ISIS databases. Here is a snippet, showing the... ...Adj-SIDs generated and advertised by R1.
The F-flag is the Address Family flag. As you can see, it is set for IPv6 Adj-SIDs and clear for IPv4 Adj-SIDs.
The B-flag is the Backup flag. It is set if the SID is eligible for protection, and clear if it is not. You can see that it is not set for the unprotected label values (73 and 103).
The V-flag is set if the Adj-SID carries a Value, which is the case in all the examples above.
The L-flag is set if the label has Local significance, which is also the case in all the examples above.
The final two flags are not set in any of the examples above - later you will see an example in which they are set.
Mapping the Topology
The key point here... ...is that a given node allocates a different label for each of its adjacencies but the... ...same value may get allocated by different nodes. This is perfectly fine, as the labels... ...only have local significance. For example, in Figure 2.1, label 16 happened to get... ...allocated by both R3 and R5.
NOTE If you’re thinking that mapping out the labels is laborious, Juniper’s... ...NorthStar Controller does it automatically, by virtue of being part of the control... ...plane of the network.
Suppose R2 wants to send traffic to R8, not via the shortest IGP path (via R5 and... ...R7) but along the path R2-R1-R3-R6-R8. It achieves this by pushing the corresponding... ...stack of adjacency labels onto the packet, as shown in Figure 2.1. Note... ...that in the label stack, there is no label corresponding to the R2-R1 hop, as R2... ...simply pushes the packet towards R1. R1 sees that the top label is 68, which triggers... ...it to pop the label and forward the packet to R3. In turn, R3 sees that the top... ...label is 17, which triggers it to pop the label and forward the packet to R6. R6 sees... ...label 20, which triggers it to pop the label and forward the packet to R8.... ...In this book you will see methods to program such label stacks on an ingress node,... ...including static CLI configuration.
Adjacency Set
assign a common label to multiple adjacencies on a node. This is... ...known as an Adjacency Set (Adj-Set)
In the previous examples, dynamically-assigned labels were used. In this example,... ...a statically-assigned label is used in order to show the difference in configuration...:
------------------------
lab@R6# set protocols mpls label-range static-label-range 600000 699999
Then put two of the interfaces into a set, and assign a corresponding label:
set protocols isis interface-group group1 interface ge-0/0/2.0 weight 1 <<< 1/4
set protocols isis interface-group group1 interface ge-0/0/4.0 weight 3 <<< 3/4
set protocols isis interface-group group1 level 2 ipv4-adjacency-segment protected label 600001 <<<...到这两个口都用这个...label
------------------------
<<<...之前是自动的每个...adjanecy...一个...label...，现在是把两个...adjancey...配到一个...label
This means that when data-plane packets arrive at R6 with a top label value of... ...600001, the label is popped and the packets are load-balanced on links ge-0/0/2... ...and ge-0/0/4 in a 1:3 ratio.
The S-Flag means that the SID refers to a set (S-) of adjacencies.
The P-Flag... ...means that the Adj-SID is persistently (P-) allocated (because it was statically configured)... ...and therefore remains consistent across router restarts and/or interface... ...flaps.
Label Per Member Link on Aggregated Ethernet
In some cases, you might want to nail a flow of packets to a particular child link. You can achieve this by allocating a separate label per member link of the aggregated interface (while still having an Adj-SID for the aggregated interface as a whole). These labels per member link are statically configured, and are not advertised in the IGP.
!!!!...一定要有...enhance-ip...，否则没办法...commit
On R7:
set protocols mpls label-range static-label-range 700000 799999
set protocols mpls static-label-switched-path ae0-ge-0-0-2 transit 700002 next-hop 10.0.0.12
set protocols mpls static-label-switched-path ae0-ge-0-0-2 transit 700002 member-interface ge-0/0/2
set protocols mpls static-label-switched-path ae0-ge-0-0-2 transit 700002 pop
set protocols mpls static-label-switched-path ae0-ge-0-0-3 transit 700003 next-hop 10.0.0.12
set protocols mpls static-label-switched-path ae0-ge-0-0-3 transit 700003 member-interface ge-0/0/3
set protocols mpls static-label-switched-path ae0-ge-0-0-3 transit 700003 pop
The configuration means that when a packet arrives at R7 with top label 700002, the label is popped and the packet is sent out on member link ge-0/0/2 of ae0. On the other hand, when a packet arrives at R7 with top label 700003, the label is popped and the packet is sent out on member link ge-0/0/3 of ae0. You can verify this by looking at the mpls.0 table on R7:
C3 Node Segments and Anycast Segments  (follow shortest path)
****label...不是像之前自动分配的，而是手动配，但路径还是走...IGP shortest path...。这个是...SR+MPLS(no need of LDP or RSVP)
R8 and R9 do not support SR, but do support LDP. Routers R1, R2, R3, and R4 will not run LDP, only SR. R6, at the border between the two regions,
will run both LDP and SR. R6 will be responsible for swapping MPLS labels from LDP to SR node labels and vice versa, in order to allow traffic to travel between the two regions.
Each node in the network has a node segment associated with its loopback address. Any other node in the network can send packets to it along the shortest IGP path by using that node segment.
R2 wants to send packets to R6 using the shortest IGP path. It identifies its shortest path neighbor towards R6, namely R5. It pushes the MPLS label onto the packet that R5 associates with R6’s node segment and sends it on the link to R5. R5 in turn swaps that label for the label that its shortest path neighbor, R7, associates with R6, and sends the packet to R7. R7 pops the label and sends the underlying packet to R6. But how does each router involved know what label its shortest path neighbor is expecting to see in order to forward to R6? In order to achieve this, each router is configured with two key pieces of information:
A node index: Each router must have a unique node index. This is also known as a Node-SID (Node Segment Identifier).
A label block: This is defined in terms of a start-label and a label-range. The label range must be wide enough to accommodate all of the routers in the domain (including anticipated future growth). This label range is known as the segment routing global block (SRGB).
<<< Notice that each node label is unique. However, the label-ranges can overlap – for example, R3 happens to use the same start-label as R5.
Let’s now return to our example of R2 sending packets to destination R6 along the shortest path. R2 calculates what label its shortest path
neighbor R5 associates with R6’s node segment. This is calculated as follows:
Start-label of R5’s label range + index advertised by R6
2000 + 406 = 2406
R2 pushes label 2406 onto the packet and forwards it to R5. R5 swaps label 2406 for the label that R7 associates with R6’s node segment (6000 + 406 = 6406) and sends it to R7. R7 pops the label and sends the rest of the packet to R6.
<<<local router push label...：...nh router start label+destination router SID
This is a generic example in which the routers involved have different label range allocated for node segments. Now look at Figure 3.2. In this example, each router is configured with the same start-label, 1000, and it has a very useful effect: the label required to reach R6 via the shortest path is the same at each hop in the network (1000 + 406 = 1406). This makes troubleshooting much easier– when looking at routing or forwarding tables on any router in the network, label 1406 means shortest path forwarding to R6.
R1:
set protocols isis source-packet-routing srgb start-label 1000
set protocols isis source-packet-routing srgb index-range 9000
set protocols isis source-packet-routing node-segment ipv4-index 401
set protocols isis source-packet-routing node-segment ipv6-index 601
R2:
set protocols isis source-packet-routing srgb start-label 1000
set protocols isis source-packet-routing srgb index-range 9000
set protocols isis source-packet-routing node-segment ipv4-index 402
set protocols isis source-packet-routing node-segment ipv6-index 602
R3…R9 same
lab@R2> show isis overview | no-more
Source Packet Routing (SPRING): Enabled
SRGB Config Range:
SRGB Start-Label : 1000, SRGB Index-Range : 9000
SRGB Block Allocation: Success
SRGB Start Index : 1000, SRGB Size : 9000, Label-Range: [ 1000, 9999 ]
Node Segments: Enabled
Ipv4 Index : 402, Ipv6 Index : 602
<<< L-ISIS(SR)...是在...inet3...里...(...类似于...ldp/rsvp)...，因此...L3VPN...可以通过这个协议解析...nh...。它只需要配...SR+protocol MPLS,...不需要...ldp...或...rsvp...了。
-------------------------
[edit logical-systems r1 protocols isis]
'source-packet-routing'
IS-IS:  SPRING configuration is not allowed without 'protocols mpls'
error: configuration check-out failed
-------------------------
Let’s focus first of all on the entry to 192.168.0.6 (the loopback address of R6). R2 has determined that its shortest path neighbor to R6 is R5. It has added the starting value of the label-range advertised by R5 (1000) to the index advertised by R6 (406) to give a label value of 1406. Hence the action is to push label 1406 onto the packet and push it out on the link to R5, ge-0/0/1. If you now look at the corresponding entry in the mpls.0 table on R5, you see that it swaps the incoming label value of 1406 to an outgoing label value of 1406, because R5’s shortest path neighbor to R6, namely R7, is also configured with 1000 as the starting value of its
<<< mpls.0...是一个label行为转发表，查这个label在local的行为，这里是收到包带...1406 ...label，swap成...1406 ...从ae...0...这个nh口发出去
<<<...沿途每一跳只要是到...r6...，都...swap1406...这个一样的...label
Ingress router上show inet...3...路由，可能label是什么，然后去nh router上查mpls...0...里这个label的nh和label行为，这样逐跳查mpls...0...里的label就能知道走的什么路了。
ECMP
Let’s now look at an ECMP case. R2 has two ECMP paths to R4, via R1 and R5. Looking again at R2’s inet.3 table above, you see two next hops to R4’s loopback,192.168.0.4 the link to R1, and the link to R5. In either case, the label operation is to push label 1404 onto the packet.
labroot@jtac-mx480-r2605-re0# run show route 44.44.44.2 logical-system r2
r2-ce2.inet.0: 16 destinations, 28 routes (16 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both
44.44.44.0/24      *[BGP/170] 00:21:50, localpref 100, from 192.168.0.4
AS path: I, validation-state: unverified
to 10.0.0.0 via ge-0/1/3.0, Push 20, Push 1408(top)
>  to 10.0.0.5 via ge-0/1/6.0, Push 20, Push 1408(top)
[BGP/170] 00:21:50, localpref 100, from 192.168.0.7
AS path: I, validation-state: unverified
to 10.0.0.0 via ge-0/1/3.0, Push 20, Push 1408(top)
>  to 10.0.0.5 via ge-0/1/6.0, Push 20, Push 1408(top)
<<< r2...收到...r8...连接的...ce4...的这条路由，带着...vpn 20 ...和... SR 1408 label
<<< non-SR: IGP + mpls protocol + label distribution protocole -ldp/rsvp
SR...：... IGP (route+label distribution)+ mpls protocol  << ...减少了一个协议，控制层面...free...了很多
Anycast Segments ...（Prefix SID）
One application of Prefix SIDs is to achieve anycast operation. You can ascribe the... ...same IP address, and associated Anycast-SID, to multiple nodes. If in the data... ...plane a packet arrives at a node with the anycast label on top, the node forwards... ...the packet towards the nearest member of the group. In typical deployments, routers... ...in an anycast group would advertise regular node SIDs as well.
R3&R4:
set interfaces lo0 unit 0 family inet address 192.168.0.99/32 <<...给所有属于一个...prefix-sid...的...node...配相同的...2nd lo0...地址
set policy-options policy-statement ISIS-EXPORT term ANYCAST from protocol direct
set policy-options policy-statement ISIS-EXPORT term ANYCAST from interface lo0.0
set policy-options policy-statement ISIS-EXPORT term ANYCAST from route-filter 192.168.0.99/32 exact
set policy-options policy-statement ISIS-EXPORT term ANYCAST then prefix-segment index 499
set policy-options policy-statement ISIS-EXPORT term ANYCAST then accept
set protocols isis export ISIS-EXPORT
<<< R3 R4...配成一样的...lo00...地址，让他两成为一个...group
Let’s now see what happens on R1. R1 is directly connected to both members of the anycast group, R4 and R3, and the metric to each is the same. As you can see, R1 pops the label and ECMPs the traffic to R4 (via ge-0/0/1) and R3 (via ge-0/0/2):
C4 Interworking Between Segment Routing and LDP
Note that this scheme is not needed in order to migrate from LDP to SR – such migration can be achieved in a way similar to migration from one IGP to another. Rather, it is aimed at scenarios in which some of the routers in the network do not support SR and will continue to use LDP for the foreseeable future.
R8 and R9 do not support SR, but do support LDP. Routers R1, R2, R3, and R4 will not run LDP, only SR. R6, at the border between the two regions, will run both LDP and SR. R6 will be responsible for swapping MPLS labels from LDP to SR node labels and vice versa,
LDP >>> SR:
lab@R6# set protocols ldp sr-mapping-client
**...要配...router-id ...否则...ldp session...起不来，除非手动配指向...physical interface address
This triggers R6 to allocate LDP labels corresponding to the node SIDs of R1, R2, R3, and R4. It advertises them to its LDP neighbors (R8 in the example network). For example, R6 allocates a label of value 29 corresponding to the loopback address of R2, 192.168.0.2. (you’ll also notice that an LDP label has been allocated for the 192.168.0.99 anycast address that was configured in Chapter 3):
*** r2...路由进入...r9...的...inet3...条件是，...inet0...里...r2...这个路由...nh...也要是从...r8...学来的
SR >>>>LDP:
The mapping... ...server can be any SR speaker in the domain. To emphasize this, let’s use R1 as the... ...mapping server, as it is nowhere near R6, R8, or R9.
<<< ...想知道在...local...设备上到...destination router push...什么标签，看...inet3.
...想知道某个...label...在...local router...到...nh router...上要做设么样的...label...行为，看...mpls.0
user@R6# set protocols isis source-packet-routing ldp-stitching
<<<...没有这个，在...r6...上...show mpls table label 1409...是没有的。加了这个之后：
-----------------
regress@R6_re0# run show route table mpls.0 label 1409
mpls.0: 64 destinations, 64 routes (64 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both
1409               *[L-ISIS/14] 00:00:04, metric 20
>  to 10.0.0.17 via ge-0/0/1.0, Swap 21... <<<<
---------------
user@R6> show route table mpls.0
<snip>
1408 *[L-ISIS/14] 00:00:05, metric 1000
> to 10.0.0.17 via ge-0/0/1.0, Pop
1408(S=0) *[L-ISIS/14] 00:00:05, metric 1000
> to 10.0.0.17 via ge-0/0/1.0, Pop
1409 *[L-ISIS/14] 00:00:05, metric 2000
> to 10.0.0.17 via ge-0/0/1.0, Swap 54
This triggers R6 to install entries in the mpls.0 table, to stitch incoming SR labels... ...corresponding to R8 and R9 to the corresponding LDP label values. R8 advertised... ...label 3 to R9 (penultimate hop-popping), hence the “pop” action for label 1408.... ...R8 advertised label 54 for the loopback address of R9, hence the incoming label... ...value of 1409 is swapped for label value 54:
C5 ...Topology Independent Loop Free Alternate (TILFA)
This chapter shows you how the fast re-route mechanisms work in SPRING networks
Global Repair:
achieving sub-100ms convergence, global (network-wide) convergence is difficult.... ...In these cases, local repair comes into the picture.
Local Repair:
If, on the node detecting local failure... ...(when discussing local repair techniques such nodes are commonly called... ...Point of Local Repair – PLR), another (backup) next hop redirecting the traffic... ...around the failure was already installed in HW FIB, the only action that needs to... ...be performed during failure events is to detect the failure itself and remove the... ...original (primary) next hops associated with the failed link or node from the HW... ...FIB. All the other steps are no longer required for local repair....  This can achieve to sub-100ms, or even sub-50ms, in many cases.
Strictly speaking, local repair is complementary (and not an alternative) to global... ...repair. Indeed, local repair and global repair take place in parallel. Local repair... ...quickly reroutes the traffic around the failure by using a pre-computed and preinstalled... ...temporary backup path while global repair computes the final converged... ...path.
While preparation of the network for global convergence is typically straightforward... ...(basically tuning some parameters), local repair is much more challenging.... ...The backup next hop must be selected and programmed at least in a way to avoid... ...traffic looping. Local repair happens very fast. Often traffic is redirected over a... ...backup next hop within 50ms (the term frequently used to describe local repair is... ...fast reroute ), therefore traffic redirected by PLR traverses nodes that have an old... ...(pre-failure) view of the network, since global convergence still didn’t happen on... ...them. Therefore, these nodes may still believe the shortest path to the destination... ...is via failed link or node. Thus, the challenge is how to ensure that in such transient... ...state loop doesn’t occur
SPRING Fast Traffic Restoration Basic Concepts
The basic idea with SPRING protection is very simple: instead of redirecting the traffic around failure over some temporary path, and then, after global convergence happens, redirecting that same traffic again using a newly calculated global path, you make the backup path, right from the beginning, equal to the shortest post-convergence path.
The main difference between... ...RSVP 1:1 backup and SPRING fast-reroute is how the backup path is set up.
RSVP uses signaling to allocate labels and create states across all transit nodes... ...along the backup path from PLR to the final destination, while in SPRING the... ...backup path is enforced by using appropriate label stack pushed by PLR on the... ...top of the redirected packet.
In both cases (RSVP 1:1 and SPRING) the shortest... ...post-convergence backup path can be created (enforced) in any arbitrary topology– it is topology independent. Therefore, in SPRING terminology, this kind of protection... ...is called Topology Independent Loop Free Alternate (TI-LFA).
How can the post-convergence path be calculated? Very simply: just use the for the... ...SPF calculation modified link state database (LSDB), where the network resource... ...(for example, link or node) being protected is removed from the LSDB.
Backup Next-Hop Selection
In Junos implementation,
legacy LFA:  selects the backup next hop closest to the destination (please note, RFC 5286 – Basic Specification for IP Fast Reroute: Loop-Free Alternates – does not specify which LFA backup next hop should be selected when there are multiple non-looping backup next hops possible; in such case, it is up to the vendor implementation to freely chose the backup next hop). In this particular case, to reach R9 from R1, the shortest path is via R4, with total path cost 4000. The legacy LFA backup next hop is R3, with path cost from R3 to R9 equal to 3000. Legacy LFA does not select R2 as the backup next hop, since the path cost from R2 to R9 is higher (3500) than from R3 to R9 (3000).
TI-LFA: on the other hand, takes into account total post-convergence cost (from the source R1) of the backup path. Via R3 it is 5000, while via R2 it’s only 4500. Therefore, the choice for TI-LFA backup next hop is R2.
legacy LFA (RLFA):  (node-link-protection)
r1...有两个...nh...可选，...r3...和...r2...，它会计算从这个...nh router(r2/r3)...到...destination router...的...cost...，...r3-r9...是...3000...，而...r2-r9...是...3500...，所以他会选从...R3...走
use-source-packet-routing...:... configuration knob enables SPRING-based... ...LFA protection for prefixes in inet.0 (plain IPv4 traffic) and inet6.0 (plain IPv6
traffic) RIBs. Without this knob being configured, only SPRING prefixes in inet.3... ...(ingress labeled IPv4 traffic), inet6.3 (ingress labeled IPv6 traffic), and mpls.0... ...(transit labeled traffic) RIBs are subject for SPRING-based LFA protection.
TI-LFA:
...他是算从...source router(r1)...到...destination...经过两个...nh...的总...cost...，...r1-r3-r9...是...5000...，而...r1-r2-r9...是...4500...，所以选从...r2...走
Link Versus Node Protection
TI-LFA, by default, uses link-protection backup next hops.
<<<link protection...就是看到的...backup nh...用的可能是和...primary  nh...相同的...node...只是不一样的...link...。那如果这个...node down...，就有问题了。...node-protection...是...backup nh...选不同于...primary nh node...的，这样即使...primary node down...，也没问题。但如果用...node-protection...，但没有另一个...node...可作为...nh...，那么就没有...backup nh...了。所以这是需要...enable node-link-protection...。
To use node protection with TI-LFA, you must enable it on the IGP interface level:
PR 1343744 [Confidential, Mistaken, single-source-commit] - RLI::33638::OSPF TILFA:: TI-LFA with node-protection knob, backup path is not avoiding NODE which is connected to Primary path.
Node-protection configured on a LAN interface doesn't result in a backup path that
avoids the far end node on the LAN.  This limitation is documented in the functional spec.
2.2.1.7 Protection on LANs
If a LAN interface on a PLR is configured for link-protection, the backup path will avoid the failed LAN interface.  The backup path does not avoid the LAN itself.
If a LAN interface on a PLR is configured for node-protection, the backup path will avoid the LAN. The backup path does not avoid far end destination nodes on the LAN.
Strict/Loose Node-protection:
W...hen TI-LFA node protection is enabled, there is no... ...backup next hop, because going through node R6 is the only way to reach R9。
Therefore, it would be beneficial to fall back to link protection, where node protection... ...is not possible. In legacy (R)LFA configurations, you could use the node...-l...ink-degradation knob to enable such behavior.
TI-LFA uses a different approach, namely you can use strict or loose node protection:
S...trict node protection mode...: ...the protected node is... ...completely removed from LSDB during SPF calculations to determine the backup... ...next hop.
Loose node protection mode: instead of completely removing the protected node from the LSDB, the cost of all the links (other than the primary
link, for which the backup path is being calculated) to the protected node will be increased by the configured node protection cost value. Thus, the use of the protected node will be strongly (if configured node protection cost value is high) discouraged, but the protected node will be still considered as a backup, even if there is no other option. Therefore, it can be used as a fallback to link protection, if node protection is not possible.
CAUTION Configuring node-protection cost to be equal to the maximum IGP link... ...metric (IS-IS: 16777215, OSPF: 65535) keeps the strict node protection behavior... ...in place. You need to configure node-protection cost to be smaller (at least by 1)... ...than the maximum IGP link metric to enable loose node-protection behavior.
Fate-Sharing
For a backup path to work effectively it must not share links or physical fiber... ...paths with the primary path, ensuring that a single point of failure will not affect... ...the primary and backup paths simultaneously.
Thus, the use of network elements sharing fate will be highly discouraged (if the... ...configured fate-sharing cost value is high), but these network elements will be still... ...considered as backup, if there is no other option. Therefore, through fate-sharing,... ...you can configure backup paths that minimize, as much as possible, the number of... ...shared links and fiber paths with the primary paths. That ensures that if a fiber is... ...cut, a minimum amount of data is lost and a path still exists to the destination.
To verify fate-sharing operation, let’s assume (using the network topology from Figure 5.3) that R1-R3 and R4-R5 links are (partially) carried over the same fiber infrastructure. Therefore, the backup path should avoid R4-R5 link. Let’s consider this configuration on R1:
If the cost configuration knob is omitted, the default value used during TI-LFA calculations is 1.
labroot@jtac-mx480-r2605-re0# run show mpls fate-sharing logical-system r1   <<< hidden command
Group fs-test {
Cost 65535
10.0.0.6  10.0.0.7
10.0.0.22  10.0.0.23
}
kszarkowicz@R1# run show isis interface ge-* extensive | match “ge-|Protection”
ge-0/0/0.0
Post convergence Protection:Enabled, Fate sharing: Yes, Srlg: No, Node cost: 16777214
ge-0/0/1.0
Post convergence Protection:Enabled, Fate sharing: Yes, Srlg: No, Node cost: 16777214
ge-0/0/2.0
Post convergence Protection:Enabled, Fate sharing: Yes, Srlg: No, Node cost: 16777214
kszarkowicz@R1# run show route 192.168.0.3 table inet.3 detail | match “entry|LISIS|
weight|operation”
192.168.0.3/32 (1 entry, 1 announced)
*L-ISIS Preference: 14
Next hop: 10.0.0.23 via ge-0/0/2.0 weight 0x1, selected # R3
Next hop: 10.0.0.1 via ge-0/0/0.0 weight 0xf000 # R2
Label operation: Push 1403, Push 1406(top)
https://www.juniper.net/documentation/us/en/software/junos/static-routing/mpls/topics/ref/statement/fate-sharing-edit-routing-options.html
Changing the fate-sharing database does not affect existing established LSPs until the next CSPF reoptimization. The fate-sharing database does affect fast-reroute detour path computations.
Label Stack for TI-LFA Backup Path
How is the label stack for backup path actually determined?
The most straightforward answer would be: Use an Adj-SID for each link (each... ...IGP adjacency) along the backup path to enforce strict path routing through the... ...network. With this method the required label stack can very easily be determined:... ...you simply take the result of SPT_new(R, X) calculation, determine all the links... ...(IGP adjacencies) provided by this calculation, and use the Adj-SID associated... ...with each link (IGP adjacency) to create a label stack that should be pushed on the... ...packet forwarded over the backup next hop.... ...The biggest problem with this approach is that it can potentially... ...produce very large label stacks (containing large numbers of Adj-SIDs) that should... ...be pushed by the PLR on the packet.
So, the imposed label stack, while is... ...not required to be an explicit hop-by-hop (no need for Adj-SID of each link) must... ...achieve loop-free forwarding over the backup path. Here comes the LFA (loop-free... ...alternate) part of the TI-LFA acronym.
To determine label stack for TI-LFA, we use below:
<<<P,Q space...都不包含...destination...的直连...neighbor
P-Space:
This is a set of routers reachable from a PLR router (denoted S) using a shortest path (using LSDB_old) and without traversing the protected link. In
the case of ECMP, this requirement applies to all the equal-cost shortest paths from S to a node in the P-space. None of these paths can traverse the protected link; otherwise, the node is not in the P-space.
Extended P-space:
The union of P-space computed for PLR router (denoted S) as well as P-spaces computed for each direct neighbor of S, excluding primary
next-hop router (denoted E). This is a set of routers that can reach the primary next hop (denoted E) using a shortest path (using LSDB_old) and without traversing the protected link. In the case of ECMP, this requirement applies to all the equal-cost shortest paths from a node in the Q-space to E.
Q-Space:
This is a set of routers that can reach the primary next hop (denoted... ...E) using a shortest path (using LSDB_old) and without traversing the protected
link. In the case of ECMP, this requirement applies to all the equal-cost shortest... ...paths from a node in the Q-space to E.
PQ-node:
This is a node that is a member of both the extended P-space and the Q-space. The PQ-node does not need to be directly connected to S (or to E).
To determine PQ-nodes, PLR performs the following multiple SPF calculations (using LSDB_old):
One primary SPF calculation, using itself (PLR) as the root of the SPF tree. Routers always perform these types of SPF calculations, regardless of whether
LFA is enabled, to determine primary next hops due to normal IGP operation.
Multiple backup SPF calculations, with each calculation using a different direct IGP neighbor node as the root of the SPF tree. Routers perform this type of SPF calculation to only determine backup next hops, if the LFA feature is enabled.
TI-LFA P-Space and Extended P-Space
As you can see, router R1 can reach only routers R2 and R3 without crossing... ...R1>R4 link (link under protection). It is the P-space of R1 for the R1>R4 link. On... ...the other hand, you can reach the rest of the topology without crossing link under... ...protection either from R2 or R3 – direct neighbors of R1. This is the Extended Pspace... ...of R1 for R1>R4 link. If the TI-LFA (post-convergence) backup path towardsthe destination (in this case R9) traverses only nodes found in the ... ...Extended... ...P-space, the calculation of Q-space is not required. Therefore, in this example, R1... ...performed only three SPF calculations, each time placing different routers (R1,R2, R3) as the root of the SPF tree.
If the TI-LFA (post-convergence) backup path uses only nodes in the... ...P-space, single SPF calculation is enough – there is no need to discover Extended
P-space.
If the TI-LFA backup path towards its destination uses only the routers from P... ...space... ...or Extended P-space (including the destination router), when traffic is being... ...redirected over the backup path during failure event, then no additional labels... ...must be pushed on the redirected packet
As per (Extended) P-space definition, traffic from such node can reach other nodes... ...in (Extended) P-space (including destination node) without crossing the link under... ...protection – and still using the old (pre-failure) IGP database. So, no special attention... ...is required regarding loop prevention. In other words, you don’t need the list... ...of explicit next hops for TI-LFA backup path. You can simply redirect the traffic!
Single Additional Label for TI-LFA Backup Path
the destination node (R6) doesn’t belong to either P-space or to Extended P-space
The solution of the problem is to find routers on the TI-LFA post-convergence backup paths that belong to both (Extended) P-space and Q-space. Such routers are called PQ-nodes. In our example, PQ-nodes are the routers R5, R7, and R9. It is enough to force the traffic to some PQ-node, since from PQ-node, based on Qspace definition, the shortest path towards the destination doesn’t use link under protection.
Two Additional Labels for the TI-LFA Backup Path
This time, there is no overlap between (Extended) P-space (in the example, Pspace is equal to Extended P-space) and Q-space. Consequently, there is no PQnode. The way traffic can be forwarded in such a scenario is as follows:
The last (bottom of stack) label is R6 Node-SID (R6 is a loose next hop). Therefore, it can be concluded that two additional labels were required to program the TI-LFA backup path in this use case. In summary, three use cases for TI-LFA link-protection discussed so far are:
1. A TI-LFA backup path traverses over P-nodes only – no additional MPLS
backup label is required for protection.
2. TI-LFA backup paths traverse over the PQ-node – one additional MPLS backup
label (Node-SID associated with PQ-node) is required for protection.
3. TI-LFA backup paths traverse over adjacent (directly connected) P- and Qnodes
– and two additional MPLS backup labels (Node-SID associated with
P-node, and
More Additional Labels for a TI-LFA Backup Path
TI-LFA with node protection is configured in the network. Investigating the routing state for the R8 loopback on router R4 you can notice an interesting
observation:
The primary next hop has a single label (R8 Node-SID). However, the backup next hop this time has a label stack of 3 labels. These labels are actually unprotected Adj-SIDs (which were manually configured to have predictive values) for ISIS adjacencies on the TI-LFA backup path (for example, label 50007 is Adj-SID for the R5>R7 adjacency). It basically means, this time the backup path is encoded as the chain of three strict next hops, represented by three Adj-SIDs. If you look at the topology, paying special attention to the link metric, there is actually no other possibility to encode a backup path using some loose next hops (Node-SIDs). Due to the low link metrics between R5/R7/R9 and R6, using loose next-hop (Node-SIDs) would result in the traffic being redirected back towards R6 node for any R5/R7/R9 node. This, however, due to configured node protection, should be avoided.
Protection for SR-TE Tunnels
SPRING machinery can provide protection not only for shortest path-based tunnels—what was discussed so far in this chapter—but as well for SPRING Traffic... ...Engineered (SR-TE) tunnels
All routers are configured to provide loose node protection. So first, each transit router tries to find a backup next hop
protecting against upstream node failure, and if that’s not possible, it tries to use a backup next hop protecting against upstream link failure.
<<<...就是先找可以保护...nh node...的...failure...路径，如果没有退而求其次找可以保护...nh link failure...的...path...。
Configure two Adj-SIDs configured for R6>R7 adjacency – protected and unprotected – as follows:
Only configure on R6:
[edit logical-systems r6 protocols isis]
interface ge-0/2/2.0 {
level 2 {
ipv4-adjacency-segment {
protected label 600077;
unprotected label 600007;
}
metric 1000;
}
node-link-protection;  <<< must configure
point-to-point;
[edit logical-systems r6 protocols mpls]
labroot@jtac-mx480-r2605-re0# show
label-range {
static-label-range 600000 699999;  <<< configured in the previous chapters
labroot@jtac-mx480-r2605-re0# run show isis database logical-system r6 jtac-mx480-r2605-re0-r6.00-00 extensive
IS neighbor: jtac-mx480-r2605-re0-r7.00    Metric:     1000
Two-way fragment: jtac-mx480-r2605-re0-r7.00-00, Two-way first fragment: jtac-mx480-r2605-re0-r7.00-00
P2P IPv4 Adj-SID:  600077, Weight:   0, Flags: -BVL-P
P2P IPv4 Adj-SID:  600007, Weight:   0, Flags: --VL-P
