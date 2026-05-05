









----------------


Hi Naveen,

I can’t tell what’s wrong as you didn’t even capture the whole “show interface extensive” output for the affected interface multiple times to see if the MAC statistics was moving at all or getting any exceptions / MQ counters to trace if any packet drop was recorded under any block.

I would suggest you to try replicating that -on those 100Mbps port, keep flapping the link and see if after a few rounds, you will run into the same issue.

Regards,
Steven

---------------

Hi MX Escalations team,

We request your support on this Singtel case.

Hostname: MX-BP-02-06
Model: mx480
Junos: 23.2R2-S3.8
FPC 3- MPC2E NG PQ & Flex Q
MIC 0 -2x 10GE SFPP / 20x 1GE SFP MACsec


Customer encountered problem with FPC 3 MIC 0 , where interface shows up up but not able to ping and no traffic. Customer informed they could not see any fpc3 log error/ system alarm/ chassis alarm . 

Summary of troubleshooting given by customer-

1) Singtel reported service impact at the following interfaces at approximately 1230PM 29 April SGT. (ge-3/0/1, ge-3/0/3, ge-3/0/4,ge-3/1/4,ge-3/1/6,ge-3/1/7).
Based on traffic monitoring graph, no traffic since 4pm 28 April SGT. 

Initial interface state before troubleshooting


ge-3/0/0                up    up
ge-3/0/0.0              up    up   inet     172.16.24.189/30
ge-3/0/1                up    up
ge-3/0/1.0              up    up   inet     172.16.20.45/30
ge-3/0/2                up    up
ge-3/0/2.0              up    up   inet     198.18.85.101/30
ge-3/0/3                up    up
ge-3/0/3.0              up    up   inet     172.16.20.65/30
ge-3/0/4                up    up
ge-3/0/4.0              up    up   inet     172.16.20.177/30
ge-3/0/5                up    up
ge-3/0/5.0              up    up   inet     172.16.1.113/30
ge-3/0/6                up    down
ge-3/0/7                up    up
ge-3/0/7.0              up    up   inet     192.169.3.254/24
ge-3/0/8                up    down
ge-3/0/8.0              up    down inet     192.169.4.254/24
ge-3/0/9                up    up
ge-3/0/9.0              up    up   inet     192.169.14.254/24
ge-3/1/0                up    up
ge-3/1/0.0              up    up   inet     192.169.10.254/24
ge-3/1/1                up    up
ge-3/1/1.0              up    up   inet     192.169.9.254/24
ge-3/1/2                up    up
ge-3/1/2.0              up    up   inet     192.169.13.254/24
ge-3/1/3                up    up
ge-3/1/3.0              up    up   inet     192.169.7.254/24
ge-3/1/4                up    up
ge-3/1/4.0              up    up   inet     172.16.1.33/30  
ge-3/1/5                up    down
ge-3/1/5.0              up    down ccc    
ge-3/1/6                up    up
ge-3/1/6.0              up    up   inet     172.16.20.53/30
ge-3/1/7                up    up
ge-3/1/7.0              up    up   inet     172.16.20.37/30
ge-3/1/8                up    down
ge-3/1/8.16386          up    down
ge-3/1/9                up    down
ge-3/1/9.16386          up    down



2) They reinitialized ge-3/1/7, ge-3/1/6,ge-3/0/4. Not able to recover traffic.

P1259651_RW@MX-BP-02-06> start shell pfe network fpc3 
Apr 29 13:51:33

NGMPC3(MX-BP-02-06 vty)# set parser security 10
Security level set to 10

NGMPC3(MX-BP-02-06 vty)# show sfp list


                                                                         diag
