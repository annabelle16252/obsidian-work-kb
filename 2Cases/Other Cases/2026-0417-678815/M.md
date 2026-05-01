2026-0417-678815 [Singtel:Singtel-IX:xn-sincck1-bo102] BGP Session Flaps when the 1G link is fully congested in both directions | Ticket #20320

Hostname: lab-MX960-3d-04-P3-re0
Model: mx960
Junos: 23.2R2-S3.8

/volume/case_2026/2026-0417-678815
ssh annaw@p-jtac-lnx02.juniper.net
rsi-varlog /volume/CSdata/annaw/case/2026-0417-678815




--------------


Hi @Tong Chee Loong,

We have tested the congestion in below setups and could not see BGP flapping issue with the attached configuration.

We have created an outbound firewall filter which will match locally generated BGP packets and map to Q3. This filter is attached to lo0 out direction. 
We have used ARP hidden commands which will make ARP packets to go via Q3 instead of Q0. 
Host outbound command is deactivated. 

set system arp arp-request-bump-priority
set system arp arp-reply-bump-priority


Setup 1-
image.png
jtac-mx960-r2038-re0

set logical-systems LS1-LAB-MX960-04 interfaces lo0 unit 1 family inet filter output LOCAL_BGP
set interfaces lo0 unit 0 family inet filter output LOCAL_BGP
set firewall family inet filter LOCAL_BGP term 1 from protocol tcp
set firewall family inet filter LOCAL_BGP term 1 from port bgp
set firewall family inet filter LOCAL_BGP term 1 then count test
set firewall family inet filter LOCAL_BGP term 1 then log
set firewall family inet filter LOCAL_BGP term 1 then loss-priority low
set firewall family inet filter LOCAL_BGP term 1 then forwarding-class nc_premiumnrt
set firewall family inet filter LOCAL_BGP term 1 then accept
set firewall family inet filter LOCAL_BGP term 1 then dscp cs6
set firewall family inet filter LOCAL_BGP term 2 then accept

set class-of-service host-outbound-traffic forwarding-class nc_premiumnrt
set class-of-service host-outbound-traffic dscp-code-point cs6
deactivate class-of-service host-outbound-traffic

set system arp arp-request-bump-priority
set system arp arp-reply-bump-priority


labroot@jtac-mx960-r2038-re0> show bgp summary                                
Threading mode: BGP I/O
Default eBGP mode: advertise - accept, receive - accept
Groups: 4 Peers: 4 Down peers: 0
Table          Tot Paths  Act Paths Suppressed    History Damp State    Pending
inet.0               
                       0          0          0          0          0          0
Peer                     AS      InPkt     OutPkt    OutQ   Flaps Last Up/Dwn State|#Active/Received/Accepted/Damped...
192.168.1.2           65020       1179       1169       0      94     8:50:52 Establ
  inet.0: 0/0/0/0
192.168.2.2           65120       1870       1860       0      32    14:05:08 Establ
  inet.0: 0/0/0/0
192.168.10.1          65000       2826       3105       0       2    23:31:58 Establ
  inet.0: 0/0/0/0
192.168.30.1          65100       2826       3106       0       3    23:31:59 Establ

Setup 2-

image.png
jtac-mx960-r2038-re0
>>>>>>>
set interfaces lo0 unit 0 family inet filter output LOCAL_BGP
set firewall family inet filter LOCAL_BGP term 1 from protocol tcp
set firewall family inet filter LOCAL_BGP term 1 from port bgp
set firewall family inet filter LOCAL_BGP term 1 then count test
set firewall family inet filter LOCAL_BGP term 1 then log
set firewall family inet filter LOCAL_BGP term 1 then loss-priority low
set firewall family inet filter LOCAL_BGP term 1 then forwarding-class nc_premiumnrt
set firewall family inet filter LOCAL_BGP term 1 then accept
set firewall family inet filter LOCAL_BGP term 1 then dscp cs6
set firewall family inet filter LOCAL_BGP term 2 then accept

set class-of-service host-outbound-traffic forwarding-class nc_premiumnrt
set class-of-service host-outbound-traffic dscp-code-point cs6
deactivate class-of-service host-outbound-traffic

set system arp arp-request-bump-priority
set system arp arp-reply-bump-priority

>>>>>
jtac-mx960-r2053-re0

