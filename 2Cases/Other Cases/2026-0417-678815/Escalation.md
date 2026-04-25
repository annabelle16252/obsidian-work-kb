



```
Without flex knob:
NGMPC1(jtac-mx960-r2038-re0 vty)# show cos halp ifd  305

--------------------------------------------------------------------------------
    rich queueing enabled: 0
    Q chip present: 0
IFD name: ge-1/2/0   (Index 305) egress information
    XM chip id: 0
    XM : chip Scheduler: 0
    XM chip L1 index: 64
    XM chip dummy L2 index: 128
    XM chip base Q index: 512
    Number of queues: 8
    Rich queuing support: 0 (ifl queued:0)
--------------------------------------------------------------------------------------------
Queue  State        Max           Guaranteed    Burst       Weight  G-Pri  E-Pri  WRED  TAIL
Index               Rate          Rate          Size                              Rule  Rule
--------------------------------------------------------------------------------------------
512    Configured   1000000000    950000000     131064      118     GL     EL     4     89  
513    Configured   1000000000    0             131064      1       GL     EL     0     1   
514    Configured   1000000000    0             131064      1       GL     EL     0     1   
515    Configured   1000000000    50000000      131064      6       GL     EL     4     24  
516    Configured   1000000000    0             131064      1       GL     EL     0     1   
517    Configured   1000000000    0             131064      1       GL     EL     0     1   
518    Configured   1000000000    0             131064      1       GL     EL     0     1   
519    Configured   1000000000    0             131064      1       GL     EL     0     1   
----------------------------------------------------------------------------------

With flex knob:

NGMPC1(jtac-mx960-r2038-re0 vty)# show cos halp ifd 305

--------------------------------------------------------------------------------
    rich queueing enabled: 1
    Q chip present: 1
IFD name: ge-1/2/0   (Index 305) egress information
    XQ chip id: 0
    XQ : chip Scheduler: 0
    XQ chip L1 index: 0
    XQ chip dummy L2 index: 0
    XQ chip dummy L3 index: 0
    XQ chip dummy L4 index: 1
    Number of queues: 8
    XQ chip base Q index: 8
Queue    State        Max       Guaranteed   Burst  Weight Priorities Drop-Rules  Scaling-profile 
Index                 rate         rate      size            G    E   Wred  Tail       ID
------ ----------- ----------- ------------ ------- ------ ---------- ----------  ----------------
     8  Configured  1000000000    950000000 16777216     95   GL    EL    4   397        3
     9  Configured  1000000000            0 16777216      1   GL    EL    0    72        3
    10  Configured  1000000000            0 16777216      1   GL    EL    0    72        3
    11  Configured  1000000000     50000000 16777216      5   GL    EL    4   211        3
    12  Configured  1000000000            0 16777216      1   GL    EL    0    72        3
    13  Configured  1000000000            0 16777216      1   GL    EL    0    72        3
    14  Configured  1000000000            0 16777216      1   GL    EL    0    72        3
    15  Configured  1000000000            0 16777216      1   GL    EL    0    72        3

```

--------------


Dear Fujita San,

Oh, yes! I forgot this. I have very very vague memory that I saw the link you gave many years back.

No, that knob just enables XQCHIP on flexible-Q MPC which helps 'reducing' the chance of Q0 drop. 'host-outbound-traffic' knob is still required to put the host-outbound packets to Q3.
```
<Annabelle> From lab, I see Q3 traffic doesnt change much, but queue 0 traffic changed as below:

With flex knob:
Queued: 245498 pps
Transmitted: 122749 pps
Tail drop: 122749 pps
RED drop: 0 pps
Queue-depth: about 11.9 MB

Without flex knob:
Queued: 245000 pps
Transmitted: 122498 pps
Tail drop: 3720 pps
RED drop: 118782 pps
Queue-depth: about 12.0 MB

The difference I can see is that without the knob, there will be tail&RED drop whereas there will be only tail drop with the flex knob.

Can you tell some more detail about the XQchip help to reduce the chance of queue 0 drop? Why no bgp flap can be seen after enable it?

```



-------

