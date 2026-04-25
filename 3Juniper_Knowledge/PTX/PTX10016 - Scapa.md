# PTX10016 - Scapa

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

PTX10016 - Scapa
https://www.juniper.net/documentation/us/en/hardware/ptx10016/topics/topic-map/ptx10016-system-overview.html
21U...，...16 FPC slots...，...6 SIBs...，硬件架构与...PTX10008相似
# RCB #
JNP10K-RE0
JNP10K-RE1
JNP10K-RE1-LT
JNP10K-RE1-128G
PTX10016 routers support the following Routing Engines:
#... SIB #
The PTX10016 comes with two models of SIBs:
The JNP10016-SF SIB has a switching capacity of 14.4 Tbps in one direction and supports Junos OS. 5+1 redundency
The JNP10016-SF3 SIB has a switching capacity of 45.9 Tbps in one direction and supports:
Junos OS Evolved Release 21.2R2 and later 21.2 releases
Junos OS Evolved Release 21.4R1 and later
SIBs are hot-removable and hot-insertable field-replaceable units (FRUs). They are not visible from the outside of the router chassis. You must remove one of the fan trays in order to view the SIBs.
JNP10016-SF
PTX10K-LC1101... ...PTX10K-LC1102... ...PTX10K-LC1104... ...PTX10K-LC1105... ...QFX10000-60S-6Q
JNP10016-SF3
PTX10K-LC1201-36CD... ...PTX10K-LC1202-36MR...
# FPC #
PTX10016 chassis supports up to 16 line cards.... FPCs type same as PTX10008.
PTX10K-LC1101
30 ports with default 100G.To change from the default mode (100GbE) to 40GbE channelized mode...:
# ...set chassis fpc slot slot-number pic 0 port port number channelization-speed 10g.
