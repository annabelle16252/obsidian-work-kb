- WLAN primarily operates at Layer 2, but it can also extend across Layer 3 subnets with the support of a core switch or router.

- license
AP Licenses: Required for an AP to come up on a controller.
PEFN (formerly PEF-NG) Licenses: Needed for operational APs using policy enforcement features like firewall rules.
MM (Mobility Management) Licenses: Required starting with AOS-8.0.1.0 to support controllers or APs on Mobility Conductor.
RFP Licenses: Used to enable features like WIPS violations, intrusion detection, and prevention systems.

# AP boot process
1.Power On & Bootloader: The AP powers up, initializes, and loads the operating system (AOS) from flash memory.
2.IP Acquisition: 
The AP sends a DHCP request to obtain an IP address, gateway, and DNS information.
3.Controller Discovery (ADP): The AP attempts to locate a Mobility Controller or Instant Virtual Controller (VC):
— DHCP Option 43/60: The preferred method where the DHCP server provides the controller's IP address.
DHCP Option 43：用于指定AP的控制器或管理服务器的IP地址。
DHCP Option 60：用于传递其他配置信息。
— ADP Multicast/Broadcast: If no DHCP option is found, the AP sends ADP packets to find a controller.
— DNS Lookup: The AP queries for aruba-master.
4.Cloud/Activate Provisioning: 
In modern environments, the AP checks in with Aruba Activate for provisioning rules.
5.Image & Config Download: 
Once a controller is found, the AP downloads the required firmware/configuration, reboots if necessary, and connects.
6.LED Status: A blinking light typically indicates the boot/update process, while a solid color (white/blue) indicates it is active and managed. 
# SSID
SSID 是无线网络的一个标识符，AP（接入点）使用它来让设备识别并连接到特定的无线网络。
SSID 是由接入点（AP）广播的，用于设备识别和连接到特定的无线网络。
不同AP有不同的SSID,一个AP可以配置多个SSID。
设备通过无线网络（Wi-Fi）接收 AP 广播的 SSID。

# UAP
UAP（Unified Access Point）是Aruba网络设备的一种类型，它结合了接入点和控制器的功能。以下是UAP的启动和配置过程：
初始启动：
UAP在电源开启后会初始化并加载操作系统。
获取IP地址：
UAP通过DHCP请求获取一个IP地址、网关和其他必要的网络设置信息。
控制器发现（ADP）：
如果通过DHCP获取到控制器的IP地址，则UAP会尝试连接到该控制器。
如果未找到DHCP选项，UAP将发送ADP包以发现控制器。
配置和初始化：
UAP与控制器建立连接后，控制器会提供必要的配置信息，如SSID、安全设置等。
UAP根据这些配置信息进行初始化，并开始广播其SSID供设备连接。

# AOS 8
![[Pasted image 20260411160130.png]]
MCr： MC conductor, manages MD(MC),  =MM

GRE：AP 到 MC 的用户业务流量隧道
IPsec：MC 到 MCr 的安全隧道
PAPI：AP、MC、MCr 之间的控制平面通信
AMON：发给管理平台的监控与分析数据

为什么mcr和mc之间跑ipsec，ap和mc之间跑gre？
核心原因是“性能路径”和“安全边界”不一样。
AP ↔ MC 用 GRE
主要是做用户流量隧道封装，简单、开销小、吞吐高。
AP 数量大、流量重，优先考虑转发效率。
在园区/内网场景里，这段链路通常被认为是受控网络，所以常用 GRE 承载。
MC ↔ MCR（Mobility Conductor）用 IPsec
这段更偏跨网段/跨数据中心/管理平面互通，需要机密性和完整性。
IPsec 提供加密、认证、防篡改，适合管理与控制相关通信。
MC 到 MCR 的连接规模通常比 AP 到 MC 小很多，可以接受更高加密开销。
一句话总结：
AP 到 MC 追求“高效封装转发”所以 GRE；MC 到 MCR 追求“安全管理互联”所以 IPsec。

AP（硬件）
├── CAP  → 连到 Controller，集中转发（AOS8 典型）Campus AP
├── IAP  → 自组 cluster，不需要单独 Controller（Instant 模式）
└── RAP  → Remote AP，远程分支通过 VPN 回连

“PAPI”是“Proprietary Code Protocol for PAPI”的缩写，这是一个专有协议，用于APs和移动控制器之间的通信。

