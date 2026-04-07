# Lab -SPC3

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

Lab -SPC3
jtac-mx5-t-r2007 xe-1/0/0  ------- xe-0/1/0... ... jtac-mx480-r2033 xe-0/0/0-------xe-1/0/0 jtac-mx5-t-r2008
jtac-mx480-r2033(xe-0/2/0)-----------------(11/1-10G)jtac-spirent-spt-n11u-part2-r2001
10.219.45.203
SPC3 CGNAT checklist commands
https://juniper.lightning.force.com/lightning/r/Knowledge__kav/ka0Dp000000iYfUIAU/view
set services nat traceoptions file natlogs
set services nat traceoptions file size 1g
set services nat traceoptions flag all
set services adaptive-services-pics traceoptions file ssetlogs
set services adaptive-services-pics traceoptions flag all
set services traceoptions file nsdlogs
set services traceoptions file size 1g
set services traceoptions file files 5
set services traceoptions flag all
MX-SPC3 sample dynamic-NAT44 Configuration with next-hop style
https://juniper.lightning.force.com/lightning/r/Knowledge__kav/ka0Dp000000iTJbIAM/view
service-set
nat source pool
nat source rule-set
interfaces ams0
routing-instances
route 216.172.80.0/24 next-hop 192.168.1.2;
spirent tester app，tester接口（地址142.1.1.1）与路由器直连接口（地址142.1.1.2），可以ping通，从tester口想发10个stream到路由器上，地址是从8.8.8.1开始，如何在tester上配置
