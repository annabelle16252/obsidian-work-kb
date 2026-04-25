2026-0422-685188 - 20522 - SINGTEL -- SINGNET - NERA - SN-SINPL1-BO406 - [Singtel:Singtel-Singnet:SN-SINPL1-BO406] FPC0 rebooted | Ticket #20522





Dear Neo,

Normally the PFE on which the HMC errors is observed should be disabled automatically and Major alarm raised by software limiting service disruption. To recover a Line card with a disabled pfe, the line card has to be rebooted manually. This is the issue seen in KB78500.

However, KB78500 also mentions that 'This error can lead to the FPC  restarting and generating a core dump'. This is what happened in our case. That's because during the above mentioned process, accidentally the HMC errors lead to LLM (read/write interface to ASIC) timeouts in the control plane leading to a CPU hog and FPC crashed.  We can see this from coredump debug. This happens sometimes for this kind of error. The major alarm is not triggered in a timely manner before the process becomes stuck。

'functional level: major' can be seen from our device:
<<<
/fpc/0/pfe/0/cm/0/EA[2:0]/2/EACHIP_CMERROR_HMCIF_RX_LINK_INT_REG_HMC_FATAL_ERR (0x2400c1) in module: EA[2:0] with scope: pfe category: functional level: major, oc_category: default 
<<<


------------------

Hi Annabelle,

1./ May I check with you why there is no chassis alarm/system alarm showing for this issue as per reported by customer. From the KB78500, it states that there should be an alarm set "Major alarm set, FPC 7 Major Errors".

--------------


Dear Neo,

As per coredump analysis result, this issue matches below KB:
KB78500 [MX] FPC Major Errors - EACHIP_CMERROR_HMCIF_RX_LINK_INT_REG_HMC_FATAL_ERR (0x2400c1)
https://supportportal.juniper.net/s/article/MX-FPC-Major-Errors-EACHIP-CMERROR-HMCIF-RX-LINK-INT-REG-HMC-FATAL-ERR-0x2400c1

EACHIP_CMERROR_HMCIF_RX_LINK_INT_REG_HMC_FATAL_ERR indicates that the Hybrid Memory Cube (HMC) has encountered a multi-bit uncorrectable error in its internal logic from which it cannot recover without a reset. This error can lead to the FPC  restarting and generating a core dump. This error is typically a transient hardware failure. These errors disable the PFE and impact traffic.

We can see CMERROR from the device:
<<<
Apr 22 00:37:46.570 2026  SN-SINPL1-BO406 : %PFE-5: fpc0 CMError: /fpc/0/pfe/0/cm/0/EA[2:0]/2/EACHIP_CMERROR_HMCIF_RX_LINK_INT_REG_HMC_FATAL_ERR (0x2400c1), scope: pfe, category: functional, severity: major, module: EA[2:0], type: HMCIF RX link int reg HMC fatal err, oc_category: default 
Apr 22 00:37:46.571 2026  SN-SINPL1-BO406 : %PFE-5: fpc0 Performing action get-state for error /fpc/0/pfe/0/cm/0/EA[2:0]/2/EACHIP_CMERROR_HMCIF_RX_LINK_INT_REG_HMC_FATAL_ERR (0x2400c1) in module: EA[2:0] with scope: pfe category: functional level: major, oc_category: default 
<<<

Since it's a transit HW issue and FPC has restarted , please keep monitor the device.


----------

```
Apr 22 00:37:46  fpc0 CMError: EACHIP_CMERROR_HMCIF_RX_LINK_INT_REG_HMC_FATAL_ERR
Apr 22 00:37:46  fpc0 Performing action get-state ...
Apr 22 00:37:47  EA[2:0]-1 Congestion Detected
Apr 22 00:37:47  MQSS(2): DRD reorder ID timeout
Apr 22 00:37:47  PFE_ERROR_FAIL_OPERATION: Wedge op single step failed
Apr 22 00:37:49  listmgr_host_xtxn_idle_debug_capture: xtxn idle failure
Apr 22 00:37:50  Host XTXN 2 busy waiting for Response
Apr 22 00:37:50-00:39  show_llm_host_xtxn_dbg ... repeated
Apr 22 00:46:47 onward  PFEMAN resync in progress
Apr 22 00:49:02      Message rcvd from pb 0 ... pb_up:0
```


---------

Dear Neo,

This is Annabelle Wang from Japan CFTS Team and I will be assisting you with your case.

I will go through the case and provide you and update later.


# Case
Hi CFTS,

1./ Customer reported MX2008 FPC0 rebooted at 0040hrs, there a core dump generated from this incident. Please assist to look into this issue and advise if any RMA is required.

2./ RSI and /var/log and core dump have been uploaded to JSP