Index   Name             Presence     ID Eprom  PNO         SNO          calibr   Toxic
-----   --------------   ----------   --------  ----------  ----------- -------  -------
   19      MIC(3/2)(0)      Present   Complete  740-021487  SQBW060087      int    Unknown
   20      MIC(3/2)(1)      Present   Complete  740-021487  SQBW060096      int    Unknown
   21      MIC(3/2)(2)      Present   Complete  740-021487  SLC4620091      int    Unknown
   22      MIC(3/2)(3)      Present   Complete  740-021487  SQBW060109      int    Unknown
   23      MIC(3/2)(4)      Present   Complete  740-021487  SQBW060113      int    Unknown
   24      MIC(3/2)(5)      Present   Complete  740-021487  SQBW060092      int    Unknown
   25      MIC(3/2)(6)      Present   Complete  740-021487  SQBW060095      int    Unknown
   26      MIC(3/2)(7)      Present   Complete  740-021487  SQBW060098      int    Unknown
   27      MIC(3/2)(8)      Present   Complete  740-021487  SQBW060089      int    Unknown
   28      MIC(3/2)(9)      Present   Complete  740-021487  SQBW060091      int    Unknown
   29      MIC(3/3)(0)      Present   Complete  740-021487  SQBW060118      int    Unknown
   30      MIC(3/3)(1)      Present   Complete  740-021487  SQBW060117      int    Unknown
   31      MIC(3/3)(2)      Present   Complete  740-021487  SQBW060081      int    Unknown
   32      MIC(3/3)(3)      Present   Complete  740-021487  SQBW060112      int    Unknown
   33      MIC(3/3)(4)      Present   Complete  740-021487  SQBW060082      int    Unknown
   34      MIC(3/3)(5)      Present   Complete  740-021487  SQBW060116      int    Unknown
   35      MIC(3/3)(6)      Present   Complete  740-021487  SQBW060093      int    Unknown
   36      MIC(3/3)(7)      Present   Complete  740-021487  SQBW060107      int    Unknown
   37      MIC(3/0)(0)      Present   Complete  740-021487  SQBW060088      int    Unknown
   38      MIC(3/0)(1)      Present   Complete  740-021487  SQBW060104      int    Unknown
   39      MIC(3/0)(2)      Present   Complete  740-021487  SQBW060110      int    Unknown
   40      MIC(3/0)(3)      Present   Complete  740-021487  SQBW060101      int    Unknown
   41      MIC(3/0)(4)      Present   Complete  740-021487  SQBW060100      int    Unknown
   42      MIC(3/0)(5)      Present   Complete  740-021487  SQBW060115      int    Unknown
   43      MIC(3/0)(6)      Present   Complete  740-021487  SQBW060075      int    Unknown
   44      MIC(3/0)(7)      Present   Complete  740-021487  SQBW060103      int    Unknown
   45      MIC(3/0)(8)      Present   Complete  740-021487  SQBW060086      int    Unknown
   46      MIC(3/0)(9)      Present   Complete  740-021487  SQBW060090      int    Unknown
   47      MIC(3/1)(0)      Present   Complete  740-021487  SQBW060097      int    Unknown
   48      MIC(3/1)(1)      Present   Complete  740-021487  SQBW060094      int    Unknown
   49      MIC(3/1)(2)      Present   Complete  740-021487  SQBW060108      int    Unknown
   50      MIC(3/1)(3)      Present   Complete  740-021487  SQBW060105      int    Unknown
   51      MIC(3/1)(4)      Present   Complete  740-021487  SQBW060106      int    Unknown
   52      MIC(3/1)(5)      Present   Complete  740-021487  SQBW060119      int    Unknown
   53      MIC(3/1)(6)      Present   Complete  740-021487  SQBW060102      int    Unknown
   54      MIC(3/1)(7)      Present   Complete  740-021487  SQBW060111      int    Unknown


NGMPC3(MX-BP-02-06 vty)# test sfp 54 reinitialize

NGMPC3(MX-BP-02-06 vty)# [Apr 29 05:52:40.359 LOG: Notice] MIC(3/1): Link 7 SXFX Phy based SFP  - plugged in.

NGMPC3(MX-BP-02-06 vty)# test sfp 54 reinitialize

NGMPC3(MX-BP-02-06 vty)# [Apr 29 05:53:20.316 LOG: Notice] MIC(3/1): Link 7 SXFX Phy based SFP  - plugged in.

NGMPC3(MX-BP-02-06 vty)# test sfp 53 reinitialize

NGMPC3(MX-BP-02-06 vty)# [Apr 29 05:54:19.802 LOG: Notice] MIC(3/1): Link 6 SXFX Phy based SFP  - plugged in.

NGMPC3(MX-BP-02-06 vty)# test sfp 53 reinitialize

NGMPC3(MX-BP-02-06 vty)# [Apr 29 05:54:49.467 LOG: Notice] MIC(3/1): Link 6 SXFX Phy based SFP  - plugged in.

NGMPC3(MX-BP-02-06 vty)# test sfp 41 reinitialize

