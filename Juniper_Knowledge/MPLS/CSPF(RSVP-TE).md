# CSPF(RSVP-TE)

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

CSPF(RSVP-TE)
Commands
> show ted database extensive
TED database: 0 ISIS nodes 8 INET nodes
NodeID: 12.82.74.4
Type: Rtr, Age: 134248 secs, LinkIn: 2, LinkOut: 2
Protocol: OSPF(10.10.1.32)
To: 12.82.74.5, Local: 12.82.75.42, Remote: 12.82.75.41
Local interface index: 85, Remote interface index: 0
Color: 0x20 EDGE-UPLINK
Metric: 100
Static BW: 10Gbps
Reservable BW: 10Gbps
Available BW [priority] bps:
[0] 9.998Gbps    [1] 9.998Gbps   [2] 9.998Gbps   [3] 9.998Gbps
[4] 9.998Gbps    [5] 9.998Gbps   [6] 9.998Gbps   [7] 9.998Gbps
Interface Switching Capability Descriptor(1):
Switching type: Packet
Encoding type: Packet
Maximum LSP BW [priority] bps:
[0] 9.998Gbps    [1] 9.998Gbps   [2] 9.998Gbps   [3] 9.998Gbps
[4] 9.998Gbps    [5] 9.998Gbps   [6] 9.998Gbps   [7] 9.998Gbps
# 路径选择 #
CSPF...：... 根据ted的信息计算出最优路径...，路它不管rsvp.../ldp是否能建立起来...。只要最优径igp通就会切到最优路径....所以如果igp起来了...，lsp还是down去查ldp.../rsvp
MBB...：... 看的是...rsvp.../ldp是否建立起来...，只要起来就会切到最优路径不管这条路径是否link... up...。
CSPF ...是有约束性的...SPF...算法。... SPF...是用于...ospf...的，就是单纯根据...metric...算最优路径。但是...cspf...可以根据...n...多条件算最优路径。
如果是...isis...，不用...no-csp...因为...default cspf...就...up...。但是...ospf...，必须...no-cspf...（...default is cspf up...），因为...default...他的...cspf ted...不...up...。除非...ospf...开了...traffic-engineer...，这个会是...cspf ted up...，那么...default up...的...cspf...就...ok...，...lsp...就可以起来了。
# CSPF Selects a Path #
The following list provides details on CSPF path selection:
1. CSPF computes LSPs one at a time, beginning with the highest priority LSP (the one with the lowest setup priority value). Among LSPs of equal priority, CSPF starts with those that have the highest bandwidth requirement.
2. CSPF prunes the traffic engineering database of all the links that are not full duplex and do not have sufficient reservable bandwidth.
3.If the LSP configuration uses the include statement, CSPF prunes all links that do not share any included colors, including links with no color assigned.
4. If the LSP configuration uses the exclude statement, CSPF prunes all links containing excluded colors; links with no color are not pruned.
5. CSPF finds the shortest path towards the LSP's egress router, taking into account explicit-path constraints. For example, if the path must pass through Router A, two separate SPFs are computed, one from the ingress router to Router A, the other from Router A to the egress router.
6.If several paths have equal cost, CSPF chooses the one whose last hop address is the same as the LSP's destination.
7.If several equal-cost paths remain, CSPF selects the one with the fewest number of hops.
8.If several equal-cost paths remain, CSPF applies the CSPF load-balancing rule configured on the LSP (least-fill, most-fill, or random).
<Operations Performed by the Ingress LSR>
The entire CSPF process has the following six major parts:
-Information propagation: Traffic engineering extensions to either IS-IS or OSPF carry traffic engineering topology information.
-Information storage: The router stores traffic engineering link-state information in the traffic engineering database.
-User constraints: The user specifies constraints for a specific LSP.
-Physical path calculation: The CSPF algorithm finds the shortest path of links that comply with the user constraints.
-Explicit route generation: The router forms an explicit route (object) from the list of IP addresses that represent the shortest path.
-RSVP signaling: The router uses the explicit route object to determine the forwarding path for RSVP path messages.
IGP Extensions
Distributes topology and traffic engineering information using IGP extensions
Maximum reservable bandwidth
Remaining reservable bandwidth
Link administrative groups (color)
-The OSPF no-topology and IS-IS traffic engineering disable options both have the same effect. They are used in conjunction with the traffic engineering shortcuts feature; the router does not use the TED for CSPF calculations but can install downstream prefixes for the LSP into inet.3.
Mechanisms
OSPF uses  type 10 LSAs to propagate information within an area. This LSA is an opaque LSA with area scope. Opaque LSAs carry information not essential to the OSPF protocol; the protocol does not take action upon their contents. IS-IS uses TLVs to carry the traffic engineering information.
# Optimize timer #
MPLS option provides an option to recompute LSP paths periodically to find more optimal paths (less congested, lower metric or less hops).
LSP optimization requires CSPF LSP computation (enabled by default).
After reoptimization is run, the result is accepted if:
New path has lower IGP metric.
New path has an equal IGP metric and lower hop count
With least-fill configured, if new path reduces congestion by 10% aggregated over all links traversed.
New path does not cause preemption.
Configuration:
[edit protocols mpls]
user@r1# set optimize-timer 1800
