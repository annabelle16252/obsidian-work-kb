/volume/case_2026/2026-0122-049996

/volume/CSdata/annaw/case/2026-0122-049996

Hostname: MX-BD-05-07

Model: mx480

Junos: 23.2R2-S3.8

annaw@p-jtac-lnx02:/volume/CSdata/annaw/case/2026-0122-049996/var-re1/var/log$ cat inventory | grep "FPC 0 MIC 1"

Jan 18 01:09:20 FPC 0 MIC 1 - part number 750-028387, serial number CAJY08959<<< normal MIC

Jan 18 01:41:29 FPC 0 MIC 1 - part number 750-077332, serial number EBBT9107<<<MIC-Macsec

Jan 18 02:16:30 FPC 0 MIC 1 - part number 750-077332, serial number EBBT9116<<<MIC-Macsec

Jan 18 04:18:38 FPC 0 MIC 1 - part number 750-028392, serial number CALS7579<<< normal MIC

Jan 18 04:25:42 FPC 0 MIC 1 - part number 750-028392, serial number CALS7579<<< normal MIC

Jan 18 04:37:36 FPC 0 MIC 1 - part number 750-077332, serial number EBBT9116<<<MIC-Macsec

annaw@p-jtac-lnx02:/volume/CSdata/annaw/case/2026-0122-049996/var-re1/var/log$  cat inventory | grep "FPC 0 MIC 0"

Jan 18 01:09:18 FPC 0 MIC 0 - part number 750-028387, serial number CAJY0728

Jan 18 01:09:18 FPC 0 MIC 0 - part number 750-028387, serial number CAJY0728

Jan 18 01:41:18 FPC 0 MIC 0 - part number 750-033307, serial number EBBX2454

annaw@p-jtac-lnx02:.../log$ cat inventory | grep "FPC 0 - part number"

Jan 18 01:06:18 FPC 0 - part number 750-063741, serial number CAJV8757

Jan 18 01:06:23 FPC 0 - part number 750-063741, serial number CAJV8757

Jan 18 01:35:55 FPC 0 - part number 750-063180, serial number EBAD9225

MIC1-0/2 0/3

$ cat 20260118_mx-bd-05-07_remotesyslog.log | grep -v "UI_" | grep -v "LIBJSNMP" | grep "Jan 18 02:1"

=====

annaw@p-jtac-lnx02:.../2026-0122-049996$ cat 20260118_mx-bd-05-07_remotesyslog.log | grep -v "UI_" | grep -v "LIBJSNMP" | grep " 0/2/"

annaw@p-jtac-lnx02:.../2026-0122-049996$ cat 20260118_mx-bd-05-07_remotesyslog.log | grep "fpc0 Reset media mux for mic slot 1" | more

Jan 18 02:13:45 MX-BD-05-07 fpc0 Reset media mux for mic slot 1

Jan 18 02:13:51 MX-BD-05-07 fpc0 Reset media mux for mic slot 1

Jan 18 02:34:05 MX-BD-05-07 fpc0 Reset media mux for mic slot 1

Jan 18 03:04:45 MX-BD-05-07 fpc0 Reset media mux for mic slot 1

Jan 18 04:25:40 MX-BD-05-07 fpc0 Reset media mux for mic slot 1

Jan 18 04:48:46 MX-BD-05-07 fpc0 Reset media mux for mic slot 1

Jan 18 06:20:13 MX-BD-05-07 fpc0 Reset media mux for mic slot 1

Jan 18 02:13:45 MX-BD-05-07 fpc0 mpcs_i2c_single_io: MPCS(0) ctlr 0 group 1 addr 0x51 prio 0 flags 0x0 failed status 0x1

Jan 18 02:13:45 MX-BD-05-07 fpc0 I2C Failed device: group 0x41 address 0x51

Jan 18 02:13:45 MX-BD-05-07 fpc0 mpcs_i2c_single_io: MPCS(0) ctlr 0 group 1 addr 0x51 prio 0 flags 0x0 failed status 0x1

Jan 18 02:13:45 MX-BD-05-07 fpc0 I2C Failed device: group 0x41 address 0x51

Jan 18 02:13:45 MX-BD-05-07 fpc0 MIC_SFP_MUX_RESET: mic_sfp_pca9548_swtbl_clean: MIC(0/2) Reset MUX to SFP ports

Jan 18 02:13:45 MX-BD-05-07 xntpd[21178]: 1 7 0 32 32

Jan 18 02:13:45 MX-BD-05-07 fpc0 Reset media mux for mic slot 1

Jan 18 02:16:01 MX-BD-05-07 fpc0 PCI ERROR: 0:0:0:0 Timestamp 2306914 msec.

Jan 18 02:16:01 MX-BD-05-07 fpc0 PCI ERROR: 0:0:0:0 (0x0006)                         Status : 0x00004010

Jan 18 02:16:01 MX-BD-05-07 fpc0 PCI ERROR: 0:0:0:0 (0x001e)           Secondary bus status : 0x00004000