```
Just to clarify,

#This is the RPD/BGP code that sets 0xc0 to TOS.

layer3/usr.sbin/rpd/bgp/bgp_init.c
/*
 * bgp_set_socket_precedence - set precedence bits on the task's socket
 */
static void
bgp_set_socket_precedence (task *tp)
{
    if (socktype(task_get_addr(tp)) == GF_INET6) {
      setsocktrafficclass_in6(task_get_addr(tp),
                        IPTOS_PREC_INTERNETCONTROL);
      (void) task_set_option(tp, TASKOPTION_TOS, IPTOS_PREC_INTERNETCONTROL);
    } else {
      (void) task_set_option(tp, TASKOPTION_TOS, IPTOS_PREC_INTERNETCONTROL);
    }
}

#TCP driver on RE kernel puts retransmitted TCP packets to Q3.
junos/bsd/sys/netinet/tcp_output.c 

            /*
             * JUNOS:
             * Push up the priority of this packet in one of the below
             * two cases:
                 *
             * 1) If we are doing retransmissions, and the ToS is elevated
             * 2) If always_bump_ip_prec_internet_control is set and the
             *    the internet control precedence is set on this packet.
             *    (high ToS)
             */
            if (((ss_bump_retrans) && ((tp->t_flags & TF_REXMT) ||
                  /* JUNOS */
                  prioritize) &&
                  (IPV6_GET_CLASS(inp->inp_flow) >=
                        IPTOS_PREC_INTERNETCONTROL)) ||
                  ((always_bump_ip_prec_internet_control) &&
                  (IPV6_GET_CLASS(inp->inp_flow) >=
                        IPTOS_PREC_INTERNETCONTROL))) {
                  m->m_flags |= M_KEEPALIVE;
            }
            tp->t_flags &= ~TF_REXMT;

Thanks
-Naotaka
```
--------------------

RPD sets 0xc0 to all BGP packets despite of queue selection which is the day-1 behavior.

https://www.juniper.net/documentation/us/en/software/junos/cos/topics/concept/cos-host-outbound-traffic-default-classification-and-dscp-remarking.html
Default Queuing and Marking of Host Outbound Traffic

By default, the router assigns host outbound traffic to the best-effort forwarding class (which maps to queue 0) or to the network-control forwarding class (which maps to queue 3) based on protocol. For more information, see Default Routing Engine Protocol Queue Assignments.

For most protocols, the router marks the type of service (ToS) field of Layer 3 packets in the host outbound traffic flow with DiffServ code point (DSCP) bits 000000 (which correlate with the best-effort forwarding class). For some protocols, such as BGP, the router marks the ToS field with 802.1p bits 110 (which correlate with the network-control forwarding class), while still assigning the packets to queue 0. The router does not remark Layer 2 traffic such as LACP control traffic on aggregated Ethernet. For more information, see Default DSCP and DSCP IPv6 Classifiers.


-------------------

Dear Fujita San,

Without 'chassis flexible-queuing-mode' &'class-of service host-outbound-traffic' configuration, I see bgp flaps. I expected to see BGP packets in queue0 and TCP re-transmission packets in queue 3.

However, from pacp files, all packets has CS6 cos bit which is 3:
```

labroot@jtac-mx960-r2038-re0> show class-of-service interface ge-1/2/0 
Apr 22 10:26:46
Physical interface: ge-1/2/0, Index: 305
Maximum usable queues: 8, Queues in use: 8
Exclude aggregate overhead bytes: disabled
Logical interface aggregate statistics: disabled
  Scheduler map: <default>, Index: 2
  Congestion-notification: Disabled

  Logical interface: ge-1/2/0.0, Index: 338
Object                  Name                   Type                    Index
Classifier              ipprec-compatibility   ip                         13

DSCP 48 = CS6
inet-precedence 的 110/111 -> forwarding-class nc_premiumnrt <<< which is q3
```

we also checked when enable 'chassis flexible-queuing-mode', packets all go via q3, but no re-transmission packets and no bgp flap.

There's q0 drop, I am wondering how this can lead to bgp flap.

without 'chassis flexible-queuing-mode'
test-ae11.log
test-ae22.log

with 'chassis flexible-queuing-mode'
ge121.pcap
ge120.pcap


--------------

You can use the setup.

