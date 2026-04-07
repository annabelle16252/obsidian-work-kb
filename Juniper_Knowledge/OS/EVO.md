# EVO

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

EVO
# Command #
> request system debug-info since 3h node all
> show system nodes
> request system debug-info node re0 （takes quite some time）
> request system application restart ? <<< only restart re0
> request system application restart node re0 app ?
Troubleshooting object issue:
> show platform dependency-state summary
> show platform dependency-state node re0 app rpdagent detail
> show platform interfaces if-objects et-0/0/0 app ifmand
EVO commands:
https://junipernetworks.sharepoint.com/teams/CSS/global_support/jtac/Global%20Support%20Switching/docs/EVO/EVO-commands.txt
EVO 没 有 junos 了 , 各 个 deamon 变 成 app 跑 在 linx 上 
A 
MGD 
PPMD 
PFE Software 
P 
A 
Junos CP 
PFE 
1 
P 
VM 
Software 
RPD 
CoS 
Platform 
S 
DCD 
... 
(RPD, CoS, 
Software 
S 
DCD, .... ) 
Platform 
Software 
Distributed State Infrastructure 
Linux 
Linux 
CPU 
CPU 
CPU 
CPU 
CPU 
CPU 
CPU 
CPU 
CPU 
CPU 
CPU 
CPU 
CPU 
... 
... 
· Separate control plane & 
· 
Code in self-contained applications 
platform code 
· High horizontal & vertical scale 
· Rich and extensive APIs 
· Application level HA 
· Fine-grained and pervasive telemetry 
· 
Model driven API's and software ...EVO 没 有 junos 了 , 各 个 deamon 变 成 app 跑 在 linx 上 
A 
MGD 
PPMD 
PFE Software 
P 
A 
Junos CP 
PFE 
1 
P 
VM 
Software 
RPD 
CoS 
Platform 
S 
DCD 
... 
(RPD, CoS, 
Software 
S 
DCD, .... ) 
Platform 
Software 
Distributed State Infrastructure 
Linux 
Linux 
CPU 
CPU 
CPU 
CPU 
CPU 
CPU 
CPU 
CPU 
CPU 
CPU 
CPU 
CPU 
CPU 
... 
... 
· Separate control plane & 
· 
Code in self-contained applications 
platform code 
· High horizontal & vertical scale 
· Rich and extensive APIs 
· Application level HA 
· Fine-grained and pervasive telemetry 
· 
Model driven API's and software
JUNOS 
EVO 
MANAGEMENT 
& APPLICATION 
APP3 
APP2 
APP1 
PLANE 
(ACTIVE) 
(PASIVE) 
(ACTIVE) 
LC 
CONTROL PLANE 
State 
(ROUTING 
ACTIVE 
STANDBY 
APP1 
APP2 
APP3 
ENGINES) 
(ACTIVE) 
(ACTIVE) 
exchange 
(ACTIVE) 
RE 
Infra 
LC 
DATA PLANE 
ACTIVE 
ACTIVE 
ACTIVE 
ACTIVE 
APP3 
APP1 
APP 
(LINE CARDS) 
(ACTIVE) 
(ACTIVE) 
(PASIVE) 
LC 
RE 
任 何 的 app,hw 只 要 连 接 到 infra 上 都 是 node. Infra 可 以 让 app 独 立 于 hw ...JUNOS 
EVO 
MANAGEMENT 
& APPLICATION 
APP3 
APP2 
APP1 
PLANE 
(ACTIVE) 
(PASIVE) 
(ACTIVE) 
LC 
CONTROL PLANE 
State 
(ROUTING 
ACTIVE 
STANDBY 
APP1 
APP2 
APP3 
ENGINES) 
(ACTIVE) 
(ACTIVE) 
exchange 
(ACTIVE) 
RE 
Infra 
LC 
DATA PLANE 
ACTIVE 
ACTIVE 
ACTIVE 
ACTIVE 
APP3 
APP1 
APP 
(LINE CARDS) 
(ACTIVE) 
(ACTIVE) 
(PASIVE) 
LC 
RE 
任 何 的 app,hw 只 要 连 接 到 infra 上 都 是 node. Infra 可 以 让 app 独 立 于 hw
node 
node 
OFP, IPCs, 
Zookeeper 
node 
node 之 间 的 联 系 通 过 这 里 
这 里 连 着 linux, 每 个 node 都 跑 在 linux 上 
node ...node 
node 
OFP, IPCs, 
Zookeeper 
node 
node 之 间 的 联 系 通 过 这 里 
这 里 连 着 linux, 每 个 node 都 跑 在 linux 上 
node
· Object Flooding Protocol (OFP) is used 
to carry the information to various nodes 
Application 
Code 
using Multicast. 
rpd 
Application runtime 
· 
SysEpochMan will help to organize 
EVL App common API layer 
ifmand 
configd 
rpdagent 
various components into a cohesive 
Binding 
Publish 
App Infra 
App Infra 
App Infra 
system and monitor system integrity. It 
queue 
interface 
also runs ZooKeeper. 
EVL runtime managed 
Data store 
Distributor 
· ZooKeeper maintains the database to 
store synchronous updates and 
SysEpochMan 
SysMan 
coordinates various system services 
OFP 
Zookeeper 
(leader election, namespace 
allocation ... etc). 
Linux Kernel ...· Object Flooding Protocol (OFP) is used 
to carry the information to various nodes 
Application 
Code 
using Multicast. 
rpd 
Application runtime 
· 
SysEpochMan will help to organize 
EVL App common API layer 
ifmand 
configd 
rpdagent 
various components into a cohesive 
Binding 
Publish 
App Infra 
App Infra 
App Infra 
system and monitor system integrity. It 
queue 
interface 
also runs ZooKeeper. 
EVL runtime managed 
Data store 
Distributor 
· ZooKeeper maintains the database to 
store synchronous updates and 
SysEpochMan 
SysMan 
coordinates various system services 
OFP 
Zookeeper 
(leader election, namespace 
allocation ... etc). 
Linux Kernel
RPD仍然用rtsocket通过krtqueue发给rpdagent
比如rpd他会subscriber他需要的ospf...，bgp等，他就会consume这些objects
Zookeeper会监视哪个app是object的active/backup producer...，当active... goes down他会让app切换到backup
Junos chassisd -> Evo hwdre / hwdlc
上层控制平面 ↔ DDS ↔ DDX ↔ ASIC
Junos...：...RPD → Kernel → PFE
Evo...：...RIB → AFT → DDS/DDX → ASIC
AFT = Abstract Forwarding Table...，负责将...RIB转化为抽象的FIB,交给DDS/DDX下发给ASIC...，即使...ASIC 或
DDX 重启，转发表的“抽象视图”仍然存在，可快速恢复.AFT = 硬件无关的转发表抽象层
# EVO new Modules #
ARPD
- An application located in the Routing Engine node which is responsible for ARP resolution
process based on the request received.
JTD
- Juniper Tunnel Driver (JTD) module is a kernel loadable module loaded on the Routing Engine
node which is responsible to handle all TTP packets.
FIBD
- Forwarding Information Base Service Daemon (FIBD) is an application consolidating all the
WAN interface Route/Nexthop related objects within the system to construct the forwarding
tables.
EVO-aftmand
The Advanced Forwarding Toolkit (AFT) manager process running on the FPC node which is
responsible for handling Route/Nexthop/Class-of-Service/Firewall related PFE programming
from the PFE side. Each FPC node will have one evo-aftmand process running to manage all
the PFEs within the FPC.
PacketIO
- A process running on the FPC node which is responsible for handling all Hostbound traffic.
Each FPC node will have one instance of PacketIO running.
Lkup ASIC
- Lookup ASIC is a common term to represent the ASIC type on the PFE.
What products/platforms use EVO so far?​
QFX5200, QFX5220, PTX10003, PTX10008, PTX10001, PTX10004 , ACX6000, ACX7100…​
But there is also EVO on linecard/PFE on Junos box, e.g. MPC10/11 (AFT/ULC)
Overview for Junos OS Evolved
https://www.juniper.net/documentation/us/en/software/junos/overview-evo/topics/concept/evo-overview.html
EVO ...compare syslog messages...:
https://apps.juniper.net/syslog-explorer/
Getting Started with Junos OS Evolved
User Access and Authentication Administration Guide for Junos OS Evolved
Network Management and Monitoring Guide—Procedures on SNMP, remote monitoring (RMON), destination class usage (DCU) and source class usage (SCU) data, accounting profiles, and logging.
# EVO Components #
https://www.juniper.net/documentation/us/en/software/junos/overview-evo/topics/concept/evo-software-components-and-processes.html
A Junos OS Evolved system is comprised of one or more Linux nodes, coupled together with an efficient communications substrate, and supplied with a distributed application launcher.
A horizontal software layer decouples application processes from the specific hardware node where they can be run.
Applications use the Distributed Data Store (DDS) to share state, and state is synchronized between nodes.
Node
user@host-re0> show system nodesNode: fpc0  Node Id    : 2201170739216  Node Nonce : 2632845278  Status     : online, apps-ready  Attributes : ASICS (Active), BT (Active), FABRIC_PFE (Active), FPC (Active), PIC (Active), TIMINGD_FPC (Active), MSVCSD (Active)
Node: re0  Node Id    : 2201170739204  Node Nonce : 1829978227  Status     : online, apps-ready  Attributes : FABRIC_CONTROL (Active), FABRIC_FCHIP_PARALLEL (Active), RE (Active), TIMINGD_RE (Active), MasterRE (Active), GlobalIPOwner (Active)
Node: re1                                                                                                                 Node Id    : 2201170739205  Node Nonce : 3166228206  Status     : online, apps-ready  Attributes : FABRIC_CONTROL (Spare), FABRIC_FCHIP_PARALLEL (Spare), RE (Spare), TIMINGD_RE (Spare), BackupRE (Active)
Application
evo pfemand application : PFE Manager Daemon
> ...show system applications
On ACX7000 series of routers, when an application restarts for more than three times in a five-minute window, then the application and its dependent applications goes into a failure state.
> restart app-name
Linux Kernel
Junos OS Evolved is built on top of a stock Linux kernel. Functionality performed by the router like configuration management, interface management and routing are processes that run as Linux processes. All applications run natively on the Linux kernel, including Juniper and non-Juniper applications.
Initialization Process(init)
System Epoch Management Process(SysEpochMan)
The system epoch management process (SysEpochMan) is responsible for organizing the various Linux nodes into a cohesive system, and to monitor the system to ensure integrity if any nodes fail. If the system needs to be restarted, SysEpochMan ensures a clean transition from the previous system state to the new system state.
System Manager Process(SysMan)
responsible for the launching, coordination, and monitoring of applications in Junos OS Evolved.
Management Process(mgd)
Routing Protocol Process(rpd)
Interface Process(Ifmand)
Ifmand creates all the operational state related to interfaces (IFD, IFL, IFF, IFA) as well as the necessary interface specific routes and nexthops.
Distributor Process(DDS)
SNMP and MIB II Processes(SNMP)
ZooKeeper Process
The ZooKeeper process is a synchronous transport service that helps in the election of active services, locks resources to avoid data inconsistency, and allocates resources like IP addresses.
EVO将任务分配到多个专用CPU核心，是硬件级别的，而非LINUX的虚拟化技术。
# Logs #
vmhost log
○ 记 录 底 层 Linux VM Host ( 虚 拟 机 主 机 ) 的 运 行 状 态 , 包 括 : 
■ 虚 拟 化 平 台 ( 如 KVM) 的 启 动 、 异 常 、 资 源 分 配 (CPU/ 内 存 / 磁 盘 ) 。 
■ 虚 拟 机 ( 如 vMX/vRR 等 虚 拟 实 例 ) 的 生 命 周 期 管 理 事 件 。 
■ 硬 件 驱 动 、 Hypervisor 或 宿 主 机 的 系 统 级 错 误 ( 如 PCle 设 备 初 始 化 失 败 ) 。 ...○ 记 录 底 层 Linux VM Host ( 虚 拟 机 主 机 ) 的 运 行 状 态 , 包 括 : 
■ 虚 拟 化 平 台 ( 如 KVM) 的 启 动 、 异 常 、 资 源 分 配 (CPU/ 内 存 / 磁 盘 ) 。 
■ 虚 拟 机 ( 如 vMX/vRR 等 虚 拟 实 例 ) 的 生 命 周 期 管 理 事 件 。 
■ 硬 件 驱 动 、 Hypervisor 或 宿 主 机 的 系 统 级 错 误 ( 如 PCle 设 备 初 始 化 失 败 ) 。
> ...show vmhost log messages  # 查看系统消息日志...,...存放于 /var/log/vmhost/
Node log
关于junos OS node的log，位于 /var/log/
# VMHOST #
Enter into VMhost
# vhclient -s
# DDS #
The DDS does not interpret state. Its only job is to hold state received from subscribers and propagate state to consumers. ... ...Applications can be individually restarted without requiring a system reboot. Restarted applications reload the state that is preserved in the DDS.
Figure 1: Publish-Subscribe Model 
Forwarding 
Plane 
Management 
Routing 
(MGD) 
(RPD) 
Platform 
Linux Tools 
DDS 
Linux 
Hardware - Juniper Networks or 3rd Party 
g200501 ...Figure 1: Publish-Subscribe Model 
Forwarding 
Plane 
Management 
Routing 
(MGD) 
(RPD) 
Platform 
Linux Tools 
DDS 
Linux 
Hardware - Juniper Networks or 3rd Party 
g200501
> ...show system services dds
# Object #
Producer is application and it produce object. Object ID is unique
app之间通信都是靠object的传递
# DDX #
APP通信...，主要通过...DDS的object...，但也可以直接通过...DDX通信
DDX is a point-to-point communication between two Apps on on-demand purpose.
# ...Secure Boot... #
EVO中的...一项 硬件级安全功能，用于确保设备启动过程中所有软件组件的完整性和真实性，防止恶意固件或未授权代码的执行。
Figure 2: Secure Boot Model 
BIOS checks 
Loader checks 
Loader Signature 
before handoff 
Kernel Signatures 
before handoff 
Start 
Run 
BIOS 
Loader 
Kernel 
Core Root of Trust 
g200108 ...Figure 2: Secure Boot Model 
BIOS checks 
Loader checks 
Loader Signature 
before handoff 
Kernel Signatures 
before handoff 
Start 
Run 
BIOS 
Loader 
Kernel 
Core Root of Trust 
g200108
# set system secure-boot enforce
> sh...ow system secure-boot status
# System Nodes #
Junos OS Evolved uses a node-based model, where system refers to all nodes, including Routing Engines, Flexible PIC Concentrators (FPCs), and more.
> sh...ow system nodes
> ...show node ( reboot | statistics ) node-name
> ...show system applications <node node-name>
> ...show system core-dumps <node node-name>
> ...show system errors active... ...— instead of the show chassis errors active
> sh...ow system processes <node node-name> <detail> — Display process information for all nodes or a specific node.
> sh...ow system storage node ( re0 | re1 | fpc0 | fpc1 | ...)
> ...show version node ( all | node-name )
> ...request node ( halt | offline | online | power-off/on | reboot ) node-name
> ...request system reboot — In Junos OS Evolved this command will reboot all nodes.
Every Junos OS Evolved process runs as an application...,...many high availability features are per-application rather than per-node.
> ...show system applications <node node-name>
> ...restart process
— In Junos OS this command restarts a specific process. In Junos OS Evolved the same command restarts a specific application (process) on the same node from which the command is issued.
> ...request system application restart app application node node
— This command is specific to Junos OS Evolved and restarts a specific application on a specific node.
# Shell Commands #
sync... - ...Synchronize the Routing Engines
reboot... - ...Reboot the current Routing Engine.
/sbin/upgrade /var/tmp/iso - Upgrade the current Routing Engine using the specified .iso file.
<<< 上面这三条只有CLI不能用的时候才使用
vssh node-name - Open a SSH session to the remote node from the Routing and Control Board.
chvrf iri network-command - Creates the network context required to access the control plane and reach other nodes.
systemctl enable --now docker.service... - ...Enable automatic startup for the Docker container service
who... - ...Displays a list of users logged into the device. Hostname is displayed for users connected via telent and IP address is displayed for users connected via SSH.
ecmp-tracer --interface & - Displays the forwarding packets going through an interface.
# Central Resiliency Entity (jResil) #
jResil is a software function in Junos OS Evolved that acts as a ...watchdog/monitoring/exception aggregation... service.
When it detects an error or threshold (e.g., active disk write rate exceeded) it logs an entry, possibly triggering further action (switchover of routing engine, fault escalation) depending on the device/platform configuration.
# Software Installation #
https://www.juniper.net/documentation/us/en/software/junos/junos-install-upgrade-evo/topics/concept/software-install-and-upgrade-overview-evo.html
> request system software rollback
> request system software rollback image-name with-old-snapshot-config
insert new RE with same version as Primary RE:
new RE joins system and auto sync configuration
insert new RE with different version as Primary RE:
user@host-re0> show system alarms 
2 alarms currently active 
Alarm time 
Class 
Description 
2021-04-19 16:02:26 PDT Major 
Re1 Node unreachable 
2021-04-19 16:04:46 PDT Major Software Version Mismatch on re1: junos-evo-install-ptx-x86-64-20.4R2.6-EVO ...user@host-re0> show system alarms 
2 alarms currently active 
Alarm time 
Class 
Description 
2021-04-19 16:02:26 PDT Major 
Re1 Node unreachable 
2021-04-19 16:04:46 PDT Major Software Version Mismatch on re1: junos-evo-install-ptx-x86-64-20.4R2.6-EVO
enable auto-sw-sync  statement at the [edit system] hierarchy
or
request system software sync all-versions
Partition
EVO default use ...MBR... partition, from ...24.2R1... migrate to ...GPT....
once you migrate a disk to GPT disk partitioning, it continues to use GPT disk partitioning, even if you install older software and reboot the system.
Backup
> ...request system snapshot
the system backs up the /root file system and the /config file system to the secondary solid-state drive (SSD).
> show system snapshot
> request node reboot re0 disk2
junos-evo-install*
junos-evo-install-media*
request system firmware upgrade
Each time you issue the request system software add operational mode command, the previous software image and configuration is preserved automatically. Each version of the software is stored in a distinct area in the /soft directory
In EVO, y...ou upgrade both Routing Engines using a single command issued on the primary Routing Engine.
user@host> file copy scp://filename /var/tmp/filename
> show system software list> show system nodes
Status     : online, apps-ready
If the status is Status : offline
> request node online re1
# delete system node offline re1
> request chassis cb slot 1 offline
> request chassis cb slot 1 online
If system alarms operational mode command shows that the software versions do not match (Software Version Mismatch on re1:package-name)
> request system software sync all-versions
> request system software add /var/tmp/ptx.iso
> request system reboot
Recover from a Failed Installation Attempt If the CLI Is Working
> request system software rollback with-old-snapshot-config
For early initialization failures, use the software stored on the inactive solid-state drive (SSD) to repair the software on the active SSD of the affected Routing Engine
> request node reboot re0 disk2
> request system snapshot
Rollback
After an upgrade, if the installation fails during early boot, the Routing Engine automatically reverts to booting from the secondary SSD, where snapshots are stored. You can then reboot the Routing Engine using the snapshot saved on the secondary SSD.
Rescue
https://www.juniper.net/documentation/us/en/software/junos/junos-install-upgrade-evo/topics/topic-map/evo-backup-recover-configuration.html
During a successful upgrade, the upgrade package completely re-installs the existing operating system. It retains the juniper.conf, rescue.conf, SNMP ifIndexes, /var/home, /config/scripts, SSH files, and other filesystem files. Other information is removed. Therefore, you should back up your current configuration in case you need to return to the current software installation after running the installation program.
> request system configuration rescue save
> test configuration /config/rescue.conf.gz
# rollback rescue
Unified ISSU
https://www.juniper.net/documentation/us/en/software/junos/junos-install-upgrade-evo/topics/topic-map/issu-for-junos-os-evolved.html
system restarts the upgraded software (kernel and applications) without reinitializing the underlying hardware
> ...show system software upgrade-mode
Application restart... <<< 0 ...traffic loss
In-service kernel warm restart... <<< ...minimizes traffic loss
System reboot
If BGP configured, graceful restart >= 300 seconds needs to be configured to apply Unified ISSU:
# set routing-options graceful-restart
# set protocols bgp graceful-restart restart-time 300
Zeroize
reverts the system to the factory-default configuration
> request system zeroize
# GRES #
platforms running Junos OS Evolved, GRES is enabled by default and cannot be disabled.
When you insert a new RE (routing engine) with a different software version from the currently active one on the Primary RE, the newly-inserted RE is kept out of the system.
Solutions:
Method 1....Manually sync-up the software between REs every time RE is replaced:
user@ptx10008-re0> request system software sync ?
Method 2....# set system auto-sw-sync enable
# ZTP #
https://www.juniper.net/documentation/us/en/software/junos/junos-install-upgrade-evo/junos-install-upgrade/topics/topic-map/zero-touch-provision.html
When you physically connect a device to the network and boot it with a default factory configuration, the device upgrades (or downgrades) the software release and autoinstalls a configuration file from the network. The configuration file can be a configuration or a script.
DHCP
Use ...management interface of Routing Engine 0 (RE0) or over WAN interfaces
Workflow
If the first line contains the characters #! followed by an interpreter path, the operating system treats the file as a script and executes it with the specified interpreter.
读 作 "Shebang" 或 "Hashbang" 
· 条 件 : 如 果 文 件 的 第 一 行 以 #! 开 头 , 并 紧 跟 一 个 解 释 器 的 路 径 ( 如 /bin/bash) 。 
● 系 统 行 为 : 操 作 系 统 会 将 该 文 件 视 为 可 执 行 脚 本 , 并 自 动 用 指 定 的 解 释 器 运 行 它 。 
○ 例 如 , 若 脚 本 文 件 
test.sh 
内 容 如 下 : 
bash 
#!/bin/bash 
echo "Hello World" 
当 你 在 终 端 执 行 
./test.sh 时 , 系 统 会 实 际 调 用 /bin/bash ./test.sh 
来 运 行 脚 本 。 ...读 作 "Shebang" 或 "Hashbang" 
· 条 件 : 如 果 文 件 的 第 一 行 以 #! 开 头 , 并 紧 跟 一 个 解 释 器 的 路 径 ( 如 /bin/bash) 。 
● 系 统 行 为 : 操 作 系 统 会 将 该 文 件 视 为 可 执 行 脚 本 , 并 自 动 用 指 定 的 解 释 器 运 行 它 。 
○ 例 如 , 若 脚 本 文 件 
test.sh 
内 容 如 下 : 
bash 
#!/bin/bash 
echo "Hello World" 
当 你 在 终 端 执 行 
./test.sh 时 , 系 统 会 实 际 调 用 /bin/bash ./test.sh 
来 运 行 脚 本 。
Table 1: Scripts Supported During ZTP 
Script Type 
Interpreter Path 
Platform Support 
Shell script 
#!/bin/sh 
All devices 
SLAX script 
#!/usr/libexec/ui/cscript 
All devices 
Python script 
#!/usr/bin/python 
Devices running Junos OS with Enhanced Automation 
Devices running Junos OS Evolved ...Table 1: Scripts Supported During ZTP 
Script Type 
Interpreter Path 
Platform Support 
Shell script 
#!/bin/sh 
All devices 
SLAX script 
#!/usr/libexec/ui/cscript 
All devices 
Python script 
#!/usr/bin/python 
Devices running Junos OS with Enhanced Automation 
Devices running Junos OS Evolved
he base DHCP packet contains the IPv4 address of the management or WAN interface.
DHCP option 43 (vendor-specific options)... <<< ipv4
DHCP option 17 (vendor-specific options)... <<< ipv6
SZTP
DHCP-based legacy ZTP is disabled. We do not support DHCP-based legacy ZTP on hardware that supports SZTP.
# Software Management #
Junos OS Evolved stores software images in the /softdirectory.
When you reboot the system after a software package installation, all the Routing Engines and FPCs in the system run the new version of the software.
Junos OS Evolved ensures that all Routing Engines and FPCs in the system are running the same software version. When you install a software image on the primary Routing Engine, the system installs the new version of software on both Routing Engines, if the Routing Engines are online and part of the system. If you insert a Routing Engine that has a different software version into the system and you have not configured the system auto-sw-sync enable statement, the Routing Engine is kept outside the system, and the system generates a software mismatch alarm.
> show system software list
— On Junos OS Evolved, view the currently installed images on each node.
Ç...request system software delete
> request system snapshot
> request system software rollback reboot <package-name> <with-old-snapshot-config>
> request system software sync ( all-versions | current | rollback )
> ...set system auto-sw-sync enable
# Management Interface #
Management interfaces in Junos OS Evolved do not use the same names as Junos OS (fxp0, em0, me0). Instead, the Junos OS Evolved management interface name format is device-name:type-port. For example: re0:mgmt-0, re0:mgmt-1, re1:mgmt-0, re1:mgmt-1.
set system host-name my-host ->  my-host-re0
set system host-name %s_my_host ->  re0_my-host
# Firewall #
Junos OS, filters applied to the loopback interface apply to both network control traffic and management traffic.
Junos OS Evolved...:
filters applied to the loopback interface apply only to network control traffic.
filters applied... ...to the management interface... ...t...o... management traffic.
# Logs #
relay-eventd -> master-eventd
no messages file on the backup RE, all in master RE.
set system syslog alternate-format
— ch...anges the format of the Junos OS Evolved system log messages.
# Trace #
All running applications on Junos OS Evolved create trace information at the info level by default
trace-relay -> trace-writer -> ... ...write... to file
In Junos OS Evolved, you do not view trace files directly, and you should never add, edit, or remove trace files under the /var/log/traces directory because this can corrupt the traces. Instead, you use the ...> ...show trace application application-name node node-name command to read and decode trace messages stored in the trace files.
# LAG #
For Junos OS Evolved, if you add a new member interface to the aggregated Ethernet bundle, a link flap event is generated. The physical interface is deleted as a regular interface and then added back as a member. During this time, the details of the physical interface are lost.
# TPA #
因为EVO是distributed nodes，所以当application产生error时，可能无法判断error会被分到哪个node上。
If you configure this feature, during route installations the consumer notifies the producing application when there are errors in processing the state update sent by the producer. The producer then attaches a TPA object on top of the errored object with details of the error and publishes it.
使用 gRPC + JTI（Juniper Telemetry Interface） 实时推送
Configure FIP streaming on the client device：
set system fib-streamingset system services extension-service request-response grpc max-connections numberset system services extension-service request-response grpc skip-authenticationset system services extension-service notification allow-clients address ip-addressset system services extension-service request-response grpc clear-text port port-number
On the collector, subscribe to the xpath /state/system/anomalies/fib/ to get both the IPv4 and IPv6 error routes.
CLI Commands for Viewing Error Details：
show system applications
show fib-streaming
show agent sensors
# EVO Directory #
https://www.juniper.net/documentation/us/en/software/junos/overview-evo/topics/concept/evo-software-default-file-storage-directories.html
/boot
/data
/config，/etc，/var，/var_db，/var_db_scripts，/var_etc，/var_pfe，/var_rundb
/config
juniper.conf, juniper.conf.1, juniper.conf.2,juniper.conf.3
/config/scripts
/soft—This directory is the software install area. All software versions are installed here.
/u—This directory is a read-only file system for the running version of Junos OS Evolved.
/var
/home，/db/config，/log，/core，/tmp
# Behavioral Differences Between Junos OS Evolved and Junos OS #
https://www.juniper.net/documentation/us/en/software/junos/overview-evo/topics/concept/evo-how-it-differs-from-junos.html
Behavioral Differences Between Junos OS Evolved and Junos OS
New CLI Statements and Commands (Junos OS Evolved)
Modified CLI Statements and Commands (Junos OS Evolved)
Changed CLI Command Output (Junos OS Evolved)
Removed CLI Statements and Commands (Junos OS Evolved)
Item
EVO
JUNOS
Access and Authentication
Runs on
Linux
FreeBSD
Retain state
Yes，publish-subscribe
No
Secure Boot
硬件→固件→内核→微服务全链
仅内核签名
System
node-based model
Routing Engine centric model
lo0 filter
need to have seperate filter for both lo0 &management interface
lo0 filter all traffic to RE
Authentication
no configuration needed, will use pw authentication when radius authentication fails
need to configure pw authentication
edit system login retry-options
EVO doesnt support some options under this
supported
Interfaces
Management interfaces
device-name:type-port
re0:mgmt-0/re0:mgmt-1
fxp0, em0, me0
ifl for LAG
no-vlan：ge-0/0/0.0 自动生成.0的子接口
vlan-tagged：ae0.100 只产生vlan的子接口
都会自动产生ifl
add an interface to a LAG
interface 会先被delete，down up然后再加回来
不会被delete，也不会down
member limit for LAG
ae no limit，only depends on platform
64
LAG configuration优先
ae的配置会覆盖ifd的配置
互不影响，但最终会被ae的配置覆盖
可以commit duplicate IP
No
Yes
FEC
FEC91
FEC74
High Availability
GRES
PTX10004 & PTX10008 enabled by default and cannot be disabled
need to be configured
> show system switchover
Object database and Applications' ready state
Kernel database
QFX5220-32CD switches ISSU
> request system software add restart
> request system software in-service-upgrade
Junos XML API and Scripting
jcs:open extension function in SLAX or XSLT scripts
password-less login
supports both a supplied password and interactive password,
eventd
if there are duplicate event policies. Instead eventd accepts the event policy on a first-come, first-served basis.
The eventd process gives a warning message if you try to create duplicate event policies.
For op scripts run with the max-datasize configuration statement
error is "Out of memory."
error is "Memory allocation failed."
sysctl() extension function in a script and request an invalid sysctl variable name
error: No such file or directory error.
Junos OS does not generate any error.
trace data
Junos OS Evolved collects the trace data for all scripts in trace files that correspond to the cscript application instead of in separate log files.
Junos OS stores the trace data for each type of script in a different log file in the /var/log directory.
Messaging
log message location
The messages file located under /var/log is only written on the primary Routing Engine. Backup Routing Engine messages are found in the messages file on the primary Routing Engine.
on both REs
log message name
appends the node name to the hostname in system log messages.
[edit system syslog] hierarchy level to attach the node name to the process name instead of the hostname.
No
sending syslog messages to a remote host
You do not need to configure the mgmt_junos routing instance at the [edit system syslog host ip-address routing-instance] hierarchy.
Configure the mgmt_junos routing instance at the [edit system syslog host ip-address routing-instance] hierarchy
a regular expression returns empty pattern matches,
there is no error message.
error: regex error: empty (sub)expression
/var/log/inventory
does not support a /var/log/inventory file.
use > show chassis hardware operational mode command to display hardware inventory.
Yes
Routing Policy and Firewall Filters
show firewall filter ?
flowspec filters are not listed.
To see the names of the configured Flowspec filters, use > show firewall application routing command.
flowspec filters is listed
a term of fitler failed to commit
In Junos OS Evolved, if a match action term on your filter configuration fails on commit, the entire filter is not applied.
the remainder of the filter is applied.
Pv6 filter with packet length matching
doesnt include ipv6 header size.
set firewall family inet6 filter filter-name term term-name from packet-length packet-length
includes the IPv6 header size.
filter with icmp match conditions
only supports a single icmp-type value along with an icmp-code value.
multiple icmp-type values only when an icmp-code value is not specified
multiple icmp-type values and an icmp-code value
Software Installation and Upgrade
software install
Multiple releases of the software can be installed on the device simultaneously as long as there is space.
Only two versions
snapshot
copies all of these files onto an alternate solid-state drive.
the software and configuration and saves it to the /packages/sets directory.
request system storage cleanup
does not remove Junos OS Evolved images from the device after Release 20.1R1
removes all Junos OS images
validation of a software upgrade
installs the image in a temporary storage location
in a standard storage location.
return to the original software before newly installed image reboot
request system software delete package-name command to remove the newly added package and cancel the installation.
request system software rollback
ZTP process log message
/var/log/ztp.log
spread out over several system log files
ZTP interface
supports WAN interfaces as well as the management interface
only supports management interfaces.
ZTP fails to donwload file
ZTP retries on other interfaces
If the download fails, ZTP stops.
unsigned script
accepts unsigned scripts in DHCP option 43, suboption 1.
unsigned scripts in DHCP option 43, suboption 1; otherwise, scripts must be signed.
DHCP suboption
only suboption 5
suboption 5 & suboption 8
ZTP default route
does not change the default route.
a DHCP client gets selected for ZTP activity
System Management
hostname
hostnames cannot be configured with an underscore or special characters
can be any combination
request system reboot
reboots the entire system (all nodes) at once
only this RE
after reboot，initializes time from
from the hardware clock
system issues an ntpdate request
Troubleshooting
trace data
trace data from all applications on all nodes is collected on the Routing Engine.
Configure traceoptions to enable trace logging
coredump
early bootup is stored in /var/core/re
stored in /var/core/re0 or /var/core/re1
in /var/crash or /var/tmp
User Interface
virtual-memory-mapping
does not support the virtual-memory-mapping option
supported
show system reboot
for the system and not for a specific Routing Engine
for a specific Routing Engine
set system switchover-on-routing-crash
only rpd crash will trigger switchover，kill rpd or restart RE will not trigger switchover
NSR + rpd crash will trigger swo
set system processes routing failover other-routing-engine
In Junos OS Evolved, when set system processes routing failover other-routing-engine is configured, repeating commands like restart routing and restart routing immediatelywithin the CLI will not cause a Routing Engine mastership switchover when entered more than 4 times in 30 seconds.
Junos OS triggers a switchover when edit system processes routing failover other-routing-engine is configured and certain commands such as restart routingand restart routing immediately are used many times in short succession.
menu used for root password
GRUB menu
Junos Main Menu
firmware information
The firmware information is cached，the fault is visible with the commands show chassis alarms, show chassis fpc, and so on.
When the FRU is offline, the cached firmware information of the FRU is not available to view
Removed
[edit forwarding-options analyzer]
The analyzer application for port mirroring is not supported on Junos OS Evolved.
[edit forwarding-options enhanced-hash-key ecmp-dlb ether-type]
[edit forwarding-options enhanced-hash-key lag-dlb ether-type]
On QFX5130 and QFX5700 devices, ether-type is not supported on Junos OS Evolved
[edit system services extension-service notification]
Junos OS Evolved does not support the notification service for JET applications.
[set chassis fabric degraded action-on-non-blackhole-degradation percentage]
[set chassis fabric degraded action-on-per-plane-fpc-degradation percentage]
These commands are replaced by [set chassis fabric event reachability-fault degraded error-threshold percentage].
gigether-options
To configure link aggregation groups (LAG), use the set interfaces interface-nameether-options command instead.
request chassis beacon service-node
removed
request system core-dump
removed
request system recover
removed
request system scripts (delete | rollback)
AI-Scripts and Service Now are not supported on Junos OS Evolved.
request system software abort
This command is removed because the request system software add command has a built-in feature not to start an upgrade if a reboot is pending after an upgrade or rollback.
request system software (add | delete) set
Junos OS Evolved bundles all packages into one single ISO file, so the set option serves no purpose in the request system software add and request system software delete commands.
request system software in-service-upgrade
Use the request system software add restart command for ISSU. The request system software add command has a built-in feature not to start upgrade if a reboot is pending after an upgrade or rollback.
request system software set
To set the current system to an installed software version, use the request system software rollback reboot command.
request system storage user-disk
There are no satellite packages in Junos OS Evolved.
show bridge
replaced by the command show vlan
show chassis fabric unreachability
show system errors command for similar functionality
show chassis memory-usage-chassisd
The functionality for this command and all options under this command are moved to show system memory.
show chassis network-services
This command is not supported.
show chassis routing-engine errors
This command has been replaced by show system errors in Junos OS Evolved.
show class-of-service forwarding-table
The removed options include classifier, classifier mapping, drop-profile, policer, rewrite-rule, rewrite-rule mapping, scheduler-map, and shaper.
show database-replication
This command is not supported.
show firewall family inet filter filter-name term term-name then traffic-class-count
The traffic-class-count option is not supported under the firewall hierarchy in Junos OS Evolved.
show interfaces mac-database
This command is not supported.
show interfaces mc-ae
This command has been replaced with show multi-chassis mc-lag.
show system buffers
This command is removed starting in Junos OS Evolved Releases 21.1R1 and 20.3R2. This command is not applicable in Junos OS Evolved because the command displays the status of kernel mbufs, which are not used in Linux-based systems like Junos OS Evolved.
show system software detail
Use show system software list to display a list of the software versions installed on all nodes. For more details about the software, use show version detail.
show system uptime invoke-on
removed
traceoptions
Junos OS Evolved removes the traceoptions option at many hierarchy levels because trace messages are now logged, viewed, and configured per application. However, some routing protocols (the [edit protocols] hierarchy level) and a few other applications still use traceoptions.