NGMPC3(MX-BP-02-06 vty)# [Apr 29 05:56:42.098 LOG: Notice] MIC(3/0): Link 4 SXFX Phy based SFP  - plugged in.
   
NGMPC3(MX-BP-02-06 vty)# exit


3) They made MIC offline/online.

Apr 29 15:37:37 <-- MIC offline
1256869_RW@MX-BP-02-06> request chassis mic offline fpc-slot 3 mic-slot 0   


Apr 29 15:38:23 <---MIC online


P1256869_RW@MX-BP-02-06> request chassis mic online fpc-slot 3 mic-slot 0 

4) They noticed that some more interfaces are in UP DOWN state. 

ge-3/0/0        up    down CUST~MP~LTA~Singapore~00201350SNG~HS-EL~2M~LTA-1886~~NISA
ge-3/0/0.0      up    down CUST~MP~LTA~Singapore~00201350SNG~HS-EL~2M~LTA-1886~~NISA
ge-3/0/1        up    up   CUST~MP~LTA~~00059134SNG~HS-EL~2M~LTA-1886~~TINA
ge-3/0/1.0      up    up   CUST~MP~LTA~~00059134SNG~HS-EL~2M~LTA-1886~~TINA
ge-3/0/2        up    down CUST~MP~MOD~~00098007SNG~HS-EL~20M~MOD-19829~~TINA
ge-3/0/2.0      up    down CUST~MP~MOD~~00098007SNG~HS-EL~20M~MOD-19829~~TINA
ge-3/0/3        up    up   CUST~MP~LTA~Singapore~00059177SNG~HS-EL~2M~LTA-1886~~NISA
ge-3/0/3.0      up    up   CUST~MP~LTA~Singapore~00059177SNG~HS-EL~2M~LTA-1886~~NISA
ge-3/0/4        up    down CUST~MP~LTA~~00063975SNG~HS-EL~4M~LTA-1886~~TINA
ge-3/0/4.0      up    down CUST~MP~LTA~~00063975SNG~HS-EL~4M~LTA-1886~~TINA
ge-3/0/5        up    up   CUST~MP~LTA~~00235051SNG~HS-EL~2M~LTA-1886~~TINA
ge-3/0/5.0      up    up   CUST~MP~LTA~~00235051SNG~HS-EL~2M~LTA-1886~~TINA
ge-3/0/7        up    up   CUST~MP~POWERGAS-2~Singapore~00021879SNG~HS-EL~20M~POWERGAS-2-2861~~NISA
ge-3/0/7.0      up    up   CUST~MP~POWERGAS-2~Singapore~00021879SNG~HS-EL~20M~POWERGAS-2-2861~~NISA
ge-3/0/8        up    down CUST~MP~POWERGAS-3~Singapore~00021880SNG~HS-EL~20M~POWERGAS-3-2861~~NISA
ge-3/0/8.0      up    down CUST~MP~POWERGAS-3~Singapore~00021880SNG~HS-EL~20M~POWERGAS-3-2861~~NISA
ge-3/0/9        up    up   CUST~MP~POWERGAS-4~Singapore~00021889SNG~HS-EL~20M~POWERGAS-4-2861~~NISA
ge-3/0/9.0      up    up   CUST~MP~POWERGAS-4~Singapore~00021889SNG~HS-EL~20M~POWERGAS-4-2861~~NISA
ge-3/1/0        up    up   CUST~MP~POWERGAS-5~Singapore~00021875SNG~HS-EL~2M~POWERGAS-5-2861~~NISA
ge-3/1/0.0      up    up   CUST~MP~POWERGAS-5~Singapore~00021875SNG~HS-EL~2M~POWERGAS-5-2861~~NISA
ge-3/1/1        up    up   CUST~MP~POWERGAS-6~Singapore~00021888SNG~HS-EL~20M~POWERGAS-6-2861~~NISA
ge-3/1/1.0      up    up   CUST~MP~POWERGAS-6~Singapore~00021888SNG~HS-EL~20M~POWERGAS-6-2861~~NISA
ge-3/1/2        up    down CUST~MP~POWERGAS-7~Singapore~00021885SNG~HS-EL~20M~POWERGAS-7-2861~~NISA
ge-3/1/2.0      up    down CUST~MP~POWERGAS-7~Singapore~00021885SNG~HS-EL~20M~POWERGAS-7-2861~~NISA
ge-3/1/3        up    up   CUST~MP~POWERGAS-8~Singapore~00021883SNG~HS-EL~20M~POWERGAS-8-2861~~NISA
ge-3/1/3.0      up    up   CUST~MP~POWERGAS-8~Singapore~00021883SNG~HS-EL~20M~POWERGAS-8-2861~~NISA
ge-3/1/4        up    up   CUST~MP~LTA~Singapore~00053482SNG~HS-EL~2M~LTA-1886~~NISA
ge-3/1/4.0      up    up   CUST~MP~LTA~Singapore~00053482SNG~HS-EL~2M~LTA-1886~~NISA
ge-3/1/6        up    down CUST~MP~LTA~Singapore~00059136SNG~HS-EL~20M~LTA-1886~~NISA
ge-3/1/6.0      up    down CUST~MP~LTA~Singapore~00059136SNG~HS-EL~20M~LTA-1886~~NISA
ge-3/1/7        up    up   CUST~MP~LTA~Singapore~00059132SNG~HS-EL~2M~LTA-1886~~NISA
ge-3/1/7.0      up    up   CUST~MP~LTA~Singapore~00059132SNG~HS-EL~2M~LTA-1886~~NISA


