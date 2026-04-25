# Interface

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

Interface
fe-百兆 100M
ge- 千兆 1G/1000M
xe- ...万兆... 10G
et- ...1...0G/40G/100G/200G/400G 可以channelized成多个接口
单位
全称
换算关系
1 ...Tb
1 ...Tb
1 ...T...b = 1000 ...G...b
1 Gb
1 Gigabit
1 Gb = 1000 Mb
1 Mb
1 Megabit
1 Mb = 1000 Kb
1 Kb
1 Kilobit
1 Kb = 1000 bit
1 B
1 Byte
1 B = 8 bit
SFP种类
SFP
QSFP
CFP
Interface Range
[edit interfaces interface-range range1]user@device# set member et-0/*/*set member et-0/[1-10]/0set member et-0/[1,2,3]/3
Channelize...：
[edit chassis fpc 1 pic 0 port 3]user@host# set number-of-sub-ports 4
et-1/0/3 100 Gbps
four sub-ports are et-1/0/3:0, et-1/0/3:1, et-1/0/3:2, and et-1/0/3:3 with 25G each
Forward Error Correction (FEC)
https://www.juniper.net/documentation/us/en/software/junos/interfaces-fundamentals-evo/interfaces-fundamentals/topics/topic-map/physical-interface-properties.html
Enhances the reliability of the connection
Enables the receiver to correct transmission errors without requiring retransmission of the data
Extends the reach of optics
cli-pfe
root@re0:pfe> show picd config
pic_info_table               :              default   config     config       config   computedpic_or_port   speed     pic_mode   port_speed   valid    speed      supported_speeds                                           hidden_speeds-----------   -------   --------   ----------   ------   --------   --------------------------------------------------------   -------------pic-0/0       -         -          -            yes      -port-0/0/0    1x400G    -          1x100G       -        1x100G     [ 1x400G 1x200G 4x10G 1x40G 4x25G 1x100G 4x100G 2x200G ]   [ ]port-0/0/1    1x400G    -          1x100G       -        1x100G     [ 1x400G 1x200G 4x10G 1x40G 4x25G 1x100G 4x100G 2x200G ]   [ ]port-0/0/2    1x400G    -          1x100G       -        1x100G     [ 1x400G 1x200G 4x10G 1x40G 4x25G 1x100G 4x100G 2x200G ]   [ ]port-0/0/3    1x400G    -          1x100G       -        1x100G     [ 1x400G 1x200G 4x10G 1x40G 4x25G 1x100G 4x100G 2x200G ]   [ ]...
fpc0:pfe> show picd channel summary
0/0/2:0  100G  Chan_Online  Tune_Ok    up   up   up   up    1    8
channel-0/0/3:0  100G  Chan_Online  Tune_Ok    up   up   up   up    1    12
channel-0/0/4:0  10G   Chan_Online  Init_Done   up   down  down  down   1    8
channel-0/0/5:0  10G   Chan_Online  Init_Done   up   down  up   up    1    12
channel-
root@JTAC-fpc0:pfe> show picd pic fpc 0 pic 0
# Optic #
>show interfaces diagnostics optics <interface-name>
For getting detailed information:
request pfe execute command 'show picd xcvr summary' target <target>
request pfe execute command 'show picd xcvr fpc <fpc no> pic <pic no> port <port no> cmd alarm' target <target>
request pfe execute command 'show picd xcvr fpc <fpc no> pic <pic no> port <port no> cmd diagnostics' target <target>
request pfe execute command 'show picd xcvr fpc <fpc no> pic <pic no> port <port no> cmd identifier' target <target>
request pfe execute command 'show picd xcvr fpc <fpc no> pic <pic no> port <port no> cmd info' target <target>
request pfe execute command 'show picd xcvr fpc <fpc no> pic <pic no> port <port no> cmd firmware-info' target <target>
KB73201... ...[MX304] How to check detailed optic diagnostic info and alarm history for SFP and SFP+ Optics
# Serdes #
SerDes（串行器/解串器）是一种将并行数据转换为高速串行信号（或反向转换）的技术，广泛应用于高速接口如PCIe、以太网、SATA等。
root@re0-router1-fpc0:pfe> show picd serdes summary
# QSFP #
QSFP+：支持 40Gbps（4×10Gbps 通道）
QSFP28：支持 100Gbps（4×25Gbps 通道）
Interface
fe-百兆 100M
ge- 千兆 1G/1000M
xe- ...万兆... 10G
et- ...1...0G/40G/100G/200G/400G 可以channelized成多个接口
单位
全称
换算关系
1 ...Tb
1 ...Tb
1 ...T...b = 1000 ...G...b
1 Gb
1 Gigabit
1 Gb = 1000 Mb
1 Mb
1 Megabit
1 Mb = 1000 Kb
1 Kb
1 Kilobit
1 Kb = 1000 bit
1 B
1 Byte
1 B = 8 bit
1 Mbps = ...1,000,000 bps... bit
bps 
pps = 
包 大 小 × 8 ...bps 
pps = 
包 大 小 × 8
Hex:16  / Decimal...：...10 / Oct...：...8
# Command #
star shell pfe network fpcx
show sfp list
show sfp X info <<<<< X -index from previous output, the check the read errors
show chassis pic fpc-slot <> pic-slot <>'. <<< 这个命令能看出sfp vendor 名字PIC port information:                                    Fiber                               Xcvr vendor       Wave-                     Xcvr              JNPR     MSAPort Cable type        type  Xcvr vendor        part number       length                    Firmware      Rev      Version0    GIGE 100FX PHY    MM    EOPTOLINK INC      EOLS13032MDMGJ4   1310 nm                   0.0           REV 01   SFF-8472 ver 11.3
Interface Range
[edit interfaces interface-range range1]user@device# set member et-0/*/*set member et-0/[1-10]/0set member et-0/[1,2,3]/3
Channelize...：
[edit chassis fpc 1 pic 0 port 3]user@host# set number-of-sub-ports 4
et-1/0/3 100 Gbps
four sub-ports are et-1/0/3:0, et-1/0/3:1, et-1/0/3:2, and et-1/0/3:3 with 25G each
Forward Error Correction (FEC)
https://www.juniper.net/documentation/us/en/software/junos/interfaces-fundamentals-evo/interfaces-fundamentals/topics/topic-map/physical-interface-properties.html
Enhances the reliability of the connection
Enables the receiver to correct transmission errors without requiring retransmission of the data
Extends the reach of optics
cli-pfe
root@re0:pfe> show picd config
pic_info_table               :              default   config     config       config   computedpic_or_port   speed     pic_mode   port_speed   valid    speed      supported_speeds                                           hidden_speeds-----------   -------   --------   ----------   ------   --------   --------------------------------------------------------   -------------pic-0/0       -         -          -            yes      -port-0/0/0    1x400G    -          1x100G       -        1x100G     [ 1x400G 1x200G 4x10G 1x40G 4x25G 1x100G 4x100G 2x200G ]   [ ]port-0/0/1    1x400G    -          1x100G       -        1x100G     [ 1x400G 1x200G 4x10G 1x40G 4x25G 1x100G 4x100G 2x200G ]   [ ]port-0/0/2    1x400G    -          1x100G       -        1x100G     [ 1x400G 1x200G 4x10G 1x40G 4x25G 1x100G 4x100G 2x200G ]   [ ]port-0/0/3    1x400G    -          1x100G       -        1x100G     [ 1x400G 1x200G 4x10G 1x40G 4x25G 1x100G 4x100G 2x200G ]   [ ]...
fpc0:pfe> show picd channel summary
0/0/2:0  100G  Chan_Online  Tune_Ok    up   up   up   up    1    8
channel-0/0/3:0  100G  Chan_Online  Tune_Ok    up   up   up   up    1    12
channel-0/0/4:0  10G   Chan_Online  Init_Done   up   down  down  down   1    8
channel-0/0/5:0  10G   Chan_Online  Init_Done   up   down  up   up    1    12
channel-
root@JTAC-fpc0:pfe> show picd pic fpc 0 pic 0
# Optic #
>show interfaces diagnostics optics <interface-name>
For getting detailed information:
request pfe execute command 'show picd xcvr summary' target <target>
request pfe execute command 'show picd xcvr fpc <fpc no> pic <pic no> port <port no> cmd alarm' target <target>
request pfe execute command 'show picd xcvr fpc <fpc no> pic <pic no> port <port no> cmd diagnostics' target <target>
request pfe execute command 'show picd xcvr fpc <fpc no> pic <pic no> port <port no> cmd identifier' target <target>
request pfe execute command 'show picd xcvr fpc <fpc no> pic <pic no> port <port no> cmd info' target <target>
request pfe execute command 'show picd xcvr fpc <fpc no> pic <pic no> port <port no> cmd firmware-info' target <target>
KB73201... ...[MX304] How to check detailed optic diagnostic info and alarm history for SFP and SFP+ Optics
# Serdes #
SerDes（串行器/解串器）是一种将并行数据转换为高速串行信号（或反向转换）的技术，广泛应用于高速接口如PCIe、以太网、SATA等。
3. SerDes 在 MX304 中 的 应 用 场 景 
● 高 速 以 太 网 接 口 : 
MX304 的 100G/400G 端 口 依 赖 SerDes 实 现 物 理 层 (PHY) 的 数 据 串 行 化 , 例 如 通 过 QSFP-DD 或 CFP2 光 
模 块 1 5 。 
● 背 板 通 信 : 
用 于 路 由 引 擎 与 接 口 卡 之 间 的 高 速 数 据 交 换 , 减 少 布 线 复 杂 度 9 。 
● 时 钟 恢 复 : 
通 过 CDR( 时 钟 数 据 恢 复 ) 技 术 从 串 行 数 据 流 中 提 取 时 钟 , 避 免 单 独 时 钟 线 
6 8. ...3. SerDes 在 MX304 中 的 应 用 场 景 
● 高 速 以 太 网 接 口 : 
MX304 的 100G/400G 端 口 依 赖 SerDes 实 现 物 理 层 (PHY) 的 数 据 串 行 化 , 例 如 通 过 QSFP-DD 或 CFP2 光 
模 块 1 5 。 
● 背 板 通 信 : 
用 于 路 由 引 擎 与 接 口 卡 之 间 的 高 速 数 据 交 换 , 减 少 布 线 复 杂 度 9 。 
● 时 钟 恢 复 : 
通 过 CDR( 时 钟 数 据 恢 复 ) 技 术 从 串 行 数 据 流 中 提 取 时 钟 , 避 免 单 独 时 钟 线 
6 8.
root@re0-router1-fpc0:pfe> show picd serdes summary
# QSFP #
QSFP+：支持 40Gbps（4×10Gbps 通道）
QSFP28：支持 100Gbps（4×25Gbps 通道）
MAC layer
Packet comes to MAC layer and then goes to the Pre-classifier engine.
In MAC layer we can check the MAC statistics, port speed and stream number
labroot@jtac-mx304-r2012-re0> show interfaces xe-0/0/5:0 extensive. 
Physical interface: xe-0/0/5:0, Enabled, Physical link is Up 
Interface index: 156, SNMP ifIndex: 2554, Generation: 159 
Link-level type: Flexible-Ethernet, MTU: 1522, MRU: 1530, LAN-PHY mode, Speed: 10Gbps, BPDU Error: None, Loop Detect PDU 
Error: None, 
MAC statistics: 
Receive 
Transmit 
Total octets 
151160702 
3750846 ...labroot@jtac-mx304-r2012-re0> show interfaces xe-0/0/5:0 extensive. 
Physical interface: xe-0/0/5:0, Enabled, Physical link is Up 
Interface index: 156, SNMP ifIndex: 2554, Generation: 159 
Link-level type: Flexible-Ethernet, MTU: 1522, MRU: 1530, LAN-PHY mode, Speed: 10Gbps, BPDU Error: None, Loop Detect PDU 
Error: None, 
MAC statistics: 
Receive 
Transmit 
Total octets 
151160702 
3750846
LNX-FPC0 (vty) # show precl-eng sum >>> with this command we can find precl to ifd map, in our example, PFE 1 PG 1 
ID 
precl_eng name 
FPC PIC ASIC-ID ASIC-INST Port-Group 
(ptr) 
1 MQSS_engine.0.0.112 
0 
0 
112 
0 
0 
ec364bb8 
2 MQSS_engine.0.0.65648 
0 
0 
112 
0 
1 
ed7151c8 
3 MQSS_engine.0.0.131184 
0 
0 
112 
1 
0 
ed724740 
4 MQSS engine.0.0.196720 
0 
0 
112 
1 
1 
ed733cb8 
LNX-FPC0 (vty) # show precl-eng 4 ifd-details 
IFD 
stream 
port eng-bitmap tcam-bitmap primap-index 
== 
xe-0/0/5:2 
4 
4 
0x0000003f 
0x0000 
xe-0/0/5:3 
5 
5 
0x0000003f 
0x0000 
xe-0/0/5:0 
6 
6 
0x0000003f 
0x0000 
xe-0/0/5:1 
7 
7 
0x0000003f 
0x0000 
labroot@jtac-mx304-r2012-re0> show interfaces xe-0/0/5:0 extensive | 
find preclassi 
Preclassifier statistics: 
Traffic Class 
Received Packets 
Transmitted Packets 
Dropped Packets 
real-time 
0 
0 
0 
network-control 
0 
0 
0 
best-effort 
2880 
2880 
0 <<< Our ping packet classified here ...LNX-FPC0 (vty) # show precl-eng sum >>> with this command we can find precl to ifd map, in our example, PFE 1 PG 1 
ID 
precl_eng name 
FPC PIC ASIC-ID ASIC-INST Port-Group 
(ptr) 
1 MQSS_engine.0.0.112 
0 
0 
112 
0 
0 
ec364bb8 
2 MQSS_engine.0.0.65648 
0 
0 
112 
0 
1 
ed7151c8 
3 MQSS_engine.0.0.131184 
0 
0 
112 
1 
0 
ed724740 
4 MQSS engine.0.0.196720 
0 
0 
112 
1 
1 
ed733cb8 
LNX-FPC0 (vty) # show precl-eng 4 ifd-details 
IFD 
stream 
port eng-bitmap tcam-bitmap primap-index 
== 
xe-0/0/5:2 
4 
4 
0x0000003f 
0x0000 
xe-0/0/5:3 
5 
5 
0x0000003f 
0x0000 
xe-0/0/5:0 
6 
6 
0x0000003f 
0x0000 
xe-0/0/5:1 
7 
7 
0x0000003f 
0x0000 
labroot@jtac-mx304-r2012-re0> show interfaces xe-0/0/5:0 extensive | 
find preclassi 
Preclassifier statistics: 
Traffic Class 
Received Packets 
Transmitted Packets 
Dropped Packets 
real-time 
0 
0 
0 
network-control 
0 
0 
0 
best-effort 
2880 
2880 
0 <<< Our ping packet classified here
Preclassifier
进入wan后进行入口分queue
Pre-class engine is usefull when there is mac failure or packet getting dropped because of some issue at mac level, We can check here the tcam entry as well as configured mac and dump that packet to find if have any issue.
# show precl-eng 
<> 
intr-error 
# show precl-eng 
<> configured-my-macs 
# show precl-eng 
<> tcam-port-mapping 
LNX-FPC0 (vty) # show precl-eng 4 configured-my-macs 
Configured My-Mac Addresses 
Port 04 : 20 : 93 : 39 : fd : 38 :fe 
& 
ff : ff : ff : ff : ff :ff 
Port 05: 20 : 93 : 39 : fd: 38: ff 
& 
ff : ff : ff : ff : ff :ff 
Port 06: 20:93:39 : fd: 38: fc 
& 
ff : ff : ff : ff : ff :ff 
Port 07: 20 : 93 : 39 : fd : 38 : fd 
& 
ff : ff : ff : ff : ff :ff 
Configured Special-Mac Addresses 
GMRP : 
01:80:c2:00:00:20 
& 
ff : ff : ff : ff : ff :ff 
GVRP 
01:80:c2:00:00:21 
ff : ff : ff : ff : ff :ff 
.. 
STP 
: 
01:80:c2:00:00:00 
& 
ff : ff : ff : ff : ff : ff 
PVST 
01:00:0c:cc:cc:cd 
& 
ff : ff : ff : ff : ff :ff 
8021g: 
01:80:c2:00:00:02 
& 
ff : ff : ff : ff : ff :ff 
8021x: 
01:80:c2:00:00:03 
& 
ff : ff : ff : ff : ff :ff ...# show precl-eng 
<> 
intr-error 
# show precl-eng 
<> configured-my-macs 
# show precl-eng 
<> tcam-port-mapping 
LNX-FPC0 (vty) # show precl-eng 4 configured-my-macs 
Configured My-Mac Addresses 
Port 04 : 20 : 93 : 39 : fd : 38 :fe 
& 
ff : ff : ff : ff : ff :ff 
Port 05: 20 : 93 : 39 : fd: 38: ff 
& 
ff : ff : ff : ff : ff :ff 
Port 06: 20:93:39 : fd: 38: fc 
& 
ff : ff : ff : ff : ff :ff 
Port 07: 20 : 93 : 39 : fd : 38 : fd 
& 
ff : ff : ff : ff : ff :ff 
Configured Special-Mac Addresses 
GMRP : 
01:80:c2:00:00:20 
& 
ff : ff : ff : ff : ff :ff 
GVRP 
01:80:c2:00:00:21 
ff : ff : ff : ff : ff :ff 
.. 
STP 
: 
01:80:c2:00:00:00 
& 
ff : ff : ff : ff : ff : ff 
PVST 
01:00:0c:cc:cc:cd 
& 
ff : ff : ff : ff : ff :ff 
8021g: 
01:80:c2:00:00:02 
& 
ff : ff : ff : ff : ff :ff 
8021x: 
01:80:c2:00:00:03 
& 
ff : ff : ff : ff : ff :ff
We can check here the tcam entry as well as configured mac and dump that packet to find if have any issue.
root@inet-pue-fuertes-97> show interfaces xe-0/0/1:1 extensive | find preclass
Preclassifier statistics:
Traffic Class        Received Packets   Transmitted  Packets      Dropped Packets
real-time                           0                      0                    0
network-control                     0                      0                    0
best-effort                     10156                  10156                    0
