# Linux-based-FPC

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

Linux-based-FPC
YT Chipset: ...TRIO 6 chipset
Trio 7 will support 16K ...MTU ...on all interfaces.
AFT based module：which includes MPC10, MPC11, LC9600 and MX304.
LC1202 based on our express silicon BT
MX304 is based on Trio silicon YT
KB37372 How to collect linux based FPC logs
the latest FPCs that can only run on vmhost based RE generally have its base OS, such as MPC7E , LC9600 and MPC10E on MX or FPC3 on PTX.  When those FPCs get an error, the message logs on its base OS might need further troubleshooting.
> start shell user root<enter root password># rsh -Ji fpc3root@fpc3:~#
# FPC Linux log #
> start shell user root<enter root password># rsh -Ji fpc3
> start shell user root
# ssh -Ji root@128.0.0.16 <<<to FPC0
Then follow below link:
https://supportportal.juniper.net/s/article/How-to-collect-linux-based-FPC-logs
# vPFE #
MPC 类型
ASIC 版本
vPFE 支持
典型机型
虚拟化能力
MPC11E
Trio 6.0
是
MX304
高（灵活分割逻辑实例）
MPC9E
Trio 5.0
部分
MX960
中（依赖物理 PFE 数量）
MPC7E
Trio 4.0
有限
MX480
低（需特定 Junos 版本）
MPC3
I-chip
否
MX240 (旧型号)
不支持
set chassis network-services enhanced-ip  # 启用增强 IP 模式（支持 vPFE）
Junos 版本要求：
Trio 6.0 需 Junos OS 20.4R1 或更高版本。
Trio 5.0 需 Junos OS 18.4R1 或更高版本。