Customer reinitialized ge-3/0/0, ge-3/0/3, ge-3/0/6, ge-3/1/6, ge-3/1/0, ge-3/0/7, ge-3/0/8, ge-3/1/3, ge-3/1/2, ge-3/0/2 and able to recover traffic and the interface to up up state.(refer session logs) 


Final state after service recovery-

ge-3/0/0                up    up
ge-3/0/0.0              up    up   inet     172.16.24.189/30
ge-3/0/1                up    up
ge-3/0/1.0              up    up   inet     172.16.20.45/30
ge-3/0/2                up    up
ge-3/0/2.0              up    up   inet     198.18.85.101/30
ge-3/0/3                up    up
ge-3/0/3.0              up    up   inet     172.16.20.65/30
ge-3/0/4                up    up
ge-3/0/4.0              up    up   inet     172.16.20.177/30
ge-3/0/5                up    up
ge-3/0/5.0              up    up   inet     172.16.1.113/30
ge-3/0/6                up    down
ge-3/0/7                up    up
ge-3/0/7.0              up    up   inet     192.169.3.254/24
ge-3/0/8                up    down
ge-3/0/8.0              up    down inet     192.169.4.254/24
ge-3/0/9                up    up
ge-3/0/9.0              up    up   inet     192.169.14.254/24
ge-3/1/0                up    up
ge-3/1/0.0              up    up   inet     192.169.10.254/24
ge-3/1/1                up    up
ge-3/1/1.0              up    up   inet     192.169.9.254/24
ge-3/1/2                up    up
ge-3/1/2.0              up    up   inet     192.169.13.254/24
ge-3/1/3                up    up
ge-3/1/3.0              up    up   inet     192.169.7.254/24
ge-3/1/4                up    up
ge-3/1/4.0              up    up   inet     172.16.1.33/30  
ge-3/1/5                up    down
ge-3/1/5.0              up    down ccc    
ge-3/1/6                up    up
ge-3/1/6.0              up    up   inet     172.16.20.53/30
ge-3/1/7                up    up
ge-3/1/7.0              up    up   inet     172.16.20.37/30
ge-3/1/8                up    down
ge-3/1/8.16386          up    down
ge-3/1/9                up    down
ge-3/1/9.16386          up    down

JTAC troubleshooting-

1) We noticed traffic went down at 28 April 15:45 SGT on interfaces ge-3/0/1, ge-3/0/3, ge-3/0/4,ge-3/1/4,ge-3/1/6,ge-3/1/7 at the same time(kindly refer the graphs)

