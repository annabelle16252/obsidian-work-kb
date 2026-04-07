# MPC10 - ZT Chipset

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

MPC10 - ZT Chipset
re0# run start shell
re0:~ # vty fpc1.0
# show jspec client
ID       Name
1       MPCS[0]
2       ZTCHIP[0]
3       ZTCHIP[1]
4       ZTCHIP[2]
MPC10-10C:两个ZT chips...，每个PFE有一个...ZT chip
ZT0--PIC0, ZT1--PIC1
MPC10e 10C Internal Architecture 
MX MPC10e 10C Linecard 
Linecard 
CPU 
+From / To RE- 
500Gbps 
PIC SLOT O 
5×QSFPP 
ZT 
From / To Fabric 
500Gbps! 
PIC SLOT 1 
5xQSFPP 
ZT 
From / To Fabric 
Support QSFP56-DD ...MPC10e 10C Internal Architecture 
MX MPC10e 10C Linecard 
Linecard 
CPU 
+From / To RE- 
500Gbps 
PIC SLOT O 
5×QSFPP 
ZT 
From / To Fabric 
500Gbps! 
PIC SLOT 1 
5xQSFPP 
ZT 
From / To Fabric 
Support QSFP56-DD
MPC10-15C:三个ZT chips...，每个PFE有一个...ZT chip
ZT0--PIC0, ZT1--PIC1...，...ZT2--PIC2
MPC10E-15C Architecture 
MPC10E-15C 
PIC-0 
ZT-0 (PFE-0) 
(5xQSFPP) 
500G 
HBM 
ZT-1 (PFE-1) 
PIC-1 
(5×QSFPP) 
500G 
Switching 
Fabric 
HBM 
PIC-2 
ZT-2 (PFE-2) 
(5×QSFPP) 
500G 
HBM ...MPC10E-15C Architecture 
MPC10E-15C 
PIC-0 
ZT-0 (PFE-0) 
(5xQSFPP) 
500G 
HBM 
ZT-1 (PFE-1) 
PIC-1 
(5×QSFPP) 
500G 
Switching 
Fabric 
HBM 
PIC-2 
ZT-2 (PFE-2) 
(5×QSFPP) 
500G 
HBM
No MIC???
FPC 5            REV 60   750-070395   EBAK0530          MPC10E 3D MRATE-15xQSFPP
CPU            REV 21   750-072571   EBAJ5205          FMPC PMB
PIC 0                   BUILTIN      BUILTIN           MRATE-5xQSFPP
Xcvr 0       REV 02   740-071175   1D2JQLA628046     QSFP-100GBASE-ER4L
Xcvr 1       REV 01   740-120240   1W1CQVAA0300H     UNKNOWN
PIC 1                   BUILTIN      BUILTIN           MRATE-5xQSFPP
Xcvr 0       REV 01   740-120240   1W1CQVAA0300K     UNKNOWN
Xcvr 1       REV 01   740-061409   1A1MQAA7220KF     QSFP-100GBASE-LR4-T2
Xcvr 2       REV 01   740-061409   1A1MQAA7220NJ     QSFP-100GBASE-LR4-T2
Xcvr 3       REV 01   740-058732   1G3TQAA7430PQ     QSFP-100GBASE-LR4
Xcvr 4       REV 01   740-058732   1A1MQAA84602B     QSFP-100GBASE-LR4
PIC 2                   BUILTIN      BUILTIN           MRATE-5xQSFPP
Xcvr 1       REV 01   740-058732   1A1MQAA84600T     QSFP-100GBASE-LR4
Xcvr 2       REV 01   740-058732   1A1MQAA84601Z     QSFP-100GBASE-LR4
Xcvr 3       REV 01   740-061409   1A1MQAA7220GU     QSFP-100GBASE-LR4-T2
