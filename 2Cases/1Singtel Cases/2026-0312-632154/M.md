


Dear Chee Loong,

Alarm came back shortly after CB replacement, right? I see the last alarm was cleared after FPC10's removal:
<<<
Alarm cleared after FPC10 removal:
Mar 14 00:21:12 CHASSISD_FRU_OFFLINE_NOTICE: Taking FPC 10 offline: Offlined by cli command
Mar 14 00:21:15  xn-paoeq1-bo101 alarmd[24777]: %DAEMON-4: Alarm cleared: CB id=150995574, color=YELLOW, class=CHASSIS, reason=Check CB 2 Fabric Chip 0
Mar 14 00:21:15  xn-paoeq1-bo101 craftd[16834]: %DAEMON-4: Minor alarm cleared, Check CB 2 Fabric Chip 0
Mar 14 00:23:02 CHASSISD_SNMP_TRAP7: SNMP trap generated: FRU removal (jnxFruContentsIndex 7, jnxFruL1Index 11, jnxFruL2Index 0, jnxFruL3Index 0, jnxFruName FPC: MPC5E 3D 2CGE+4XGE @ 10/*/*, jnxFruType 3, jnxFruSlot 10)
<<<


Have you returned the defective FPC10 to Juniper? If not, you may inserted into a chassis to collect the syslog if by your convenience. RA has been flagged.

I will put the case under monitoring.








---------------
I think you have an typo below.
 
From logs, I can see alarm was cleared once FPC10 CB was removed and no further alarm after replacement:
<<< 
CB replacement:
Mar 13 23:30:39  xn-paoeq1-bo101 craftd[16834]: %DAEMON-4:  Minor alarm set, CB 2 Removed
Mar 13 23:31:51  xn-paoeq1-bo101 alarmd[24777]: %DAEMON-4: Alarm cleared: CB id=33555062, color=YELLOW, class=CHASSIS, reason=CB 2 Removed
Mar 13 23:31:51  xn-paoeq1-bo101 craftd[16834]: %DAEMON-4: Minor alarm cleared, CB 2 Removed
 
2./ Since FPC10 slot is empty now, the “request pfe execute command “show syslog messages” target fpc10 | no-more” will not work.
I have tested it in Nera lab and there are no syslog output when FPC10 slot is empty.
 
3./ Please keep this case under monitoring until the defective parts are received by Juniper AR.

--------------


Dear Chee Loong,


From logs, I can see alarm was cleared once FPC10 was removed and no further alarm after replacement:
<<<
CB replacement:
Mar 13 23:30:39  xn-paoeq1-bo101 craftd[16834]: %DAEMON-4:  Minor alarm set, CB 2 Removed
Mar 13 23:31:51  xn-paoeq1-bo101 alarmd[24777]: %DAEMON-4: Alarm cleared: CB id=33555062, color=YELLOW, class=CHASSIS, reason=CB 2 Removed
Mar 13 23:31:51  xn-paoeq1-bo101 craftd[16834]: %DAEMON-4: Minor alarm cleared, CB 2 Removed


Alarm came back:
phpang@xn-paoeq1-bo101> show chassis alarms 
Mar 13 23:42:46
3 alarms currently active
Alarm time               Class  Description
2026-03-13 23:39:22 +08  Minor  Check CB 2 Fabric Chip 0
phpang@xn-paoeq1-bo101> ## after replacing CB2 error was cleared but came back now
Mar 13 23:45:06


Alarm cleared after FPC10 removal:
Mar 14 00:21:12 CHASSISD_FRU_OFFLINE_NOTICE: Taking FPC 10 offline: Offlined by cli command
Mar 14 00:21:15  xn-paoeq1-bo101 alarmd[24777]: %DAEMON-4: Alarm cleared: CB id=150995574, color=YELLOW, class=CHASSIS, reason=Check CB 2 Fabric Chip 0
Mar 14 00:21:15  xn-paoeq1-bo101 craftd[16834]: %DAEMON-4: Minor alarm cleared, Check CB 2 Fabric Chip 0
Mar 14 00:23:02 CHASSISD_SNMP_TRAP7: SNMP trap generated: FRU removal (jnxFruContentsIndex 7, jnxFruL1Index 11, jnxFruL2Index 0, jnxFruL3Index 0, jnxFruName FPC: MPC5E 3D 2CGE+4XGE @ 10/*/*, jnxFruType 3, jnxFruSlot 10)


