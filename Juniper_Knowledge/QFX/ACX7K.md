# ACX7K

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

ACX7K
# Command #
> start shell user root
Password:
root@jtac-acx7100-48l-ao-proto-r2001:~# jbcmcmd.py "command"
root@jtac-acx7100-48l-ao-proto-r2001:~# jbcmsh
BCM.0>
AFT Mode: 
show picd config 
show picd global 
show picd channel summary 
show picd port summary 
show picd port fpc 0 pic 0 port <port#> 
show picd channel by-name chan_name channel-0/0/0:0 
show picd optics fpc_slot 0 pic_slot 0 port 0 cmd ? 
Possible completions: 
alarm 
Alarm information of the Xcvr 
diagnostics 
Diagnostics information of the Xcvr 
dump_xi_info 
Xcvr Interface dump 
error 
Xcvr Error counters 
identifier 
Identifier information of the Xcvr 
info 
Information of the Xcvr 
list 
Available Xcvr in the PIC ...AFT Mode: 
show picd config 
show picd global 
show picd channel summary 
show picd port summary 
show picd port fpc 0 pic 0 port <port#> 
show picd channel by-name chan_name channel-0/0/0:0 
show picd optics fpc_slot 0 pic_slot 0 port 0 cmd ? 
Possible completions: 
alarm 
Alarm information of the Xcvr 
diagnostics 
Diagnostics information of the Xcvr 
dump_xi_info 
Xcvr Interface dump 
error 
Xcvr Error counters 
identifier 
Identifier information of the Xcvr 
info 
Information of the Xcvr 
list 
Available Xcvr in the PIC
picd专注接口和硬件 I/O 层
Port Mapping: 
root@antman-plat-p1b-dc-01:pfe> show evo-pfemand interface asic-port fpc 0 pic 0 
Interface 
IfdIndex Speed Unit Port core Name 
Link 
pfe# 
et-0/0/0 
1216 
25G 
0 
14 
0 
eth1 
!ena 
et-0/0/1 
1217 
25G 
0 
2 
0 
eth2 
down 
et-0/0/2 
1218 
25G 
0 
3 
0 
eth3 
down 
et-0/0/3 
1219 
25G 
0 
4 
0 
eth4 
down 
et-0/0/4 
1220 
10G 
0 
5 
0 
eth5 
down 
et-0/0/5 
1221 
10G 
0 
6 
0 
eth6 
down 
et-0/0/6 
1222 
10G 
0 
7 
0 
eth7 
down 
et-0/0/7 
1223 
10G 
0 
8 
0 
eth8 
down 
et-0/0/8 
1224 
10G 
0 
9 
0 
eth9 
down 
et-0/0/9 
1225 
10G 
0 
10 
0 
eth10 
down 
et-0/0/18 
1234 
10G 
0 
19 
0 
eth19 
down 
sxe-0/0/1 
1236 
10G 
0 
20 
0 
eth20 
up ...Port Mapping: 
root@antman-plat-p1b-dc-01:pfe> show evo-pfemand interface asic-port fpc 0 pic 0 
Interface 
IfdIndex Speed Unit Port core Name 
Link 
pfe# 
et-0/0/0 
1216 
25G 
0 
14 
0 
eth1 
!ena 
et-0/0/1 
1217 
25G 
0 
2 
0 
eth2 
down 
et-0/0/2 
1218 
25G 
0 
3 
0 
eth3 
down 
et-0/0/3 
1219 
25G 
0 
4 
0 
eth4 
down 
et-0/0/4 
1220 
10G 
0 
5 
0 
eth5 
down 
et-0/0/5 
1221 
10G 
0 
6 
0 
eth6 
down 
et-0/0/6 
1222 
10G 
0 
7 
0 
eth7 
down 
et-0/0/7 
1223 
10G 
0 
8 
0 
eth8 
down 
et-0/0/8 
1224 
10G 
0 
9 
0 
eth9 
down 
et-0/0/9 
1225 
10G 
0 
10 
0 
eth10 
down 
et-0/0/18 
1234 
10G 
0 
19 
0 
eth19 
down 
sxe-0/0/1 
1236 
10G 
0 
20 
0 
eth20 
up
BCM.0> port status 
BCM.0> 12 show 
A 
BCM.0> show counters 
BCM.0> diag count g ...BCM.0> port status 
BCM.0> 12 show 
A 
BCM.0> show counters 
BCM.0> diag count g
s
Broadcom PFE chipset：... ericho 2 ...，... qumran 2
VERSATILE ACX7K Family 
3 
1G/10G/25G/50G/100G/400G Generation 
DNX 
Juniper Paragon 
Junos® 
EVOLVED 
Fixed 
Fixed 
Fixed 
Fixed 
Dual RE 
Modular 
Q2N 
Q2U 
J2 
Q2C 
Q2C+OP2 
J2C + J2C 
===== 
1111 
FRS 1H2025 
ACX7348 
ACX7332 
ACX7509 
ACX7020 
ACX7024 
ACX7024X 
ACX7100-48L 
ACX7100-32C 
3RU 
3RU 
5RU, Modular 
1RU, Fixed 
1RU, Fixed 
1RU, Fixed 
1RU, Fixed 
1RU, Fixed 
2.4T 
2.4T 
4.8T 
100G 
48× 1-25GE 
32× 1-25GE 
20× 1-50GE 
4x 1/10/25G 
360G 
360G 
4.8T 
4.8T 
8x 100GE 
8x 100GE 
16× 100GE 
(SFP28) 
4x 100 GE 
4x 100 GE 
6× 400G 
32× 100G 
4× 400GE 
16x 1/10G 
24x 1/10/25GE 
24x 1/10/25GE 
48x 10G/25G 
4× 400GE 
2x 800G + 1x 400G 
2x 800G + 1× 400G 
16x SFP56 
16× SFP56 
(SFPP) 
2x QSFP56+4x 
2x QSFP56+4x 
QSFP28 
QSFP28 
iTemp 
iTemp 
¡Temp 
eTemp ...VERSATILE ACX7K Family 
3 
1G/10G/25G/50G/100G/400G Generation 
DNX 
Juniper Paragon 
Junos® 
EVOLVED 
Fixed 
Fixed 
Fixed 
Fixed 
Dual RE 
Modular 
Q2N 
Q2U 
J2 
Q2C 
Q2C+OP2 
J2C + J2C 
===== 
1111 
FRS 1H2025 
ACX7348 
ACX7332 
ACX7509 
ACX7020 
ACX7024 
ACX7024X 
ACX7100-48L 
ACX7100-32C 
3RU 
3RU 
5RU, Modular 
1RU, Fixed 
1RU, Fixed 
1RU, Fixed 
1RU, Fixed 
1RU, Fixed 
2.4T 
2.4T 
4.8T 
100G 
48× 1-25GE 
32× 1-25GE 
20× 1-50GE 
4x 1/10/25G 
360G 
360G 
4.8T 
4.8T 
8x 100GE 
8x 100GE 
16× 100GE 
(SFP28) 
4x 100 GE 
4x 100 GE 
6× 400G 
32× 100G 
4× 400GE 
16x 1/10G 
24x 1/10/25GE 
24x 1/10/25GE 
48x 10G/25G 
4× 400GE 
2x 800G + 1x 400G 
2x 800G + 1× 400G 
16x SFP56 
16× SFP56 
(SFPP) 
2x QSFP56+4x 
2x QSFP56+4x 
QSFP28 
QSFP28 
iTemp 
iTemp 
¡Temp 
eTemp
在 ACX7000 (Jericho2/ Qumran2) 里面，数据包进入 PFE 后会经过 pipeline 分片处理，不像trio那种真正的切片
Pipeline 
Slice 0 
Packet 
Slice 1 
Packet 
Slice 2 
Slice 3 
Juniper ACX7000 
PFE (Jericho2) ...Pipeline 
Slice 0 
Packet 
Slice 1 
Packet 
Slice 2 
Slice 3 
Juniper ACX7000 
PFE (Jericho2)
Broadcom Jericho2/Qumran2 的 pipeline 切片 = 把整个数据平面拆成多个并行的“小路由引擎” (slice)，每个 slice 独立完成缓存、队列、调度，再统一出口调度。这样既能做到 Tbps 级吞吐，又能保证运营商级别的深缓冲和复杂 QoS。
Slices 
Ingress Pipeline 
Egress 
Pipeline 
Slice 
Slice 
Slice 
Parser 
VoQ 
VoQ 
VoG 
Egress 
L2/L3/LPM 
Buffer 
Buffer 
Buffer 
Scheduler 
ACL 
Queue 
Queue 
Queue 
Scheduler 
Scheduler 
Scheduler 
Policy 
Ingress ...Slices 
Ingress Pipeline 
Egress 
Pipeline 
Slice 
Slice 
Slice 
Parser 
VoQ 
VoQ 
VoG 
Egress 
L2/L3/LPM 
Buffer 
Buffer 
Buffer 
Scheduler 
ACL 
Queue 
Queue 
Queue 
Scheduler 
Scheduler 
Scheduler 
Policy 
Ingress
Full Duplex? 
100Gbps 
100GbE 
Rx Tx 
Rx 
Tx 
100GbE 
. We don't count Rx+Tx 
or Ingress+Egress to 
represent bandwidth 
OCB 
Small OCB 
100Gbps 
· What goes in, goes out 
100Gbps 
Ingress Pipeline 
Egress Pipeline 
Fabric 
Interface ...Full Duplex? 
100Gbps 
100GbE 
Rx Tx 
Rx 
Tx 
100GbE 
. We don't count Rx+Tx 
or Ingress+Egress to 
represent bandwidth 
OCB 
Small OCB 
100Gbps 
· What goes in, goes out 
100Gbps 
Ingress Pipeline 
Egress Pipeline 
Fabric 
Interface
Ingress-Only Buffering 
· Packet buffering is performed in ingress only 
> with a small OCP (On-Chip Buffer): SRAM 
> with a large DBB (Delay Buffer): HBM or GDDR6 
· Very small On-Chip Egress Port Buffer for packet serialization 
Deep 
Buffer 
(DBB) 
OCB 
Small OCB 
Ingress 
Egress 
Egress 
Ingress 
Fabric 
Interface 
Small OCB 
Deep 
Buffer 
(DBB) ...Ingress-Only Buffering 
· Packet buffering is performed in ingress only 
> with a small OCP (On-Chip Buffer): SRAM 
> with a large DBB (Delay Buffer): HBM or GDDR6 
· Very small On-Chip Egress Port Buffer for packet serialization 
Deep 
Buffer 
(DBB) 
OCB 
Small OCB 
Ingress 
Egress 
Egress 
Ingress 
Fabric 
Interface 
Small OCB 
Deep 
Buffer 
(DBB)
ACX在出口如果有拥塞仅仅pause一下，不会做rewrite，buffer这些了
WAN 
WAN 
SERDES 
SERDES 
Jericho2 
Qumran2 
Fabric 
SERDES ...WAN 
WAN 
SERDES 
SERDES 
Jericho2 
Qumran2 
Fabric 
SERDES
Jericho有fabric接口...，可以从一个pfe转到另一个pfe，而Qumran没有
Evo ULC Software Architecture for merchant silicon 
MGD 
CMDD 
JTI 
HwdRE 
fabricHub 
clksyncd 
J-insight 
switchd 
HwdRE - Chassis Management 
drivers 
SDK 
ConfigD 
Protocols / 
timingd 
FabricHub, FabSpoke-fchip - Fabric Management 
Services 
fabspoke-fchip 
ResilD 
clockd 
FPA / MRA 
Clockd, Timingd, Clksyncd - Timing 
IFMan 
imgd 
drivers 
... 
drivers 
drivers 
Switchd - Control plane ethernet switch management 
J-insight, Resild, FPA / MRA - Resiliency infrastructure 
JNX connector / OF - FRU power on / off, Kernel driver load 
app-controller 
DDS / DDX 
CapDB 
Distributed 
JNX sysfs 
Other libs 
DMF 
e.g. i2c, sensors 
Imgd - FPC boot server 
Sinetd - Internal network configuration 
Linux Kernel 
FDT / Overlay DT 
JNX connector 
Kernel device 
/ subsys 
drivers 
Route Engine 
Data specs 
other apps 
HwdLC 
picd 
fabspoke-pfe 
timingd 
ResilD 
switchd 
packetio 
HwdLC - Chassis Management 
securityd 
drivers 
drivers 
drivers 
clockd 
MRA 
SDK 
PicD - Port + Optics Management 
evo-pfemand 
jflowd 
drivers 
drivers 
SDK 
FabSpoke-pfe - Fabric Management 
Evo-pfemand + BCM SDK - PFE ASIC application 
PacketIO - Host path traffic 
app-controller 
DDS / DDX 
CapDB 
Distributed 
Other libs 
Clockd, Timingd - Timing 
DMF 
JNX sysfs 
e.g. i2c, sensors 
Switchd - Control plane ethernet switch management 
Resild, FPA / MRA - Resiliency infrastructure 
Linux Kernel 
FDT / Overlay DT 
JNX connector 
Kernel device 
/ subsys 
drivers 
JNX connector / OF - Local FRU power, Kernel driver load 
FPC 
Data specs 
Sinetd - Internal network configuration ...Evo ULC Software Architecture for merchant silicon 
MGD 
CMDD 
JTI 
HwdRE 
fabricHub 
clksyncd 
J-insight 
switchd 
HwdRE - Chassis Management 
drivers 
SDK 
ConfigD 
Protocols / 
timingd 
FabricHub, FabSpoke-fchip - Fabric Management 
Services 
fabspoke-fchip 
ResilD 
clockd 
FPA / MRA 
Clockd, Timingd, Clksyncd - Timing 
IFMan 
imgd 
drivers 
... 
drivers 
drivers 
Switchd - Control plane ethernet switch management 
J-insight, Resild, FPA / MRA - Resiliency infrastructure 
JNX connector / OF - FRU power on / off, Kernel driver load 
app-controller 
DDS / DDX 
CapDB 
Distributed 
JNX sysfs 
Other libs 
DMF 
e.g. i2c, sensors 
Imgd - FPC boot server 
Sinetd - Internal network configuration 
Linux Kernel 
FDT / Overlay DT 
JNX connector 
Kernel device 
/ subsys 
drivers 
Route Engine 
Data specs 
other apps 
HwdLC 
picd 
fabspoke-pfe 
timingd 
ResilD 
switchd 
packetio 
HwdLC - Chassis Management 
securityd 
drivers 
drivers 
drivers 
clockd 
MRA 
SDK 
PicD - Port + Optics Management 
evo-pfemand 
jflowd 
drivers 
drivers 
SDK 
FabSpoke-pfe - Fabric Management 
Evo-pfemand + BCM SDK - PFE ASIC application 
PacketIO - Host path traffic 
app-controller 
DDS / DDX 
CapDB 
Distributed 
Other libs 
Clockd, Timingd - Timing 
DMF 
JNX sysfs 
e.g. i2c, sensors 
Switchd - Control plane ethernet switch management 
Resild, FPA / MRA - Resiliency infrastructure 
Linux Kernel 
FDT / Overlay DT 
JNX connector 
Kernel device 
/ subsys 
drivers 
JNX connector / OF - Local FRU power, Kernel driver load 
FPC 
Data specs 
Sinetd - Internal network configuration
EVO-DDS 
EVO-DDS 
BQ 
BQ 
APPC 
APPC 
EAL 
EAL 
AFT-CLIENT 
BRCM-SHIM 
Evo-aftmand 
BCM-SDK 
AFT-SERVER 
CDA 
Evo-pfemand 
AFT daemons 
Platforms running 
Platforms running 
Merchant Silicon PFE 
Juniper Silicon PFE 
(Broadcom PFE) ...EVO-DDS 
EVO-DDS 
BQ 
BQ 
APPC 
APPC 
EAL 
EAL 
AFT-CLIENT 
BRCM-SHIM 
Evo-aftmand 
BCM-SDK 
AFT-SERVER 
CDA 
Evo-pfemand 
AFT daemons 
Platforms running 
Platforms running 
Merchant Silicon PFE 
Juniper Silicon PFE 
(Broadcom PFE)
https://www.juniper.net/documentation/us/en/hardware/acx7332/topics/topic-map/acx7332-system-overview.html
Metro Networks（城域网） 是指覆盖城市或大都市区域的中等规模通信网络，介于广域网（WAN）和局域网（LAN）之间，主要用于连接企业、数据中心、移动基站（如5G回传）、云服务接入点等关键设施。
ACX7K
面向运营商边缘（如5G移动回传、企业边缘）和云边缘场景，强调高密度接入和聚合。
Trio芯片，支持48×10G + 6×100G
MX304
核心/边缘路由器，专注于高密度100G/400G转发，适用于大规模数据中心互联（DCI）或运营商核心网络。
Express 4芯片，支持32×100G或8×400G端口，适合高密度核心场景
On ACX7000 series of routers, when an application restarts for more than three times in a five-minute window, then the application and its dependent applications goes into a failure state.
https://www.juniper.net/documentation/us/en/software/junos/cli-reference/topics/ref/command/show-system-applications.html
If an application goes into a failed state, then you need to manually recover the application...:
> restart app-name
Display ...reason for the evo-pfemand application failure:
user@host:/etc/systemd/system# systemctl status app-name
# ACX7332 #
3U, (1 fix + 2 FRU) FPCs, 2 REs...，...4 FANs, EVO only...，...Trio chipset
Removable FPCs 
1 
2 
1 
2×400 + 4X100 
60×50G 
2X400 + 4X100 
0 0 0 
O 
0000 
000 
9009 900L 
9007 
00: 
DOOL 
50 
1000 
Ov% 
003 
DOOL 
9004 
DOOL 
8 
GO 
100G 
8888 
888883 
38988 
& ACX7K3-FPC-2CD4C 
ACX7K3-FPC-2CD4C 
FPCD 
fixed FPC 
00 
ABET 
0 
O 
10 
auss Juniper 
0 
ACX7332 
#03 02 01 00 
0 
0 
O 
LM ACT 
ORE 
9 RE 
0 
LO 
20 
CON 
89 
0 
g102440 
4 
5 
4 
3 ...Removable FPCs 
1 
2 
1 
2×400 + 4X100 
60×50G 
2X400 + 4X100 
0 0 0 
O 
0000 
000 
9009 900L 
9007 
00: 
DOOL 
50 
1000 
Ov% 
003 
DOOL 
9004 
DOOL 
8 
GO 
100G 
8888 
888883 
38988 
& ACX7K3-FPC-2CD4C 
ACX7K3-FPC-2CD4C 
FPCD 
fixed FPC 
00 
ABET 
0 
O 
10 
auss Juniper 
0 
ACX7332 
#03 02 01 00 
0 
0 
O 
LM ACT 
ORE 
9 RE 
0 
LO 
20 
CON 
89 
0 
g102440 
4 
5 
4 
3
FRU FPCs
ACX7K3-FPC-2CD4C—Two 400GbE and four 100GbE ports
ACX7K3-FPC-16Y—Sixteen 50GbE ports
Fixed FPC
SFP28 ports 
O 
1 - Thirty two 1GbE/10GbE/25GbE 
o 
FPCI 
ACX7K3-FPC-2CD4C 
03 00 
GO 
ACX7300-RE 
1880 4880 
ITS ALM A 
LM ACT 
100G 
4000 
0880 4880 
1 
MONT 
COM 
2 
100G 
100G 
0 
OMA 
JUNIPE ACX7332 
4.24 925 420 
FPC2 
oo 
Co 
O 
2 
3 01 Oz 01 00 
26 929 430 
ACX7300-RE 
PWR O 
IT'S ALM A 
NOTE: MACsec is not supported on the 8 QSFP28 
ports (ports 24-31). 
2 - Eight 40GbE/100GbE QSFP28 ports. 
1 
FPC3 
ACX7K3-FPC-2CD4C 
GO 
GO 
100G 
4000 
000 
Ovi 
1000 
400G 
0 
1000 
1000 
0 
O 
g102444 ...SFP28 ports 
O 
1 - Thirty two 1GbE/10GbE/25GbE 
o 
FPCI 
ACX7K3-FPC-2CD4C 
03 00 
GO 
ACX7300-RE 
1880 4880 
ITS ALM A 
LM ACT 
100G 
4000 
0880 4880 
1 
MONT 
COM 
2 
100G 
100G 
0 
OMA 
JUNIPE ACX7332 
4.24 925 420 
FPC2 
oo 
Co 
O 
2 
3 01 Oz 01 00 
26 929 430 
ACX7300-RE 
PWR O 
IT'S ALM A 
NOTE: MACsec is not supported on the 8 QSFP28 
ports (ports 24-31). 
2 - Eight 40GbE/100GbE QSFP28 ports. 
1 
FPC3 
ACX7K3-FPC-2CD4C 
GO 
GO 
100G 
4000 
000 
Ovi 
1000 
400G 
0 
1000 
1000 
0 
O 
g102444
# ACX7348 #
1 
2 
1 
0 
FPC3 
0880 
488V 
ABE 
0 
r 
AM 525 
3880 0880 
1880 
ΔΕΞ 
3880 0881 
GNSS 
JUNIPer ACX7348 
0 
GO3 02 01 00 
PPS OUT 
0 
0 
Po 
g102257 
1 - Forty-eight 1GbE/10GbE/25GbE SFP28 ports 
2 - Eight 40GbE/100GbE QSFP28 ports 
与 ACX7332 却 别 就 在 于 FRU FPC 的 端 口 数 量 ...1 
2 
1 
0 
FPC3 
0880 
488V 
ABE 
0 
r 
AM 525 
3880 0880 
1880 
ΔΕΞ 
3880 0881 
GNSS 
JUNIPer ACX7348 
0 
GO3 02 01 00 
PPS OUT 
0 
0 
Po 
g102257 
1 - Forty-eight 1GbE/10GbE/25GbE SFP28 ports 
2 - Eight 40GbE/100GbE QSFP28 ports 
与 ACX7332 却 别 就 在 于 FRU FPC 的 端 口 数 量