AMON 指的是 Aruba Monitoring 流量，可以把它理解成 Aruba 控制器发出去的监控/遥测数据流。

AirWave 就是 Aruba 体系里的“运维监控平台”。

# Clustering 
![[Pasted image 20260411170618.png]]
Seamless campus roaming:
用户最开始接入某个 AP 时，被某一台 MC 接管,之后用户走动，连到别的 AP，甚至这个 AP 属于另一台 controller,但这个用户的会话、策略、转发锚点，仍然挂在最开始那台 MC 上. 所以对用户来说，连接不会被打断，这就叫 Seamless Campus Roaming

Roles:
AP Anchor Controller (AAC):
Role: Handles all management traffic functions from a given AP.
Assignment: The active control assigns an AP to an AAC.
User Anchor Controller (UAC):
Role: Handles all client user traffic and creates dynamic tunnels for the AP.
Standby UAC: There is also a standby UAC role, which is similar to the primary UAC but serves as a backup.

**Cluster formation**
Hello Messages:
The nodes exchange “Hello messages” during the cluster formation.
Leader Election:
The node with the highest priority and MAC address is elected as the Cluster Leader.
Full IPSEC:
A fully meshed IPSEC tunnel is formed between each pair of nodes.
VLAN Probing:
The CM (Cluster Manager) process sends a broadcast packet with a special source MAC and a destination MAC set to FF:FF:FF:FF:FF:FF, using one of the VLANs defined on the cluster manager and other controllers.

CM 是 Cluster Manager，是运行在 controller（MC） 上的集群管理进程，负责探测/判断各 controller 在哪些 VLAN/L2 网络上可达，用来建立集群内的数据路径和锚点可达性。
一个MC上可能有多个vlan, 每个MC都会往2层网络里为每个vlan发一个探测包，目的是知道其他MC哪些vlan可达。
![[Pasted image 20260411172936.png]]

**AP Load Balancing**
![[Pasted image 20260411173112.png]]
**Client load balancing**
![[Pasted image 20260411173531.png]]

**AirMatch** 
是一种自动化的信道规划功能，能够动态分配最优信道基于实际的RF条件。它会根据实时无线环境调整功率级别、干扰和客户端负载平衡，并持续监控无线网络。

**ClientMatch**
client match 会不断优化客户端的关联，并通过扫描无线环境来共享AP和客户端之间的动态数据。这有助于根据实时数据调整客户端与AP的连接。

air match 和 client match 在优化无线网络性能方面相互协作。air match 负责信道规划和资源分配，而 client match 则负责客户端关联的优化。两者共同作用，确保网络性能和客户端体验的最优状态。

**VBR**
功能：动态调整数据传输速率，以适应不同的网络条件和客户端需求。
作用：确保在不同网络条件下提供稳定的数据传输性能。

![[Pasted image 20260411182218.png]]
一个 Mesh Cluster 里可以包含一段 linear mesh，也可以包含非线性的分支结构。

# UBT
UBT”（User Anchor Bridge Tunnel）是指用户锚点桥接隧道，用于在用户设备接入网络时建立安全的通信通道。
![[Pasted image 20260411182610.png]]SAC (Switch Anchor Gateway/Controller)：网关或控制器的锚点。
UAC (User Anchor Controller)：用户接入点的锚点。
UBT Tunnel：在 SAC 和 UAC 之间建立的安全隧道，用于维护用户的流量并进行必要的安全检查和策略应用。
![[Pasted image 20260411183759.png]]
# Aruba Central
Greenlake is where we can login to Aruba Central
![[Pasted image 20260411200315.png]]

![[Pasted image 20260411201429.png]]
![[Pasted image 20260411201511.png]]

AOS8:
Client -> AP -> Switch -> MC (起到gateway功能)-> Core/DC
AOS10:
Client -> AP -> Switch -> Gateway -> WAN/Core/DC
Client -> AP -> Gateway cluster -> Core/DC

# AOS10
![[Pasted image 20260412093255.png]]

# DPS
DPS involves inspecting network traffic to classify and manage it based on performance metrics like latency, packet loss, and jitter.
It’s important to note that DPS comes after routing,(when no PBR-policy based routing) so it will use the equal cost paths available

# IDPS
![[Pasted image 20260412100902.png]]
