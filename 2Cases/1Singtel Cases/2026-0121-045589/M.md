=====================

Hi Annabelle,

Below is the shipping address:

Defective Unit Information:

Chassis Serial Number: JN125899DAFB

Part Serial Number: EBBG7780

Part Number: MPC3E-3D-NG-Q

Shipping Details:

(1) Shipping Customer Emails

[jjhwan@singtel.com](mailto:jjhwan@singtel.com), [EGIPI@singtel.com](mailto:EGIPI@singtel.com), [sm-sp.psm.sg@nera.net](mailto:sm-sp.psm.sg@nera.net), [pieter.van-houwelingen@hpe.com](mailto:pieter.van-houwelingen@hpe.com)

(2) Ship to Contact Name:

Contact Name:    Peter Jung Jin Hwan

Contact Number:  [+82-2-3287-7592](tel:+82232877592), [+82 10 8777 8865](tel:+821087778865)

Email Address:  [jjhwan@singtel.com](mailto:jjhwan@singtel.com)

(3) Ship to Company Name:

"Singapore Telecom Korea Limited

35th Floor, Trade Tower,

511, Yeongdong-Daero, Gangnam-gu

Seoul 06164

Republic of Korea"

(4) Special delivery instructions (limit of 255 char)         

Please kindly call the contact person to arrange for RMA delivery

Please also get the delivery team to call the contact person at least 2 hours in advance before the delivery. This is an office building, the building only operation 0900~1800 working day. 

(5) Is CE required? No      

(6) Data Center Security Ticket# Required?:

      (a) Data Center Security Ticket# Required for Hardware : No

      (b) Data Center Security Ticket# Required for CE : No

==================

Dear Alex,

Please provide shipping information for use to proceed RMA.

BTW, we will be on lunch time in 40 minutes time and will create the RMA once back.

==================

Hi Annabelle,

Singtel has requested a replacement part to be on standby in case the issue reoccurs. I approve this due to the impact on Singtel’s customers. Please proceed with opening an RMA solely for the replacement part, without CE.

Hi Alex,

Could you please coordinate with Singtel to obtain the necessary address details for the RMA process?

Thank you for your support.

Pieter Van Houwelingen

==================

Dear Alex,

Yes, the issue hits KB72740.

We see that FPC1 reported both minor and major errors due to increment in Link Sanity Check counter in the XM chip. First minor alarm raised and then major alarm raised with 'PFE Disable' action of PFE0 FPC1:

<<<

Jan 18 02:59:24.466 2026  SELLB-ER4 chassisd[27847]: %DAEMON-3-CHASSISD_FPC_ASIC_ERROR: <FPC 1> ASIC Error detected errorno 0x00070138 (null)

Jan 18 02:59:24.468 2026  SELLB-ER4 alarmd[49069]: %DAEMON-4: Alarm set: FPC id=167772520, color=YELLOW, class=CHASSIS, reason=FPC 1 Minor Errors

Jan 18 02:59:24.468 2026  SELLB-ER4 craftd[27851]: %DAEMON-4:  Minor alarm set, FPC 1 Minor Errors

Jan 18 02:59:26.012 2026  SELLB-ER4 : %PFE-5: fpc1 Performing action cmalarm for error /fpc/1/pfe/0/cm/0/XMCHIP(0)/0/XMCHIP_CMERROR_FI_INT_REG_LINK_SANITY_CHECK_MINOR (0x70138) in module: XMCHIP(0) with scope: pfe category: functional level: minor

Jan 20 15:43:59.626 2026  SELLB-ER4 chassisd[27847]: %DAEMON-3-CHASSISD_FPC_ASIC_ERROR: <FPC 1> ASIC Error detected errorno 0x00070138 (null)

Jan 21 06:59:27.337 2026  SELLB-ER4 : %PFE-5: fpc1 Performing action cmalarm for error /fpc/1/pfe/0/cm/0/XMCHIP(0)/0/XMCHIP_CMERROR_FI_INT_REG_LINK_SANITY_CHECK_MAJOR (0x70139) in module: XMCHIP(0) with scope: pfe category: functional level: major

Jan 21 06:59:27.452 2026  SELLB-ER4 alarmd[49069]: %DAEMON-4: Alarm set: FPC id=150995304, color=RED, class=CHASSIS, reason=FPC 1 Major Errors

Jan 21 06:59:27.452 2026  SELLB-ER4 craftd[27851]: %DAEMON-4:  Major alarm set, FPC 1 Major Errors

