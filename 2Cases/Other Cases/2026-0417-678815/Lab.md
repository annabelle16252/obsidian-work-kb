![[Pasted image 20260418121504.png]]
jtac-VM-Shared-toolbox-BNGL-53 10.219.7.113 labroot/lab123


start shell
su
tcpdump -i ae11 -w /var/tmp/test-ae11.log
tcpdump -i ae22 -w /var/tmp/test-ae22.log

![[Pasted image 20260423081242.png]]







# BGP Packet
r1 -- r2 的 BGP keepalive/update 路径是：
发送侧 r1：rpd 生成 BGP 报文，经本机转发面从接口发出，走egress q0
接收侧 r2：报文到入接口后，被识别为本机控制流量，经过 host-bound queue/punt path 送到 RE，再交给 rpd

host/control queue 不是我们平时说的业务转发里的 egress queue，而是报文从 FPC/PFE 往 RE/control-plane 送的 host path 上的队列。

r1 -> r2 的 keepalive/update 在线路上是普通 TCP/179 报文
keepalive/update 是RE产生送往RE的，所以只走hostbound path
By default, normal BGP packets are sent to Q0 and retransmitted packets are sent to Q3 to avoid further queue drop. 

![[Pasted image 20260422140402.png]]


RPD sets 0xc0 to all BGP packets despite of queue selection which is the day-1 behavior.

https://www.juniper.net/documentation/us/en/software/junos/cos/topics/concept/cos-host-outbound-traffic-default-classification-and-dscp-remarking.html
Default Queuing and Marking of Host Outbound Traffic

By default, the router assigns host outbound traffic to the best-effort forwarding class (which maps to queue 0) or to the network-control forwarding class (which maps to queue 3) based on protocol. For more information, see Default Routing Engine Protocol Queue Assignments.

For most protocols, the router marks the type of service (ToS) field of Layer 3 packets in the host outbound traffic flow with DiffServ code point (DSCP) bits 000000 (which correlate with the best-effort forwarding class). For some protocols, such as BGP, the router marks the ToS field with 802.1p bits 110 (which correlate with the network-control forwarding class), while still assigning the packets to queue 0. The router does not remark Layer 2 traffic such as LACP control traffic on aggregated Ethernet. For more information, see Default DSCP and DSCP IPv6 Classifiers.

CS6 is commonly used for network control traffic and corresponds to IP Precedence 6. When mapping to the 8-bit Type of Service (ToS) byte, this value is often represented as 0xC0 because the DSCP value is shifted left by 2 bits.
 
RPD sets 0xc0 to all BGP packets despite of queue selection which is the day-1 behavior.---fujita
 
# RED drop
RED drop 是主动队列管理（AQM）机制触发的拥塞丢包，表明出口队列已超过配置的丢弃阈值，设备在队列满之前主动丢弃包。

tail drop是带宽没了开始丢，RED是在带宽没了之前提前丢。

引起RED drop可能原因：burst(buffer过小)或者drop profile(scheduler 配置里)

若 tail-drop 多 → 真实拥塞，需扩容或流量shaping。
若 只有 red-drop，tail-drop 很少 → 大概率 drop-profile 阈值太低，调整曲线即可

查看drop-profile：
show class-of-service interface ge-6/0/0
show class-of-service drop-profile

set class-of-service interfaces ge-6/0/0 scheduler-map TRUNK-scheduler-6COS

set class-of-service schedulers TRUNK-standard-scheduler-6COS drop-profile-map loss-priority low protocol any drop-profile standard


[edit]
labroot@jtac-mx960-r2038-re0# show | match ae11 | display set 
set interfaces ge-6/0/0 gigether-options 802.3ad ae11
set interfaces ae11 aggregated-ether-options lacp active
set interfaces ae11 unit 0 family inet address 192.168.1.1/30
set class-of-service interfaces ae11 scheduler-map TRUNK-scheduler-6COS

delete class-of-service schedulers TRUNK-standard-scheduler-6COS drop-profile-map loss-priority low protocol any drop-profile standard

BGP keepalive 默认标记为 IP Precedence 6（CS6，ToS=0xC0），对应 ipprec-compatibility 里的：
110  →  nc_premiumnrt / low  →  Queue 3

ACK 丢 → TCP 卡住 → keepalive 发不出 → hold timer 超时 → BGP flap


# Ingress 丢包的检查方法
![[Pasted image 20260419103924.png]]