No further alarm after FPC10 replacement:
Mar 14 00:46:16 CHASSISD_SNMP_TRAP7: SNMP trap generated: FRU insertion (jnxFruContentsIndex 7, jnxFruL1Index 11, jnxFruL2Index 0, jnxFruL3Index 0, jnxFruName FPC: MPC3E NG PQ & Flex Q @ 10/*/*, jnxFruType 3, jnxFruSlot 10)
Mar 14 00:59:18 CHASSISD_SNMP_TRAP7: SNMP trap generated: FRU removal (jnxFruContentsIndex 7, jnxFruL1Index 11, jnxFruL2Index 0, jnxFruL3Index 0, jnxFruName FPC: MPC3E NG PQ & Flex Q @ 10/*/*, jnxFruType 3, jnxFruSlot 10)
<<<


Please take KB31496 for reference:
https://supportportal.juniper.net/s/article/MX-How-to-check-for-alarms-on-CB-0-Fabric-Chip
This might be either FPC or CB issue. As in our case, alarm was cleared right away after FPC10's removal. This should be a FPC10 hardware failure.

I didnt see CRC error from var/log, could you check fpc syslog to see if there's any?
>request pfe execute command “show syslog messages” target fpc10 | no-more





Mar 11 02:07:17 FPC 10 - part number 750-054564, serial number CAFJ7457
Mar 14 00:46:16 FPC 10 - part number 750-063181, serial number EBBJ4943



Mar 14 00:21:15  FM: End of PFE board offline fabric event for slot 10, sub-slot 0, evq=0x0
Mar 14 00:21:15  send: yellow alarm clear, device CB 2, reason Check CB 2 Fabric Chip 0
Mar 14 00:21:15 CHASSISD_SNMP_TRAP7: SNMP trap generated: fabric plane online (jnxFruContentsIndex 12, jnxFruL1Index 3, jnxFruL2Index 0, jnxFruL3Index 0, jnxFruName C
B 2, jnxFruType 5, jnxFruSlot 2)




Mar 11 02:07:17 FPC 5 - part number 750-063181, serial number EBBJ4943
Mar 14 00:59:59 FPC 5 - part number 750-063181, serial number EBBJ4943





--------


Hi Annabelle,
 
I have uploaded 14 Mar MW session logs with latest RSI and varlogs.
 
Updated MW Summary:
After CB2 part was replaced, the alarm cleared initially but reappeared.
To minimize downtime, MPC5E-100G10G RMA part was inserted to FPC11 slot for replacement.
FPC5 linecard was reinserted to FPC10 slot for testing, and alarm did not reappear.
Linecard is now back to FPC5 slot and FPC10 slot is empty now.
 
Please review the logs and provide analysis and a KB for this issue, if available. Thank you.

-----------

Hi Naveen,

 

1./ The MW activity was successful.



2./ MW Summary:

After CB2 part was replaced, the alarm cleared initially but reappeared
After replacing FPC10, the alarm cleared


3./ Please check if there is any KB for this issue.

 

root@xn-paoeq1-bo101> show chassis alarms no-forwarding

2 alarms currently active

Alarm time               Class  Description

<snip>

2026-03-11 03:56:52 +08  Minor  Check CB 2 Fabric Chip 0

 

root@xn-paoeq1-bo101> show chassis fabric summary no-forwarding

 

Plane   State    Uptime

0      Online   1 day, 7 hours, 2 minutes, 57 seconds

1      Online   1 day, 7 hours, 2 minutes, 57 seconds

2      Online   1 day, 7 hours, 2 minutes, 57 seconds

3      Online   1 day, 7 hours, 2 minutes, 57 seconds

4      Online   1 day, 6 hours, 36 minutes, 52 seconds

