# NAT

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

NAT
# Commands #
Telnet to Service PIC...：
> start shell
% telnet -Ji fpc3.pic0
Console to Service PIC...：
Collect log from SPC3 PIC
> start shell user root
Password:
root@nantucket-re0:/var/home/labroot # cty -n fpc3 pic0
root@nantucket-re0-fpc3:~# cd /var/log/
root@nantucket-re0-fpc3:~#tar -zcf ./var-log-fpc3_pic0.tgz /var/log/*
https://juniper.lightning.force.com/lightning/r/Knowledge__kav/ka0Dp000000ivFvIAI/view
Service PIC live coredump:
In urgent situation, you may need to reboot SPIC to cover service,
but pls generate a coredump before that,it only takes no more than 3 minutes.
chrisli@MX480-c-re0> start shell
% telnet -Ji fpc3.pic0
SDPC PIC platform (1133Mhz XLR processor, 3968MB memory, 0KB flash)
sp30(vty)# write coredump
> show services nat pool xxx detail
Interface: mams-0/0/0 (ams0), Service set: IPV4_AMS0
NAT pool: AMS0_NAT44, Translation type: DYNAMIC NAT44
Address range: 1.128.128.1-1.128.133.85
Address range: 1.128.255.249-1.128.255.254
Available addresses: 0
Out of address errors: 784552, Addresses in use: 1371
> show services nat mappings summary
Service Interface:                                          ms-2/0/0
Total number of address mappings:                           1853
Total number of endpoint independent port mappings:         531054
Total number of endpoint independent filters:               534237
https://juniper.lightning.force.com/lightning/r/Knowledge__kav/ka0Dp000000izhnIAA/view
show services stateful-firewall flows
show services stateful-firewall flow-analysis
show services stateful-firewall conversations
show services stateful-firewall statistics
show services stateful-firewall statistics extensive
show services service-sets statistics packet-drops
show services service-sets cpu-usage
show services service-sets memory-usage
show services service-sets summary
show services service-sets statistics syslog
show services nat mappings
show services softwire <count/statistics>
show interfaces redundancy
request interface revert <rsp_name>
show log messages
Restart SPD: restart adaptive-services
> show services sessions source-prefix 100.86.200.1
> show services nat source mappings address-pooling-paired private 100.86.200.1/32
> ...show services sessions destination-prefix 220.239.219.33
> ...show services sessions utilization
Session  %Memory  Setup   %Rate   Drop     Teardown   %CPU
Interface  Count             Rate            Rate            Rate
ms-2/0/0    1                    6.93              0                   0            1.30 Green
vms-11/0/0 10991 29.46 0.03 88                 0           81.68 Green
> ...show services session source-prefix <subscriber-ip>...
这个source... ...session... landing到哪些service pic上
> show services sessions count service-set IPV6_NAT64
mams-0/0/0 IPV6_NAT64 51782
mams-0/1/0 IPV6_NAT64 51292
mams-0/2/0 IPV6_NAT64 46048
mams-0/3/0 IPV6_NAT64 62242
mams-1/0/0 IPV6_NAT64 53434
mams-1/1/0 IPV6_NAT64 53291
mams-1/2/0 IPV6_NAT64 43709
mams-1/3/0 IPV6_NAT64 0                            <<<<<<< service sessions count showed zero here
mams-5/0/0 IPV6_NAT64 48147
% telnet -Ji fpc3.pic0
root@ms13% mspdbg-cli -ps
MSPMAND-CLI> show msp pkt-cntrs terse
Packet Statistics:
Fifo Rcv : 1638981407017
Fifo Send : 1575460743276
Reinject count : 6201388240
No Svc Set : 65266992736 <<<<< this statistic indicates that the packet hit the MS interface that has NO programmed Service-Set
Pkts Copy-Held by Plugins : 57003896
MSPMAND-CLI> show msp service-sets
> show interfaces load-balancing detail ...Load-balancing interfaces detailInterface        : ams1         State          : Up                 Last change    : 1w2d 00:17     Member count   : 4              HA Model       : None           Members        :      Interface    Weight   State      mams-2/1/0   10       Inactive      mams-3/1/0   10       Inactive      mams-8/1/0   10       Active       mams-9/1/0   10       Inactive
filter
https://juniper.lightning.force.com/lightning/r/Knowledge__kav/ka0Dp000000jHgTIAU/view
set firewall filter TEST-FILTER term 1 from source-address <private-source-ip/32>
set firewall filter TEST-FILTER term 1 from destination-address <destination-ip/32>
set firewall filter TEST-FILTER term 1 then count INSIDE-TRAFFIC-Counter
set firewall filter TEST-FILTER term 1 then syslog
set firewall filter TEST-FILTER term 1 then accept
set firewall filter TEST-FILTER term 2 then accept
set interfaces <downstream-interface> unit <unit-id> family inet filter output TEST-FILTER
set interfaces ams0.(inside-unit) family inet filter output TEST-FILTER
If traffic is going to service card and session are not getting created, check for any exceptions and take Packet via dmem and TTRACE capture on the ingress interface FPC.
https://juniper.lightning.force.com/lightning/r/Knowledge__kav/ka0Dp000000j6Y3IAI/view
"modifying" a NAT pool include changes such as pool name, addresses range and port configuration.
It is recommended to first deactivate the service-set, make the required CGNAT changes, then activate the service-set.​
If we don't deactivate services-set before changes, there could be unexpected results in some instances leading to line card crash.
The aggregated multiservices (AMS) interface configuration in Junos OS enables you to combine services interfaces(MAMS) from multiple PICs to create a bundle of interfaces that can function as a single interface.
# NG Service #
https://www.juniper.net/documentation/us/en/software/junos/interfaces-next-gen-services/topics/concept/usf-overview.html
Services can be categorized into Adaptive Services and Next Gen Services
Adaptive Services Interfaces User Guide for Routing Devices
https://www.juniper.net/documentation/us/en/software/junos/interfaces-adaptive-services/topics/concept/adaptive-services-overview.html
# Configuration #
Configuring Service Interfaces
Configuring NAT POOL Information
Configure NAT RULES
Configure Service-set: Interface Based or Next Hop Based (CGNAT preferred, routing-instance is needed)
NAT
translation-type source dynamic
stateful-firewall-rules
nat-rules
next-hop-service
twice-nat-44
nat pool ( 1 IP , multiple port)
# service-set #
作用就是把流量引到ms-mpc/pic上作nat,相当于一个tunnel...，从ams...0.1进来...，流量jiu hui... 被送到ams0.2出去
# SPU #
是 Juniper 硬件里的 业务处理单元，主要用来处理 L4–L7 服务功能，比如：NAT,ALG,IPSEC
# ALG #
Application Layer Gateway,是一种运行在 SPU上的 应用层感知功能。当设备做 NAT（尤其是端口转换）时，一些应用协议在 报文负载（payload）里携带了 IP 地址或端口号。如果只改 L3/L4 头部，而不改 payload，连接就会失败。
检测特定应用协议的流量。
在 payload 里同步修改 IP/端口，保证 NAT 后的连接还能正常建立。
ALG 就是为了解决这个问题：
# flowd #
flowd 负责处理 NAT（Source NAT、Destination NAT、Static NAT 等）。在建立会话时完成地址和端口转换，并在会话表里记录映射关系。也负责ALG处理...。
