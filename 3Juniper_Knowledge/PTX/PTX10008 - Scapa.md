# PTX10008 - Scapa

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

PTX10008 - Scapa
https://www.juniper.net/documentation/us/en/hardware/ptx10008/topics/topic-map/ptx10008-system-overview.html
# Commands #
FPC AFT login:
start shell pfe network fpc0
FPC linux login:
start shell user rootchvrf iri ssh fpc0
400G...，...13U...，...8 fpc slots...，...6 SIBs
与...MX不同...，...PTX...接口直接集成在fpc上，而非使用独立的FPC+PIC架构
所以...PIC的位置永远是0
REAR 
1 
2 
FRONT 
FAN OS FANT 
FAN O Y 
FANLY 
9808-8-8-8-8-8-8-8-8- 
lock 
/ 080808080\ 
RE+CB 
PE 
A07 980808080 
4 
4 
6 
POWER 
SIE 
88888888888888888 
FPC 
5 
g050552 
4 
g050555 
2 ...REAR 
1 
2 
FRONT 
FAN OS FANT 
FAN O Y 
FANLY 
9808-8-8-8-8-8-8-8-8- 
lock 
/ 080808080\ 
RE+CB 
PE 
A07 980808080 
4 
4 
6 
POWER 
SIE 
88888888888888888 
FPC 
5 
g050552 
4 
g050555 
2
RCB SIB FPC PEM FAN
# RCB #
PTX10008 routers support the following Routing Engines for systems running standard Junos 
OS: 
JNP10K-REO 
· JNP10K-RE1 
· JNP10K-RE1-LT 
· JNP10K-RE1-128G 
PTX10008 routers that have JNP10008-SF3 installed support the following Routing Engines: 
· JNP10K-RE1-E (64 GB, runs Junos OS Evolved Release 19.4R1-S1 and later, spare) 
· JNP10K-RE1-ELT (64 GB, runs limited Junos OS Evolved Release 20.3R1 and later, spare) 
· JNP10K-RE1-E128 (128 GB, runs Junos OS Evolved Release 19.4R1-S1 and later, spare) 
· JNP10K-RE2-E128 (128 GB, runs Junos OS Evolved Release 22.4R1 and later, spare) ...PTX10008 routers support the following Routing Engines for systems running standard Junos 
OS: 
JNP10K-REO 
· JNP10K-RE1 
· JNP10K-RE1-LT 
· JNP10K-RE1-128G 
PTX10008 routers that have JNP10008-SF3 installed support the following Routing Engines: 
· JNP10K-RE1-E (64 GB, runs Junos OS Evolved Release 19.4R1-S1 and later, spare) 
· JNP10K-RE1-ELT (64 GB, runs limited Junos OS Evolved Release 20.3R1 and later, spare) 
· JNP10K-RE1-E128 (128 GB, runs Junos OS Evolved Release 19.4R1-S1 and later, spare) 
· JNP10K-RE2-E128 (128 GB, runs Junos OS Evolved Release 22.4R1 and later, spare)
# SIB #
Switch Fabric 一共6块 SIBs...：
JNP10008-SF... ...SIBs... >>> JUNOS
JNP10008-SF3.../...SF...5 ...SIBs... >>> EVO
must use the same SIB model type in a running chassis
hot-removable and hot-insertable... FRUs
每种SIB可以支持的FPC不一样
# FPC #
https://www.juniper.net/documentation/us/en/hardware/ptx10008/topics/topic-map/ptx10008-line-card-components.html
P...TX不同于以往的路由器...，...将PFE和接口集中在一个FPC上...，
JNP10008-SF：
PTX10K-LC1101... ...PTX10K-LC1102... ...PTX10K-LC1104... ...PTX10K-LC1105... ...QFX10000-60S-6Q
JNP10008-SF3：PTX10K-LC1201-36CD... ...PTX10K-LC1202-36MR
JNP10008-SF5：PTX10K-LC1301-36DD
QSFP28 ... ...QSFP+
PTX10K-LC1101
https://www.juniper.net/documentation/us/en/hardware/ptx10008/topics/topic-map/ptx10008-line-card-components.html
When ports are in channelization mode, the fourth port on each Packet Forwarding Engine is disabled....
PTX10K-LC1102
36 ports, 12 ports also support 100GbE QSFP28 transceivers.
JNP10K-LC1102 
34 
4 8888 
4 888B 
4 8888 
14 8888 9 
PWR O 
35 
STS O 
100G 
OFF C ...JNP10K-LC1102 
34 
4 8888 
4 888B 
4 8888 
14 8888 9 
PWR O 
35 
STS O 
100G 
OFF C
When a QSFP28 transceiver is inserted into such a port and you configure the port for 100GbE, the two adjacent ports are disabled and the QSFP28 port is enabled for 100GbE.
PTX10K-LC1104
et-x/0/0 
et-x/0/1 
ot-x/0/0 
PE O 
et-x/0/2 
ot-x/0/1 
et-x/0/3 
et-x/0/4 
ot-x/0/2 
et-x/0/5 
built-in flexible 
PE 1 
rate optical 
et-x/0/6 
transponders 
Pot-x/0/3 
et-x/0/7 
et-x/0/8 
ot-x/0/4 
et-x/0/9 
PE 2 
et-x/0/10 
ot-x/0/5 
et-x/0/11 
Microsoft ...et-x/0/0 
et-x/0/1 
ot-x/0/0 
PE O 
et-x/0/2 
ot-x/0/1 
et-x/0/3 
et-x/0/4 
ot-x/0/2 
et-x/0/5 
built-in flexible 
PE 1 
rate optical 
et-x/0/6 
transponders 
Pot-x/0/3 
et-x/0/7 
et-x/0/8 
ot-x/0/4 
et-x/0/9 
PE 2 
et-x/0/10 
ot-x/0/5 
et-x/0/11 
Microsoft
PTX10K-LC1201-36CD
The line card houses five Juniper Networks' custom ASICs, and each ASIC houses two Packet Forwarding Engines. One packet forwarding engine in the fifth ASIC is not used.
QSFP56-DD
user@host> set chassis fpc slot slot-number pic 0 pic-mode speed 400g|200g|100g|50g|40g|25g|10g
There is a single PIC in the PTX10K-LC1201-36CD; it is always numbered zero.
user@host> set chassis fpc fpc-number pic 0 port port-number speed 400g|200g|100g|50g|40g|25g|10g number-of-subports subport-number
user@host> set chassis fpc 0 pic 0 port 15 speed 100g number-of-subports 4
et-0/0/15:0et-0/0/15:1et-0/0/15:2et-0/0/15:3
For software releases Junos OS Evolved Release 20.1R2 and later, the speed and number-of-subports options are in the interfaces hierarchy.
[edit-interfaces]user@host> et-0/0/15 {speed 100g;number-of-subports 4;}et-0/0/15:0 {unit 0}et-0/0/15:1 {unit 0}et-0/0/15:2 {unit 0}et-0/0/15:3 {unit 0}
QFX10000-60S-6Q
QFX10000-60S-60 
DOOCX 
PWR O, 
OOOOOOOOOOO 
OG 
STS O 
KOX 
60 
OFF O 
100G 
1 
64 
3000 0000 40000 00007 
BOO07 BODOV DOOOT A0000 
DO000 DOOOO DOOOD DO000 
AOOOO BOOOD DOOOD BOO00 
BOO00 BODOV AOOOV A0000 
V 3888 4|78888 4 08888 
2 
65 
014 Ca 
OOC 
DOOG 
00260000000 
3038OOOOOOOO 
000 
g050747 ...QFX10000-60S-60 
DOOCX 
PWR O, 
OOOOOOOOOOO 
OG 
STS O 
KOX 
60 
OFF O 
100G 
1 
64 
3000 0000 40000 00007 
BOO07 BODOV DOOOT A0000 
DO000 DOOOO DOOOD DO000 
AOOOO BOOOD DOOOD BOO00 
BOO00 BODOV AOOOV A0000 
V 3888 4|78888 4 08888 
2 
65 
014 Ca 
OOC 
DOOG 
00260000000 
3038OOOOOOOO 
000 
g050747
# KB #
KB93770 PTX10008 serial console login failed for tacacs users
The behavior of local authentication through console and remote authentication through management interface is the expected day 1 behavior on Junos EVO. This is tracked via PR1565251
KB37278... ...[PTX10008 Junos Evolved] How to shut down the standby RCB
On PTX10008 Evo，If we perform the node halt or node power-off operation to shut down the standby RCB 1, we only need to bring CB 1 up. RE comes Backup online by default.
However, if we perform the node offline operation to shut down the standby RCB 1, we bring CB 1 up and need to do node online .
Operation 3. Backup node (RE 1) goes to Present state with " node power-off " operation.
RE 1 is Backup and CB 1 is Online Standby.
RE 1 is node power-off , then CB 1 is  Fault Standby due to PR1581476  and RE 1 is Present.
CB 1 is offlined , then CB 1 is Offline Standby and RE 1 is Present.
RCB 1 shutdown completed. We can exchange RCB 1.
CB 1 is onlined , then CB 1 is Online Standby and RE 1 is Backup.
[21.1R1.11-EVO]
KB99269... ...Fans Fail to Operate After Hot-Insertion of Fan Tray on MX10004/MX10008/MX10016 and PTX10004/PTX10008/PTX10016
