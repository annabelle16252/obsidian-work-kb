
 - Server 地址： 10.104.8.126
 - 用户名：annaw
 - Log 文件路径：/volume/CSdata/annaw/case/2025-1021-908930/var-re0


/volume/case_2025/2025-1021-908930

/volume/CSdata/annaw/case/2025-1021-908930

Hostname: SNGCCK-ER1

Model: mx480

Junos: 21.2R3-S9.21

JUNOS Release 23.2R2-S4.5

root@SNGCCK-ER1> show chassis alarms no-forwarding

1 alarms currently active

Alarm time               Class  Description

2025-08-07 15:47:01 +08  Minor  FPC 3 Minor Errors

FPC 3            REV 35   750-063180   EBBJ6550          MPC3E NG HQoS

Aug  7 15:47:01 CHASSISD_FPC_ASIC_ERROR: <FPC 3> ASIC Error detected errorno 0x000700fa (null)

Aug  7 15:47:01  send: yellow alarm set, device FPC 3, reason FPC 3 Minor Errors

KB81667

[https://juniper.lightning.force.com/lightning/r/Knowledge__kav/ka0Dp000000irdjIAA/view](https://juniper.lightning.force.com/lightning/r/Knowledge__kav/ka0Dp000000irdjIAA/view)

2024-0813-232478

=================

Aug  7 15:47:00 SNGCCK-ER1 rshd[10830]: root@re1 as root: cmd='ls -i /var/etc/filters/filter-define.conf'

Aug  7 15:47:01 SNGCCK-ER1 rshd[10834]: root@re1 as root: cmd='ls -i /var/etc/filters/filter-define.conf'

Aug  7 15:47:01 SNGCCK-ER1 fpc3 XMCHIP(0): XMCHIP(0): EPM0: Protect: Log Error 0x1, Log Address 0x868, Multiple Errors 0x0

Aug  7 15:47:01 SNGCCK-ER1 fpc3 Cmerror: Draining ASIC error message queue

Aug  7 15:47:01 SNGCCK-ER1 fpc3 cmerror_process_queue: module = XMCHIP(0)

Aug  7 15:47:01 SNGCCK-ER1 fpc3 Cmerror: processing the task op_type 1 for scope 1 and category 0 level 0 level_count 0 occur_count 0 clear_count 0 l

evel_threshold 1 level_action 0x5   item errid 459002 item_threshold 1 item_count 0 item_sub_err_state 0 sub_item errid 0 sub_item_state 0 item_times

tamp 0 current timestamp -20

Aug  7 15:47:01 SNGCCK-ER1 chassisd[10765]: CHASSISD_FPC_ASIC_ERROR: <FPC 3> ASIC Error detected errorno 0x000700fa (null)

Aug  7 15:47:01 SNGCCK-ER1 alarmd[12418]: Alarm set: FPC id=167773032, color=YELLOW, class=CHASSIS, reason=FPC 3 Minor Errors

Aug  7 15:47:01 SNGCCK-ER1 craftd[10769]:  Minor alarm set, FPC 3 Minor Errors

Aug  7 15:47:01 SNGCCK-ER1 mosquitto[10823]: Allocated node for mosq : 0x79a780, Client : client-1-re0-NA_periodic_subscriber-re0, topic : /1001/1/0,

 max bytes in queue : 10485760, hash_size is 500, hashIndex is 0x1221000

Aug  7 15:47:01 SNGCCK-ER1 fpc3 Cmerror: Level 0 count increment 1 occur_count 1 clear_count 0

Aug  7 15:47:01 SNGCCK-ER1 fpc3 CMError: /fpc/3/pfe/0/cm/0/XMCHIP(0)/0/XMCHIP_CMERROR_EPM_PAR_PROTECT_FSET_REG_DETECTED_FP (0x700fa), scope: pfe, cat

egory: functional, severity: minor, module: XMCHIP(0), type: EPM_PROTECT: Detected: Parity error for free pool single port SRAM

Aug  7 15:47:02 SNGCCK-ER1 rshd[10838]: root@re1 as root: cmd='ls -i /var/etc/filters/filter-define.conf'

Aug  7 15:47:02 SNGCCK-ER1 fpc3 Cmerror: Level 0 count 1 (occur_count 1 clear_count 0)crossed threshold 1 action 0x5

Aug  7 15:47:02 SNGCCK-ER1 fpc3 is_managed_external 1, category 0

Aug  7 15:47:02 SNGCCK-ER1 fpc3 alarm count for level 0 is incremented  and set to 1

Aug  7 15:47:02 SNGCCK-ER1 fpc3 is_managed_external 1, category 0

Aug  7 15:47:02 SNGCCK-ER1 fpc3 Cmerror category is NOT internal 

Aug  7 15:47:02 SNGCCK-ER1 fpc3 Performing action log for error /fpc/3/pfe/0/cm/0/XMCHIP(0)/0/XMCHIP_CMERROR_EPM_PAR_PROTECT_FSET_REG_DETECTED_FP (0x

700fa) in module: XMCHIP(0) with scope: pfe category: functional level: minor

Aug  7 15:47:02 SNGCCK-ER1 fpc3 cmerror_take_action_helper: performing action 1 for scope 1 category 0 level 0 err_id /fpc/3/pfe/0/cm/0/XMCHIP(0)/0/X

MCHIP_CMERROR_EPM_PAR_PROTECT_FSET_REG_DETECTED_FP(0x700fa) module id 21

Aug  7 15:47:03 SNGCCK-ER1 fpc3 Performing action cmalarm for error /fpc/3/pfe/0/cm/0/XMCHIP(0)/0/XMCHIP_CMERROR_EPM_PAR_PROTECT_FSET_REG_DETECTED_FP

 (0x700fa) in module: XMCHIP(0) with scope: pfe category: functional level: minor

Aug  7 15:47:03 SNGCCK-ER1 rshd[10842]: root@re1 as root: cmd='ls -i /var/etc/filters/filter-define.conf'

Aug  7 15:47:03 SNGCCK-ER1 fpc3 cmerror_take_action_helper: performing action 4 for scope 1 category 0 level 0 err_id /fpc/3/pfe/0/cm/0/XMCHIP(0)/0/X

MCHIP_CMERROR_EPM_PAR_PROTECT_FSET_REG_DETECTED_FP(0x700fa) module id 21

Aug  7 15:47:03 SNGCCK-ER1 fpc3 Cmerror Op Set: XMCHIP(0): XMCHIP(0): EPM0: Protect: Parity error for free pool single port SRAM  (URI: /fpc/3/pfe/0/

cm/0/XMCHIP(0)/0/XMCHIP_CMERROR_EPM_PAR_PROTECT_FSET_REG_DETECTED_FP)