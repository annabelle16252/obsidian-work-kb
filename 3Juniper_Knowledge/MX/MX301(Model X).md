# MX301(Model X)

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

MX301(Model X)
TOI Site:
https://junipernetworks.sharepoint.com/sites/JuniperTOI/TOI%20Working%20Library/Forms/AllItems.aspx?id=%2Fsites%2FJuniperTOI%2FTOI%20Working%20Library%2FModel%2DX%20%28MX301%29&viewid=d8ebf0ab%2Dc485%2D47e2%2Da551%2D61df60b91a4c&csf=1&web=1&e=Tz8pUH&cid=1fc85511%2Df0f4%2D47df%2Da1c8%2D457c08b68b27&FolderCTID=0x0120003F42CF651FA1574FBAAC0388AE2AD778
目前不支持CGNAT,但是在评估...，可能会在未来...1年支持...。
只支持单RE，需要双RE用MX...304...，需要更高bw用MX...10K
Powered& 
by 
MX301: Introduction 
Trio 6 
Junper 
1 
1 RU 
MX301 
1.6 TBps 
Complete Junos Protocol 
Security without Compromise 
Flexible Front Panel 
Stack 
L2, L3 VPN, SRv6, 
Line-Rate MACSec 
1G Jumbo Frames 
MPLS, EVPN, Tunnels 
600G IPSEC 
Native 1/10G SFP 
and Filters 
FIPS/CC Ready 
QSFP 100/400G 
BNG Support 
OpenConfig, Telemetry, and 
NETCONF 
Flexible System Pricing 
48K subscriber scale 
Robust Automation Support 
PAYG Pricing Optimized 
Paragon and Mist automation 
starting 800 Gbps ...Powered& 
by 
MX301: Introduction 
Trio 6 
Junper 
1 
1 RU 
MX301 
1.6 TBps 
Complete Junos Protocol 
Security without Compromise 
Flexible Front Panel 
Stack 
L2, L3 VPN, SRv6, 
Line-Rate MACSec 
1G Jumbo Frames 
MPLS, EVPN, Tunnels 
600G IPSEC 
Native 1/10G SFP 
and Filters 
FIPS/CC Ready 
QSFP 100/400G 
BNG Support 
OpenConfig, Telemetry, and 
NETCONF 
Flexible System Pricing 
48K subscriber scale 
Robust Automation Support 
PAYG Pricing Optimized 
Paragon and Mist automation 
starting 800 Gbps
Endless Opportunities Await Across SP 
Edge 
Cloud 
Compute 
Connect 
ACX 
Business 
Wholesale 
Data Center 
Mobile 
DCI 
Business 
Core 
MX301 
ACX 
PTX 
PTX 
PT> 
MX 
DCE 
Residential 
PTX 
5G 
Public 
Cloud 
BNG 
Mobile 
Wholesale 
Internet 
CDN 
Peer 
Access 
Aggregation 
Edge 
Core 
Peering 
Scalable, Secure, and Reliable Connectivity for Diverse Use Cases ...Endless Opportunities Await Across SP 
Edge 
Cloud 
Compute 
Connect 
ACX 
Business 
Wholesale 
Data Center 
Mobile 
DCI 
Business 
Core 
MX301 
ACX 
PTX 
PTX 
PT> 
MX 
DCE 
Residential 
PTX 
5G 
Public 
Cloud 
BNG 
Mobile 
Wholesale 
Internet 
CDN 
Peer 
Access 
Aggregation 
Edge 
Core 
Peering 
Scalable, Secure, and Reliable Connectivity for Diverse Use Cases
AI requires two things from a router, one is need to have bandwidth in order for host path to get the training data to the cloud. 2nd is large scale of routing-table.
System Spec Summary 
Form Factor 
1 RU 
System BW; PFE & Switch Fabric 
1.6 Tbps 1x YT (No Fabric) 
Port Configurations 
10 QSFP Ports - 4 QSFP-DD, 6 QSFP-28 
16 SFP Ports - SFP56, SFP28, SFP+, SFP 
At any point overall throughput cannot exceed beyond 1.6 Tbps 
RE CPU - Ice Lake LCC, 10 Core with QAT, 3.0GHz, 90W; 
CPU Subsystem 
4×DIMM socket, 32GB/DIMM; 
200GB SSD: 2xM.2" NVMe SSD 
Timing 
SyncE, Class-C PTP (TOD, 10M & PPS clock I/P Or O/P) 
Cooling 
6x Fan Tray; Front to Back cooling 
Power Supply 
1+1 AC, DC and HVAC/DC PSU 850W (PSU already in Production) 
RE Ports 
1x USB3.0, 1xRJ45 for MGT, 1xRJ45 for Console 
Timing Ports 
1XRJ45 for TOD, 4XSMB 10M & PPS I/P & O/P ...System Spec Summary 
Form Factor 
1 RU 
System BW; PFE & Switch Fabric 
1.6 Tbps 1x YT (No Fabric) 
Port Configurations 
10 QSFP Ports - 4 QSFP-DD, 6 QSFP-28 
16 SFP Ports - SFP56, SFP28, SFP+, SFP 
At any point overall throughput cannot exceed beyond 1.6 Tbps 
RE CPU - Ice Lake LCC, 10 Core with QAT, 3.0GHz, 90W; 
CPU Subsystem 
4×DIMM socket, 32GB/DIMM; 
200GB SSD: 2xM.2" NVMe SSD 
Timing 
SyncE, Class-C PTP (TOD, 10M & PPS clock I/P Or O/P) 
Cooling 
6x Fan Tray; Front to Back cooling 
Power Supply 
1+1 AC, DC and HVAC/DC PSU 850W (PSU already in Production) 
RE Ports 
1x USB3.0, 1xRJ45 for MGT, 1xRJ45 for Console 
Timing Ports 
1XRJ45 for TOD, 4XSMB 10M & PPS I/P & O/P
-YT chipset: everything is loopback inside the chassis thus no need for fabric
PFE & PHY CONNECTIONS 
· YT1.1 ASIC as PFE, supports full 1.6T max throughput 
in Model-X 
GEARBOX #0 
0 
QSFP-DD 
4×100G/8>50G/4×25G 
PO[0:7] 
PFE SLICE 1 
· There are 4 port groups of 400G each in YT ASIC 
2 
QSFP-28 
-4x25G 
P1[0:3] 
PO[0:7] 
Slice 1[0:7] 
8x500 
PGO 
· Each port group from the YT connects to a group of 
1 
QSFP-28 
4×25G 
P1[7:4] 
P1[7:4] 
ports. 
m 
3 
QSFP-28 
4x25G 
FABIO 
. Identical set of port types between the 2 slices of the 
SFP-112 
SFP-112 
GEARBOX #1 
YT for equal distribution of ports between slices 
SFP-112 
PO[0.3] 
10 
SFP-112 
4x100G/4x50G 
5 
SFP-56 
PO[4:7] 
Slice1[8:15] 
· Each port group has different port speed options 
SFP - 56 
-4x50G 
PG1 
9 
11 
SFP-56 
PO[O:7] 
-8×50G 
enabling multi-speed option without needing breakout 
SFP-56 
12 
QSFP-DD 
4×100G/8x50G/4×25G 
P1[0:7 
36TX+ 36RX 
cables 
YT ASIC 
50G 
FABRIC LINKS 
GEARBOX #2 
· YT supports 56G SERDES on WAN side and fabric side 
13 1416 1820 1517 191 
13 
QSFP-DD 
4x100G/8x50G/4×25G 
PO[0:7] 
. 100G WAN link speed supported towards the optics 
16 
SFP-112 
SFP-112 
SFP-112 
4x100G/4×50G 
P1[0:3] 
Slice0[15:8] 
using gearbox on selective ports 
SFP-112 
PO[0:7] 
-8×50G 
PG1 
15 
SFP-56 
4×500 
P1 [4:7 
· YT is operated in fabric-less, standalone mode in 
19 
SFP - 56 
SFP-56 
SFP-56 
FABIO 
Model-X 
GEARBOX #3 
4x100G/8x50G/4×25G 
PO[0:7] 
· YT ASIC's fabric side internal loopback is enabled 
22 
QSFP-DD 
24 
QSFP-28 
PO[0:7] 
Slice0[7:0] 
4× 250 
P1[0:3] 
and used currently 
8×500 
PGO 
. There are external loopback traces routed 
23 
QSFP-28 
4×25G 
P1[4: 
P1[7:4] 
PFE SLICE 0 
between Tx and Rx on the PCB as well, as a 
25 
QSFP-28 
4×25G 
backup ...PFE & PHY CONNECTIONS 
· YT1.1 ASIC as PFE, supports full 1.6T max throughput 
in Model-X 
GEARBOX #0 
0 
QSFP-DD 
4×100G/8>50G/4×25G 
PO[0:7] 
PFE SLICE 1 
· There are 4 port groups of 400G each in YT ASIC 
2 
QSFP-28 
-4x25G 
P1[0:3] 
PO[0:7] 
Slice 1[0:7] 
8x500 
PGO 
· Each port group from the YT connects to a group of 
1 
QSFP-28 
4×25G 
P1[7:4] 
P1[7:4] 
ports. 
m 
3 
QSFP-28 
4x25G 
FABIO 
. Identical set of port types between the 2 slices of the 
SFP-112 
SFP-112 
GEARBOX #1 
YT for equal distribution of ports between slices 
SFP-112 
PO[0.3] 
10 
SFP-112 
4x100G/4x50G 
5 
SFP-56 
PO[4:7] 
Slice1[8:15] 
· Each port group has different port speed options 
SFP - 56 
-4x50G 
PG1 
9 
11 
SFP-56 
PO[O:7] 
-8×50G 
enabling multi-speed option without needing breakout 
SFP-56 
12 
QSFP-DD 
4×100G/8x50G/4×25G 
P1[0:7 
36TX+ 36RX 
cables 
YT ASIC 
50G 
FABRIC LINKS 
GEARBOX #2 
· YT supports 56G SERDES on WAN side and fabric side 
13 1416 1820 1517 191 
13 
QSFP-DD 
4x100G/8x50G/4×25G 
PO[0:7] 
. 100G WAN link speed supported towards the optics 
16 
SFP-112 
SFP-112 
SFP-112 
4x100G/4×50G 
P1[0:3] 
Slice0[15:8] 
using gearbox on selective ports 
SFP-112 
PO[0:7] 
-8×50G 
PG1 
15 
SFP-56 
4×500 
P1 [4:7 
· YT is operated in fabric-less, standalone mode in 
19 
SFP - 56 
SFP-56 
SFP-56 
FABIO 
Model-X 
GEARBOX #3 
4x100G/8x50G/4×25G 
PO[0:7] 
· YT ASIC's fabric side internal loopback is enabled 
22 
QSFP-DD 
24 
QSFP-28 
PO[0:7] 
Slice0[7:0] 
4× 250 
P1[0:3] 
and used currently 
8×500 
PGO 
. There are external loopback traces routed 
23 
QSFP-28 
4×25G 
P1[4: 
P1[7:4] 
PFE SLICE 0 
between Tx and Rx on the PCB as well, as a 
25 
QSFP-28 
4×25G 
backup
MX301 Port Configuration 
· MX301 Supports various optics - 400G, 100G, 40G, 25G, 10G, 1G 
. Port Checker Tool for Optics - https://apps.juniper.net/port-checker/mx301/ 
Port speeds range from 1GbE to 400GbE. 
Junper 
388 
818:8:8;8:8 888488 
0 
2 
4 
6 
8 
10 
12 
14 
16 
18 
20 
22 
24 
MOM! 
100 
O 
1 
3 
5 
7 
9 
11 
13 
15 
17 
19 
21 
23 
25 
1/10/25/50/100/400GbE 
1/10/25/40/100GbE 
1/10/25/100GbE 
1/10/25/50GbE 
1/10/25/40/50/100/400GbE 
. MX301 has #4 Port Groups 
. Each Port Group will support max throughput of 400G 
Juniper 
0 
2 
4 6 8 10 
12 
14 
16 
18 
20 
22 
24 
25 
1 
3 
5 7 9 
11 
13 
15 
17 
19 
21 
23 
25 
KARAR 
PG 0 
PG 1 
PG 2 
PG 3 ...MX301 Port Configuration 
· MX301 Supports various optics - 400G, 100G, 40G, 25G, 10G, 1G 
. Port Checker Tool for Optics - https://apps.juniper.net/port-checker/mx301/ 
Port speeds range from 1GbE to 400GbE. 
Junper 
388 
818:8:8;8:8 888488 
0 
2 
4 
6 
8 
10 
12 
14 
16 
18 
20 
22 
24 
MOM! 
100 
O 
1 
3 
5 
7 
9 
11 
13 
15 
17 
19 
21 
23 
25 
1/10/25/50/100/400GbE 
1/10/25/40/100GbE 
1/10/25/100GbE 
1/10/25/50GbE 
1/10/25/40/50/100/400GbE 
. MX301 has #4 Port Groups 
. Each Port Group will support max throughput of 400G 
Juniper 
0 
2 
4 6 8 10 
12 
14 
16 
18 
20 
22 
24 
25 
1 
3 
5 7 9 
11 
13 
15 
17 
19 
21 
23 
25 
KARAR 
PG 0 
PG 1 
PG 2 
PG 3
Mx301 Architecture - 10K ft view 
JUNOS 
VM 
ULC/AFT VM 
WRL VMHOST 
10 cores x86 Intel 
PFE O 
PFE 1 
YT 1.1 ASIC 
MX301 ...Mx301 Architecture - 10K ft view 
JUNOS 
VM 
ULC/AFT VM 
WRL VMHOST 
10 cores x86 Intel 
PFE O 
PFE 1 
YT 1.1 ASIC 
MX301
Mx304 vs Mx301 - Architectural differences 
RE 
VJUNOS 
JUNOS 
ULC/AFT 
WRL VMHOST 
VM 
VM 
8 cores x86 Intel 
WRL VMHOST 
10 cores x86 Intel 
ULC/AFT 
0 
8 cores x86 AMD 
1 
YT 1.1 
O 
0 
O 
1 
1 
1 
MX301 
YT 
YT 
YT 
FPC 
Chassis 
CPU Core 
DRAM (GB) 
Mx304 
8 + 8 (~2.6 GHz) 
128 + 32 = 160 
Mx304 
Mx301 
10 (~3.0 GHz) 
128 (80:32:14) ...Mx304 vs Mx301 - Architectural differences 
RE 
VJUNOS 
JUNOS 
ULC/AFT 
WRL VMHOST 
VM 
VM 
8 cores x86 Intel 
WRL VMHOST 
10 cores x86 Intel 
ULC/AFT 
0 
8 cores x86 AMD 
1 
YT 1.1 
O 
0 
O 
1 
1 
1 
MX301 
YT 
YT 
YT 
FPC 
Chassis 
CPU Core 
DRAM (GB) 
Mx304 
8 + 8 (~2.6 GHz) 
128 + 32 = 160 
Mx304 
Mx301 
10 (~3.0 GHz) 
128 (80:32:14)
CPU Allocation:
MX304 
MX301 
[Total - 32] 
[Total - 20] 
Host OS/VMHOST 
4 
4 
vJUNOS 
12 
8 
VULC/vPMB 
16 
6 ...MX304 
MX301 
[Total - 32] 
[Total - 20] 
Host OS/VMHOST 
4 
4 
vJUNOS 
12 
8 
VULC/vPMB 
16 
6
TVP SW Architecture for MX301 
Routing 
JUNOS VM 
Engine 
LCMD 
t 
DST 
FANS, PSM, 
Sensors, LEDs 
WRL VMHOST Linux 
FPC-BUILTIN 
AFT/CDA 
YT 
Sensors 
ULC Daemons 
WRL EVO Linux ...TVP SW Architecture for MX301 
Routing 
JUNOS VM 
Engine 
LCMD 
t 
DST 
FANS, PSM, 
Sensors, LEDs 
WRL VMHOST Linux 
FPC-BUILTIN 
AFT/CDA 
YT 
Sensors 
ULC Daemons 
WRL EVO Linux
ULC Software stack (recap) - Leveraging the time-tested SW 
Junos RE 
Junos VM 
mgd 
dcd 
rpd 
chasissd 
BSD Kernel 
IPO Junos 
A 
uLC Linecard 
IF State 
securityd 
junosd 
resiliencyd 
mgd 
AFTMAN Junos 
PKT-IO 
EVO Infra 
hwc 
platformd 
fabspoked 
Manageability 
AFTServer 
Trace client 
picd 
clksyncd 
switchd 
Sysman 
CDA 
JDID Diags 
EVO app 
Distributord 
Platform Components 
Diagnostics 
AFT components 
EVO infra 
Orchestratord 
syslog 
babbletrace 
Linux app / drivers 
FPA 
bridges 
Junos IPC 
IFState Messages 
Dev DB 
Cap DB 
: Linux Applications / Infra 
Packet path 
WRL Linux Kernel + Drivers 
o 
Hardware 
CPU Subsystem HW 
FRU HW 
Fabric HW 
PFE HW ...ULC Software stack (recap) - Leveraging the time-tested SW 
Junos RE 
Junos VM 
mgd 
dcd 
rpd 
chasissd 
BSD Kernel 
IPO Junos 
A 
uLC Linecard 
IF State 
securityd 
junosd 
resiliencyd 
mgd 
AFTMAN Junos 
PKT-IO 
EVO Infra 
hwc 
platformd 
fabspoked 
Manageability 
AFTServer 
Trace client 
picd 
clksyncd 
switchd 
Sysman 
CDA 
JDID Diags 
EVO app 
Distributord 
Platform Components 
Diagnostics 
AFT components 
EVO infra 
Orchestratord 
syslog 
babbletrace 
Linux app / drivers 
FPA 
bridges 
Junos IPC 
IFState Messages 
Dev DB 
Cap DB 
: Linux Applications / Infra 
Packet path 
WRL Linux Kernel + Drivers 
o 
Hardware 
CPU Subsystem HW 
FRU HW 
Fabric HW 
PFE HW
YT ASIC Overview - recap 
YT ASIC (1.6T) 
Fabric 
Fabric 
YT Slice 0 (800G) 
YT Slice 1 (800G) 
· 1200 MHz core clock 
· 1200 MHz core clock 
· 80 PPES 
DMem 
· 80 PPEs 
· 64K WAN queues 
Interconnect 
· 64K WAN queues 
. 1K fabric queues 
· 1K fabric queues 
· 10/25/40/50/100/200/400GE 
· 10/25/40/50/100/200/400GE 
with MACsec 
with MACsec 
· IPsec (300G) 
· IPsec (300G) 
HBM 
HBM 
4GB 
LUSS FlexMem 
LUSS FlexMem 
4GB 
8MB 
OCPMem and 
8MB 
PMem 
MQSS FlexMem 
Interconnect 
MQSS FlexMem 
32MB 
32MB 
WAN PG 0 
WAN PG 1 
WAN PG 0 
WAN PG 1 
(400G) 
(400G) 
(400G) 
(400G) 
+ 
WAN 
WAN ...YT ASIC Overview - recap 
YT ASIC (1.6T) 
Fabric 
Fabric 
YT Slice 0 (800G) 
YT Slice 1 (800G) 
· 1200 MHz core clock 
· 1200 MHz core clock 
· 80 PPES 
DMem 
· 80 PPEs 
· 64K WAN queues 
Interconnect 
· 64K WAN queues 
. 1K fabric queues 
· 1K fabric queues 
· 10/25/40/50/100/200/400GE 
· 10/25/40/50/100/200/400GE 
with MACsec 
with MACsec 
· IPsec (300G) 
· IPsec (300G) 
HBM 
HBM 
4GB 
LUSS FlexMem 
LUSS FlexMem 
4GB 
8MB 
OCPMem and 
8MB 
PMem 
MQSS FlexMem 
Interconnect 
MQSS FlexMem 
32MB 
32MB 
WAN PG 0 
WAN PG 1 
WAN PG 0 
WAN PG 1 
(400G) 
(400G) 
(400G) 
(400G) 
+ 
WAN 
WAN
Uniqueness of MX301 and associated challenges 
By now, you may have noticed many 'firsts in MX' and might be thinking about their impact on customer experience 
.Additional SW: 
Two 
stability, 
VMs 
debuggability 
· VM's performance 
overhead 
CPU 
haircut 
·MX's massive multi- 
~20% 
D performance ask 
Juniper- 
NETWORKS 
DRAM 
. MX's massive multi- 
haircut 
D scale ask 
. New 3rd party SW 
QAT 
& HW: Stability, 
debuggability 
New 
MTK 
.New 3rd party SW 
& HW: Stability, 
gearbox 
debuggability ...Uniqueness of MX301 and associated challenges 
By now, you may have noticed many 'firsts in MX' and might be thinking about their impact on customer experience 
.Additional SW: 
Two 
stability, 
VMs 
debuggability 
· VM's performance 
overhead 
CPU 
haircut 
·MX's massive multi- 
~20% 
D performance ask 
Juniper- 
NETWORKS 
DRAM 
. MX's massive multi- 
haircut 
D scale ask 
. New 3rd party SW 
QAT 
& HW: Stability, 
debuggability 
New 
MTK 
.New 3rd party SW 
& HW: Stability, 
gearbox 
debuggability
JUNOS Chassisd process has connection with LINUX LCMD process.
Chassisd and LCMD establish connection with each other
LCMD handles presence detection, and reads EEPROM and send JDAF message to chassisd.
FRU info/status, power commands are exchanged between Chassisd and LCMD as JDAF messages.
FPC:MX301 removed PMB, CPU&DRAM are shared with RE
Key HW components 
· YT 1.1 ASIC 
. Marvell Ethernet Switch 
· PFE FPGA 
· PFE CPLD 
· PTP FPGA 
· MTK Gearboxes 
· Missing: PMB CPU, DRAM ...Key HW components 
· YT 1.1 ASIC 
. Marvell Ethernet Switch 
· PFE FPGA 
· PFE CPLD 
· PTP FPGA 
· MTK Gearboxes 
· Missing: PMB CPU, DRAM
VULC VM bootup & run time 
RE Junos VM 
ULC VM 
CM-EVOD 
PICD 
RPD 
ChassisD 
uKernel 
AFT 
DCD 
PTP-FPGA 
.... 
Vhclient utility 
... 
(Init script) 
1 
LCMD 
QEMU, 
veHostD 
SSHD 
Shell Scripts 
Libvirt 
libvirt 
Qemu .. 
WRL Linux Kernel 
KVM 
x86 Intel Ice Lakh CPU 
ETH NAC 
PTP FPGA 
YT ASIC 
1/2 
RE FPGA 
PFE FPGA 
GE SWITCH 
ETH NAC 2/2 ...VULC VM bootup & run time 
RE Junos VM 
ULC VM 
CM-EVOD 
PICD 
RPD 
ChassisD 
uKernel 
AFT 
DCD 
PTP-FPGA 
.... 
Vhclient utility 
... 
(Init script) 
1 
LCMD 
QEMU, 
veHostD 
SSHD 
Shell Scripts 
Libvirt 
libvirt 
Qemu .. 
WRL Linux Kernel 
KVM 
x86 Intel Ice Lakh CPU 
ETH NAC 
PTP FPGA 
YT ASIC 
1/2 
RE FPGA 
PFE FPGA 
GE SWITCH 
ETH NAC 2/2
vULC = Virtual User Line Card（虚拟用户线卡）
Standard virsh commands：
root@modelx-plat-p1b-d-node :~ # virsh list 
Id 
Name 
State 
1 
vjunos 
running 
2 
VULC 
running ...root@modelx-plat-p1b-d-node :~ # virsh list 
Id 
Name 
State 
1 
vjunos 
running 
2 
VULC 
running
root@modelx-plat-p1b-d-node :~ # virsh dumpxml vULC 
<domain type='kvm' id='2' xmlns: qemu='http://libvirt.org/schemas/domain/qemu/1.0'> 
<name>VULC</name> 
<uuid>f8bc7ee6-ele7-4aab-87aa-11f744bf0046</uuid> 
<memory unit='KiB'>33554432</memory> 
<currentMemory unit='KiB' >33554432</currentMemory> 
<vcpu placement='static' cpuset='7-9, 17-19'>6</vcpu> 
<cputune> 
<vcpupin vcpu='0' cpuset='7'/> 
<vcpupin vcpu='1' cpuset='8' /> 
<vcpupin vcpu='2' cpuset='9'/> 
<vcpupin vcpu='3' cpuset='17' /> 
<vcpupin vcpu='4' cpuset='18' /> 
<vcpupin vcpu='5' cpuset='19' /> 
</cputune> 
<resource> 
<partition>/machine</partition> 
</resource> 
<0s> 
<type arch='x86_64' machine='pc-q35-6.2' >hvm</type> 
<loader readonly='yes' type='pflash'>/usr/share/ovmf/ovmf. code.fd</loader> 
<nvram>/usr/share/ovmf/ovmf . vars.fd</nvram> 
<boot dev='network' /> 
</os> 
<features> ...root@modelx-plat-p1b-d-node :~ # virsh dumpxml vULC 
<domain type='kvm' id='2' xmlns: qemu='http://libvirt.org/schemas/domain/qemu/1.0'> 
<name>VULC</name> 
<uuid>f8bc7ee6-ele7-4aab-87aa-11f744bf0046</uuid> 
<memory unit='KiB'>33554432</memory> 
<currentMemory unit='KiB' >33554432</currentMemory> 
<vcpu placement='static' cpuset='7-9, 17-19'>6</vcpu> 
<cputune> 
<vcpupin vcpu='0' cpuset='7'/> 
<vcpupin vcpu='1' cpuset='8' /> 
<vcpupin vcpu='2' cpuset='9'/> 
<vcpupin vcpu='3' cpuset='17' /> 
<vcpupin vcpu='4' cpuset='18' /> 
<vcpupin vcpu='5' cpuset='19' /> 
</cputune> 
<resource> 
<partition>/machine</partition> 
</resource> 
<0s> 
<type arch='x86_64' machine='pc-q35-6.2' >hvm</type> 
<loader readonly='yes' type='pflash'>/usr/share/ovmf/ovmf. code.fd</loader> 
<nvram>/usr/share/ovmf/ovmf . vars.fd</nvram> 
<boot dev='network' /> 
</os> 
<features>
Console log is super critical for debuggability
root@modelx-plat-p1b-d-node: /var/log# 
root@modelx-plat-p1b-d-node:/var/log# ls -al vulc_console.log* 
-rw- 
1 root root 
50960 Nov 23 20:32 vulc_console.log 
-rw- 
1 root root 2097152 Nov 23 20:28 vulc_console. log.0 
-rw- 
1 root root 2097152 Nov 9 21:28 vulc_console. log.1 
root@modelx-plat-p1b-d-node: /var/log# ...root@modelx-plat-p1b-d-node: /var/log# 
root@modelx-plat-p1b-d-node:/var/log# ls -al vulc_console.log* 
-rw- 
1 root root 
50960 Nov 23 20:32 vulc_console.log 
-rw- 
1 root root 2097152 Nov 23 20:28 vulc_console. log.0 
-rw- 
1 root root 2097152 Nov 9 21:28 vulc_console. log.1 
root@modelx-plat-p1b-d-node: /var/log#
MX301 FPC LOGIN and CLI 
Linux 
· cty fpc0 
· CLI>start shell pfe direct uart-O fpc0 
· Issh fpc0 
· Access restricted to root 
Platformd 
· vty fpc0.0 
· CLI> start shell pfe network fpc0.0 
AFT CLI 
· vty fpc0 
· CLI> start shell pfe network fpc0 ...MX301 FPC LOGIN and CLI 
Linux 
· cty fpc0 
· CLI>start shell pfe direct uart-O fpc0 
· Issh fpc0 
· Access restricted to root 
Platformd 
· vty fpc0.0 
· CLI> start shell pfe network fpc0.0 
AFT CLI 
· vty fpc0 
· CLI> start shell pfe network fpc0
Host path and Ethernet switch connectivity 
JUNOS 
ULC/AFT 
VM 
VM (PMB) 
- 
SW Bridge 
HW Host Path 
WRL VMHOST 
Intel NAC, 10G Ports 
eth 3 
eth 2! 
eth 1 
Port 2 
Port 1 
Port 0 
PTP 
Port 5 
FPGA 
Marvell Switch 
1G 
Port 3 
Port 4 
1G 
1 
PFE 0 
PFE 1 
YT 
WAN IFDs ...Host path and Ethernet switch connectivity 
JUNOS 
ULC/AFT 
VM 
VM (PMB) 
- 
SW Bridge 
HW Host Path 
WRL VMHOST 
Intel NAC, 10G Ports 
eth 3 
eth 2! 
eth 1 
Port 2 
Port 1 
Port 0 
PTP 
Port 5 
FPGA 
Marvell Switch 
1G 
Port 3 
Port 4 
1G 
1 
PFE 0 
PFE 1 
YT 
WAN IFDs
host bound traffic通过marvell switchtransit
PMB在host bound traffic中的作用...：
Ingress Port 
Trio / PFE 
1 
[ PMB ] 
1 
Host-bound Queue 
1 
CPU / RE ...Ingress Port 
Trio / PFE 
1 
[ PMB ] 
1 
Host-bound Queue 
1 
CPU / RE
MX301没有硬件PMB了...，换成vmPMB
[root@modelx-plat-p1b-b-fpc0:pfe> show host-path ports 
Name 
State 
Tx-Packets 
Tx-Rate(pps) Rx-Packets 
Rx-Rate(pps) Drop-Packets Drop-Rate(pps) 
HHHOO00 
fp1 
READY 
18934 
1 
18934 
1 
0 
fp0 
READY 
18934 
18934 
1 
0 
fp-hb0 
READY 
19044 
19044 
1 
0 
0 
mka0 
READY 
0 
0 
0 
0 
0 
pcap0 
READY 
0 
0 
0 
0 
18 
0 
kats0 
READY 
0 
0 
0 
0 
ptp0 
READY 
0 
0 
0 
ppm0 
READY 
0 
0 
0 
0 
pktin0 
READY 
0 
0 
0 
0 
0 
am0 
READY 
0 
0 
0 
0 
cp0 
READY 
0 
0 
0 
0 
0 
0 
cp-hb0 
READY 
19044 
1 
19044 
1 
0 
0 ...[root@modelx-plat-p1b-b-fpc0:pfe> show host-path ports 
Name 
State 
Tx-Packets 
Tx-Rate(pps) Rx-Packets 
Rx-Rate(pps) Drop-Packets Drop-Rate(pps) 
HHHOO00 
fp1 
READY 
18934 
1 
18934 
1 
0 
fp0 
READY 
18934 
18934 
1 
0 
fp-hb0 
READY 
19044 
19044 
1 
0 
0 
mka0 
READY 
0 
0 
0 
0 
0 
pcap0 
READY 
0 
0 
0 
0 
18 
0 
kats0 
READY 
0 
0 
0 
0 
ptp0 
READY 
0 
0 
0 
ppm0 
READY 
0 
0 
0 
0 
pktin0 
READY 
0 
0 
0 
0 
0 
am0 
READY 
0 
0 
0 
0 
cp0 
READY 
0 
0 
0 
0 
0 
0 
cp-hb0 
READY 
19044 
1 
19044 
1 
0 
0
Host path debuggability - Inside Marvell Switch 
FPC Linux shell : telnet 127.0.0.1 12345 
[Console# show interfaces status all 
Dev/Port 
Mode 
Link 
Speed 
Duplex 
Loopback Mode 
0/0 
KR 
Up 
10G 
Full 
Internal 
0/1 
KR 
Up 
10G 
Full 
None 
0/2 
KR 
Up 
10G 
Full 
None 
0/3 
1000_BaseX 
Up 
1G 
Full 
Internal 
0/4 
1000_BaseX 
Up 
1G 
Full 
None 
0/5 
1000_BaseX 
Up 
1G 
Full 
None 
[Console# show vlan device 0 
.. .. 
VLAN 
Ports 
Tag 
MAC-Learning 
FDB-mode 
2 
0/1 
tagged 
Automatic 
FID 
0/2 
untagged 
456 
0/0,3-4 
0/3-5 
tagged 
Automatic 
FID 
untagged 
Automatic 
FID 
0/3-4 
tagged 
Automatic 
FID 
0/2 
20 
0/0,3-4 
untagged 
tagged 
Automatic 
FID 
Console# ...Host path debuggability - Inside Marvell Switch 
FPC Linux shell : telnet 127.0.0.1 12345 
[Console# show interfaces status all 
Dev/Port 
Mode 
Link 
Speed 
Duplex 
Loopback Mode 
0/0 
KR 
Up 
10G 
Full 
Internal 
0/1 
KR 
Up 
10G 
Full 
None 
0/2 
KR 
Up 
10G 
Full 
None 
0/3 
1000_BaseX 
Up 
1G 
Full 
Internal 
0/4 
1000_BaseX 
Up 
1G 
Full 
None 
0/5 
1000_BaseX 
Up 
1G 
Full 
None 
[Console# show vlan device 0 
.. .. 
VLAN 
Ports 
Tag 
MAC-Learning 
FDB-mode 
2 
0/1 
tagged 
Automatic 
FID 
0/2 
untagged 
456 
0/0,3-4 
0/3-5 
tagged 
Automatic 
FID 
untagged 
Automatic 
FID 
0/3-4 
tagged 
Automatic 
FID 
0/2 
20 
0/0,3-4 
untagged 
tagged 
Automatic 
FID 
Console#
Fabric-Less design 
· Fabric-less fabric architecture 
. No fabric asic, single forwarding asic, 2 data path pipelines, internal fabric for switching data 
. Use cascade links between PFEs to achieve inter-pfe traffic 
. Avoids physical connection between PFEs: 
CCLO 
o Reduces SI concerns 
o Usage of internal resource previously 
PFEO 
FABIO 
CCL1 
unutilized 
o "Fabric errors" almost completely 
CCLZ 
CASC_SLICER_1 
CASC_SLICE1_0 
disappears 
o No fabric chip latency 
ČČLO 
o Same bandwidth as with fabric chip model 
o Less power consumption 
TX 
PFEI 
FABIO 
CCL1 
TX 
CCL2 ...Fabric-Less design 
· Fabric-less fabric architecture 
. No fabric asic, single forwarding asic, 2 data path pipelines, internal fabric for switching data 
. Use cascade links between PFEs to achieve inter-pfe traffic 
. Avoids physical connection between PFEs: 
CCLO 
o Reduces SI concerns 
o Usage of internal resource previously 
PFEO 
FABIO 
CCL1 
unutilized 
o "Fabric errors" almost completely 
CCLZ 
CASC_SLICER_1 
CASC_SLICE1_0 
disappears 
o No fabric chip latency 
ČČLO 
o Same bandwidth as with fabric chip model 
o Less power consumption 
TX 
PFEI 
FABIO 
CCL1 
TX 
CCL2
54:50