Jan 21 06:59:32.154 2026  SELLB-ER4 : %PFE-3: fpc1 Cmerror Op Set: XMCHIP(0): XMCHIP(0): FI: Link sanity checks - Type 2, Seq Number 1504, Stream 0, Link0 0x20, Link1 0xc0, Link2 0x1  (URI: /fpc/1/pfe/0/cm/0/XMCHIP(0)/0/XMCHIP_CMERROR_FI_INT_REG_LINK_SANITY_CHECK_MAJOR)

Jan 21 06:59:32.626 2026  SELLB-ER4 : %PFE-5: fpc1 PFE 0: 'PFE Disable' action performed. Bringing down ifd ge-1/2/1 267

Jan 21 06:59:32.735 2026  SELLB-ER4 : %PFE-5: fpc1 PFE 0: 'PFE Disable' action performed. Bringing down ifd ge-1/2/4 270

Jan 21 06:59:32.845 2026  SELLB-ER4 : %PFE-5: fpc1 PFE 0: 'PFE Disable' action performed. Bringing down ifd xe-1/0/0 256

<<<

I saw the card was restarted on 21st January and no further recurrences have been reported.

<<<

Jan 21 07:24:13.393 2026  SELLB-ER4 chassisd[27847]: %DAEMON-5-CHASSISD_FRU_OFFLINE_NOTICE: Taking FPC 1 offline: Offlined by cli command

Jan 21 07:24:13.950 2026  SELLB-ER4 chassisd[27847]: %DAEMON-5-CHASSISD_SNMP_TRAP10: SNMP trap generated: FRU power off (jnxFruContentsIndex 7, jnxFruL1Index 2, jnxFruL2Index 0, jnxFruL3Index 0, jnxFruName FPC: MPC3E NG HQoS @ 1/*/*, jnxFruType 3, jnxFruSlot 1, jnxFruOfflineReason 7, jnxFruLastPowerOff 1781269837, jnxFruLastPowerOn 4896758)

Jan 21 07:27:36.316 2026  SELLB-ER4 craftd[27851]: %DAEMON-4: Major alarm cleared, FPC 1 Major Errors

Jan 21 07:27:36.342 2026  SELLB-ER4 craftd[27851]: %DAEMON-4: Minor alarm cleared, FPC 1 Minor Errors

Jan 21 07:27:51.998 2026  SELLB-ER4 chassisd[27847]: %DAEMON-5-CHASSISD_SNMP_TRAP7: SNMP trap generated: FRU insertion (jnxFruContentsIndex 20, jnxFruL1Index 2, jnxFruL2Index 1, jnxFruL3Index 0, jnxFruName MIC:  @ 1/0/*, jnxFruType 11, jnxFruSlot 1)

Jan 21 07:27:51.999 2026  SELLB-ER4 chassisd[27847]: %DAEMON-5-CHASSISD_SNMP_TRAP7: SNMP trap generated: FRU insertion (jnxFruContentsIndex 20, jnxFruL1Index 2, jnxFruL2Index 2, jnxFruL3Index 0, jnxFruName MIC:  @ 1/1/*, jnxFruType 11, jnxFruSlot 1)

Jan 21 07:28:07.333 2026  SELLB-ER4 chassisd[27847]: %DAEMON-5-CHASSISD_SNMP_TRAP10: SNMP trap generated: FRU power on (jnxFruContentsIndex 20, jnxFruL1Index 2, jnxFruL2Index 1, jnxFruL3Index 0, jnxFruName MIC: 10X10GE SFPP @ 1/0/*, jnxFruType 11, jnxFruSlot 1, jnxFruOfflineReason 2, jnxFruLastPowerOff 0, jnxFruLastPowerOn 4915017)

<<<

Singtel request for RMA as this issue impacted 73 customer and they cant afford for this issue to reoccurrence.

<Annabelle> This issue is normally a transient HW issue (due to environmental changes or due to weak or bad HW). I would recommend to monitor the FPC for now and proceed RMA only on re-occurance. Please kindly help to communicate with customer.  
 

==============

Hi Annabelle,

Does this issue matches KBKB72740? Singtel request for RMA as this issue impacted 73 customer and they cant afford for this issue to reoccurrence.

===================

Dear Alex,

This is Annabelle Wang from Japan CFTS Team and I will be assisting you with your case.

I will go through the case and provide you and update later.