5      Check    1 day, 5 hours, 47 minutes, 40 seconds


root@xn-paoeq1-bo101> show chassis fabric plane no-forwarding

Fabric management PLANE state



 Plane 5

  Plane state: ACTIVE

      FPC 10

          PFE 0 :Link error

          PFE 1 :Links ok






--------
To:
CFTS-Bangalore-Telco@juniper.net;CFTS-Bangalore-Routing@juniper.net

CC:
balamurugan.sekar@hpe.com;jaeho.seo@hpe.com;Japan-CFTS@juniper.net;support-private@juniper.net

[Handoff to BNG] Singtel | MX960 | 2026-0312-632154 - 19982 - SINGTEL -- IX - NERA - xn-paoeq1-bo101 - [Singtel:Singtel-IX:xn-paoeq1-bo101] CB2 and PEM3 Fan Failed alarms appeared after ticket 19536 MW | Ticket #19982

Hello CFTS Bangalore team,

Please accept this case and create RMA for SCB2&FPC10 once customer provided shipping information.

REASON FOR HANDOVER:
RMA creation request. 

ISSUE DESCRIPTION:
Alarm time Class Description
2026-03-11 12:21:07 +08 Minor PEM 3 Fan Failed
2026-03-11 03:56:52 +08 Minor Check CB 2 Fabric Chip 0
Need to proceed RMA for SCB2&FPC10.

ACTION PERFORMED:
Troubleshooting has been done, please refer to case history.

RESPONSIBLE MANAGER:
John Seo

CUSTOMER CONTACT:
FRANCIS (Partner)

ENGINEERING ENGAGED:
NA

TELEPHONE BRIEFING FOR ACCEPTING ENGINEER: YES OR NO
No.

LAB SETUP AVAILABILITY: YES OR NO:
No.

SERVICE MANAGER ENGAGED:
Pieter

NEXT-UPDATE:
Base on customer execution of action plan.

-------------

Dear Chee Loong,

May I know if you need me to handoff the case & 2026-0312-632353 if RMA shipping information is not available within my shift? 

We can create RMA for both CB2&FPC10 and you may replace CB2 first. If issue resolved, please help to keep FPC2 unopened and return it back to Juniper.

------------

Hi Annabelle,
 
We also suspect CB2 to be faulty but to maximise the MW, we suggest to replace both CB2 and FPC10 at once.
 
I will send the shipping information once the CB2 part is tied to the chassis serial number.
 

----------

Dear Chee Loong,

All the FI errors are pointing to 'fabric stream 40' which is the fabric stream connecting to FPC10:
<<<
annaw@p-jtac-lnx01:.../log$ cat messages | grep "FI: Reorder cell timeout"
Mar 11 23:52:28  xn-paoeq1-bo101 : %PFE-3: fpc10 XMCHIP(0): XMCHIP(0): FI: Reorder cell timeout - Stream 40, Count 1 
Mar 11 23:56:19  xn-paoeq1-bo101 : %PFE-3: fpc10 XMCHIP(1): XMCHIP(1): FI: Reorder cell timeout - Stream 40, Count 1 
Mar 12 00:04:34  xn-paoeq1-bo101 : %PFE-3: fpc0 XMCHIP(0): XMCHIP(0): FI: Reorder cell timeout - Stream 40, Count 1 
Mar 12 00:10:30  xn-paoeq1-bo101 : %PFE-3: fpc10 XMCHIP(0): XMCHIP(0): FI: Reorder cell timeout - Stream 40, Count 1 
Mar 12 00:26:51  xn-paoeq1-bo101 : %PFE-3: fpc10 XMCHIP(1): XMCHIP(1): FI: Reorder cell timeout - Stream 40, Count 1 
Mar 12 00:36:36  xn-paoeq1-bo101 : %PFE-3: fpc0 XMCHIP(0): XMCHIP(0): FI: Reorder cell timeout - Stream 40, Count 1 
Mar 12 00:57:39  xn-paoeq1-bo101 : %PFE-3: fpc0 XMCHIP(0): XMCHIP(0): FI: Reorder cell timeout - Stream 40, Count 1 
<snip .. >
<<<


