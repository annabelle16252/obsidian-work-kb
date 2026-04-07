# SPC3

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

SPC3
https://www.juniper.net/documentation/us/en/software/junos/interfaces-next-gen-services/topics/concept/usf-overview.html
SPC3 training session 1&2
https://junipernetworks.sharepoint.com/sites/gs-knowledge-hub/TechathonRouting1/Forms/AllItems.aspx?FolderCTID=0x0120005F0522908BD4DC40B3D23283C1AB6F20&id=%2Fsites%2Fgs%2Dknowledge%2Dhub%2FTechathonRouting1%2FROUTING%2FMX%2DSPC3%20%28OLYMPUS%29
https://www.juniper.net/documentation/us/en/software/junos/interfaces-next-gen-services/topics/task/enabling-usf.html
jpfe-spc3* software package is required on next-generation routing engine for SPC3 card to come online.
SPC3 CGNAT checklist commands
http://juniper.lightning.force.com/lightning/r/Knowledge__kav/ka0Dp000000iYfUIAU/view
> request system enable unified-services
> show system unified-services status
Unified Services : Enabled
> show version detail no-forwarding
Hostname: GP-JINX-BNG01
Model: mx480
Junos: 23.2R2-S1.3
JUNOS Packet Forwarding Engine Support (MXSPC3) [23.2R2-S1.3]
<<<jpfe package is needed for SPC3 and it should match junos version
https://www.juniper.net/documentation/us/en/software/junos/interfaces-next-gen-services/topics/task/enabling-usf.html
/volume/build/junos/24.2/release/24.2R2-S2.5-images/jpfe-spc3-mx-x86-32-22.4R3-S2.11.tgz
> request system software add /var/tmp/jpfe-spc3-mx-x86-32-19.4R1.9.tgz reboot
show version |match spc3
[MX-SPC3] SPC3 card in Present state in a new chassis
https://juniper.lightning.force.com/lightning/r/Knowledge__kav/ka0Dp000000jizFIAQ/view
> start shell
% vty fpc7.pic0
security ttrace flow:
set security flow traceoptions file flow-trace size 10m file 5 world-readable
set security flow traceoptions flag all
set security flow traceoptions packet-filter forward-packet source-prefix 100.86.200.1/32
set security flow traceoptions packet-filter reverse-packet destination-prefix 100.86.200.1/32
This issue might be seen if the following conditions are met:* On MX Junos devices supporting MX-SPC3 cards (MX240, MX480 and MX960)* High CPU utilization due to bursty traffic and subsequent DMA RX queue handling failures on the SPC3 card
https://prsearch.juniper.net/PR1841859
KB79209 If enabled unified-services , MS-MPC can't be online and it is showing "---FPC in unsupported mode---".
Configuring Twice Dynamic NAT for Next Gen Services
https://www.juniper.net/documentation/us/en/software/junos/interfaces-next-gen-services/topics/task/usf-twice-dynamic-nat-configuring.html
# session dump#
show services sessions interface vms-11/0/0 | save /var/tmp/<filename.txt>
Session ID: 51565583549, Service-set: sset-2, Policy name: default-service-set-policy/32783, Timeout: 30, Session State: Valid
Member name: mams-11/0/0
In: 2407:1140:9:5257:4c1f:6b7:4fb6:c7d4/618 --> 2407:1140:1::a/128;icmp6, Conn Tag: 0x0, If: ams0.3, Pkts: 3302746581, Bytes: 343485644424,
Out: 2407:1140:1::a/128 --> 2407:1140:9:5257:4c1f:6b7:4fb6:c7d4/618;icmp6, Conn Tag: 0x0, If: ams0.4, Pkts: 0, Bytes: 0,
http://juniper.lightning.force.com/lightning/r/Knowledge__kav/ka0Dp000000jDsZIAU/view