2) Apr 28 15:42:20 MX-BP-02-06 lfmd[16811]: LFMD_3AH_LINKDOWN: (ge-3/1/6): 802.3ah link-fault status changed to fault with reason [Adjacency lost]
Apr 28 15:42:20 MX-BP-02-06 lfmd[16811]: Action Syslog on ge-3/1/6 [transition]: :Adjacency Lost
Apr 28 15:42:20 MX-BP-02-06 fpc3 mic_gi2_mac_lnk_ioctl: ge lnk ioctl 21  
Apr 28 15:42:20 MX-BP-02-06 fpc3 mic_gi2_mac_lnk_ioctl: ge lnk ioctl 21  
Apr 28 15:42:20 MX-BP-02-06 rpd[16796]: RPD_IFL_NOTIFICATION: IF_TRACE: EVENT [UpDown] ge-3/1/6.0 index 4914 [Broadcast Multicast] address #0 f4.a7.39.d1.35.16
Apr 28 15:42:20 MX-BP-02-06 rpd[16796]: RPD_IFA_NOTIFICATION: IF_TRACE: EVENT <UpDown> ge-3/1/6.0 index 4914 172.16.20.53/30 -> 172.16.20.55 <Broadcast Multicast Localup>
Apr 28 15:42:20 MX-BP-02-06 rpd[16796]: RPD_IFD_NOTIFICATION: IF_TRACE: EVENT <UpDown> ge-3/1/6 index 572 <Broadcast Multicast> agg_parent 0 address #0 f4.a7.39.d1.35.16
Apr 28 15:42:20 MX-BP-02-06 mib2d[18654]: SNMP_TRAP_LINK_DOWN: ifIndex 737, ifAdminStatus up(1), ifOperStatus down(2), ifName ge-3/1/6

We could see link down log only for interface ge-3/1/6.

3) No suspicious logs under fpc3 shell around the time of service down. 

[Apr 28 07:15:21.627 LOG: Notice] gi2mic_vsc8584_link_status_get: PHY LINK down detected for ifd ge-3/0/5
[Apr 28 07:15:21.627 LOG: Info]         vsc8584 PMA/PMD reg for this ifd reads 1c9
[Apr 28 07:15:21.627 LOG: Info] ifp ge-3/0/5 ifd_mdown: 7823189150 ms
[Apr 28 07:15:28.751 LOG: Notice] MIC(3/0) link 5 SFP receive power low  alarm set
[Apr 28 07:15:28.751 LOG: Notice] MIC(3/0) link 5 SFP receive power low  warning set
[Apr 28 07:16:05.488 LOG: Notice] MIC(3/0) link 5 SFP receive power low  alarm cleared
[Apr 28 07:16:05.488 LOG: Notice] MIC(3/0) link 5 SFP receive power low  warning cleared
[Apr 28 07:16:18.627 LOG: Info] mic_mac_periodic:ge-3/0/5: ifd_mup
[Apr 28 07:16:20.517 LOG: Notice] gi2mic_vsc8584_link_status_get: PHY LINK down detected for ifd ge-3/0/5
[Apr 28 07:16:20.517 LOG: Info]         vsc8584 PMA/PMD reg for this ifd reads 1c9
[Apr 28 07:16:20.629 LOG: Info] ifp ge-3/0/5 ifd_mdown: 7823248155 ms
[Apr 28 07:16:21.630 LOG: Info] mic_mac_periodic:ge-3/0/5: ifd_mup
[Apr 28 07:24:59.636 LOG: Notice] gi2mic_vsc8584_link_status_get: PHY LINK down detected for ifd ge-3/0/5
[Apr 28 07:24:59.636 LOG: Info]         vsc8584 PMA/PMD reg for this ifd reads 1c9
[Apr 28 07:24:59.636 LOG: Info] ifp ge-3/0/5 ifd_mdown: 7823767150 ms
[Apr 28 07:25:07.635 LOG: Info] mic_mac_periodic:ge-3/0/5: ifd_mup
[Apr 28 07:25:08.647 LOG: Notice] gi2mic_vsc8584_link_status_get: PHY LINK down detected for ifd ge-3/0/5
[Apr 28 07:25:08.647 LOG: Info]         vsc8584 PMA/PMD reg for this ifd reads 1c9
[Apr 28 07:29:44.657 LOG: Notice] gi2mic_vsc8584_link_status_get: PHY LINK down detected for ifd ge-3/0/5
[Apr 28 07:29:44.657 LOG: Info]         vsc8584 PMA/PMD reg for this ifd reads 1c9
[Apr 28 07:29:44.657 LOG: Info] ifp ge-3/0/5 ifd_mdown: 7824052170 ms
[Apr 28 07:30:06.117 LOG: Notice] MIC(3/0) link 5 SFP receive power low  alarm set
[Apr 28 07:30:06.117 LOG: Notice] MIC(3/0) link 5 SFP receive power low  warning set
[Apr 28 07:31:44.279 LOG: Notice] MIC(3/0) link 5 SFP receive power low  alarm cleared
[Apr 28 07:31:44.279 LOG: Notice] MIC(3/0) link 5 SFP receive power low  warning cleared
[Apr 28 07:35:01.659 LOG: Info] mic_mac_periodic:ge-3/0/5: ifd_mup
[Apr 28 07:42:20.461 LOG: Info] mic_gi2_mac_lnk_ioctl: ge lnk ioctl 21 
[Apr 28 07:42:20.464 LOG: Info] mic_gi2_mac_lnk_ioctl: ge lnk ioctl 21 


