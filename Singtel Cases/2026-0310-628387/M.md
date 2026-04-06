
----------------

Dear Neo,

For point 1&point2, no inbound traffic on MX interfaces may due to packet loss either outside MX or discarded on MX like MAC layer or IXchip.  That's why we asked to collect the PFE output in next occurrence.


Based on the remaining checks (during failure) outlined in your email, it seems to point toward a possible hardware failure? 
<Annabelle> No, we cannot say it now. Need to confirm with the output in next occurrence. The issue might also be related to the transmission path or the peer device. A successful recovery via MIC reseat is insufficient evidence to conclude a hardware failure at this stage.

---------

Hi Annabelle,
 
1./ We have a query from Singtel team. Below are their response, can you assist to provide an explanation on it.
 
1. Ping test from RE
    • Continuously ping the ARP neighbor with large packet size.
    • Confirm ping failure while ARP is still learned. << Yes, I’m observing the same behavior. I can see ARP entries, and there is outbound traffic during the ping, but no inbound traffic is visible on the interface - ping failed. I’ve cleared the ARP table twice on the affected interface, but it did not resolve the issue.

2. Interface counters
    • Capture show interfaces <ifd> extensive twice, with ~5 seconds interval.
    • Check for counter increments or anomalies. <<< I can see increment in the TX but not in the RX during the time of the failure.
 
Based on the remaining checks (during failure) outlined in your email, it seems to point toward a possible hardware failure? 

-------------

Hi, Neo,

1./ From my understanding of your previous email, we are currently unable to identify the root cause of the issue. We will need to capture the specific outputs again when the issue reoccurs?
<Annabelle> Yes.

2./ When you mentioned a different HW combination, are you referring to FPC (of a different model) together with a MIC MACsec combination?
<Annabelle> From past two cases, issue only happened to MIC-Macsec. So I want to know if there's any other FPC type also has MIC-Macsec inside. Especially for those MPCs higher then MPC type 6 which doesnt have XMchip.

-----------


Hi Annabelle,
 
1./ From my understanding of your previous email, we are currently unable to identify the root cause of the issue. We will need to capture the specific outputs again when the issue reoccurs?
 
2./ When you mentioned a different HW combination, are you referring to FPC (of a different model) together with a MIC MACsec combination?



----------

Dear Neo,

As per discussion with our escalation team, we need to proceed below to  identify where the packet is dropped during next occurrence. It might be a little bit complicate, you may request CFTS to join the meeting to assist. Below is the work plan:

Hardware combination is for  MPC2E NG PQ & Flex Q + MIC MACsec. Since below is only applicable for XM chip based FPCs, please advice if there's other HW combination inside Singtel production.

Let's take ge-0/2/0 as an example.

Step 1: Collect output
-> From RE, keep pinging to arp neighbor.
labroot@jtac-mx480-r2033-re0> ping 10.0.0.2 size 65000
<ping fails...>

-> check interface counter at least twice.
Router>show interfaces ge-0/2/0 extensive| no-more
<wait for 5 sec>
Router>show interfaces ge-0/2/0 extensive| no-more

-> check the phy-stream number for DUT interface. 
NGMPC0(jtac-mx480-r2033-re0 vty)# sh xmchip 0 ifd list 1

--------------------------------------------------------------------
IFD name         IFD    PHY     Scheduler  L1 Node  Base   Number of
                 Index  Stream                      Queue  Queues
--------------------------------------------------------------------
ge-0/2/0         152    1188    WAN        64       512    8        -> Phy-stream: 1188 


-> Here is how to identify WO block# from phy-stream
Block 0: 1024..1151, Block 1: 1152..1279  -> 'Phy-stream: 1188' belongs to wo block 1
NOTE: last digit '1' indicates wo Block. If Phy=stream is in the range of 1024 - 1151, block should be 0. For 'MPC2E NG PQ & Flex Q' card only has one XM chip, so xmchip # is 0, for other MPC type it varies.

NGMPC0(jtac-mx480-r2033-re0 vty)# sh xmchip 0 wo stats 1
WO statistics (WAN block 1)
---------------------------

Counter set 0

    Stream number mask       : 0x7f
    Stream number match      : 0x24
    Transmitted packets      : 171871 (44 pps)             -> check this
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
   152     ge-0/2/0      2       8        8/40/72       8/40      -> IX:2 port 8
   153     ge-0/2/1      2       9        9/41/73       9/41


NGMPC0(jtac-mx480-r2033-re0 vty)# sh ixchip 2 statistics wan_port 8 <<< 
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
  3 mtip_ge.0.2.0          0   2 ge-0/2/0             251c1680     <--- index 3
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


Step 2: Collect FPC live coredump twice:
NGMPC0(jtac-mx480-r2033-re0 vty)#write core
NGMPC0(jtac-mx480-r2033-re0 vty)#write core





------------------


Dear Neo,

May I know if you have uploaded /var/log as well?

------------
Hi Annabelle,
 
1./ I have uploaded the syslog covering from 02 March to 09 March to JSP.
 
2./ The customer only reported service impact on 10 March, which is when we initiated the troubleshooting session.

------------

Hi Annabelle,
Starview xcvr only used in cplus/magapop for 100M interface.
Singnet does not use starview at all, it was 1G SFP in their case.

----------------

Dear Chunxian,

Thanks for your information and Suhua has mentioned about SR 2025-1222-002440.

Could you confirm if SFPs for both cases are Starview's?

Let's wait for the required information to be collected to do further investigation on this case.

---------------
Hello Annabelle,
 
1./ I remember that we encountered the same symptoms issue with MIC-MACSEC-20GE in another Singtel network, SingNet.
 
2./ Refer to SR 2025-1222-002440.
 
In this case, it was the same.
The MIC suddenly lost connectivity to the peers and the interfaces were UP but not able to ping to remote peer although ARP is learnt.
 
For 2025-1222-002440, we did not reboot the MIC but we rebooted the MPC to resolve the issue.
 
There was no root cause for SR 2025-1222-002440.

------------

Dear Alex,

1./ Singtel informed us about the issue approximate 1415 SGT. No action was performed until we joined the troubleshooting session
<Annabelle> As what I heard on the meeting that issue occurred couple days ago that interfaces went down on MIC0-FPC3 and suddenly UP again but not pingable.

Could you help to check the chronological order of the failure. Also please ensure syslog covers all time periods.

----------

Hi Annabelle,
 
1./ Singtel informed us about the issue approximate 1415 SGT. No action was performed until we joined the troubleshooting session
 
2./ I have uploaded the session log to JSP.
 
 3./ We are still awaiting for the /var/log and syslog of the last few day, will update you once we get it. Thanks

--------


Dear  Neo,

All the interfaces in MIC0 FPC3 cannot ping and issue was recovered by MIC0 reset.

Please help to provide below information:
1.When was the issue first reported? Any action right before that?
2.VAR/LOG Session log
3.Syslog covering the time period that issue first occurred.




---------------------
Dear  Neo,

This is Annabelle Wang from Japan CFTS Team and I will be assisting you with your case.

I will go through the case and provide you and update later.
