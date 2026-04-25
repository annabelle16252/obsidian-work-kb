
2026-0413-672364 - 20393 - SINGTEL -- CPLUS -NERA - FFTEQ-BB1 - [Singtel:Singtel-CPlus:FFTEQ-BB1] Major FPC 0 Major Errors | Ticket #20393





---------

Dear Alex,

As per checking, the major alarm is due to below kind of 'HOST_LOOPBACK_MAKE_CMERROR' which triggered pfe-disable action:
<<<
Apr 12 09:46:23.338 2026  FFTEQ-BB1 : %PFE-5: fpc0 CMError: /fpc/0/pfe/0/cm/0/Host_Loopback/0/HOST_LOOPBACK_MAKE_CMERROR_ID[1] (0x20002), scope: pfe, category: functional, severity: major, module: Host Loopback, type: Host Loopback Path Id 1 
Apr 12 09:46:23.346 2026  FFTEQ-BB1 rpd: %USER-3-JTASK_SEND_RECV_ERROR: : Send msg call failed for task rsvp-io with error : Network is down
Apr 12 09:46:23.937 2026  FFTEQ-BB1 last message repeated 6 times
Apr 12 09:46:23.979 2026  FFTEQ-BB1 : %PFE-5: fpc0 Performing action get-state for error /fpc/0/pfe/0/cm/0/Host_Loopback/0/HOST_LOOPBACK_MAKE_CMERROR_ID[1] (0x20002) in module: Host Loopback with scope: pfe category: functional level: major 
Apr 12 09:46:24.326 2026  FFTEQ-BB1 rpd: %USER-3-JTASK_SEND_RECV_ERROR: : Send msg call failed for task rsvp-io with error : Network is down
Apr 12 09:46:24.430 2026  FFTEQ-BB1 : %PFE-5: fpc0 Performing action cmalarm for error /fpc/0/pfe/0/cm/0/Host_Loopback/0/HOST_LOOPBACK_MAKE_CMERROR_ID[1] (0x20002) in module: Host Loopback with scope: pfe category: functional level: major 
Apr 12 09:46:24.879 2026  FFTEQ-BB1 : %PFE-5: fpc0 Performing action disable-pfe for error /fpc/0/pfe/0/cm/0/
<<<

We have below KB75140 for the same issue. This is a transient FPC HW issue and can be resolved by FPC reboot.
https://supportportal.juniper.net/s/article/MX-Syslog-MessageFPC-1-Major-Errors

Since issue has been resolved by FPC0 restart in our case, we can monitor the device for now.




--------

ext-singtel-cs@juniper.net;gso-advancedandpremiumcare@juniper.net;cfts-apac-singtel@juniper.net;sm-sp.psm.sg@nera.net;support@juniper.net;ext-Singtel-cs@groups.ext.hpe.com


Dear Alex,

This is Annabelle Wang from Japan CFTS Team and I will be assisting you with your case.

I will go through the case and provide you and update later.



# Case
Hi CFTS,
Singtel reported that they received FPC 0 major alarm. They restart the FPC and the alarm was cleared. Please help to investigate if it is a transient issue or hardware failure. RSI and /var/log had been uploaded to JSP.