4) ge-3/0/1 (sfp 38) and ge-3/1/4 (sfp 51) which has traffic issue were never reinitialized by customer.

5) sfp 41 (ge-3/0/4) — reinitialized ×1 (before reboot) and — reinitialized ×1 (after reboot)

6) sfp 40 (ge-3/0/3) — reinitialized ×2 (after reboot)

7) During troubleshooting-

Apr 29 12:57:24 MX-BP-02-06 mgd[99474]: UI_CMDLINE_READ_LINE: User 'P1362636_RW', command 'delete groups orchestream protocols oam ethernet link-fault-management interface ge-3/1/6 '
Apr 29 12:57:24 MX-BP-02-06 mgd[99474]: UI_CFG_AUDIT_OTHER: User 'P1362636_RW' delete: [groups orchestream protocols oam ethernet link-fault-management interface ge-3/1/6] 
Apr 29 12:57:43 MX-BP-02-06 mgd[99474]: UI_COMMIT_COMPLETED:  : commit complete 

Interface coming UP, but traffic never restored.

Apr 29 13:00:10 MX-BP-02-06 mib2d[18654]: SNMP_TRAP_LINK_UP: ifIndex 737, ifAdminStatus up(1), ifOperStatus up(1), ifName ge-3/1/6
Apr 29 13:00:10 MX-BP-02-06 mib2d[18654]: SNMP_TRAP_LINK_UP: ifIndex 779, ifAdminStatus up(1), ifOperStatus up(1), ifName ge-3/1/6.0

After MIC reboot, ge-3/1/6 still flapping.

Below config changes were done-

