





----------
You can try on other combo in the lab if you want.

>Also may I confirm if need to collect below 'Transmitted packets' multiple times or only one time is enough and record the number? <<<< NGMPC0(jtac-mx480-r2033-re0 vty)# sh xmchip 0 wo stats 1 WO statistics (WAN block 1)
I guess one time would be enough as we have pps counter. Of course getting multiple times won't hurt anything. 

Thx,
-Naotaka



-------------

Dear Fujita San,

Thanks for the detail workplan and I will ask customer to collect. Since it's xmchip based, it will be applicable for all FPC cards from MPC2 to MPC6 cards, right?

Also may I confirm if need to collect below 'Transmitted packets' multiple times or only one time is enough and record the number?
<<<<
NGMPC0(jtac-mx480-r2033-re0 vty)# sh xmchip 0 wo stats 1
WO statistics (WAN block 1)
---------------------------

Counter set 0

    Stream number mask       : 0x7f
    Stream number match      : 0x24
    Transmitted packets      : 171871 (44 pps)       -> check this
    Transmitted bytes        : 259766760 (532032 bps)
    Transmitted cells        : 4116961 (1054 cps)

Counter set 1

    Stream number mask       : 0x70
    Stream number match      : 0x0
    Transmitted packets      : 0 (0 pps)
    Transmitted bytes        : 0 (0 bps)
    Transmitted cells        : 0 (0 cps)
<<<<

--------------


Hi Annabelle,

When the problem happens again, please get followings before resolving the problem. This would identify where the packet is dropped.



Let's take ge-0/2/0 as an example.
labroot@jtac-mx480-r2033-re0> show arp no-resolve
MAC Address       Address         Interface                Flags
10:0e:7e:3f:b2:a0 10.0.0.2        ge-0/2/0.0               none

-> From RE, keep pinging to arp neighbor.
labroot@jtac-mx480-r2033-re0> ping 10.0.0.2 size 65000
PING 10.0.0.2 (10.0.0.2): 65000 data bytes
65008 bytes from 10.0.0.2: icmp_seq=0 ttl=64 time=9.869 ms
65008 bytes from 10.0.0.2: icmp_seq=1 ttl=64 time=11.631 ms


-> check interface counter at least twice.

Router>show interfaces ge-0/2/0 extensive| no-more
<wait for 5 sec>
Router>show interfaces ge-0/2/0 extensive| no-more

Below need to be done from FPC shell, 

-> check the phy-stream number for DUT interface. 
NGMPC0(jtac-mx480-r2033-re0 vty)# sh xmchip 0 ifd list 1

Egress IFD list
---------------

--------------------------------------------------------------------
IFD name         IFD    PHY     Scheduler  L1 Node  Base   Number of
                 Index  Stream                      Queue  Queues
--------------------------------------------------------------------
xe-0/0/0         150    1024    WAN        0        0      8
xe-0/1/0         151    1025    WAN        32       256    8
ge-0/2/0         152    1188    WAN        64       512    8                        -> Phy-stream: 1188 
ge-0/2/1         153    1189    WAN        66       528    8
ge-0/2/2         154    1190    WAN        67       536    8
ge-0/2/3         155    1191    WAN        68       544    8
ge-0/2/4         156    1192    WAN        69       552    8
ge-0/2/5         157    1193    WAN        70       560    8
ge-0/2/6         158    1194    WAN        71       568    8
ge-0/2/7         159    1195    WAN        74       592    8
ge-0/2/8         160    1196    WAN        75       600    8
ge-0/2/9         161    1197    WAN        76       608    8
ge-0/3/0         162    1198    WAN        96       768    8
ge-0/3/1         163    1199    WAN        97       776    8
ge-0/3/2         164    1200    WAN        98       784    8
ge-0/3/3         165    1201    WAN        99       792    8
ge-0/3/4         166    1202    WAN        100      800    8
ge-0/3/5         167    1203    WAN        101      808    8
ge-0/3/6         168    1204    WAN        102      816    8
ge-0/3/7         169    1205    WAN        103      824    8
ge-0/3/8         170    1206    WAN        104      832    8
ge-0/3/9         171    1207    WAN        105      840    8
--------------------------------------------------------------------

-> set WO stats stream# for the DUT interface 
NGMPC0(jtac-mx480-r2033-re0 vty)# test xmchip 0 wo stats stream 0 1188

-> Here is how to identify WO block# from phy-stream
Block 0: 1024..1151, Block 1: 1152..1279
        

-> NOTE: last digit '1' indicates wo Block. If Phy=stream is in the range of 1024 - 1151, block should be 0
NGMPC0(jtac-mx480-r2033-re0 vty)# sh xmchip 0 wo stats 1

WO statistics (WAN block 1)
---------------------------