FO are reporting to different different fabric streams and it indicate issue with Fabric or destination FPC:
<<<
annaw@p-jtac-lnx01:.../log$ cat messages | grep "FO: Request timeout error"
Mar 12 00:40:10  xn-paoeq1-bo101 : %PFE-3: fpc3 MQSS(1): FO: Request timeout error - Number of timeouts 1, RC select 4, Stream 40 
Mar 12 01:08:01  xn-paoeq1-bo101 : %PFE-3: fpc10 XMCHIP(0): XMCHIP(0): FO: Request timeout error - Number of timeouts 1, RC select 1, Stream 141 
Mar 12 01:14:41  xn-paoeq1-bo101 : %PFE-3: fpc8 XMCHIP(1): XMCHIP(1): FO: Request timeout error - Number of timeouts 1, RC select 1, Stream 168 
Mar 12 01:46:52  xn-paoeq1-bo101 : %PFE-3: fpc0 XMCHIP(0): XMCHIP(0): FO: Request timeout error - Number of timeouts 1, RC select 17, Stream 40 
Mar 12 02:53:03  xn-paoeq1-bo101 : %PFE-3: fpc10 XMCHIP(1): XMCHIP(1): FO: Request timeout error - Number of timeouts 1, RC select 1, Stream 40 
Mar 12 03:07:41  xn-paoeq1-bo101 : %PFE-3: fpc3 MQSS(1): FO: Request timeout error - Number of timeouts 1, RC select 4, Stream 40 
Mar 12 03:17:48  xn-paoeq1-bo101 : %PFE-3: fpc10 XMCHIP(0): XMCHIP(0): FO: Request timeout error - Number of timeouts 1, RC select 1, Stream 141 
Mar 12 03:32:51  xn-paoeq1-bo101 : %PFE-3: fpc3 MQSS(1): FO: Request timeout error - Number of timeouts 1, RC select 4, Stream 40 
<snip .. >
<<<

Can also see chassis made an attempt to self-heal the link but failed:
<<<
Mar 12 01:46:51  xn-paoeq1-bo101 : %PFE-3: fpc0 CMTFPC: Fabric request time out pfe 0 plane 5 fab_stream 40, attempting recovery 
Mar 12 02:53:03  xn-paoeq1-bo101 : %PFE-3: fpc10 CMTFPC: Fabric request time out pfe 1 plane 5 fab_stream 40, attempting recovery 
Mar 12 03:07:40  xn-paoeq1-bo101 : %PFE-3: fpc3 CMTFPC: Fabric request time out pfe 1 plane 5 fab_stream 40, attempting recovery 
Mar 12 03:17:48  xn-paoeq1-bo101 : %PFE-3: fpc10 CMTFPC: Fabric request time out pfe 0 plane 5 fab_stream 141, attempting recovery 
Mar 12 03:32:50  xn-paoeq1-bo101 : %PFE-3: fpc3 CMTFPC: Fabric request time out pfe 1 plane 5 fab_stream 40, attempting recovery 
Mar 12 03:41:37  xn-paoeq1-bo101 : %PFE-3: fpc10 CMTFPC: Fabric request time out pfe 0 plane 5 fab_stream 33, attempting recovery 
Mar 12 03:52:03  xn-paoeq1-bo101 : %PFE-3: fpc10 CMTFPC: Fabric request time out pfe 0 plane 5 fab_stream 33, attempting recovery 
Mar 12 04:03:05  xn-paoeq1-bo101 : %PFE-3: fpc8 CMTFPC: Fabric request time out pfe 1 plane 5 fab_stream 168, attempting recovery 
Mar 12 04:37:06  xn-paoeq1-bo101 : %PFE-3: fpc3 CMTFPC: Fabric request time out pfe 0 plane 5 fab_stream 168, attempting recovery 
Mar 12 05:21:23  xn-paoeq1-bo101 : %PFE-3: fpc10 CMTFPC: Fabric request time out pfe 0 plane 5 fab_stream 140, attempting recovery 
Mar 12 05:26:50  xn-paoeq1-bo101 : %PFE-3: fpc10 CMTFPC: Fabric request time out pfe 1 plane 5 fab_stream 40, attempting recovery 
Mar 12 06:02:58  xn-paoeq1-bo101 : %PFE-3: fpc0 CMTFPC: Fabric request time out pfe 0 plane 5 fab_stream 40, attempting recovery 
<<<