Apr 29 16:17:41 MX-BP-02-06 mgd[67468]: UI_CMDLINE_READ_LINE: User 'P1362636_RW', command 'edit '
Apr 29 16:17:41 MX-BP-02-06 mgd[67468]: UI_DBASE_LOGIN_EVENT: User 'P1362636_RW' entering configuration mode
Apr 29 16:17:47 MX-BP-02-06 mgd[67468]: UI_CMDLINE_READ_LINE: User 'P1362636_RW', command 'edit groups orchestream interfaces ge-3/1/6 '
Apr 29 16:17:50 MX-BP-02-06 mgd[67468]: UI_CFG_AUDIT_SET: User 'P1362636_RW' set: [groups orchestream interfaces ge-3/1/6] unconfigured -- "disable"
Apr 29 16:17:50 MX-BP-02-06 mgd[67468]: UI_CMDLINE_READ_LINE: User 'P1362636_RW', command 'set disable '
Apr 29 16:17:58 MX-BP-02-06 mgd[67468]: UI_CMDLINE_READ_LINE: User 'P1362636_RW', command 'delete speed 100m '
Apr 29 16:17:58 MX-BP-02-06 mgd[67468]: UI_CFG_AUDIT_OTHER: User 'P1362636_RW' delete: [groups orchestream interfaces ge-3/1/6 speed] 
Apr 29 16:18:06 MX-BP-02-06 mgd[67468]: UI_CMDLINE_READ_LINE: User 'P1362636_RW', command 'delete link-mode full-duplex '
Apr 29 16:18:06 MX-BP-02-06 mgd[67468]: UI_CFG_AUDIT_OTHER: User 'P1362636_RW' delete: [groups orchestream interfaces ge-3/1/6 link-mode] 
Apr 29 16:18:09 MX-BP-02-06 mgd[67468]: UI_CMDLINE_READ_LINE: User 'P1362636_RW', command 'show | compare '
Apr 29 16:18:19 MX-BP-02-06 mgd[67468]: UI_CMDLINE_READ_LINE: User 'P1362636_RW', command 'commit synchronize and-quit '
Apr 29 16:18:19 MX-BP-02-06 mgd[67468]: UI_COMMIT: User 'P1362636_RW' requested 'commit synchronize' operation (comment: none)
Apr 29 16:18:29 MX-BP-02-06 mgd[67468]: UI_DBASE_LOGOUT_EVENT: User 'P1362636_RW' exiting configuration mode
Apr 29 16:20:29 MX-BP-02-06 mgd[67468]: UI_CMDLINE_READ_LINE: User 'P1362636_RW', command 'edit '
Apr 29 16:20:29 MX-BP-02-06 mgd[67468]: UI_DBASE_LOGIN_EVENT: User 'P1362636_RW' entering configuration mode
Apr 29 16:20:33 MX-BP-02-06 mgd[67468]: UI_CMDLINE_READ_LINE: User 'P1362636_RW', command 'exit '
Apr 29 16:20:33 MX-BP-02-06 mgd[67468]: UI_DBASE_LOGOUT_EVENT: User 'P1362636_RW' exiting configuration mode
Apr 29 16:20:43 MX-BP-02-06 mgd[67468]: UI_CMDLINE_READ_LINE: User 'P1362636_RW', command 'show interfaces terse ge-3/1/6 '
Apr 29 16:20:48 MX-BP-02-06 mgd[67468]: UI_CMDLINE_READ_LINE: User 'P1362636_RW', command 'show configuration | display set | match ge-3/1/6 '
Apr 29 16:20:51 MX-BP-02-06 mgd[67468]: UI_CMDLINE_READ_LINE: User 'P1362636_RW', command 'edit '
Apr 29 16:20:51 MX-BP-02-06 mgd[67468]: UI_DBASE_LOGIN_EVENT: User 'P1362636_RW' entering configuration mode
Apr 29 16:20:56 MX-BP-02-06 mgd[67468]: UI_CMDLINE_READ_LINE: User 'P1362636_RW', command 'edit groups orchestream interfaces ge-3/1/6 '
Apr 29 16:20:59 MX-BP-02-06 mgd[67468]: UI_CMDLINE_READ_LINE: User 'P1362636_RW', command 'delete disable '
Apr 29 16:20:59 MX-BP-02-06 mgd[67468]: UI_CFG_AUDIT_OTHER: User 'P1362636_RW' delete: [groups orchestream interfaces ge-3/1/6] "disable
Apr 29 16:21:04 MX-BP-02-06 mgd[67468]: UI_CFG_AUDIT_SET: User 'P1362636_RW' set: [groups orchestream interfaces ge-3/1/6 speed] unconfigured -- "100m"
Apr 29 16:21:04 MX-BP-02-06 mgd[67468]: UI_CMDLINE_READ_LINE: User 'P1362636_RW', command 'set speed 100m '
Apr 29 16:21:13 MX-BP-02-06 mgd[67468]: UI_CFG_AUDIT_SET: User 'P1362636_RW' set: [groups orchestream interfaces ge-3/1/6 link-mode] unconfigured -- "full-duplex"
Apr 29 16:21:13 MX-BP-02-06 mgd[67468]: UI_CMDLINE_READ_LINE: User 'P1362636_RW', command 'set link-mode full-duplex '
Apr 29 16:21:16 MX-BP-02-06 mgd[67468]: UI_CMDLINE_READ_LINE: User 'P1362636_RW', command 'show | compare '
Apr 29 16:21:23 MX-BP-02-06 mgd[67468]: UI_CMDLINE_READ_LINE: User 'P1362636_RW', command 'commit synchronize and-quit '


8) Multiple flaps observed on 3/1/7, 3/0/3, 3/0/1, 3/0/4, 3/1/4 before troubleshooting window-

nsivakumaran@qnc-css-lnx04:/volume/CSdata/nsivakumaran/2026-0429-695320/2026-04-29-MX-BP-02-06-re0/var/log$ cat default-log-messages | grep -i "2026-04-28" | grep -i "ge-3/1/7"
<28>1 2026-04-28T23:12:48.188+08:00 MX-BP-02-06 mib2d 18654 SNMP_TRAP_LINK_DOWN [junos@2636.1.1.1.2.25 snmp-interface-index="738" admin-status="up(1)" operational-status="down(2)" interface-name="ge-3/1/7"] ifIndex 738, ifAdminStatus up(1), ifOperStatus down(2), ifName ge-3/1/7
<29>1 2026-04-28T23:13:08.788+08:00 MX-BP-02-06 mib2d 18654 SNMP_TRAP_LINK_UP [junos@2636.1.1.1.2.25 snmp-interface-index="738" admin-status="up(1)" operational-status="up(1)" interface-name="ge-3/1/7"] ifIndex 738, ifAdminStatus up(1), ifOperStatus up(1), ifName ge-3/1/7


