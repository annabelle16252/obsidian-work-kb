/volume/case_2026/2026-0312-632154
/volume/CSdata/annaw/case/2026-0312-632154

Hostname: xn-paoeq1-bo101
Model: mx960
Junos: 23.2R2-S3.8


MW on 11 March to replace RE and SCBE from RE-S-1800X4-32G-S/SCBE2-MX-S to RE-S-X6-128G-S-S/SCBE3-MX-S.


2 alarms currently active
Alarm time               Class  Description
2026-03-11 12:21:07 +08  Minor  PEM 3 Fan Failed
2026-03-11 03:56:52 +08  Minor  Check CB 2 Fabric Chip 0


    Start time                     2026-03-11 01:56:19 +08

    Start time                     2026-03-11 01:32:54 +08
    Uptime                         1 day, 7 hours, 38 minutes, 15 seconds
    Last reboot reason             0x1:power cycle/failure



$ cat 20260312-xn-paoeq1-bo101-logs_1  | grep -v BGP | grep -v NTP | grep -v "trap_io_send_trap" | grep -v 'DDOS' | grep -v bgp | grep -v nsa


# KB
KB31496  [MX/EX] How to check for alarms on CB 0 Fabric Chip 1
https://juniper.lightning.force.com/lightning/r/Knowledge__kav/ka0Dp000000ivYzIAI/view

KB31717 Syslog message: MQSS.*FO: Request timeout error - Number of timeouts .*, RC select .*, Stream
https://juniper.lightning.force.com/lightning/r/Knowledge__kav/ka0Dp000000ikxgIAA/view

KB28102 Syslog message: XMCHIP(.): FI: Reorder cell timeout - Stream .*, Count .*
https://juniper.lightning.force.com/lightning/r/Knowledge__kav/ka0Dp000000j0KSIAY/view


KB35971 Syslog message: fpc.* CMTFPC: Fabric request time out pfe .* plane .* .*, attempting recovery
https://juniper.lightning.force.com/lightning/r/Knowledge__kav/ka0Dp000000jkEKIAY/view





# Fabric Stream
https://junipernetworks.sharepoint.com/:x:/r/teams/CSS/ee/routingandswitching/_layouts/15/Doc.aspx?sourcedoc=%7BD2BEBDB5-DC18-408A-B894-48FF9ACC303D%7D&file=fabric-streams-rev3.xlsx&action=default&mobileredirect=true&DefaultItemOpen=1