Counter set 0

    Stream number mask       : 0x7f
    Stream number match      : 0x24
    Transmitted packets      : 171871 (44 pps)                                -> check this
    Transmitted bytes        : 259766760 (532032 bps)
    Transmitted cells        : 4116961 (1054 cps)

Counter set 1

    Stream number mask       : 0x70
    Stream number match      : 0x0
    Transmitted packets      : 0 (0 pps)
    Transmitted bytes        : 0 (0 bps)
    Transmitted cells        : 0 (0 cps)


--> IX counter

NGMPC0(jtac-mx480-r2033-re0 vty)# sh ixchip ifd

   IFD       IFD        IX     WAN       Ing Queue     Egr Queue
  Index      Name       Id     Port      Rt/Ct/Be      H/L
  ======  ==========  ======  ======  ==============  ======
   150     xe-0/0/0      0      24       24/56/88      24/56
   151     xe-0/1/0      0      25       25/57/89      25/57
   168     ge-0/3/6      2       0        0/32/64       0/32
   169     ge-0/3/7      2       1        1/33/65       1/33
   170     ge-0/3/8      2       2        2/34/66       2/34
   171     ge-0/3/9      2       4        4/36/68       4/36
   152     ge-0/2/0      2       8        8/40/72       8/40                  -> IX:2 port 8
   153     ge-0/2/1      2       9        9/41/73       9/41
   154     ge-0/2/2      2      10       10/42/74      10/42
   155     ge-0/2/3      2      11       11/43/75      11/43
   156     ge-0/2/4      2      12       12/44/76      12/44
   157     ge-0/2/5      2      13       13/45/77      13/45
   158     ge-0/2/6      2      14       14/46/78      14/46
   159     ge-0/2/7      2      15       15/47/79      15/47
   160     ge-0/2/8      2      16       16/48/80      16/48
   161     ge-0/2/9      2      17       17/49/81      17/49
   162     ge-0/3/0      2      18       18/50/82      18/50
   163     ge-0/3/1      2      19       19/51/83      19/51
   164     ge-0/3/2      2      20       20/52/84      20/52
   165     ge-0/3/3      2      21       21/53/85      21/53
   166     ge-0/3/4      2      22       22/54/86      22/54
   167     ge-0/3/5      2      23       23/55/87      23/55
   
   
NGMPC0(jtac-mx480-r2033-re0 vty)# sh ixchip 2 statistics wan_port 8
IXCHIP 2 IX wan port 8 stats:

<>

EGRESS Traffic class - Low, Queue 8
IXCHIP 2 Egress Buffer Mgr Queue 8 stats:

               Counter Name            Total           Rate      Peak Rate
   ------------------------ ---------------- -------------- --------------
        Receive  Byte Count        836880221          66504        6118368
        Receive  EOP  Count           553857             44           4048
        Receive  EOPE Count                0              0              0
        Transmit Byte Count        836880221          66504        6118368
        Transmit EOP  Count           553857             44           4048
        Transmit EOPE Count                0              0              0

EGRESS Traffic class - High, Queue 40
IXCHIP 2 Egress Buffer Mgr Queue 40 stats:

               Counter Name            Total           Rate      Peak Rate
   ------------------------ ---------------- -------------- --------------
        Receive  Byte Count                0              0              0
        Receive  EOP  Count                0              0              0
        Receive  EOPE Count                0              0              0
        Transmit Byte Count                0              0              0
        Transmit EOP  Count                0              0              0
        Transmit EOPE Count                0              0              0


NGMPC0(jtac-mx480-r2033-re0 vty)# sh mtip-ge summary
 ID mtip_ge name         FPC PIC ifd                  (ptr)
--- -------------------- --- --- -------------------- --------
  1 mtip_ge.32.32.0       32  32                      1a310408
  2 mtip_ge.32.32.1       32  32                      1a310360
  3 mtip_ge.0.2.0          0   2 ge-0/2/0             251c1680                      <--- index 3
  4 mtip_ge.0.2.1          0   2 ge-0/2/1             251c1a70
  5 mtip_ge.0.2.2          0   2 ge-0/2/2             251c1530
  6 mtip_ge.0.2.3          0   2 ge-0/2/3             251c1488
  7 mtip_ge.0.2.4          0   2 ge-0/2/4             251c1290
  8 mtip_ge.0.2.5          0   2 ge-0/2/5             251c11e8
  9 mtip_ge.0.2.6          0   2 ge-0/2/6             251c0ff0
 10 mtip_ge.0.2.7          0   2 ge-0/2/7             251c0f48
 11 mtip_ge.0.2.8          0   2 ge-0/2/8             251c0ea0
 12 mtip_ge.0.2.9          0   2 ge-0/2/9             251c0df8
 13 mtip_ge.0.3.0          0   3 ge-0/3/0             251c2100
 14 mtip_ge.0.3.1          0   3 ge-0/3/1             251c2058
 15 mtip_ge.0.3.2          0   3 ge-0/3/2             251c2250
 16 mtip_ge.0.3.3          0   3 ge-0/3/3             251c1140
 17 mtip_ge.0.3.4          0   3 ge-0/3/4             251c21a8
 18 mtip_ge.0.3.5          0   3 ge-0/3/5             251c1098
 19 mtip_ge.0.3.6          0   3 ge-0/3/6             251c17d0
 20 mtip_ge.0.3.7          0   3 ge-0/3/7             251c0c00
 21 mtip_ge.0.3.8          0   3 ge-0/3/8             251c1338
 22 mtip_ge.0.3.9          0   3 ge-0/3/9             251c19c8