Also we can see 'plane 5 link error' even after offline/online the plane, plus seeing alarm 'Check CB 2 Fabric Chip 0'.

We suspect the issue is with SCB2, so we would recommend below troubleshooting steps:
1.Reseat SCB2
2.If cannot recover the issue, reseat FPC10
3.If issue still persist, replace SCB2
4.If still cannot resolve it, then replace FPC10







-----------

Hi Annabelle,
 
I have uploaded the session logs from the MW.
 
From the session logs, it seems that FPC10 had the same link error before, and not fpc8.
 
We may have miscommunicated internally on the FPC number.
 
Please check on this. Thank you.
 
      FPC 10
          PFE 0 :Link error
          PFE 1 :Links ok
----------

Dear Chee Loong,

Thanks for uploading varlogs and yes we also suspect it's CB2 hardware failure.a

After reviewing logs, multiple FPCs reported fabric timeout errors and fabric plane 5 has link error.
Since 'CB 2 Fabric Chip 0' alarm reoccurs after renounce, we can proceed RMA for it.


For 'PEM 3 Fan Failed' alarm, could you help to open a new case for it? 

------------

Hi Annabelle,
 
I have uploaded the varlogs.
 
We are suspecting the CB2 to be faulty. In the previous MW, the FPC8 had link error for plane 5 and now it’s for FPC10.
 
There are also multiple FPCs fabric request timeout errors seen.
 