>I can see 'set chassis fpc 1 flexible-queuing-mode' stopped BGP flap.
>So this make the FPC support Q and make host packets all go via queue3?
No, that knob just enables XQCHIP on flexible-Q MPC which helps 'reducing' the chance of Q0 drop. 'host-outbound-traffic' knob is still required to put the host-outbound packets to Q3.


-----------

It was way around.
By default, XQCHIP on Flex Q board is powered off. Once I configured below (which reboots the FPC automatically) to FPC1, BGP drop has stopped.

set chassis fpc 1 flexible-queuing-mode

https://www.juniper.net/documentation/us/en/software/junos/cos/topics/topic-map/flexible-queuing-mode.html

So the source of behavior difference was XQCHIP which give us better queuing .


Dear Fujita San,

I am working on this case on behalf of Naveen, as he is on leave today.

I can see 'set chassis fpc 1 flexible-queuing-mode' stopped BGP flap.
So this make the FPC support Q and make host packets all go via queue3?

I am testing in lab to see the behavior difference. Are you using the lab now?


------------------


Hi Naotaka,

We have checked 'enhanced-queueing off’ and then by swapping MICs but could not see BGP flap.

labroot@jtac-mx960-r2038-re0# run show chassis hardware 
Hardware inventory:
Item             Version  Part number  Serial number     Description
Chassis                                JN125F641AFA      MX960
Midplane         REV 44   750-047849   ACRJ8153          Enhanced MX960 Backplane
FPM Board        REV 03   711-058968   CAJC6039          Front Panel Display
PDM              Rev 01   740-063049   QCS21195066       Power Distribution Module
PEM 0            Rev 01   740-063047   QCS2112N03T       PS 4.1kW; 200-240V AC in
PEM 1            Rev 01   740-063047   QCS2112N057       PS 4.1kW; 200-240V AC in
PEM 2            Rev 01   740-063047   QCS2112N07B       PS 4.1kW; 200-240V AC in
Routing Engine 0 REV 08   740-031116   9009126967        RE-S-1800x4
CB 0             REV 08   750-055976   CAFD3135          Enhanced MX SCB 2
FPC 1            REV 26   750-063181   CASL4704          MPC3E NG PQ & Flex Q
  CPU            REV 13   711-045719   CASL2647          RMPC PMB
  MIC 1          REV 27   750-028392   CABA7666          3D 20x 1GE(LAN) SFP
    PIC 2                 BUILTIN      BUILTIN           10x 1GE(LAN) SFP
      Xcvr 0     REV 01   740-038291   H439672           SFP-T
      Xcvr 1     REV 01   740-038291   F340199           SFP-T
    PIC 3                 BUILTIN      BUILTIN           10x 1GE(LAN) SFP
FPC 6            REV 24   750-063183   CASD9263          MPC2E NG HQoS
  CPU            REV 13   711-045719   CASD7559          RMPC PMB
  MIC 0          REV 18   750-049846   CAKA5333          3D 20x 1GE(LAN)-E,SFP
    PIC 0                 BUILTIN      BUILTIN           10x 1GE(LAN) -E  SFP
      Xcvr 1     REV 02   740-013111   9445201           SFP-T
      Xcvr 2     REV 02   740-013111   A331466           SFP-T
    PIC 1                 BUILTIN      BUILTIN           10x 1GE(LAN) -E  SFP
  MIC 1          REV 31   750-028387   CAHH4690          3D 4x 10GE  XFP
    PIC 2                 BUILTIN      BUILTIN           2x 10GE  XFP
      Xcvr 0              NON-JNPR     T15C15363         XFP-10G-SR
      Xcvr 1              NON-JNPR     T14J57608         XFP-10G-SR
    PIC 3                 BUILTIN      BUILTIN           2x 10GE  XFP
Fan Tray 0       REV 02   740-057995   ACDF8913          Enhanced Fan Tray
Fan Tray 1       REV 02   740-057995   ACDF9242          Enhanced Fan Tray


labroot@jtac-mx960-r2038-re0# run show bgp summary 
Threading mode: BGP I/O
Default eBGP mode: advertise - accept, receive - accept
Groups: 2 Peers: 2 Down peers: 0
Table          Tot Paths  Act Paths Suppressed    History Damp State    Pending
inet.0               
                       0          0          0          0          0          0