-> MTIP counter
-> Pls get below twice with 5 sec interval
NGMPC0(jtac-mx480-r2033-re0 vty)# sh mtip-ge 3 statistics
<wait 5 sec>
NGMPC0(jtac-mx480-r2033-re0 vty)# sh mtip-ge 3 statistics

-> Please get FPC live core twice
NGMPC0(jtac-mx480-r2033-re0 vty)#write core
NGMPC0(jtac-mx480-r2033-re0 vty)#write core

Thanks,
-Naotaka




---------------

Hi Fujita San,


From SFDC, I only can find below RSI for same node MX-BP-02-06 and the MAC statistics didnt show much error as this time for MIC0-FPC3.  I will ask customer to provide if you need more previous RSI to check:

/volume/CSdata/annaw/case/2026-0310-628387/prev-case-rsi:
20260212-MX-BP-02-06-RSI.log:
<<<<<
Physical interface: ge-3/0/0, Enabled, Physical link is Up
    Errors: 0, Drops: 0, Framing errors: 0, Runts: 0, Policed discards: 0, L3 incompletes: 0, L2 channel errors: 0, L2 mismatch timeouts: 0, FIFO errors: 0, Resource 
errors: 0
    Carrier transitions: 3, Errors: 0, Drops: 0, Collisions: 0, Aged packets: 0, FIFO errors: 0, HS link CRC errors: 0, MTU errors: 0, Resource errors: 0
  MAC statistics:                      Receive         Transmit
    CRC/Align errors                         0                0
    FIFO errors                              0                0
    MAC control frames                       0                0
    Total errors                             0                0
Physical interface: ge-3/0/1, Enabled, Physical link is Up
    Errors: 0, Drops: 0, Framing errors: 0, Runts: 0, Policed discards: 0, L3 incompletes: 0, L2 channel errors: 0, L2 mismatch timeouts: 0, FIFO errors: 0, Resource 
errors: 0
    Carrier transitions: 1, Errors: 0, Drops: 0, Collisions: 0, Aged packets: 0, FIFO errors: 0, HS link CRC errors: 0, MTU errors: 0, Resource errors: 0
  MAC statistics:                      Receive         Transmit
    CRC/Align errors                         0                0
    FIFO errors                              0                0
    MAC control frames                       0                0
    Total errors                             0                0
Physical interface: ge-3/0/2, Enabled, Physical link is Up
    Errors: 0, Drops: 0, Framing errors: 0, Runts: 0, Policed discards: 0, L3 incompletes: 0, L2 channel errors: 0, L2 mismatch timeouts: 0, FIFO errors: 0, Resource 
errors: 0
    Carrier transitions: 9, Errors: 0, Drops: 0, Collisions: 0, Aged packets: 0, FIFO errors: 0, HS link CRC errors: 0, MTU errors: 0, Resource errors: 0
  MAC statistics:                      Receive         Transmit
    CRC/Align errors                         0                0
    FIFO errors                              0                0
    MAC control frames                       0                0
    Total errors                             0                0
Physical interface: ge-3/0/3, Enabled, Physical link is Up
    Errors: 440, Drops: 0, Framing errors: 440, Runts: 0, Policed discards: 0, L3 incompletes: 0, L2 channel errors: 0, L2 mismatch timeouts: 0, FIFO errors: 0, Resou
rce errors: 0
    Carrier transitions: 11, Errors: 0, Drops: 0, Collisions: 0, Aged packets: 0, FIFO errors: 0, HS link CRC errors: 0, MTU errors: 0, Resource errors: 0
  MAC statistics:                      Receive         Transmit
    CRC/Align errors                       440                0
    FIFO errors                              0                0
    MAC control frames                       0                0
    Total errors                           440                0
Physical interface: ge-3/0/4, Enabled, Physical link is Up
    Errors: 0, Drops: 0, Framing errors: 0, Runts: 0, Policed discards: 0, L3 incompletes: 0, L2 channel errors: 0, L2 mismatch timeouts: 0, FIFO errors: 0, Resource 
