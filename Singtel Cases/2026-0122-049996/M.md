




Dear Chee Loong,

R200613688 has been created as per your request. I will put the case under monitoring.


--------------


Dear Chee Loong,

As per logs, MIC1 is working fine in your lab.
Maybe because lab environment/setup is different than production, we cannot see the issue.

We an proceed RMA, please provide shipping information.

-----------------

Hi Annabelle,
 
I have staged the faulty MIC1 for a day and observed no issues with it.
 
It was able come online with no flapping.
 
There was only an initial I2C error log seen when MIC1 is inserted but no flooding after.
 
Mar 17 03:17:56  lab-mx960-3d-03-re1 chassisd[16773]: CHASSISD_SNMP_TRAP7: SNMP trap generated: FRU insertion (jnxFruContentsIndex 8, jnxFruL1Index 4, jnxFruL2Index 3, jnxFruL3Index 0, jnxFruName PIC:  @ 3/2/*, jnxFruType 11, jnxFruSlot 3)
Mar 17 03:18:59  lab-mx960-3d-03-re1 fpc3 mpcs_i2c_single_io: MPCS(0) ctlr 0 group 1 addr 0x53 prio 0 flags 0x0 failed status 0x6
Mar 17 03:18:59  lab-mx960-3d-03-re1 fpc3 I2C Failed device: group 0x41 address 0x53
Mar 17 03:19:03  lab-mx960-3d-03-re1 fpc3 MIC_SFP_MUX_RESET: mic_sfp_pca9548_swtbl_clean: MIC(3/2) Reset MUX to SFP ports
Mar 17 03:19:03  lab-mx960-3d-03-re1 fpc3 Reset media mux for mic slot 1
 
I have uploaded Nera’s staging logs, RSI and varlogs. MIC1 (EBBT9116) is staged in FPC3.
 
Thank you.
 

---------------------

Dear Chee Loong,

May I know if you need me to handoff the case tonight for RMA creation?


-------------
Hi Naveen,
 
1./ We have replaced FPC0's MIC1 successfully.

It was able to come online. All Starview SFPs were detected. Monitored for 30mins and no flaps and no error logs.


2./ I am currently staging the faulty MIC1.

 

3./ We are in the midst of tying the faulty MIC1 to the chassis first before creating the RMA.



----------


To:
CFTS-Bangalore-Telco@juniper.net;CFTS-Bangalore-Routing@juniper.net

CC:
balamurugan.sekar@hpe.com;jaeho.seo@hpe.com;Japan-CFTS@juniper.net;support-private@juniper.net

[Handoff to BNG] Singtel | MX960 | 2026-0122-049996  - SINGTEL -- MEGAPOP - NERA - MX-BD-05-07 - [Singtel-Megapop:MX-BD-05-07] FPC0's MIC1 [MIC-MACSEC-20GE] unable to online from Ticket 18522 | Ticket #18756

Hello CFTS Bangalore team,

Please accept this cold standby case for Singtel MIC/SFP replacement activity.

REASON FOR HANDOVER:
MW support for MIC1-FPC0 cannot online issue. 
Please handover back to Japan CFTS  for the babysitting.

ISSUE DESCRIPTION:
15 March 2026 1am-5am SGT. ( 14 March 2026 10am-14pm PDT)
Action to be performed:
A. Replace FPC0’s MIC1 [MIC-MACSEC-20GE]
B. If issue persists, replace FPC0 linecard [MPC3E-3D-NG-Q] and use back old MIC1
C. If issue persists, replace FPC0’s MIC1 as well
D. Migrate FPC2 links back to FPC0 MIC1 [MIC-MACSEC-20GE] and FPC1 MIC1 [MIC-MACSEC-20GE]

Please join below ZOOM meeting for support:
zoom03.psm.sg is inviting you to a scheduled Zoom meeting.
Topic: Ticket #18756 - [Singtel:Singtel-Megapop:MX-BD-05-07] FPC0's MIC1 [MIC-MACSEC-20GE] unable to online from Ticket 18522
Time: Mar 15, 2026 01:00 AM Singapore
Join Zoom Meeting
https://nera.zoom.us/j/84373065368?pwd=XQcIYYjhpnVAx176GwMJSMWZ6tEGij.1
Meeting ID: 843 7306 5368
Passcode: 504108


ACTION PERFORMED:
MOP has been verified by Japan CFTS.

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



 

 
 






----------------------


Dear Chee Loong,

Thanks for your update and I will handoff the case accordingly.


---------------

Hi Annabelle,
 
1./ The MW is confirmed on 15 March 2026 1am-5am SGT.
Please remember to handover this ticket and get the assigned engineer to join the zoom call during the MW for active support.
 
zoom03.psm.sg is inviting you to a scheduled Zoom meeting.
Topic: Ticket #18756 - [Singtel:Singtel-Megapop:MX-BD-05-07] FPC0's MIC1 [MIC-MACSEC-20GE] unable to online from Ticket 18522
Time: Mar 15, 2026 01:00 AM Singapore
Join Zoom Meeting
https://nera.zoom.us/j/84373065368?pwd=XQcIYYjhpnVAx176GwMJSMWZ6tEGij.1
Meeting ID: 843 7306 5368
Passcode: 504108
 
 
2./ We will be performing the below action plan.
A. Replace FPC0’s MIC1 [MIC-MACSEC-20GE]
B. If issue persists, replace FPC0 linecard [MPC3E-3D-NG-Q] and use back old MIC1
C. If issue persists, replace FPC0’s MIC1 as well
D. Migrate FPC2 links back to FPC0 MIC1 [MIC-MACSEC-20GE] and FPC1 MIC1 [MIC-MACSEC-20GE]


-------------------
Dear Chee Loong,

Thanks for your update and I will keep the case under monitoring.


--------------------
Hi Annabelle,
 
We are currently testing a new set of Starview SFPs that will be used for the replacement MW.
 
Once the testing is completed, Singtel will revert with the MW date.



--------------


Dear Tong,

Yes, let's proceed below and please test the part in your lab:

Action plan

1. Replace FPC0’s MIC1 [MIC-MACSEC-20GE]
2. If issue persists, replace FPC0 linecard [MPC3E-3D-NG-Q] and use back old MIC1 
3. If issue persists, replace FPC0’s MIC1 as well

======================

Hi Annabelle,

1./ I have checked with the customer’s onsite engineer who was doing the replacements and she confirmed that only the affected MIC (S/N: EBBT9116) was inserted into FPC0 MIC1 slot. 

But the inventory log says otherwise.

She have already taken the MIC out.

2./ Customer does not have the session log saved.

3./ I am suspecting that the MIC1 to be faulty as EBBT9107 was able to boot up properly with no errors. 

EBBT9107 was able to boot up:

Inventory Log: Jan 18 01:41:29 FPC 0 MIC 1 - part number 750-077332, serial number EBBT9107

Chassisd Log: Jan 18 01:41:29  mic_set_online: fpc 0 mic 1 (state Ready) pic 2 (state Online)

EBBT9116’s PIC2 had hardware error and alarm:

Inventory Log: Jan 18 02:34:08 FPC 0 PIC 2 - part number 750-077332, serial number EBBT9116

Chassisd Log:

Jan 18 02:34:08 CHASSISD_PIC_HWERROR: PIC 2 in FPC 0 (PIC type 2802, version 275) had hardware error

Jan 18 02:34:08  send: red alarm set, device FPC 0 PIC 2, reason FPC 0 PIC 2 Failure

Jan 18 02:34:08 CHASSISD_SNMP_TRAP7: SNMP trap generated: Fru Failed (jnxFruContentsIndex 8, jnxFruL1Index 1, jnxFruL2Index 3, jnxFruL3Index 0, jnxFruName PIC: 1x 10GE SFPP / 10x 1GE SFP MACsec @ 0/2/*, jnxFruType 11, jnxFruSlot 0)

Jan 18 02:34:08  mic_set_online: fpc 0 mic 1 (state Ready) pic 2 (state Present)

Jan 18 02:34:08  mic_get_mic_max_power, Could not retrive MIC power from itable for MIC type [af2]..returning power [60] considering worst case power by any MIC

4./ I suggest the below action plan first, collect back the faulty part(s) and then I will stage it in Nera lab. 

Once replicated, I will send you the logs for analysis. Please let me know if you agree with this plan.

Action plan

1. Replace FPC0’s MIC1 [MIC-MACSEC-20GE]
2. If issue persists, replace FPC0 linecard [MPC3E-3D-NG-Q] and use back old MIC1 
3. If issue persists, replace FPC0’s MIC1 as well

==========================

Dear Tong,

Sure, will use this email thread moving forwading.

Since you didnt provide the exact time of issue occurance, I just guess it's after RE1 replacement which was around '2026-01-18 00:26' and I believed at that time you found FPC0 MIC1 issue. Finally RE master switchover happened on 'Jan 18 08:18:41'. So we can focus on VARLOG of RE1 for investigation.

<<<

Routing Engine status:

  Slot 0:

    Start time                     2026-01-18 01:20:46 +08

Jan 18 08:18:41 new state = master

  Slot 1:

    Current state                  Backup

    Start time                     2026-01-18 00:26:13 +08

<<<

2./ I have confirmed with Singtel and they did not try other MIC cards in FPC0 MIC1 slot for troubleshooting.They just inserted the affected MIC-MACSEC-20GE into FPC0 MIC1 slot.

<Annabelle> I am confused by what customer told, because from 'chassisd & inventory log' of VARLOG-RE1, FPC0-MIC1 had been replaced by several MICs(SN are apparently different):

<<<<

annaw@p-jtac-lnx02:/volume/CSdata/annaw/case/2026-0122-049996/var-re1/var/log$ cat inventory | grep "FPC 0 MIC 1"

Jan 18 01:09:20 FPC 0 MIC 1 - part number 750-028387, serial number CAJY08959<<< normal MIC

Jan 18 01:41:29 FPC 0 MIC 1 - part number 750-077332, serial number EBBT9107<<<MIC-Macsec

Jan 18 02:16:30 FPC 0 MIC 1 - part number 750-077332, serial number EBBT9116<<<MIC-Macsec

Jan 18 04:25:42 FPC 0 MIC 1 - part number 750-028392, serial number CALS7579<<< normal MIC

Jan 18 04:37:36 FPC 0 MIC 1 - part number 750-077332, serial number EBBT9116<<<MIC-Macsec

<<<<

So the sequence of replacements in FPC0-MIC1 is as below:

1.Normal MIC CAJY08959

2.MIC-Macsec EBBT9107

3.MIC-Macsec EBBT9116

4.Normal MIC CALS7579

5.Inserted the same MIC-Macsec in #3 EBBT9116

Could you help to provide session log for our analysis? Also please ask if customer can help to explain why they replaced the MICs for each time above? When did they noticed the issue and it was with which MIC SN or all MIC SNs?

Take below for example:

<<<

Jan 18 02:16:00 CHASSISD_SNMP_TRAP7: SNMP trap generated: FRU removal (jnxFruContentsIndex 20, jnxFruL1Index 1, jnxFruL2Index 2, jnxFruL3Index 0, jnxFruName MIC: 2x 10GE SFPP / 20x 1GE SFP MACsec @ 0/1/*, jnxFruType 11, jnxFruSlot 0)

Jan 18 02:16:00 CHASSISD_SNMP_TRAP7: SNMP trap generated: FRU removal (jnxFruContentsIndex 8, jnxFruL1Index 1, jnxFruL2Index 4, jnxFruL3Index 0, jnxFruName PIC: 1x 10GE SFPP / 10x 1GE SFP MACsec @ 0/3/*, jnxFruType 11, jnxFruSlot 0)

Jan 18 02:16:30 CHASSISD_SNMP_TRAP7: SNMP trap generated: FRU insertion (jnxFruContentsIndex 8, jnxFruL1Index 1, jnxFruL2Index 3, jnxFruL3Index 0, jnxFruName PIC:  @ 0/2/*, jnxFruType 11, jnxFruSlot 0)

Jan 18 02:16:01 CHASSISD_SNMP_TRAP10: SNMP trap generated: FRU power off (jnxFruContentsIndex 8, jnxFruL1Index 1, jnxFruL2Index 4, jnxFruL3Index 0, jnxFruName PIC:  @ 0/3/*, jnxFruType 11, jnxFruSlot 0, jnxFruOfflineReason 14, jnxFruLastPowerOff 417148, jnxFruLastPowerOn 207924)

Jan 18 01:41:29 FPC 0 MIC 1 - part number 750-077332, serial number EBBT9107

Jan 18 02:16:30 FPC 0 MIC 1 - part number 750-077332, serial number EBBT9116<<<SN changed

<<<

============================

Hi Annabelle,

1./ Please reply this email thread moving forward.

I have pasted your reply below. Please also see my replies to your queries below.

2./ I have confirmed with Singtel and they did not try other MIC cards in FPC0 MIC1 slot for troubleshooting.

They just inserted the affected MIC-MACSEC-20GE into FPC0 MIC1 slot.

3./ Please provide the KB for this issue.

4./ If the issue is really with FPC0 linecard then please confirm if the below plan of action is good.

- Replace FPC0 linecard MPC3E-3D-NG-Q first and reinsert back MIC0 and affect MIC1 card

- If issue persists, replace MIC1 [MIC-MACSEC-20GE] as well.

============================

HI Annabelle,

Just in case you missed , IXCHIP and PRE-CL are in this specific MIC not on the FPC 

Regards

Prabin

============================

Dear Tong,

Let me add some more analysis here which we suspect it's FPC0 hardware issue. There were about 6 times MIC 1 down which triggerd by system. The reason for system to bring down MIC1 were due to 'PCI error' , 'HSL2 error' or even lead to PIC HW error. Please take below log for reference:

<<<<

Jan 18 02:13:45 MX-BD-05-07 fpc0 mpcs_i2c_single_io: MPCS(0) ctlr 0 group 1 addr 0x51 prio 0 flags 0x0 failed status 0x1

Jan 18 02:13:45 MX-BD-05-07 fpc0 I2C Failed device: group 0x41 address 0x51

Jan 18 02:13:45 MX-BD-05-07 fpc0 mpcs_i2c_single_io: MPCS(0) ctlr 0 group 1 addr 0x51 prio 0 flags 0x0 failed status 0x1

Jan 18 02:13:45 MX-BD-05-07 fpc0 I2C Failed device: group 0x41 address 0x51

Jan 18 02:13:45 MX-BD-05-07 fpc0 MIC_SFP_MUX_RESET: mic_sfp_pca9548_swtbl_clean: MIC(0/2) Reset MUX to SFP ports

Jan 18 02:13:45 MX-BD-05-07 xntpd[21178]: 1 7 0 32 32

Jan 18 02:13:45 MX-BD-05-07 fpc0 Reset media mux for mic slot 1

<snip..>

Jan 18 02:34:04 MX-BD-05-07 fpc0 MIC_SFP_MUX_RESET: mic_sfp_pca9548_swtbl_clean: MIC(0/2) Reset MUX to SFP ports

Jan 18 02:34:05 MX-BD-05-07 fpc0 Reset media mux for mic slot 1

Jan 18 02:34:05 MX-BD-05-07 fpc0 ix_chip_pre_init: SYSIF PLL lock OK!

Jan 18 02:34:05 MX-BD-05-07 fpc0 setup_initial_aperture: primary aperture enable_ctr_size value: write = 0x4003, read = 0x0

Jan 18 02:34:05 MX-BD-05-07 fpc0 setup_initial_aperture: primary aperture output_base value: write = 0x0, read = 0x2003

Jan 18 02:34:05 MX-BD-05-07 fpc0 bringup_pcie_common: PCIe status 0x0000c444

Jan 18 02:34:05 MX-BD-05-07 fpc0 bringup_pcie_common: PCIe link training took 0ms

Jan 18 02:34:05 MX-BD-05-07 fpc0 mic_ix_config(): IXCHIP2 pcie base 0xea000000, size 0x2000000, intx 4

Jan 18 02:34:06 MX-BD-05-07 fpc0 setup_initial_aperture: primary aperture enable_ctr_size value: write = 0x4001, read = 0x0

Jan 18 02:34:06 MX-BD-05-07 fpc0 setup_initial_aperture: primary aperture output_base value: write = 0x0, read = 0x2001

Jan 18 02:34:06 MX-BD-05-07 fpc0 IXCHIP(2): jtag id 0x10031161 major rev 0x1 minor rev 0x0

Jan 18 02:34:06 MX-BD-05-07 fpc0 Initializing IX[2] JSPEC

Jan 18 02:34:06 MX-BD-05-07 fpc0 ix_init_deassert_reset(): ixchip 2 internal reset block 0x33fffc...

Jan 18 02:34:06 MX-BD-05-07 fpc0 ix_init_deassert_reset(): ixchip 2 internal reset done

Jan 18 02:34:06 MX-BD-05-07 fpc0 cmerror register module re-use: index: 28 name PRECL:0:IX:2  new name PRECL:0:IX:2  

Jan 18 02:34:06 MX-BD-05-07 fpc0 precl_cmalarm_info_create: idx 2 for fpc 0 pic 1 aisc-id 22 asic-inst 2 pg 0 from config idx 0 error 0x340001 level 1 threshold 1 PRECL_CMERROR_INSTMEM_PARITY_FAIL

Jan 18 02:34:06 MX-BD-05-07 fpc0 precl_cmalarm_info_create: idx 2 for fpc 0 pic 1 aisc-id 22 asic-inst 2 pg 0 from config idx 1 error 0x340003 level 1 threshold 1 PRECL_CMERROR_TCAM_COMMAND_FAIL

Jan 18 02:34:06 MX-BD-05-07 fpc0 precl_cmalarm_info_create: idx 2 for fpc 0 pic 1 aisc-id 22 asic-inst 2 pg 0 from config idx 2 error 0x340004 level 1 threshold 1 PRECL_CMERROR_TCAM_PARITY_FAIL

Jan 18 02:34:07 MX-BD-05-07 fpc0 precl_cmalarm_info_create: idx 2 for fpc 0 pic 1 aisc-id 22 asic-inst 2 pg 0 from config idx 3 error 0x340005 level 1 threshold 1 PRECL_CMERROR_COOKIE_FIFO_OVERFLOW

Jan 18 02:34:07 MX-BD-05-07 fpc0 precl_cmalarm_info_create: idx 2 for fpc 0 pic 1 aisc-id 22 asic-inst 2 pg 0 from config idx 4 error 0x340006 level 1 threshold 1 PRECL_CMERROR_INQ_BUF_OVERFLOW

Jan 18 02:34:07 MX-BD-05-07 fpc0 Allocated memory for 6 channels IXCHIP(2)

Jan 18 02:34:07 MX-BD-05-07 fpc0 chan_count 6 num_chan 6  

Jan 18 02:34:07 MX-BD-05-07 fpc0 cmerror register module re-use: index: 29 name IXCHIP(2) new name IXCHIP(2)

Jan 18 02:34:08 MX-BD-05-07 chassisd[16772]: CHASSISD_PIC_HWERROR: PIC 2 in FPC 0 (PIC type 2802, version 275) had hardware error

Jan 18 02:34:08 MX-BD-05-07 alarmd[21222]: Alarm set: PIC id=18874487, color=RED, class=CHASSIS, reason=FPC 0 PIC 2 Failure

Jan 18 02:34:08 MX-BD-05-07 craftd[16776]:  Major alarm set, FPC 0 PIC 2 Failure

Jan 18 02:34:08 MX-BD-05-07 chassisd[16772]: CHASSISD_SNMP_TRAP7: SNMP trap generated: Fru Failed (jnxFruContentsIndex 8, jnxFruL1Index 1, jnxFruL2Index 3, jnxFruL3Index 0, jnxFruName PIC: 1x 10GE SFPP / 10x 1GE SFP MACsec @ 0/2/*, jnxFruTyp

e 11, jnxFruSlot 0)

<snip..>

<<<<

Since in MW, we have changed different MICs to try but all with same issue, we suspect that's the contact point inside FPC0 got issue. We can replace FPC0 in this case.

=====================================

Dear Tong,

Logs from syslog server is quite huge and could you help to understand on FPC0-MIC1 slot, if both  'normal MIC' & 'Mic-Macsec' had 'MIC flap' issue or only with 'Mic-Macsec' ?

<<<<

annaw@p-jtac-lnx02:/volume/CSdata/annaw/case/2026-0122-049996/var-re1/var/log$ cat inventory | grep "FPC 0 MIC 1"

Jan 18 01:09:20 FPC 0 MIC 1 - part number 750-028387, serial number CAJY0895<<< normal MIC

Jan 18 01:09:20 FPC 0 MIC 1 - part number 750-028387, serial number CAJY08959<<< normal MIC

Jan 18 01:41:29 FPC 0 MIC 1 - part number 750-077332, serial number EBBT9107<<<MIC-Macsec

Jan 18 02:16:30 FPC 0 MIC 1 - part number 750-077332, serial number EBBT9116<<<MIC-Macsec

Jan 18 04:18:38 FPC 0 MIC 1 - part number 750-028392, serial number CALS75799<<< normal MIC

Jan 18 04:25:42 FPC 0 MIC 1 - part number 750-028392, serial number CALS75799<<< normal MIC

Jan 18 04:37:36 FPC 0 MIC 1 - part number 750-077332, serial number EBBT9116<<<MIC-Macsec

<<<<

I am asking this is because I saw below from syslog that 'MIC 1' was reset by fpc0 :

<<<

annaw@p-jtac-lnx02:.../2026-0122-049996$ cat 20260118_mx-bd-05-07_remotesyslog.log | grep "fpc0 Reset media mux for mic slot 1" | more

Jan 18 02:13:45 MX-BD-05-07 fpc0 Reset media mux for mic slot 1

Jan 18 02:13:51 MX-BD-05-07 fpc0 Reset media mux for mic slot 1

Jan 18 02:34:05 MX-BD-05-07 fpc0 Reset media mux for mic slot 1

Jan 18 03:04:45 MX-BD-05-07 fpc0 Reset media mux for mic slot 1  <<< normal MIC

Jan 18 04:25:40 MX-BD-05-07 fpc0 Reset media mux for mic slot 1  <<< normal MIC

Jan 18 04:48:46 MX-BD-05-07 fpc0 Reset media mux for mic slot 1

Jan 18 06:20:13 MX-BD-05-07 fpc0 Reset media mux for mic slot 1

<<<

Take one example, the reset reason seems to be related to I2C bus reading the SFPs. May I know if the SFPs are starview or other non-Juniper SFPs?

<<<<

Jan 18 02:13:45 MX-BD-05-07 fpc0 mpcs_i2c_single_io: MPCS(0) ctlr 0 group 1 addr 0x51 prio 0 flags 0x0 failed status 0x1

Jan 18 02:13:45 MX-BD-05-07 fpc0 I2C Failed device: group 0x41 address 0x51

Jan 18 02:13:45 MX-BD-05-07 fpc0 mpcs_i2c_single_io: MPCS(0) ctlr 0 group 1 addr 0x51 prio 0 flags 0x0 failed status 0x1

Jan 18 02:13:45 MX-BD-05-07 fpc0 I2C Failed device: group 0x41 address 0x51

Jan 18 02:13:45 MX-BD-05-07 fpc0 MIC_SFP_MUX_RESET: mic_sfp_pca9548_swtbl_clean: MIC(0/2) Reset MUX to SFP ports

Jan 18 02:13:45 MX-BD-05-07 xntpd[21178]: 1 7 0 32 32

Jan 18 02:13:45 MX-BD-05-07 fpc0 Reset media mux for mic slot 1

<<<<

May I know if the MIC-Macsec is working fine in other FPC slots? From RSI, I see all other MICs are the normal MICs.

Also I didnt see below command has been executed on the device:

<<<

annaw@p-jtac-lnx02:.../2026-0122-049996$ cat 20260118_mx-bd-05-07_remotesyslog.log | grep "request chassis mic fpc-slot 1 mic-slot 1 offline"

<<<

Please upload session log once available.

=========================

Dear Tong,

This is Annabelle Wang from Japan CFTS Team and I will be assisting you with your case.

I will go through the case and provide you and update later.