set interfaces lo0 unit 0 family inet filter output LOCAL_BGP
set firewall family inet filter LOCAL_BGP term 1 from dscp cs6
set firewall family inet filter LOCAL_BGP term 1 from protocol tcp
set firewall family inet filter LOCAL_BGP term 1 from port bgp
set firewall family inet filter LOCAL_BGP term 1 then count test
set firewall family inet filter LOCAL_BGP term 1 then log
set firewall family inet filter LOCAL_BGP term 1 then forwarding-class nc_premiumnrt
set firewall family inet filter LOCAL_BGP term 1 then accept
set firewall family inet filter LOCAL_BGP term 2 then accept

set class-of-service host-outbound-traffic forwarding-class nc_premiumnrt
set class-of-service host-outbound-traffic dscp-code-point cs6
deactivate class-of-service host-outbound-traffic


set system arp arp-request-bump-priority
set system arp arp-reply-bump-priority


labroot@jtac-mx480-r2053-re0> show bgp summary    
Threading mode: BGP I/O
Default eBGP mode: advertise - accept, receive - accept
Groups: 2 Peers: 2 Down peers: 0
Table          Tot Paths  Act Paths Suppressed    History Damp State    Pending
inet.0               
                       0          0          0          0          0          0
Peer                     AS      InPkt     OutPkt    OutQ   Flaps Last Up/Dwn State|#Active/Received/Accepted/Damped...
192.168.2.1           65110       1863       1870       0      24    14:05:31 Establ
  inet.0: 0/0/0/0
192.168.40.2          65140       4940       5463       0       0 1d 17:08:58 Establ
  inet.0: 0/0/0/0

Kindly check this and update us.

-----------

Hi Naveen,
 
With the host-outbound-traffic configuration below, which other protocols, apart from BGP control packets, will be directed to queue 3?
 
cheeloong.tong@lab-MX960-3d-04-P3-re0> show configuration class-of-service host-outbound-traffic
Apr 28 11:05:30
forwarding-class nc_premiumnrt;
dscp-code-point cs6;.

-----------------


Hi Naveen,
 
I have changed the line card from MPC3E-3D-NG to MPC2E-3D-NG-Q ge-3/0/0 and still observed BGP flapping but with very few occurrence.
 
It flapped 3 times from yesterday 3pm till now.
 
Do you observe any BGP flapping when testing with non-MPC3E-3D-NG line card?
 
Also, are there any complications using the host-outbound-traffic knob?


-----------------


Dear Tong,

I am working on this case on behalf of Naveen, as he is on leave today.

By default, normal BGP packets are sent to Q0 and retransmitted packets are sent to Q3 to avoid further queue drop. This is working as expected and the retransmitted packet is reaching to the peer.

However, since both directions are congested, on the peer end the ACK corresponding to the received retransmitted packet will be sent to Q0 which will be dropped. Depending on the timing, retransmit on peer side will not happen before the hold timeout of local end.

The 'host-outbound-traffic' knob  put all host bound packets to queue 3 which will avoid BGP flaps when queue 0 is fully congested.

The bgp flap issue can also be reduced/resolved by enable 'chassis fpc 1 flexible-queuing-mode' which will enable the flex-queuing for MPC3E NG PQ & Flex Q and helps reducing the chance of queue 0 drop.
https://www.juniper.net/documentation/us/en/software/junos/cos/topics/topic-map/flexible-queuing-mode.html
The queuing component on non-HQoS MPCs is disabled by default to save power. When flexible queuing is enabled on a non-HQoS MPC, the MPC is restarted with the queuing component enabled.  The 'host-outbound-traffic' knob is still required.

Flexible queuing mode mainly improves interface resilience under bursty congestion, enables finer-grained scheduling, and scales better. The tradeoff is that packet-drop behavior and bandwidth sharing under congestion may differ from the old mode, so QoS for critical traffic should be re-validated.




In practical terms, chassis flexible-queuing-mode mainly makes the interface more resilient to bursty traffic under congestion, provides finer-grained scheduling, and improves scalability, but the tradeoff is that actual packet-drop behavior and bandwidth sharing under congestion will not be exactly the same as in the old mode, so QoS for critical traffic should be re-validated.

The most likely mechanism is that, before flexible queuing was enabled, q0 congestion caused loss or excessive delay of BGP control-plane packets such as keepalives, updates, or TCP ACKs, which led to session timeout or reset. After enabling flexible queuing, the larger burst handling and different queue/drop behavior reduced that packet loss sensitivity, so the BGP session remained stable under the same congestion pattern.

-------------------------
Hi Naveen,

Input my few insights here.

The only difference between your lab and customer's (besides MPC type different) is that you have 'tail drop', whereas customer has 'RED drop'.
Normally 'RED drop' can be triggered by either 'drop-profile' or 'burst traffic'.

I tried to increase to a very aggressive burst traffic but without luck. 