errors: 0
    Carrier transitions: 3, Errors: 0, Drops: 0, Collisions: 0, Aged packets: 0, FIFO errors: 0, HS link CRC errors: 0, MTU errors: 0, Resource errors: 0
  MAC statistics:                      Receive         Transmit
    CRC/Align errors                         0                0
    FIFO errors                              0                0
    MAC control frames                       0                0
    Total errors                             0                0
Physical interface: ge-3/0/5, Enabled, Physical link is Up
    Errors: 0, Drops: 0, Framing errors: 0, Runts: 0, Policed discards: 0, L3 incompletes: 0, L2 channel errors: 0, L2 mismatch timeouts: 0, FIFO errors: 0, Resource 
errors: 0
    Carrier transitions: 5, Errors: 0, Drops: 0, Collisions: 0, Aged packets: 0, FIFO errors: 0, HS link CRC errors: 0, MTU errors: 0, Resource errors: 0
  MAC statistics:                      Receive         Transmit
    CRC/Align errors                         0                0
    FIFO errors                              0                0
    MAC control frames                       0                0
    Total errors                             0                0
Physical interface: ge-3/0/7, Enabled, Physical link is Up
    Errors: 0, Drops: 0, Framing errors: 0, Runts: 0, Policed discards: 0, L3 incompletes: 0, L2 channel errors: 0, L2 mismatch timeouts: 0, FIFO errors: 0, Resource 
errors: 0
    Carrier transitions: 3, Errors: 0, Drops: 0, Collisions: 0, Aged packets: 0, FIFO errors: 0, HS link CRC errors: 0, MTU errors: 0, Resource errors: 0
  MAC statistics:                      Receive         Transmit
    CRC/Align errors                         0                0
    FIFO errors                              0                0
    MAC control frames                       0                0
    Total errors                             0                0
Physical interface: ge-3/0/9, Enabled, Physical link is Up
    Errors: 0, Drops: 0, Framing errors: 0, Runts: 0, Policed discards: 0, L3 incompletes: 0, L2 channel errors: 0, L2 mismatch timeouts: 0, FIFO errors: 0, Resource 
errors: 0
    Carrier transitions: 1, Errors: 0, Drops: 0, Collisions: 0, Aged packets: 0, FIFO errors: 0, HS link CRC errors: 0, MTU errors: 0, Resource errors: 0
  MAC statistics:                      Receive         Transmit
    CRC/Align errors                         0                0
    FIFO errors                              0                0
    MAC control frames                       0                0
    Total errors                             0                0
Physical interface: ge-3/1/0, Enabled, Physical link is Up
    Errors: 165, Drops: 0, Framing errors: 165, Runts: 0, Policed discards: 0, L3 incompletes: 0, L2 channel errors: 0, L2 mismatch timeouts: 0, FIFO errors: 0, Resou
rce errors: 0
    Carrier transitions: 7, Errors: 0, Drops: 0, Collisions: 0, Aged packets: 0, FIFO errors: 0, HS link CRC errors: 0, MTU errors: 0, Resource errors: 0
  MAC statistics:                      Receive         Transmit
    CRC/Align errors                       165                0
    FIFO errors                              0                0
    MAC control frames                       0                0
    Total errors                           165                0
Physical interface: ge-3/1/1, Enabled, Physical link is Up
    Errors: 13970, Drops: 0, Framing errors: 13970, Runts: 0, Policed discards: 0, L3 incompletes: 0, L2 channel errors: 0, L2 mismatch timeouts: 0, FIFO errors: 0, R
esource errors: 0
    Carrier transitions: 1, Errors: 0, Drops: 0, Collisions: 0, Aged packets: 0, FIFO errors: 0, HS link CRC errors: 0, MTU errors: 0, Resource errors: 0
  MAC statistics:                      Receive         Transmit
    CRC/Align errors                     13970                0
    FIFO errors                              0                0
    MAC control frames                       0                0
    Total errors                         13988                0
Physical interface: ge-3/1/2, Enabled, Physical link is Up
    Errors: 0, Drops: 0, Framing errors: 0, Runts: 0, Policed discards: 0, L3 incompletes: 0, L2 channel errors: 0, L2 mismatch timeouts: 0, FIFO errors: 0, Resource 
errors: 0
    Carrier transitions: 1, Errors: 0, Drops: 0, Collisions: 0, Aged packets: 0, FIFO errors: 0, HS link CRC errors: 0, MTU errors: 0, Resource errors: 0
  MAC statistics:                      Receive         Transmit
    CRC/Align errors                         0                0
    FIFO errors                              0                0
    MAC control frames                       0                0
    Total errors                             0                0
Physical interface: ge-3/1/3, Enabled, Physical link is Up
    Errors: 0, Drops: 0, Framing errors: 0, Runts: 0, Policed discards: 0, L3 incompletes: 0, L2 channel errors: 0, L2 mismatch timeouts: 0, FIFO errors: 0, Resource 