Jan 18 02:16:02 MX-BD-05-07 fpc0 PCI ERROR: 0:0:0:0 (0x005e)                    Link status : 0x00000041

Jan 18 02:16:02 MX-BD-05-07 fpc0 PCI ERROR: 0:0:0:0 (0x0130)              Root error status : 0x00000054

Jan 18 02:16:02 MX-BD-05-07 fpc0 PCI ERROR: 0:0:0:0 (0x0134)                Error source ID : 0x02180218

Jan 18 02:16:02 MX-BD-05-07 fpc0 PCI ERROR: 0:2:3:0 Timestamp 2306914 msec.

Jan 18 02:16:02 MX-BD-05-07 fpc0 PCI ERROR: 0:2:3:0 (0x0006)                         Status : 0x00004010

Jan 18 02:16:02 MX-BD-05-07 fpc0 PCI ERROR: 0:2:3:0 (0x0072)                  Device status : 0x00000004

Jan 18 02:16:02 MX-BD-05-07 fpc0 PCI ERROR: 0:2:3:0 (0x007a)                    Link status : 0x00000001

Jan 18 02:16:02 MX-BD-05-07 fpc0 PCI ERROR: 0:2:3:0 (0x0fb8)     Uncorrectable error status : 0x00000020

Jan 18 02:16:02 MX-BD-05-07 fpc0 PCI ERROR: 0:2:3:0 (0x0fcc)       Advanced error cap & ctl : 0x000001e5

Jan 18 02:16:03 MX-BD-05-07 fpc0 PCI ERROR: 0:2:3:0 (0x0fd0)                   Header log 0 : 0x05008001

Jan 18 02:16:03 MX-BD-05-07 last message repeated 4 times

Jan 18 02:16:03 MX-BD-05-07 fpc0 PCI ERROR: 0:2:3:0 (0x0fd4)                   Header log 1 : 0x0000000f

Jan 18 02:16:03 MX-BD-05-07 fpc0 PCI ERROR: 0:2:3:0 (0x0fd8)                   Header log 2 : 0x50ff0000

Jan 18 02:16:03 MX-BD-05-07 fpc0 PCI ERROR: 0:2:3:0 (0x0fdc)                   Header log 3 : 0x0307777b

=================

chassisd:

Jan 18 02:34:08 CHASSISD_PIC_HWERROR: PIC 2 in FPC 0 (PIC type 2802, version 275) had hardware error

Jan 18 02:34:08  send: red alarm set, device FPC 0 PIC 2, reason FPC 0 PIC 2 Failure

Jan 18 02:34:08 CHASSISD_SNMP_TRAP7: SNMP trap generated: Fru Failed (jnxFruContentsIndex 8, jnxFruL1Index 1, jnxFruL2Index 3, jnxFruL3Index 0, jnxFruName PIC: 1x 10GE SFPP / 10x 1GE SFP MACsec @ 0/2/*, jnxFruType 11, jnxFruSlot 0)

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

=======================

PFE LOG

[Jan 17 22:02:25.254 LOG: Info] mic_abnormal_state_handle: mic slot 1 is pre-cleanup due to HW malfunction

[Jan 17 22:20:28.403 LOG: Debug] mic_an_config_change ifd:ge-0/3/9 AN configs are not matching

==============

FPC 0            REV 37   750-063180   EBAD9225          MPC3E NG HQoS

  CPU            REV 14   711-045719   EBBY6266          RMPC PMB

  MIC 0          REV 20   750-033307   EBBX2454          10X10GE SFPP

    PIC 0                 BUILTIN      BUILTIN           10X10GE SFPP

      Xcvr 0     REV 01   740-140354   1W1CUKAA191EW     SFP+-10G-ER

      Xcvr 1     REV 01   740-140354   1W1CUKAA191EV     SFP+-10G-ER

      Xcvr 2     REV 01   740-140352   1G2CT8AA211GT     SFP+-10G-LR

      Xcvr 3     REV 01   740-140352   1G2CT8AA211GF     SFP+-10G-LR

      Xcvr 4     REV 01   740-140352   1G2CT8AA211GX     SFP+-10G-LR

      Xcvr 5     REV 01   740-140352   1G2CT8AA211J5     SFP+-10G-LR

      Xcvr 6     REV 01   740-140352   1G2CT8AA211KN     SFP+-10G-LR

      Xcvr 7     REV 01   740-140354   1W1CUKAA191EY     SFP+-10G-ER

      Xcvr 8     REV 01   740-140354   1W1CUKAA191EX     SFP+-10G-ER

      Xcvr 9     REV 01   740-140354   1W1CUKAA191EZ     SFP+-10G-ER

Routing Engine status:

  Slot 0:

    Start time                     2026-01-18 01:20:46 +08

Jan 18 08:18:41 new state = master

  Slot 1:

    Current state                  Backup

    Start time                     2026-01-18 00:26:13 +08