Peer                     AS      InPkt     OutPkt    OutQ   Flaps Last Up/Dwn State|#Active/Received/Accepted/Damped...
192.168.1.2           65020         52         51       0     450       22:46 Establ
  inet.0: 0/0/0/0
192.168.10.1          65000         48         51       0       5       22:49 Establ
  inet.0: 0/0/0/0

Tcpdump packet captures are available in /var/tmp/catpure_v10.pcap (ae11 ) and /var/tmpcatpure_v11.pcap (ae22) on lab router. 


---------------

Naveen,
If 'enhanced-queueing off' doesn't change anything, can you try swapping MIC between FPC1-MIC1 and FPC6-MIC0? 
I think the PHY chip/bcm54340 resides on the MACSEC MIC (3D 20x 1GE(LAN)-E,SFP) might be playing some role.

FPC 1            REV 26   750-063181   CASL4704          MPC3E NG PQ & Flex Q
  CPU            REV 13   711-045719   CASL2647          RMPC PMB
  MIC 1          REV 18   750-049846   CAKA5333          3D 20x 1GE(LAN)-E,SFP
    PIC 2                 BUILTIN      BUILTIN           10x 1GE(LAN) -E  SFP
    PIC 3                 BUILTIN      BUILTIN           10x 1GE(LAN) -E  SFP
FPC 6            REV 24   750-063183   CASD9263          MPC2E NG HQoS
  CPU            REV 13   711-045719   CASD7559          RMPC PMB
  MIC 0          REV 27   750-028392   CABA7666          3D 20x 1GE(LAN) SFP
    PIC 0                 BUILTIN      BUILTIN           10x 1GE(LAN) SFP
    PIC 1                 BUILTIN      BUILTIN           10x 1GE(LAN) SFP
    
-------------

Naveen,
You may want to configure below (to bypass XQCHIP). If this flaps BGP on NG HQoS link, we can say the behavior diff is coming from queuing system (XM v.s. XQ)
set chassis fpc 6 enhanced-queueing off

-------------

Naveen,
Please try tcpdump on the NG HQoS link. I think the drop is happening there but less frequently which would be the reason for the behavior diff.
I don't know the way other than host-outbound-traffic.

------------

Naveen,
In this scenario (heavy congestion on both directions), we do need to configure 'host-outbound-traffic forwarding-class' to make sure all the host-outbound packets go thru network-control queue.
By default, normal BGP packets are sent to Q0 and retransmitted packets are sent to Q3 to avoid further queue drop. 
This is working as expected and the retransmitted packet is reaching to the peer.

However, since both directions are congested, on the peer end the ack corresponding to the received retransmitted packet will be sent to Q0 which will be dropped. 
Depending on the timing, retransmit on peer side will not happen before the hold timeout of local end.

Below is the packet diagram when LSYS side:192.168.1.2 was seeing hold time expire.

192.168.1.2 (LSYS) was timing out this time.
Apr 21 08:44:59.761  jtac-mx960-r2038-re0 LS1-LAB-MX960-04:rpd[20852]: bgp_io_mgmt_cb:3103: NOTIFICATION sent to 192.168.1.1 (External AS 65010): code 4 (Hold Timer Expired Error), Reason: holdtime expired for 192.168.1.1 (External AS 65010), socket buffer sndacc: 95 rcvacc: 0 , socket buffer sndccc:

pcap files are saved in /var/home/labroot under your DUT MX.


![[Pasted image 20260421133724.png]]


--------------
Hi Naotaka,

We have configured static ARP but still BGP session is flapping.

Default eBGP mode: advertise - accept, receive - accept
Groups: 2 Peers: 2 Down peers: 0
Table          Tot Paths  Act Paths Suppressed    History Damp State    Pending
inet.0               
                       0          0          0          0          0          0
Peer                     AS      InPkt     OutPkt    OutQ   Flaps Last Up/Dwn State|#Active/Received/Accepted/Damped...
192.168.1.2           65020          3          2       0     416           3 Establ
  inet.0: 0/0/0/0
192.168.10.1          65000       2624       2904       0       2    21:50:47 Establ
  inet.0: 0/0/0/0