errors: 0
    Carrier transitions: 1, Errors: 0, Drops: 0, Collisions: 0, Aged packets: 0, FIFO errors: 0, HS link CRC errors: 0, MTU errors: 0, Resource errors: 0
  MAC statistics:                      Receive         Transmit
    CRC/Align errors                         0                0
    FIFO errors                              0                0
    MAC control frames                       0                0
    Total errors                             0                0
Physical interface: ge-3/1/4, Enabled, Physical link is Up
    Errors: 0, Drops: 0, Framing errors: 0, Runts: 0, Policed discards: 0, L3 incompletes: 0, L2 channel errors: 0, L2 mismatch timeouts: 0, FIFO errors: 0, Resource 
errors: 0
    Carrier transitions: 1, Errors: 0, Drops: 0, Collisions: 0, Aged packets: 0, FIFO errors: 0, HS link CRC errors: 0, MTU errors: 0, Resource errors: 0
  MAC statistics:                      Receive         Transmit
    CRC/Align errors                         0                0
    FIFO errors                              0                0
    MAC control frames                       0                0
    Total errors                             0                0
Physical interface: ge-3/1/6, Enabled, Physical link is Up
    Errors: 0, Drops: 0, Framing errors: 0, Runts: 0, Policed discards: 0, L3 incompletes: 0, L2 channel errors: 0, L2 mismatch timeouts: 0, FIFO errors: 0, Resource 
errors: 0
    Carrier transitions: 1, Errors: 0, Drops: 0, Collisions: 0, Aged packets: 0, FIFO errors: 0, HS link CRC errors: 0, MTU errors: 0, Resource errors: 0
  MAC statistics:                      Receive         Transmit
    CRC/Align errors                         0                0
    FIFO errors                              0                0
    MAC control frames                       0                0
    Total errors                             0                0
Physical interface: ge-3/1/7, Enabled, Physical link is Up
    Errors: 0, Drops: 0, Framing errors: 0, Runts: 0, Policed discards: 0, L3 incompletes: 0, L2 channel errors: 0, L2 mismatch timeouts: 0, FIFO errors: 0, Resource 
errors: 0
    Carrier transitions: 3, Errors: 0, Drops: 0, Collisions: 0, Aged packets: 0, FIFO errors: 0, HS link CRC errors: 0, MTU errors: 0, Resource errors: 0
  MAC statistics:                      Receive         Transmit
    CRC/Align errors                         0                0
    FIFO errors                              0                0
    MAC control frames                       0                0
    Total errors                             0                0
Physical interface: ge-3/2/0, Enabled, Physical link is Up
    Errors: 0, Drops: 0, Framing errors: 0, Runts: 0, Policed discards: 0, L3 incompletes: 0, L2 channel errors: 0, L2 mismatch timeouts: 0, FIFO errors: 0, Resource 
errors: 0
    Carrier transitions: 1, Errors: 0, Drops: 0, Collisions: 0, Aged packets: 0, FIFO errors: 0, HS link CRC errors: 0, MTU errors: 0, Resource errors: 0
  MAC statistics:                      Receive         Transmit
    CRC/Align errors                         0                0
    FIFO errors                              0                0
    MAC control frames                       0                0
    Total errors                             0                0
Physical interface: ge-3/2/1, Enabled, Physical link is Up
    Errors: 0, Drops: 0, Framing errors: 0, Runts: 0, Policed discards: 0, L3 incompletes: 0, L2 channel errors: 0, L2 mismatch timeouts: 0, FIFO errors: 0, Resource 
errors: 0
    Carrier transitions: 7, Errors: 0, Drops: 0, Collisions: 0, Aged packets: 0, FIFO errors: 0, HS link CRC errors: 0, MTU errors: 0, Resource errors: 0
  MAC statistics:                      Receive         Transmit
    CRC/Align errors                         0                0
    FIFO errors                              0                0
    MAC control frames                       0                0
    Total errors                             0                0
<<<<

BTW, for the other similar case, I didnt see 'MAC statistics error' for MIC0-FPC0 after issue recovery:
/volume/CSdata/annaw/case/2025-1222-002440:
SN-SINQT1-BO313-RSI-22Dec25   <<< collected on Dec 22 16:51:29

Issue recovered around Dec 22 15:59:00:
ops_rw@SN-SINQT1-BO313> request chassis fpc slot 0 restart 
Dec 22 15:59:00

