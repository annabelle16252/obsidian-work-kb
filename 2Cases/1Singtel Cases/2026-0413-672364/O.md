/volume/case_2026/2026-0413-672364
/volume/CSdata/annaw/case/2026-0323-648596

Hostname: FFTEQ-BB1
Model: mx960
Junos: 21.2R3-S9.21

# AI_log_analysis
Juniper case analysis request
Issue: fpc0 major alarm, alarm was cleared by fpc reboot
log path: /volume/CSdata/annaw/case/2026-0413-672364/var-re0/var/log
Task: 
1.rca for the fpc0 alarm
2.check if any kb for same issue

# Log

Apr 12 09:46:08.195 2026  FFTEQ-BB1 craftd[23112]: %DAEMON-4:  Minor alarm set, FPC 0 Minor Errors
Apr 12 09:46:10.055 2026  FFTEQ-BB1 : %PFE-5: fpc0 Performing action cmalarm for error /fpc/0/pfe/0/cm/0/XMCHIP(0)/0/XMCHIP_CMERROR_WI_WICPQ_FREEPTR_SRAM_PAR_PROTECT_FSET_REG_DETECTED_WICPQ_FREEPTR_SRAM (0x7035e) in module: XMCHIP(0) with scope: pfe category: functional level: minor 
Apr 12 09:46:17.550 2026  FFTEQ-BB1 alarmd[33056]: %DAEMON-4: Alarm set: FPC id=150995048, color=RED, class=CHASSIS, reason=FPC 0 Major Errors
==Apr 12 09:46:17.550 2026  FFTEQ-BB1 craftd[23112]: %DAEMON-4:  Major alarm set, FPC 0 Major Errors==
Apr 12 09:46:24.430 2026  FFTEQ-BB1 : %PFE-5: fpc0 Performing action cmalarm for error /fpc/0/pfe/0/cm/0/Host_Loopback/0/HOST_LOOPBACK_MAKE_CMERROR_ID[1] (0x20002) in module: Host Loopback with scope: pfe category: functional level: major 
Apr 12 09:46:27.651 2026  FFTEQ-BB1 : %PFE-5: fpc0 Performing action cmalarm for error /fpc/0/pfe/0/cm/0/Host_Loopback/0/HOST_LOOPBACK_MAKE_CMERROR_ID[2] (0x20003) in module: Host Loopback with scope: pfe category: functional level: major 
Apr 12 09:46:30.752 2026  FFTEQ-BB1 : %PFE-5: fpc0 Performing action cmalarm for error /fpc/0/pfe/0/cm/0/Host_Loopback/0/HOST_LOOPBACK_MAKE_CMERROR_ID[3] (0x20004) in module: Host Loopback with scope: pfe category: functional level: major 
Apr 12 09:46:33.724 2026  FFTEQ-BB1 : %PFE-5: fpc0 Performing action cmalarm for error /fpc/0/pfe/0/cm/0/TOE-XM-0:0:0/0/TOE_ERR_TX_BLOCKED[1] (0x10409) in module: TOE-XM-0:0:0 with scope: pfe category: processing level: major 
Apr 12 09:46:41.121 2026  FFTEQ-BB1 : %PFE-5: fpc0 Performing action cmalarm for error /fpc/0/pfe/0/cm/0/TOE-XM-0:0:0/0/TOE_ERR_TX_BLOCKED[1] (0x10409) in module: TOE-XM-0:0:0 with scope: pfe category: processing level: major 
Apr 12 09:46:44.113 2026  FFTEQ-BB1 : %PFE-5: fpc0 Performing action cmalarm for error /fpc/0/pfe/0/cm/0/TOE-XM-0:0:0/0/TOE_ERR_TX_BLOCKED[1] (0x10409) in module: TOE-XM-0:0:0 with scope: pfe category: processing level: major 
Apr 12 09:46:48.087 2026  FFTEQ-BB1 : %PFE-5: fpc0 Performing action cmalarm for error /fpc/0/pfe/0/cm/0/TOE-XM-0:0:0/0/TOE_ERR_TX_BLOCKED[1] (0x10409) in module: TOE-XM-0:0:0 with scope: pfe category: processing level: major 
Apr 12 09:46:51.080 2026  FFTEQ-BB1 : %PFE-5: fpc0 Performing action cmalarm for error /fpc/0/pfe/0/cm/0/TOE-XM-0:0:0/0/TOE_ERR_TX_BLOCKED[1] (0x10409) in module: TOE-XM-0:0:0 with scope: pfe category: processing level: major 

==Apr 12 10:00:58.370 2026  FFTEQ-BB1 alarmd[33056]: %DAEMON-4: Alarm cleared: FPC id=150995048, color=RED, class=CHASSIS, reason=FPC 0 Major Errors==
Apr 12 10:00:58.370 2026  FFTEQ-BB1 craftd[23112]: %DAEMON-4: Major alarm cleared, FPC 0 Major Errors
Apr 12 10:00:58.381 2026  FFTEQ-BB1 alarmd[33056]: %DAEMON-4: Alarm cleared: FPC id=167772264, color=YELLOW, class=CHASSIS, reason=FPC 0 Minor Errors
Apr 12 10:00:58.397 2026  FFTEQ-BB1 craftd[23112]: %DAEMON-4: Minor alarm cleared, FPC 0 Minor Errors