28>1 2026-04-28T23:47:26.288+08:00 MX-BP-02-06 mib2d 18654 SNMP_TRAP_LINK_DOWN [junos@2636.1.1.1.2.25 snmp-interface-index="724" admin-status="up(1)" operational-status="down(2)" interface-name="ge-3/0/3"] ifIndex 724, ifAdminStatus up(1), ifOperStatus down(2), ifName ge-3/0/3
<29>1 2026-04-28T23:47:27.288+08:00 MX-BP-02-06 mib2d 18654 SNMP_TRAP_LINK_UP [junos@2636.1.1.1.2.25 snmp-interface-index="724" admin-status="up(1)" operational-status="up(1)" interface-name="ge-3/0/3"] ifIndex 724, ifAdminStatus up(1), ifOperStatus up(1), ifName ge-3/0/3


<28>1 2026-04-28T23:30:10.988+08:00 MX-BP-02-06 mib2d 18654 SNMP_TRAP_LINK_DOWN [junos@2636.1.1.1.2.25 snmp-interface-index="722" admin-status="up(1)" operational-status="down(2)" interface-name="ge-3/0/1"] ifIndex 722, ifAdminStatus up(1), ifOperStatus down(2), ifName ge-3/0/1
<29>1 2026-04-28T23:30:21.988+08:00 MX-BP-02-06 mib2d 18654 SNMP_TRAP_LINK_UP [junos@2636.1.1.1.2.25 snmp-interface-index="722" admin-status="up(1)" operational-status="up(1)" interface-name="ge-3/0/1"] ifIndex 722, ifAdminStatus up(1), ifOperStatus up(1), ifName ge-3/0/1


<28>1 2026-04-29T01:01:04.088+08:00 MX-BP-02-06 mib2d 18654 SNMP_TRAP_LINK_DOWN [junos@2636.1.1.1.2.25 snmp-interface-index="735" admin-status="up(1)" operational-status="down(2)" interface-name="ge-3/1/4"] ifIndex 735, ifAdminStatus up(1), ifOperStatus down(2), ifName ge-3/1/4
<29>1 2026-04-29T01:01:05.088+08:00 MX-BP-02-06 mib2d 18654 SNMP_TRAP_LINK_UP [junos@2636.1.1.1.2.25 snmp-interface-index="735" admin-status="up(1)" operational-status="up(1)" interface-name="ge-3/1/4"] ifIndex 735, ifAdminStatus up(1), ifOperStatus up(1), ifName ge-3/1/4



<28>1 2026-04-29T00:23:43.188+08:00 MX-BP-02-06 mib2d 18654 SNMP_TRAP_LINK_DOWN [junos@2636.1.1.1.2.25 snmp-interface-index="725" admin-status="up(1)" operational-status="down(2)" interface-name="ge-3/0/4"] ifIndex 725, ifAdminStatus up(1), ifOperStatus down(2), ifName ge-3/0/4
<29>1 2026-04-29T00:24:50.788+08:00 MX-BP-02-06 mib2d 18654 SNMP_TRAP_LINK_UP [junos@2636.1.1.1.2.25 snmp-interface-index="725" admin-status="up(1)" operational-status="up(1)" interface-name="ge-3/0/4"] ifIndex 725, ifAdminStatus up(1), ifOperStatus up(1), ifName ge-3/0/4

9) Self IP ping & monitor traffic was not tested during troubleshooting. 


File location


/volume/CSdata/nsivakumaran/2026-0429-695320/

RSI & varlogs

20260429-MX-BD-02-06-RSI.log 
2026-04-29-MX-BP-02-06-re0
2026-04-29-MX-BP-02-06-re1

Session logs

'20260429-MX-BD-02-06-tshoot (Full session Log).log'
'20260429- MX-BP-02-06 - Tshoot (reboot+reini).log

Traffic graphs

'MX-BP-02-06 Traffic Graph.zip

FPC logs

20260430-MX-BP-02-06-Shell-Output.txt

Syslog server logs

syslog/20260430-MX-BP-02-06_28-29AprSyslogs.txt