RSI was not collected during issue, however from session log, I can see two of the interfaces were not showing much MAC statistic error:
<<<
ops_rw@SN-SINQT1-BO313> show interfaces ge-0/0/1    extensive 
Dec 22 15:25:07
Physical interface: ge-0/0/1, Enabled, Physical link is Up
  Interface index: 154, SNMP ifIndex: 1874, Generation: 157
  Link-level type: Ethernet, MTU: 1514, MRU: 1522, LAN-PHY mode, Speed: 100mbps, BPDU Error: None, Loop Detect PDU Error: None, Ethernet-Switching Error: None, MAC-REWRITE Error: None, Loopback: Disabled,
  MAC statistics:                      Receive         Transmit
    Total octets                   13682254747      20622061845
    Total packets                    132311409        131224393
    Unicast packets                  132169774        131224288
    Broadcast packets                     5220              101
    Multicast packets                   136415                4
    CRC/Align errors                         0              184
    FIFO errors                              0                0
    MAC control frames                       0                0
    MAC pause frames                         0                0
    Oversized frames                         0

ops_rw@SN-SINQT1-BO313> show interfaces ge-0/1/7 extensive    
Dec 22 15:46:25
Physical interface: ge-0/1/7, Enabled, Physical link is Up
  MAC statistics:                      Receive         Transmit
    Total octets                           661                0
    Total packets                            5                0
    Unicast packets                          0                0
    Broadcast packets                        4                0
    Multicast packets                        1                0
    CRC/Align errors                         0                0
    FIFO errors                              0                0
    MAC control frames                       0                0
    MAC pause frames                         0                0
    Oversized frames                         0
    Jabber frames                            0
    Fragment frames                          0
    VLAN tagged frames                       0
<<<<

Lab Access:
jtac-mx480-r2033: 10.219.35.20
jtac-mx480-r2607: 10.219.34.198








-------------


Annabelle,
One thing I noticed from 2026-0310-628387 RSI is, on some interfaces under the problematic MIC (FPC 3 MIC0) have no-zero/bogus MAC error stats. 
Would this be a signature of the problem? Please check same on the previous cases and let the customer get RSI from production routers for 'working case' as base line.
Also please share the access to your setup. Just wanted to check basic things regarding this MIC mode.


p-jtac-lnx02:/volume/CSdata/annaw/case/2026-0310-628387/10-Mar-Session% less /volume/CSdata/annaw/case/2026-0310-628387/RSI_MX-BP-02-06_2026-03-10_140901.txt | egrep -A 100 "Physical interface: ge-.*Physical link is Up" | egrep "Physi|MAC statistics:|CRC/Align errors|FIFO errors|MAC control frames|Total errors"
<>
Physical interface: ge-3/0/9, Enabled, Physical link is Up
    Errors: 8589934592, Drops: 0, Framing errors: 4294967296, Runts: 4294967296, Policed discards: 0, L3 incompletes: 0, L2 channel errors: 0, L2 mismatch timeouts: 0, FIFO errors: 0, Resource errors: 0
    Carrier transitions: 9, Errors: 4294967296, Drops: 0, Collisions: 0, Aged packets: 0, FIFO errors: 0, HS link CRC errors: 0, MTU errors: 0, Resource errors: 0
  MAC statistics:                      Receive         Transmit
    CRC/Align errors                4294967296       4294967296
    FIFO errors                              0                0
    MAC control frames                       0                0
    Total errors                    4294967296       4294967296
Physical interface: ge-3/1/0, Enabled, Physical link is Up
    Errors: 165, Drops: 0, Framing errors: 165, Runts: 0, Policed discards: 0, L3 incompletes: 0, L2 channel errors: 0, L2 mismatch timeouts: 0, FIFO errors: 0, Resource errors: 0
    Carrier transitions: 7, Errors: 19, Drops: 0, Collisions: 0, Aged packets: 0, FIFO errors: 0, HS link CRC errors: 0, MTU errors: 0, Resource errors: 0
  MAC statistics:                      Receive         Transmit
    CRC/Align errors                       165               19
    FIFO errors                              0                0
    MAC control frames                       0                0
    Total errors                           165               19
Physical interface: ge-3/1/1, Enabled, Physical link is Up
    Errors: 93838, Drops: 0, Framing errors: 93834, Runts: 4, Policed discards: 0, L3 incompletes: 0, L2 channel errors: 0, L2 mismatch timeouts: 0, FIFO errors: 0, Resource errors: 0
    Carrier transitions: 1, Errors: 40328, Drops: 0, Collisions: 0, Aged packets: 0, FIFO errors: 0, HS link CRC errors: 0, MTU errors: 0, Resource errors: 0
  MAC statistics:                      Receive         Transmit
    CRC/Align errors                     93834            40328
    FIFO errors                              0                0
    MAC control frames                       0                0
    Total errors                         93926            40328
