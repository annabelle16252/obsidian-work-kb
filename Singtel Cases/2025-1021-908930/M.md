Dear Alex,

Yes, you can use below command to clear the alarm and keep monitoring.

==================================

Hi Annabelle,

1./ No need to handoff this case to next shift team.

2./ But the alarm is still currently active right? Can we use the cli command below to remove the alarm? 

> clear chassis fpc errors fpc-slot 3 all

==================================

Dear Alex,

We can see below syslog triggered the minor alarm:

<<<

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

<<<

This  is similar transit hardware issue as in KB81667:

[https://supportportal.juniper.net/s/article/MX-ASIC-Error-detected-errorno-0x000700fa](https://supportportal.juniper.net/s/article/MX-ASIC-Error-detected-errorno-0x000700fa)

In our case, it only occured once and set as minor alarm. If multiple occurance like in the KB:

<<<

May 9 20:47:47 lab-router : %PFE-3: fpc2 XMCHIP(0): XMCHIP(0): EPM0: Packet delimiter violation detected - Queue 64 

May 9 20:48:00 lab-router alarmd[9040]: %DAEMON-4: Alarm set: FPC id=150995560, color=RED, class=CHASSIS, reason=FPC 2 Major Errors

<<<

So it's save to monitor the device for now. If similar log re-occurs or alarm changed to major, we can consider to reboot FPC. If issue still persist, then we can proceed RMA.

===========================

Dear Alex,

As you can see the var/log of RE0, messages doesnt cover the time period of alarm first occurance period. Logs are overwritten by flooding logs:

<<<

root@SNGCCK-ER1> show chassis alarms no-forwarding

1 alarms currently active

Alarm time               Class  Description

2025-08-07 15:47:01 +08  Minor  FPC 3 Minor Errors

<<<

From chassisd, I can see below triggered the alarm:

<<<

Aug  7 15:47:01 CHASSISD_FPC_ASIC_ERROR: <FPC 3> ASIC Error detected errorno 0x000700fa (null)

Aug  7 15:47:01  send: yellow alarm set, device FPC 3, reason FPC 3 Minor Errors

<<<

We can refer to below KB for this error:

[https://supportportal.juniper.net/s/article/MX-ASIC-Error-detected-errorno-0x000700fa](https://supportportal.juniper.net/s/article/MX-ASIC-Error-detected-errorno-0x000700fa)

Could you help to check if we can get syslog covering the issue occurance time period?

Also we can get below output:

<<<

From fpc3 shell:

show syslog messages

show nvram

<<<

=======================

Dear Alex,

This is Annabelle Wang from Japan CFTS team and I will be assisting you with your case.

I will go through the case and provide you and update later.