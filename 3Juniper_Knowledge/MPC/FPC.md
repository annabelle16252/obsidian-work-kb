# FPC

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

FPC
show syslog messages <<<fpc reboot后，这个有可能会没有了
show nvram <<< fpc reboot后，这个不会没有
> show chassis errors active detail fpc-slot X
learning... center - all fpc
https://juniper.csod.com/lms/scorm/clientLMS/LMS3.aspx?rNum=1&aicc_sid=2771102juniper&packageId=B0&qs=%5e%5e%5e2lx68LY2plRoW6MHz%2boLM1NcqRbTmUatkzvGYm2mecW4%2bJ2Ae1gdF3EcLBVq1VHUkjpVeD0ox6pZIvuXOBFf%2bSLwU%2fGdlIQvTAPKUef0NJbj%2bj0XHRMMrWtj0N0TYrsbQTHA6u%2bfzNRsVj2YAX4eipdAXLLO%2fH2dI2MEgwJj%2bQRMTtMnSWbfwNvui8b%2f%2baF3O9E%2bC6pwkUVsQq%2fgXBrP4LDXb%2fhozeiO7SB2CD1UVpU%3d
FRU Name
Code name
PFE
Details
No of PFE
mq
lu
ix
qx
MPC Type 1 3D
Neo
Trinity
1*Trinity(20Gbps)
1
1
1
no
MPCE Type 1 3D
1*Trinity(20Gbps)
1
1
1
no
MPC Type 1 3D Q
1*Trinity(20Gbps)
1
1
1
1
MPCE Type 1 3D Q
1*Trinity(20Gbps)
1
1
1
1
MPC Type 2 3D
2*Trinity(20gbps)
2
2
2
no
MPC E Type 2 3D
2*Trinity(20gbps)
2
2
2
no
MPC Type 2 3D Q
2*Trinity(20gbps)
2
2
2
2
MPC Type 2 3D EQ
2*Trinity(20gbps)
2
2
2
2
MPC E Type 2 3D Q
2*Trinity(20gbps)
2
2
2
2
MPC E Type 2 3D EQ
2*Trinity(20gbps)
2
2
2
2
MPC E Type 2 3D P
2*Trinity(20gbps)
2
2
2
2
MPC 3D 16*10GE
Agent Smith
4*Trinity(20Gbps)
4
4
4
no
no
No of PFE
xm
lu
MPCE Type 3 3D
Hyperion
Cassis
Cassis(100gbps) Stage-1
1
1
4
MPC4E 3D 32*GE
Snorkel
2*Casis(100gbps) Stage-1
2
2
4
MPC 4E 3D 2CGE +8*GE
2*Casis(100gbps) Stage-1
2
2
4
No of PFE
xm
xl
xq
MPC5E 3D Q 2CGE+3*GE
windsurf
Cassis2
2*Cassis2(200Gbps)
1
2
1
1
MPC5E 3D Q 24*GE + 6XLGE
2*Cassis2(200Gbps)
1
2
1
1
MPC6E 3D
scuba
4*Cassis2(200Gbps)
2
4
2
No of PFE
xl
xm
xq
MPC2E NG H-QoS
aloha
XM ASIC
NG-MPC have one PFE complex
1
1
1
1
MPC2E NG PQ & Flex Q
1
1
1
no
MPC3E NG H-QoS
1
1
1
1
MPC3E NG PQ & Flex Q
1
1
1
no
No of PFE
MQSS
LUSS
EA
MPC 7E-10G
Stout
eagle
2*Eagle(400Gbps)
2
2
2
2
MPC 7E-MRATE
2
2
2
2
MPC 8E
4*Eagle(400Gbps)
4
4
4
4
MPC 9E
4
4
4
4
# ULC #
每张 ULC 通过 EVO 系统注册，获取 AFT 下发的表项。
对用户和控制平面来说，ULC 是标准化的 forwarding resource，硬件差异被抽象掉。
ULC line card (Redbull-MPC10, AlfraRomeo-MPC11 and Bugatti)
# CMERROR #
pfe shell:
show cmerror module brief
show cmerror module ...detail