Physical interface: ge-3/1/2, Enabled, Physical link is Up
    Errors: 0, Drops: 0, Framing errors: 0, Runts: 0, Policed discards: 0, L3 incompletes: 0, L2 channel errors: 0, L2 mismatch timeouts: 0, FIFO errors: 0, Resource errors: 0
    Carrier transitions: 5, Errors: 1, Drops: 0, Collisions: 0, Aged packets: 0, FIFO errors: 0, HS link CRC errors: 0, MTU errors: 0, Resource errors: 0
  MAC statistics:                      Receive         Transmit
    CRC/Align errors                         0                1
    FIFO errors                              0                0
    MAC control frames                       0                0
    Total errors                             0                1
Physical interface: ge-3/1/3, Enabled, Physical link is Up
    Errors: 0, Drops: 0, Framing errors: 0, Runts: 0, Policed discards: 0, L3 incompletes: 0, L2 channel errors: 0, L2 mismatch timeouts: 0, FIFO errors: 0, Resource errors: 0
    Carrier transitions: 2385, Errors: 0, Drops: 0, Collisions: 0, Aged packets: 0, FIFO errors: 0, HS link CRC errors: 0, MTU errors: 0, Resource errors: 0
  MAC statistics:                      Receive         Transmit
    CRC/Align errors                         0                0
    FIFO errors                              0                0
    MAC control frames                       0                0
    Total errors                             0                0
Physical interface: ge-3/1/4, Enabled, Physical link is Up
    Errors: 167, Drops: 0, Framing errors: 167, Runts: 0, Policed discards: 0, L3 incompletes: 0, L2 channel errors: 0, L2 mismatch timeouts: 0, FIFO errors: 0, Resource errors: 0
    Carrier transitions: 69, Errors: 36, Drops: 0, Collisions: 0, Aged packets: 0, FIFO errors: 0, HS link CRC errors: 0, MTU errors: 0, Resource errors: 0
  MAC statistics:                      Receive         Transmit
    CRC/Align errors                       167               36
    FIFO errors                              0                0
    MAC control frames                       0                0
    Total errors                           167               36
Physical interface: ge-3/1/6, Enabled, Physical link is Up
    Errors: 0, Drops: 0, Framing errors: 0, Runts: 0, Policed discards: 0, L3 incompletes: 0, L2 channel errors: 0, L2 mismatch timeouts: 0, FIFO errors: 0, Resource errors: 0
    Carrier transitions: 79, Errors: 4294967296, Drops: 0, Collisions: 0, Aged packets: 0, FIFO errors: 0, HS link CRC errors: 0, MTU errors: 0, Resource errors: 0
  MAC statistics:                      Receive         Transmit
    CRC/Align errors                         0       4294967296
    FIFO errors                              0                0
    MAC control frames                       0                0
    Total errors                             0       4294967296
Physical interface: ge-3/1/7, Enabled, Physical link is Up
    Errors: 0, Drops: 0, Framing errors: 0, Runts: 0, Policed discards: 0, L3 incompletes: 0, L2 channel errors: 0, L2 mismatch timeouts: 0, FIFO errors: 0, Resource errors: 0
    Carrier transitions: 75, Errors: 4294967296, Drops: 0, Collisions: 0, Aged packets: 0, FIFO errors: 0, HS link CRC errors: 0, MTU errors: 0, Resource errors: 0
  MAC statistics:                      Receive         Transmit
    CRC/Align errors                         0       4294967296
    FIFO errors                              0                0
    MAC control frames                       0                0
    Total errors                             0       4294967296
:


Thanks,
-Naotaka


--------------------

For the latest case 2026-0310-628387, customer just told they noticed the issue and soon they asked for the troubleshooting call.  Checking the session log, I guess it's just before 'Mar 10 14:29:39'.


Below RSI was collected before MIC restart. Traffic were almost all 0 for the interfaces on the FPC3-MIC0. Didnt see queue drop.
/volume/CSdata/annaw/case/2026-0310-628387/RSI_MX-BP-02-06_2026-03-10_140901.txt



----------


Can you point the log/timestamp when the problem has started? Do they have any protocol configured on the impacted interfaces (which will give hints the start of the problem)
Forgot to ask, did you see any egress queue drop in the problematic state?

------------
Suhua:


we didn't check extensively on these problematic interfaces which are all located at MIC0 , all the interfaces in this MIC have same issue, which is up/up status, with arp learnt and unable to ping the peer.
after reboot the mic0, almost all the interfaces recovered( able to ping the peer), some interfaces need to "re-init" the sfp at pfe shell level
we suspect this is MIC-MACSEC issue as there are 2 occurrences in 2 singtel networks.
how to move the investigation forward? PR? DE?

-------------
Dear Fujita San,

Was macsec enabled on the impacted node or not?
<Annabelle> No.