I then further checked the RED drop status and found drop rate is 50% and queue-depth items having similar numbers which is not likely there's no drop-profile applied.
```
Physical interface: ge-1/2/0, Enabled, Physical link is Up
  Interface index: 197, SNMP ifIndex: 571
Forwarding classes: 16 supported, 8 in use
Egress queues: 8 supported, 8 in use
Queue: 0, Forwarding classes: standard
  Queued:
    Packets              :           12450538787                176037 pps
    Bytes                :        17679757691000            1999791616 bps
  Transmitted:
    Packets              :            6225323646                 88008 pps
    Bytes                :         8839953627517             999776512 bps
    Tail-dropped packets :                   467                     0 pps
    RL-dropped packets   :                     0                     0 pps
    RL-dropped bytes     :                     0                     0 bps
    RED-dropped packets  :            6225214674                 88029 pps
     Low                 :            6225214674                 88029 pps
     Medium-low          :                     0                     0 pps
     Medium-high         :                     0                     0 pps
     High                :                     0                     0 pps
    RED-dropped bytes    :         8839803400343            1000015104 bps
     Low                 :         8839803400343            1000015104 bps
     Medium-low          :                     0                     0 bps
     Medium-high         :                     0                     0 bps
     High                :                     0                     0 bps
  Queue-depth bytes      : 
    Average              :              11992576
    Current              :              12008449
    Peak                 :              12032124
    Maximum              :              12124160
```

Then I added drop-profile to ae11&ae22 in your lab and can replicate the issue:
```
set interfaces ae11 aggregated-ether-options lacp active
set interfaces ae11 unit 0 family inet address 192.168.1.1/30
set class-of-service interfaces ae11 scheduler-map TRUNK-scheduler-6COS

set class-of-service schedulers TRUNK-standard-scheduler-6COS drop-profile-map loss-priority low protocol any drop-profile standard
<<< trigger

RED drop started and bgp flap also can be observed:
Table          Tot Paths  Act Paths Suppressed    History Damp State    Pending
inet.0               
                       0          0          0          0          0          0
Peer                     AS      InPkt     OutPkt    OutQ   Flaps Last Up/Dwn State|#Active/Received/Accepted/Damped...
192.168.1.2           65020          8          7       0       8        2:39 Establ
  inet.0: 0/0/0/0
192.168.10.1          65000       5505       6089       0       0 1d 21:51:05 Establ
  inet.0: 0/0/0/0

```

The BGP flap may due to BGP ACK packets which use queue 0 was dropped (need to do further investigation):
<<<
ACK drop → TCP stuck → keepalive stuck → hold timer expire → BGP flap
<<<

From customer's RSI, there's no drop-profile configured on ae11&ae22, however we do see RED drop which is wired. My suspect is that it might be related to some unexpected behavior on the MPC3 card. I can see there's drop-profile applied to the other interfaces on the same FPC/MIC/PIC level, which may accidentally applied to ge-1/0/2 as well. Please collect below output from customer:
<<<
show class-of-service interface ge-1/2/0
show class-of-service interface ae11
show class-of-service drop-profile
show configuration | match ae11 | display set | display inheritance 
show configuration | match ge-1/2/0 | display set | display inheritance 
<<<

You can try below in your lab:
Request a same MPC3 card and apply droop-profile to the other interfaces in the same FPC. 
Or you can add apply droop-profile to the other interfaces in the current MPC2 card to see.

# Case
Hi CFTS,

1./ Singtel has requested for a lab testing.
They wanted to check if the BGP session will flap if a 1G link is fully congested in both directions.

2./ We have tested it, you can refer to the uploaded lab network diagram.
In our lab LS1-LAB-MX960-04 is a logical system inside of LAB-MX960-04 and they are eBGP peerings over a 1G link.
Traffic is sent from SPIRENT port 1 and port 4, and are also using eBGP peering to respective routers.
When we ran 1-way 2G traffic from 192.168.10.1 to 192.168.20.1 initially and there was no flapping.
After we ran the return 2G traffic from 192.168.20.1 to 192.168.10.1, the BGP session flapped between 192.168.1.1 and 192.168.1.2 (1G link). It flapped due to hold timer expiry.

3./ In theory, BGP and data traffic uses queue 0 at 95%.
When BGP needs to retransmit the control packets, it will use queue 3 at 5%
So there shouldn't be any BGP session flapping.
In our lab, there were no queue 3 drops in "show interfaces queue ge-1/2/0 and ge-1/2/1"

4./ I have attached the RSI, LAB-MX960-04 and LS1-LAB-MX960-04 configuration.

5./ Please check why this is happening. Thank you.