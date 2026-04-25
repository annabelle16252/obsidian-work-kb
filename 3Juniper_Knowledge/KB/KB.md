# KB

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

KB
KB72194... ...[QFX-EVO]EVPN-VxLAN Overlay NH programming pointing to ZERO
https://supportportal.juniper.net/s/article/EVPN-VxLAN-Overlay-NH-programming-pointing-to-ZERO?language=en_US
In some cases, there may be a traffic blackhole occur on the EVPN-VxLAN fabric. One possibility is that overlay next-hop value may somehow be incorrectly programmed as "0" (zero), which caused the traffic blackhole.
>>> To check overlay next-hop programming, we need to login to "PFE-CLI" as below.
root@QFX5130_Border-Leaf_1:pfe> show evo-pfemand nh detail index <NH index ID extracted from RE-CLI command "show route forwarding-table destination <DST IP>">
Overlay Egress Nh       : 0  <<<<<<<<<<<<<<<<<< this is wrong
https://prsearch.juniper.net/problemreport/PR1745711
KB80907... ...ACX/PTX EVO routers do not support MPLS families on IRB
Note that PR1763785 adds support for a commit error to avoid any confusion
KB80578... ...PTX running EVO sees EvoAftManBt-main go to 99.9% after interrupting show pfe route ip
KB98109... ...EVO PFEMAND core happened on backup RE in ACX7348
PR1783844
KB76703... ...ACX7100 displaying log message EVO_PFEMAND_ASIC_PROGRAM_FAILED repeatedly
no functional impacts
