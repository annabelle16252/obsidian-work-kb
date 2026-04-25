





Hi Annabelle,
 
This type of work needs some work to make things work. Would you have an access to any lab device, possibly inside Japan office, where you can control the temperature/block air flow? Ask for Masami-san's help and see if you can be in such an environment. Or, please let me know.

----------------

Dear Shin,

I did one round test today:
1.Blocked air flow, temp went high very slowly.
2.Blocked air flow and removed top fan tray, temperature started going high smoothly:
```
Chassis suddenly shutdown:
---(refreshed at 2026-04-09 09:22:11 IST)---
                     Temp  CPU Utilization (%)   CPU Utilization (%)  Memory    Utilization (%)
Slot State            (C)  Total  Interrupt      1min   5min   15min  DRAM (MB) Heap     Buffer
  6  Online            69      6          0        5      5      5    3584        7         25
---(refreshed at 2026-04-09 09:22:12 IST)---
                     Temp  CPU Utilization (%)   CPU Utilization (%)  Memory    Utilization (%)
Slot State            (C)  Total  Interrupt      1min   5min   15min  DRAM (MB) Heap     Buffer
  6  Online            69      6          0        5      5      5    3584        7         25


Apr  9 09:18:12.457  jtac-mx960-r2038-re0 chassisd[31223]: CHASSISD_TEMP_HOT_NOTICE: FPC 6:Exhaust B temperature of 81 degrees C is above limit (80 degrees)

Apr  9 09:18:12.812  jtac-mx960-r2038-re0 chassisd[31223]: CHASSISD_OVER_TEMP_CONDITION: Chassis found temperature above over_temp condition temperature at one or more sensors (fan/impeller failure detected); routing platform will shutdown in 240 seconds if condition persists

Apr  9 09:22:07.857  jtac-mx960-r2038-re0 chassisd[31223]: CHASSISD_TEMP_HOT_NOTICE: FPC 6:Exhaust B temperature of 88 degrees C is above limit (80 degrees)
Apr  9 09:22:07.859  jtac-mx960-r2038-re0 chassisd[31223]: CHASSISD_TEMP_HOT_NOTICE: FPC 6:Intake temperature of 87 degrees C is above limit (80 degrees)
Apr  9 09:22:12.864  jtac-mx960-r2038-re0 chassisd[31223]: CHASSISD_TEMP_HOT_NOTICE: FPC 6:Exhaust B temperature of 89 degrees C is above limit (80 degrees)
Apr  9 09:22:12.866  jtac-mx960-r2038-re0 chassisd[31223]: CHASSISD_TEMP_HOT_NOTICE: FPC 6:Intake temperature of 87 degrees C is above limit (80 degrees)

Apr  9 09:22:13.222  jtac-mx960-r2038-re0 chassisd[31223]: CHASSISD_OVER_TEMP_SHUTDOWN_TIME: Chassis temperature above 80 degrees C for too long  (> 240 seconds); powering down all FRUs
```

Dont know why event-option didnt trigger FPC6 offline although I saw the time gap was within 180s.

Lab team alerted by this chassis shutdown and they pointed that this test carries the risk of an abrupt shutdown, they permitted to proceed only one final time test.  They request that we capture all necessary data and ensure the test is completed during this run itself.

Could you help to check the lab (jtac-mx960-r2038/10.219.23.134) to see why event-option didnt work and what we can change to make sure this time we can emulate the issue. Like delete the 'within 180' or any hardware removal, like remove the FPC9?





-----------



3 should work. And/or just remove both of the fan trays ;>

---------------

Dear Shin,

Thanks for checking the case.

I tried in lab (jtac-mx960-r2038/10.219.23.134), but failed to emulate the FPC temperature high:
1.use command 'test chassis fan all speed 0', but only can make top fan try speed to 0 and all bottom fan try speed are around 3000rpm. This wont make FPC6 (the only online fpc without any traffic) temperature higher than 70C.

2.Lab team removed bottom fan tray, but 'test chassis fan all speed 0' wont work,  all the top fan always run at full speed around 5k rpm.

3.Lab team will block air flow tomorrow for me and it will make the whole chassis temp going high possibly. 

Would you have any suggestion on the current and next step test? 



------------


Hi Annabelle,

It should...you or Su can try in the lab, by configuring some event options and blocking air flow or removing fan tray for a while. Event script would be better to get the FPC slot number from the syslog, or you may have to cover all the slots under event-options.

```
[edit]

labroot@radiomir-re0# show event-options 

policy proactive-offline-overtemp {

    events chassisd_over_temp_condition;

    within 180 events chassisd_temp_hot_notice;

    attributes-match {

        chassisd_temp_hot_notice.fru-name equals "FPC 1";

    }

    then {

        execute-commands {

            commands {

                "request chassis fpc slot 1 offline";

            }

        }

    }

}
```

Best regards,

Shin


----------------------

