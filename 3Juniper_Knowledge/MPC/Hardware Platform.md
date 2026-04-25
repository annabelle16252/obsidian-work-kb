# Hardware Platform

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

Hardware Platform
# SFP #
QDD-400G-ZR-M-HP...: ...400G QSFP-DD
QSFP28-100GBASE-LR4... : ...4 lanes，4 × 25Gbps = 100Gbps
QSFP28-100G-LR1：不支持channelize
数据通道（lanes），每个通道可以单独传输数据。也就是有几个lanes，就可以channelize几个口出来。
KB77176... ...Connecting Juniper MX304 to a Nokia 1830 using QSFP28-100GBASE-LR4
As the optic on the Nokia 1830 has 4 lanes at 25g each and... ...the optics QSFP28-100G-LR1 at... MX304 ...is not supported 4 lanes... ...and port will remain down....
# Fabric #
juniper路由器跨FPC数据传输通过fabric：
小盒子路由器fabric的作用是通过midplane来实现的，如...ACX7K...。
legcy MX是通过CB来实现fabric功能。
MX10K系列的SFB也是独立的fabric...。
而PTX这种大型设备专门用于转发的有专门的fabric-SIB来实现。
