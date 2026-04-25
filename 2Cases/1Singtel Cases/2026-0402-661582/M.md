2026-0402-661582 - SINGTEL -- SINGNET - NERA - Mitigation when CHASSISD_OVER_TEMP_CONDITION happens








----------
Dear Chunxian & Suhua,

I emulated the same phenomenon in lab and found that offline FPC can prevent chassis from shutting down:

Hostname: jtac-mx960-r2038-re0
Model: mx960
Junos: 23.2R2-S3.8
Only one FPC in slot 6 

Configured below event-option to trigger an auto offline of the over temp FPC:
```
labroot@jtac-mx960-r2038-re0# show event-options 
policy proactive-offline-overtemp {
    events [ chassisd_temp_hot_notice chassisd_over_temp_condition ];
    attributes-match {
        chassisd_temp_hot_notice.message matches "FPC 6";
    }
    then {
        execute-commands {
            commands {
                "request chassis fpc slot 6 offline";
            }
        }
    }
}
```

After blocked air flow & removed the top fan try, the device temp started going high. FPC6 was offlined right after the 240s log appeared and it successfully prevent the chassis from shutting down:
<<<<
Apr 10 09:09:47.482  jtac-mx960-r2038-re0 chassisd[22278]: CHASSISD_TEMP_HOT_NOTICE: FPC 6:Exhaust B temperature of 81 degrees C is above limit (80 degrees)

Apr 10 09:09:47.833  jtac-mx960-r2038-re0 chassisd[22278]: CHASSISD_OVER_TEMP_CONDITION: Chassis found temperature above over_temp condition temperature at one or more sensors (fan/impeller failure detected); routing platform will shutdown in 240 seconds if condition persists
Apr 10 09:09:47.835  jtac-mx960-r2038-re0 alarmd[22406]: Alarm set: Temp sensor id=67108966, color=RED, class=CHASSIS, reason=Temperature Hot
Apr 10 09:09:47.846  jtac-mx960-r2038-re0 craftd[22283]:  Major alarm set, Temperature Hot


Apr 10 09:09:48.189  jtac-mx960-r2038-re0 chassisd[22278]: CHASSISD_FRU_OFFLINE_NOTICE: Taking FPC 6 offline: Offlined by cli command
Apr 10 09:09:48.697  jtac-mx960-r2038-re0 chassisd[22278]: CHASSISD_SNMP_TRAP10: SNMP trap generated: FRU power off (jnxFruContentsIndex 7, jnxFruL1Index 7, jnxFruL2Index 0, jnxFruL3Index 0, jnxFruName FPC: MPC2E NG HQoS @ 6/*/*, jnxFruType 3, jnxFruSlot 6, jnxFruOfflineReason 7, jnxFruLastPowerOff 8461793, jnxFruLastPowerOn 1452)

Apr 10 09:09:52.706  jtac-mx960-r2038-re0 alarmd[22406]: Alarm cleared: Temp sensor id=67108966, color=RED, class=CHASSIS, reason=Temperature Hot
Apr 10 09:09:52.706  jtac-mx960-r2038-re0 craftd[22283]: Major alarm cleared, Temperature Hot
<<<<

Since this test exposes the device to possible HW damage, I am not allowed to test more. This result is for your information. 


-------------


Hi Annabelle,

Can we ask for some help from escalation or DE to clarify this ?

What I know is in MX2020/2010, MX will be shutting down the individual FRU instead of entire chassis. Since this mechanism exists, I assume that by offline the high temperatures FRU in MX960/480 will mitigate the over temperatures situation for this FRU. It is just not automatic on MX960.

Below is the statement in MX Power Management.pdf in our internal site.

If a FRU starts to overheat, the fan speed is increased and alarms are raised. If the temperature continues to rise and
reaches a critical level, the FRU is shutdown. The overheating condition is reached when the exhaust sensor temperature
reading exceeds the High Temperature (HT) threshold. The critical condition is reached when the sensor reading exceeds
the Over Temperature (OT) threshold. If the FRU remains in critical condition for 240 seconds it will be shut down. If the sensor reading exceeds the Fire Temperature (FT) threshold for 4 seconds it will be shut down on an MX2010/MX2020 whereas the entire chassis will be shut down on an MX240/480/960.

Some old PR also saying the same.

https://gnats.juniper.net/web/default/1245884#audit_tab



----------

Dear Chunxian,

I didnt find any document indicates that offlining/removing the overheated FRU within the 240-second window will explicitly cancel the pending chassis shutdown. The available code confirms the presence of thermal thresholds, chassis over-temperature shutdown logic, and FRU/FPC offline handling, but no direct logic was found stating that an offline/remove action aborts the timer once started.

The most reasonable is that such an action may help only if it clears the chassis over-temperature condition before timer expiry. However, this is not guaranteed, since thermal sensing/state refresh and cooldown are not instantaneous.



# Case
For MX router, we know there is 240s timer countdown to shutdown all the FRUs after Temperature Hot alarm being generate.
Refer to below syslog from a real temperature related case, the question is if user offline FPC1 within 240s, would the shutdown process be stopped?
FYI:
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