# Logs
```






re1 master - link ok





re0 master switch
show chassis fabric plane
Plane 5
  Plane state: ACTIVE
      FPC 0
          PFE 0 :Links ok
      FPC 3
          PFE 0 :Links ok
          PFE 1 :Links ok
      FPC 5
          PFE 0 :Links ok
      FPC 8
          PFE 0 :Links ok
          PFE 1 :Links ok
      FPC 9
          PFE 0 :Links ok
          PFE 1 :Links ok
      FPC 10
          PFE 0 :Link error
          PFE 1 :Links ok
yelin.htun@xn-paoeq1-bo101> show chassis alarms 
Mar 11 03:16:13
1 alarms currently active
Alarm time               Class  Description
2026-03-11 02:52:09 +08  Minor  Check CB 2 Fabric Chip 0


yelin.htun@xn-paoeq1-bo101> request chassis fabric plane 5 offline     
Mar 11 03:22:42
Offline initiated, use "show chassis fabric plane" to verify


Mar 11 02:31:54  xn-paoeq1-bo101 alarmd[24777]: %DAEMON-4: Alarm set: CB id=134218358, color=YELLOW, class=CHASSIS, reason=CB 2 Fabric Chip 0 Not Online
Mar 11 02:31:54  xn-paoeq1-bo101 craftd[16834]: %DAEMON-4:  Minor alarm set, CB 2 Fabric Chip 0 Not Online
Mar 11 02:31:54  xn-paoeq1-bo101 : %PFE-3: fpc3 MQSS(0): FO: Request timeout error - Number of timeouts 1, RC select 4, Stream 40 
Mar 11 02:31:55  xn-paoeq1-bo101 : %PFE-3: fpc9 XMCHIP(1): XMCHIP(1): FO: Request timeout error - Number of timeouts 1, RC select 1, Stream 40 
Mar 11 02:31:55  xn-paoeq1-bo101 : %PFE-3: fpc10 XMCHIP(0): XMCHIP(0): FO: Request timeout error - Number of timeouts 7, RC select 1, Stream 0 
Mar 11 02:31:55  xn-paoeq1-bo101 : %PFE-3: fpc8 XMCHIP(1): XMCHIP(1): FO: Request timeout error - Number of timeouts 1, RC select 1, Stream 40 
Mar 11 02:31:57  xn-paoeq1-bo101 alarmd[24777]: %DAEMON-4: Alarm set: CB id=268436086, color=YELLOW, class=CHASSIS, reason=CB 2 Not Online
Mar 11 02:31:57  xn-paoeq1-bo101 craftd[16834]: %DAEMON-4:  Minor alarm set, CB 2 Not Online
Mar 11 02:31:57  xn-paoeq1-bo101 craftd[16834]: %DAEMON-4: Minor alarm cleared, Check CB 2 Fabric Chip 0



Mar 11 03:22:42  xn-paoeq1-bo101 : %PFE-3: fpc10 XMCHIP(0): XMCHIP(0): FO: Request timeout error - Number of timeouts 7, RC select 1, Stream 0 
Mar 11 03:22:42  xn-paoeq1-bo101 : %PFE-3: fpc8 XMCHIP(1): XMCHIP(1): FO: Request timeout error - Number of timeouts 1, RC select 1, Stream 40 
Mar 11 03:22:42  xn-paoeq1-bo101 : %PFE-3: fpc9 XMCHIP(0): XMCHIP(0): FO: Request timeout error - Number of timeouts 1, RC select 1, Stream 40 
Mar 11 03:22:43  xn-paoeq1-bo101 : %PFE-3: fpc9 XMCHIP(1): XMCHIP(1): FO: Request timeout error - Number of timeouts 1, RC select 1, Stream 40 
Mar 11 03:22:43  xn-paoeq1-bo101 : %PFE-3: fpc3 MQSS(0): FO: Request timeout error - Number of timeouts 2, RC select 4, Stream 168 
Mar 11 03:22:44  xn-paoeq1-bo101 alarmd[24777]: %DAEMON-4: Alarm set: CB id=134218358, color=YELLOW, class=CHASSIS, reason=CB 2 Fabric Chip 0 Not Online
Mar 11 03:22:44  xn-paoeq1-bo101 craftd[16834]: %DAEMON-4:  Minor alarm set, CB 2 Fabric Chip 0 Not Online
Mar 11 03:23:44  xn-paoeq1-bo101 alarmd[24777]: %DAEMON-4: Alarm cleared: CB id=134218358, color=YELLOW, class=CHASSIS, reason=CB 2 Fabric Chip 0 Not Online
Mar 11 03:23:44  xn-paoeq1-bo101 craftd[16834]: %DAEMON-4: Minor alarm cleared, CB 2 Fabric Chip 0 Not Online


Mar 11 05:51:27  xn-paoeq1-bo101 : %PFE-3: fpc10 CMTFPC: Fabric request time out pfe 1 plane 5 fab_stream 40, attempting recovery 
Mar 11 05:51:27  xn-paoeq1-bo101 : %PFE-3: fpc10 XMCHIP(1): XMCHIP(1): FO: Request timeout error - Number of timeouts 1, RC select 1, Stream 40 

Mar 11 06:29:14  xn-paoeq1-bo101 : %PFE-3: fpc3 CMTFPC: Fabric request time out pfe 0 plane 5 fab_stream 168, attempting recovery 



annaw@p-jtac-lnx01:.../log$ zcat messages.*.gz | grep "FI: Reorder cell timeout"
Mar 11 13:04:33  xn-paoeq1-bo101 : %PFE-3: fpc3 MQSS(0): FI: Reorder cell timeout - Stream 40, Count 1 
Mar 11 13:13:07  xn-paoeq1-bo101 : %PFE-3: fpc9 XMCHIP(1): XMCHIP(1): FI: Reorder cell timeout - Stream 40, Count 1 
Mar 11 13:36:24  xn-paoeq1-bo101 : %PFE-3: fpc10 XMCHIP(1): XMCHIP(1): FI: Reorder cell timeout - Stream 40, Count 1 
Mar 11 13:40:23  xn-paoeq1-bo101 : %PFE-3: fpc3 MQSS(0): FI: Reorder cell timeout - Stream 40, Count 1 
Mar 11 13:45:09  xn-paoeq1-bo101 : %PFE-3: fpc8 XMCHIP(0): XMCHIP(0): FI: Reorder cell timeout - Stream 40, Count 1 
Mar 11 13:45:46  xn-paoeq1-bo101 : %PFE-3: fpc10 XMCHIP(1): XMCHIP(1): FI: Reorder cell timeout - Stream 40, Count 1 
Mar 11 14:02:20  xn-paoeq1-bo101 : %PFE-3: fpc8 XMCHIP(0): XMCHIP(0): FI: Reorder cell timeout - Stream 40, Count 1 
Mar 11 14:20:00  xn-paoeq1-bo101 : %PFE-3: fpc10 XMCHIP(1): XMCHIP(1): FI: Reorder cell timeout - Stream 40, Count 1 
Mar 11 14:20:49  xn-paoeq1-bo101 : %PFE-3: fpc10 XMCHIP(1): XMCHIP(1): FI: Reorder cell timeout - Stream 40, Count 1 
Mar 11 14:28:12  xn-paoeq1-bo101 : %PFE-3: fpc0 XMCHIP(0): XMCHIP(0): FI: Reorder cell timeout - Stream 40, Count 1 
Mar 11 14:35:29  xn-paoeq1-bo101 : %PFE-3: fpc0 XMCHIP(0): XMCHIP(0): FI: Reorder cell timeout - Stream 40, Count 1 
Mar 11 14:43:27  xn-paoeq1-bo101 : %PFE-3: fpc3 MQSS(0): FI: Reorder cell timeout - Stream 40, Count 1 
Mar 11 15:20:35  xn-paoeq1-bo101 : %PFE-3: fpc3 MQSS(0): FI: Reorder cell timeout - Stream 40, Count 1 


          
```


# command
``
```
 show chassis fabric plane
 
 
 
```