set interfaces ae11 unit 0 family inet address 192.168.1.1/30 arp 192.168.1.2 mac 58:00:bb:06:47:ce
set logical-systems LS1-LAB-MX960-04 interfaces ae22 unit 0 family inet address 192.168.1.2/30 arp 192.168.1.1 mac 58:00:bb:06:47:cd


labroot@jtac-mx960-r2038-re0# run show arp interface ae11.0 
MAC Address       Address         Name                      Interface               Flags
58:00:bb:06:47:ce 192.168.1.2     192.168.1.2               ae11.0                  permanent

[edit]
labroot@jtac-mx960-r2038-re0# run show arp interface ae22.0    
MAC Address       Address         Name                      Interface               Flags
58:00:bb:06:47:cd 192.168.1.1     192.168.1.1               ae22.0                  permanent


------------
Have you rule out ARP expire due to the congestion scenario?

>3) We noticed that on MPC3E NG PQ & Flex Q when we activate class-of-service host-outbound-traffic cli, BGP flapping was not observed. 
>Configs-
>set class-of-service host-outbound-traffic forwarding-class nc_premiumnrt
>set class-of-service host-outbound-traffic dscp-code-point cs6

If this was working, what if you configure static arp on both peers on the congested link without host-outbound-traffic knob? If it helps we can say this was due to arp drop.

Thanks,
-Nataka

----------

Hi MX Escalations team,

We request your support on this case. 
Customer SingTel is facing BGP flapping issue when 1G link is congested in both directions.

Hostname: lab-MX960-3d-04-P3-re0
Model: mx960
Junos: 23.2R2-S3.8
MPC model- MPC3E NG PQ & Flex Q
MIC model- 3D 20x 1GE(LAN)-E,SFP

Customer tested in their lab and below are the customer findings- 

LS1-LAB-MX960-04 is a logical system inside of LAB-MX960-04 and they are eBGP peering over a 1G link.
Traffic is sent from SPIRENT port 1 and port 4 and are also using eBGP peering to respective routers.
When they ran 1-way 2G traffic from 192.168.10.1 to 192.168.20.1 initially and there was no flapping.
After it they ran the return 2G traffic from 192.168.20.1 to 192.168.10.1, the BGP session flapped between 192.168.1.1 and 192.168.1.2 (1G link). It flapped due to hold timer expiry.


JTAC troubleshooting-

Lab device- jtac-mx960-r2038-re0 
Lab setup- 
![[Pasted image 20260421114115.png]]


1) We have replicated the same customer setup in JTAC lab with same MPC/MIC model (MPC3E NG PQ & Flex Q) and able to reproduce the issue. 
LS1-LAB-MX960-04 is a logical instance of jtac-mx960-r2038-re0.

Spirent tester(AS 65000) 10G port 4/8---------- port xe-6/2/0(jtac-mx960-r2038-re0 AS 65010) ---port ge-1/2/1(ae11) --------(ae22)port ge-1/2/2(logical instance LS1-LAB-MX960-04 AS 65020)----port xe-6/2/1---------10G port 4/9 Sprient tester (AS 65030)

2G traffic is sent from tester bi-directionally.

2) We have tested the same with other MPC model (MPC2E NG HQoS) but could not see the BGP flapping issue. 

3) We noticed that on MPC3E NG PQ & Flex Q when we activate class-of-service host-outbound-traffic cli, BGP flapping was not observed. 
Configs-
set class-of-service host-outbound-traffic forwarding-class nc_premiumnrt
set class-of-service host-outbound-traffic dscp-code-point cs6

4) Below are lab outputs and configs attached in mail.

labroot@jtac-mx960-r2038-re0> show bgp summary 
Threading mode: BGP I/O
Default eBGP mode: advertise - accept, receive - accept
Groups: 2 Peers: 2 Down peers: 1
Table          Tot Paths  Act Paths Suppressed    History Damp State    Pending
inet.0               
                       0          0          0          0          0          0
Peer                     AS      InPkt     OutPkt    OutQ   Flaps Last Up/Dwn State|#Active/Received/Accepted/Damped...
192.168.1.2           65020          0          0       0     232        1:27 Connect
192.168.10.1          65000        218        238       0       2     1:47:53 Establ
  inet.0: 0/0/0/0