Was the problem happening on specific interface? Or all the interfaces within same PFE got affected?
<Annabelle> Issue affecting all interfaces on the same MIC ( PIC0 & PIC1). Both of the combination of FPC+MIC is as below:
<<<
FPC 3            REV 36   750-063184   EBBM8184          MPC2E NG PQ & Flex Q
  MIC 0          REV 19   750-077332   EBBT1962          2x 10GE SFPP / 20x 1GE SFP MACsec
FPC 0            REV 36   750-063184   EBBM4253          MPC2E NG PQ & Flex Q
  MIC 0          REV 19   750-077332   EBBN4400          2x 10GE SFPP / 20x 1GE SFP MACsec
<<<
Since it's Aloha FPC, all interfaces belong to same PFE.

Did you check if TX counters on MAC stats were moving? Did they observe framing error on peer devices? What was macsec stats?
<Annabelle> Unfortunately, we didnt check if the MAC stats were moving. Only can see 'MAC statistics' from below kind of output, but didnt check if it's increasing:
<<<
P1206753_RW@MX-BP-02-06# run show interfaces ge-3/1/6 media    
Mar 10 15:18:14
Physical interface: ge-3/1/6, Enabled, Physical link is Up
<snip..>
  MAC statistics:
    Input bytes: 1661756581447, Input packets: 26881020459, Output bytes: 105314906413, Output packets: 4343640670
<<<
In the meting, customer dont have access to check peer device and we have very limit time to recover the issue. 

Sorry that I know that not much useful information provided, we actually have a JTAC lab with two MX routers, same FPC+MIC combination. Do you think we can try some methods to replicate the issue?



-------------
Hi Annabelle,
You will need to understand where the packets are dropped. Was macsec enabled on the impacted node or not?
Was the problem happening on specific interface? Or all the interfaces within same PFE got affected?
From your description, the problem seems happening on TX direction because you were seeing incoming ARP.
Did you check if TX counters on MAC stats were moving? Did they observe framing error on peer devices? What was macsec stats?

Thx,
-Naotaka

-----------


[Escalation] 2026-0310-628387 - 19958 - SINGTEL -- MEGAPOP - NERA - MX-BP-02-06 - [Singtel:Singtel-Megapop:MX-BP-02-06] MULTIPLE LTA circuit down | Ticket #19958

Hello Escalations team, 

Could you please assist me with below two Singtel cases which for similar issue?

All interfaces on a MIC-Macsec suddenly cannot ping. ARP was there that we can receive peer arp and also sent out response. All interfaces were UP and also OK seeing from 'show sfp list'. Toggled with no-auto-negotiation/interface disable... didnt help.

2025-1222-002440:
Hostname: SN-SINQT1-BO313
Model: mx960
Junos: 23.2R2-S3.8
<<<
Summary of the remote session:
a) We noticed that the interfaces in MIC0 are UP but not able to ping to remote peer although ARP is learnt
b) We tried various troubleshooting methods but not able to ping to remote peer in MIC0
c) From customer graphs, we saw that the interfaces in MIC0 has no traffic
d) Therefore, we proceed to reboot FPC0
e) During the reboot of FPC0, customer informed us that ge-0/3/0 in MIC1 has traffic.
f) Some of the interfaces in MIC0 were recovered after FPC0 reboot
g) Rest of the interfaces in MIC0 were recovered after bouncing no-auto-negotiation knob

Issue recovered by FPC reboot and MIC RMA was created
<<<
/volume/CSdata/annaw/case/2025-1222-002440:
QT1-BO313-22Dec25-Tshoot-session.log  
SN-SINQT1-BO313-RSI-22Dec25  
var-re0  
var-re1

2026-0310-628387:
Hostname: SNGCCK-BB1
Model: mx960
Junos: 23.2R2-S3.8
<<<
Before the remote session, customer tried below but failed to recover the issue:
Replaced the media converter
Conducted end-to-end testing with the Field Engineer (FE) tester
Performed port bounce on the access interface
Removed the OEM configuration and reconfigured the interface
Reconfigured and verified duplex settings

Summary of the remote session:
-ARP was there，received peer arp and sent out response.
-All interfaces were UP and OK seeing from 'show sfp list'
-Toggled with no-auto-negotiation/interface disable... didnt help.

Issue was recovered by MIC0 reseat.
<<<
/volume/CSdata/annaw/case/2026-0310-628387:
10-Mar-Session  <<< contains var + session log
MX-BP-02-06_session_Logs_2026-03-10_142845.log  
RSI_MX-BP-02-06_2026-03-10_140901.txt  
syslog_0302-09
<<<


We didnt find existing any suspicious logs or PR/KB for the issue. Since it happened to 2 different nodes already, Singtel would like to know the root cause.

FYI. Singtel has deployed many MIC-MACsec in their production to replace the legcy MICs. However, they encountered quite some issue with it. Like PR1925162 / 2025-1208-973736 which has been stuck in resolution. So they are very sensitive to MIC-macsec related issue.  

Could you help to take a look?