We are still waiting for the session logs and in the midst of getting CB2 to be tied to the chassis for RMA.
 
        Line 1974: Mar 11 06:29:14  xn-paoeq1-bo101 : %PFE-3: fpc3 CMTFPC: Fabric request time out pfe 0 plane 5 fab_stream 168, attempting recovery
        Line 3157: Mar 11 09:06:41  xn-paoeq1-bo101 : %PFE-3: fpc10 XMCHIP(1): XMCHIP(1): FI: Reorder cell timeout - Stream 40, Count 1
        Line 3185: Mar 11 09:10:17  xn-paoeq1-bo101 : %PFE-3: fpc8 XMCHIP(0): XMCHIP(0): FI: Reorder cell timeout - Stream 40, Count 1
        Line 3259: Mar 11 09:20:05  xn-paoeq1-bo101 : %PFE-3: fpc3 MQSS(0): FI: Reorder cell timeout - Stream 40, Count 1
        Line 3343: Mar 11 09:31:03  xn-paoeq1-bo101 : %PFE-3: fpc10 XMCHIP(1): XMCHIP(1): FI: Reorder cell timeout - Stream 40, Count 1
        Line 3373: Mar 11 09:35:26  xn-paoeq1-bo101 : %PFE-3: fpc3 MQSS(0): FI: Reorder cell timeout - Stream 40, Count 1
        Line 3377: Mar 11 09:35:32  xn-paoeq1-bo101 : %PFE-3: fpc10 CMTFPC: Fabric request time out pfe 0 plane 5 fab_stream 169, attempting recovery
        Line 3378: Mar 11 09:35:32  xn-paoeq1-bo101 : %PFE-3: fpc10 XMCHIP(0): XMCHIP(0): FO: Request timeout error - Number of timeouts 1, RC select 1, Stream 169
        Line 3454: Mar 11 09:45:39  xn-paoeq1-bo101 : %PFE-3: fpc10 XMCHIP(1): XMCHIP(1): FI: Reorder cell timeout - Stream 40, Count 1
        Line 3487: Mar 11 09:49:29  xn-paoeq1-bo101 : %PFE-3: fpc10 XMCHIP(1): XMCHIP(1): FO: Request timeout error - Number of timeouts 1, RC select 1, Stream 40
        Line 3488: Mar 11 09:49:30  xn-paoeq1-bo101 : %PFE-3: fpc10 CMTFPC: Fabric request time out pfe 1 plane 5 fab_stream 40, attempting recovery
        Line 3549: Mar 11 09:57:59  xn-paoeq1-bo101 : %PFE-3: fpc10 XMCHIP(1): XMCHIP(1): FI: Reorder cell timeout - Stream 40, Count 1
        Line 3556: Mar 11 09:59:27  xn-paoeq1-bo101 : %PFE-3: fpc3 MQSS(0): FI: Reorder cell timeout - Stream 40, Count 1
        Line 3597: Mar 11 10:04:31  xn-paoeq1-bo101 : %PFE-3: fpc0 XMCHIP(0): XMCHIP(0): FI: Reorder cell timeout - Stream 40, Count 1
        Line 3780: Mar 11 10:29:13  xn-paoeq1-bo101 : %PFE-3: fpc10 XMCHIP(1): XMCHIP(1): FI: Reorder cell timeout - Stream 40, Count 1
        Line 3803: Mar 11 10:32:13  xn-paoeq1-bo101 : %PFE-3: fpc9 XMCHIP(1): XMCHIP(1): FI: Reorder cell timeout - Stream 40, Count 1
        Line 3834: Mar 11 10:36:22  xn-paoeq1-bo101 : %PFE-3: fpc8 XMCHIP(0): XMCHIP(0): FI: Reorder cell timeout - Stream 40, Count 1
        Line 3931: Mar 11 10:48:44  xn-paoeq1-bo101 : %PFE-3: fpc10 XMCHIP(1): XMCHIP(1): FI: Reorder cell timeout - Stream 40, Count 1
        Line 4014: Mar 11 10:59:42  xn-paoeq1-bo101 : %PFE-3: fpc3 MQSS(0): FI: Reorder cell timeout - Stream 40, Count 1
        Line 4213: Mar 11 11:26:42  xn-paoeq1-bo101 : %PFE-3: fpc8 XMCHIP(0): XMCHIP(0): FI: Reorder cell timeout - Stream 40, Count 1
        Line 4257: Mar 11 11:32:17  xn-paoeq1-bo101 : %PFE-3: fpc9 XMCHIP(1): XMCHIP(1): FI: Reorder cell timeout - Stream 40, Count 1
        Line 4295: Mar 11 11:36:36  xn-paoeq1-bo101 : %PFE-3: fpc10 CMTFPC: Fabric request time out pfe 0 plane 5 fab_stream 141, attempting recovery
        Line 4296: Mar 11 11:36:36  xn-paoeq1-bo101 : %PFE-3: fpc10 XMCHIP(0): XMCHIP(0): FO: Request timeout error - Number of timeouts 1, RC select 1, Stream 141
        Line 4324: Mar 11 11:40:58  xn-paoeq1-bo101 : %PFE-3: fpc8 CMTFPC: Fabric request time out pfe 0 plane 5 fab_stream 40, attempting recovery
        Line 4328: Mar 11 11:40:59  xn-paoeq1-bo101 : %PFE-3: fpc8 XMCHIP(0): XMCHIP(0): FO: Request timeout error - Number of timeouts 1, RC select 1, Stream 40
        Line 4380: Mar 11 11:47:43  xn-paoeq1-bo101 : %PFE-3: fpc10 CMTFPC: Fabric request time out pfe 0 plane 5 fab_stream 141, attempting recovery
        Line 4381: Mar 11 11:47:43  xn-paoeq1-bo101 : %PFE-3: fpc10 XMCHIP(0): XMCHIP(0): FO: Request timeout error - Number of timeouts 1, RC select 1, Stream 141
        Line 4406: Mar 11 11:51:01  xn-paoeq1-bo101 : %PFE-3: fpc10 XMCHIP(1): XMCHIP(1): FI: Reorder cell timeout - Stream 40, Count 1
 
 


-------------------

Dear Chee Loong,

This is Annabelle Wang from Japan CFTS Team and I will be assisting you with your case.

I will go through the case and provide you and update later.