Physical interface: ae11, Enabled, Physical link is Up
  Interface index: 173, SNMP ifIndex: 1033
Forwarding classes: 16 supported, 8 in use
Egress queues: 8 supported, 8 in use
Queue: 0, Forwarding classes: standard
  Queued:
    Packets              :           66697413726                244999 pps
    Bytes                :        66875755013830            1999199744 bps
  Transmitted:
    Packets              :           24485618475                122500 pps
    Bytes                :        24389155715669             999603968 bps
    Tail-dropped packets :           20481346904                  3730 pps
    RL-dropped packets   :                     0                     0 pps
    RL-dropped bytes     :                     0                     0 bps
    RED-dropped packets  :           21730448347                118769 pps
     Low                 :           21730448347                118769 pps
     Medium-low          :                     0                     0 pps
     Medium-high         :                     0                     0 pps
     High                :                     0                     0 pps
    RED-dropped bytes    :        22165050472732             969154944 bps
     Low                 :        22165050472732             969154944 bps
     Medium-low          :                     0                     0 bps
     Medium-high         :                     0                     0 bps
     High                :                     0                     0 bps
  Queue-depth bytes      : 
    Average              :              11994496
    Current              :              11782913
    Peak                 :              12124160
    Maximum              :              12124160



5) Without activating host-outbound from cli-
labroot@jtac-mx960-r2038-re0# run show class-of-service host-outbound-traffic      
---------        ----  ------  -----  -----  --- ----  --------   -------------
protocol         dscp  802.1p  FC-id  queue  restrict  override   use 802.1p
   name                                       queue     firewall   rewrite rule
---------        ----  ------  -----  -----  --- ----  --------   -------------
default             0       0      0      0        0         no        no

After activating from cli-
labroot@jtac-mx960-r2038-re0# run show class-of-service host-outbound-traffic             
---------        ----  ------  -----  -----  --- ----  --------   -------------
protocol         dscp  802.1p  FC-id  queue  restrict  override   use 802.1p
   name                                       queue     firewall   rewrite rule
---------        ----  ------  -----  -----  --- ----  --------   -------------
default            48       0      3      3        3         no        no

6) We noticed that with MPC2E NG HQoS we could see TAIL drops in queue 0 but in MPC3E NG PQ & Flex Q we are seeing RED drops without any drop profile configuration.

7) We observed the CLI op from PFE CoS does not change. 

NGMPC1(jtac-mx960-r2038-re0 vty)# show cos halp ifd 189

--------------------------------------------------------------------------------
    rich queueing enabled: 0
    Q chip present: 0
IFD name: ge-1/2/1   (Index 189) egress information
    XM chip id: 0
    XM : chip Scheduler: 0
    XM chip L1 index: 66
    XM chip dummy L2 index: 132
    XM chip base Q index: 528
    Number of queues: 8
    Rich queuing support: 0 (ifl queued:0)
--------------------------------------------------------------------------------------------
Queue  State        Max           Guaranteed    Burst       Weight  G-Pri  E-Pri  WRED  TAIL
Index               Rate          Rate          Size                              Rule  Rule
--------------------------------------------------------------------------------------------
528    Configured   1000000000    950000000     131064      118     GL     EL     4     89  
529    Configured   1000000000    0             131064      1       GL     EL     0     1   
530    Configured   1000000000    0             131064      1       GL     EL     0     1   
531    Configured   1000000000    50000000      131064      6       GL     EL     4     24  
532    Configured   1000000000    0             131064      1       GL     EL     0     1   
533    Configured   1000000000    0             131064      1       GL     EL     0     1   
534    Configured   1000000000    0             131064      1       GL     EL     0     1   
535    Configured   1000000000    0             131064      1       GL     EL     0     1   


File location-


/volume/CSdata/nsivakumaran/2026-0417-678815/

20260417_lab-mx960-04_RSI_Config.log
20260417_lab-mx960-04_varlogs/var/log/
'Ticket 20320 diagram with Spirent.pptx'- Customer lab setup diagram
20260417_lab-mx960-04_bgp1gsessionflapped15times_logs.txt - cli outputs
20260420_lab-mx960-04_outputs.log - cli outputs
20260420_lab-mx960-04_outputs_2.log - FPC qos outputs