To: jtac-hw-escalation@juniper.net
CC: support-private@juniper.net


[Escalation] 2026-0402-661582 - SINGTEL -- SINGNET - NERA - Mitigation when CHASSISD_OVER_TEMP_CONDITION happens


Hello Escalations team, 

Could you please assist me with this Singtel case:

Hostname: HCMSPT-ER4
Model: mx960
Junos: 23.2R2-S3.8

Issue description:
For MX router, we know there is 240s timer countdown to shutdown all the FRUs after Temperature Hot alarm being generate. Refer to below syslog from a real temperature related case, and customer's question is that if offline the FPC1 within 240s would prevent the shutdown process?
<<<<
red alarm:
Any FRU excess the red alarm threshold for 240s continuiously , system will power off the entire router including all FRUs( RE, CB, FPC). Human intervention is needed to power on the router.

chassisd msg:
(base) huasu@JNPR-MAC-7546XD log % zgrep temper * -r | more
chassisd:Jul 30 17:44:16 CHASSISD_TEMP_HOT_NOTICE: FPC 1 temperature of 91 degrees C is above limit     (90 degrees)
chassisd:Jul 30 17:44:16 CHASSISD_TEMP_HOT_NOTICE: FPC 1 temperature of 91 degrees C is above limit (90 degrees)
chassisd:Jul 30 17:44:16 CHASSISD_TEMP_HOT_NOTICE: FPC 1 temperature of 91 degrees C is above limit (90 degrees)
chassisd:Jul 30 17:44:16 CHASSISD_TEMP_HOT_NOTICE: FPC 1 temperature of 91 degrees C is above limit (90 degrees)
chassisd:Jul 30 17:44:17 CHASSISD_OVER_TEMP_CONDITION: Chassis temperature over 90 degrees C (but no fan/impeller failure detected); routing platform will shutdown in 240 seconds if condition persists
chassisd:Jul 30 17:44:21 CHASSISD_TEMP_HOT_NOTICE: FPC 1 temperature of 91 degrees C is above limit (90 degrees)
chassisd:Jul 30 17:44:21 CHASSISD_TEMP_HOT_NOTICE: FPC 1 temperature of 91 degrees C is above limit (90 degrees)
chassisd:Jul 30 17:44:21 CHASSISD_TEMP_HOT_NOTICE: FPC 1 temperature of 91 degrees C is above limit (90 degrees)
chassisd:Jul 30 17:44:21 CHASSISD_TEMP_HOT_NOTICE: FPC 1 temperature of 91 degrees C is above limit (90 degrees)
<....>
chassisd:Jul 30 17:48:52 CHASSISD_OVER_TEMP_CONDITION: Chassis temperature over 90 degrees C (but no fan/impeller failure detected); routing platform will shutdown in 4 seconds if condition persists
chassisd:Jul 30 17:48:57 CHASSISD_TEMP_HOT_NOTICE: FPC 1 temperature of 91 degrees C is above limit (90 degrees)
chassisd:Jul 30 17:48:57 CHASSISD_TEMP_HOT_NOTICE: FPC 1 temperature of 91 degrees C is above limit (90 degrees)
chassisd:Jul 30 17:48:57 CHASSISD_TEMP_HOT_NOTICE: FPC 1 temperature of 91 degrees C is above limit (90 degrees)
chassisd:Jul 30 17:48:57 CHASSISD_TEMP_HOT_NOTICE: FPC 1 temperature of 91 degrees C is above limit (90 degrees)
chassisd:Jul 30 17:48:57 CHASSISD_OVER_TEMP_SHUTDOWN_TIME: Chassis temperature above 90 degrees C for too long (> 240 seconds); powering down all FRUs
chassisd:Jul 30 17:48:57 FPC#0 - power off [addr 0x12] reason: Over temperature
chassisd:Jul 30 17:48:57 FPC#1 - power off [addr 0x13] reason: Over temperature
chassisd:Jul 30 17:48:57 FPC#2 - power off [addr 0x14] reason: Over temperature              
<<<<

JTAC Analysis:
Based on the MX Power Management document:
https://junipernetworks.sharepoint.com/:w:/r/sites/plm/_layouts/15/Doc.aspx?sourcedoc=%7BF37B4B07-F7C0-40AC-A5A8-68CCBAB77C64%7D&file=MX_Power_Management.docx&action=default&mobileredirect=true&DefaultItemOpen=1

My understanding is that MX2010/MX2020 explicitly support FRU-level thermal shutdown, so in those platforms an overheated FRU can be shut down without taking down the entire chassis.

Could the same mechanism be applied to MX960/MX480, such that manually offlining an overheated FRU would be considered a valid thermal mitigation from a platform design perspective?

Also, there is an old PR requesting similar behavior for MX960/MX480, but it appears to have had no further progress:  
https://gnats.juniper.net/web/default/1245884#audit_tab

VAR/LOG Location:
/volume/CSdata/annaw/case/2026-0402-661582