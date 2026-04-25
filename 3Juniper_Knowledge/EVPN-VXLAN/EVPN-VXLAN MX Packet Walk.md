# EVPN-VXLAN MX Packet Walk

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

EVPN-VXLAN MX Packet Walk
Pod8
vmm config /vmm/data/user_disks/annaw/lab/evpn/vmm.conf  -g vmm-default
deactivate groups global interfaces lo0
deactivate groups global routing-options router-id
Configuration
PE1
regress@PE1# show interfaces
ge-0/0/0 {
gigether-options {
802.3ad ae0;
}
}
ge-0/0/1 {
vlan-tagging;
unit 100 {
vlan-id 100;
}
unit 200 {
vlan-id 200;
}
}
ae0 {
flexible-vlan-tagging;
encapsulation flexible-ethernet-services;
esi {
00:11:11:11:11:11:11:11:11:11;
all-active;
}
aggregated-ether-options {
lacp {
active;
system-id 00:00:00:00:00:01;
}
}
unit 100 {
encapsulation vlan-bridge;
vlan-id 100;
family bridge;
}
unit 200 {
encapsulation vlan-bridge;
vlan-id 200;
family bridge;
}
}
irb {   <<< ...同一个...ESI...的两台设备的...irb ...的...ip...地址和...virtaul ...地址都是一样的
unit 100 {
virtual-gateway-accept-data;
family inet {
address 100.100.100.251/24 {
virtual-gateway-address 100.100.100.254;
}
}
}
unit 200 {
virtual-gateway-accept-data;
family inet {
address 200.200.200.251/24 {
virtual-gateway-address 200.200.200.254;
}
}
}
}
lo0 {
unit 0 {
family inet {
address 2.2.2.2/32;
}
}
}
regress@PE2# show routing-instances
evpn {
protocols {
evpn {
encapsulation vxlan;
extended-vni-list [ 100 200 ];
multicast-mode ingress-replication;
default-gateway no-gateway-community; <<<...只有...ESI...的...routers...才需要配这个
}
}
vtep-source-interface lo0.0;
instance-type virtual-switch;
bridge-domains {
vlan-100 {
vlan-id 100;
interface ae0.100;
routing-interface irb.100;
vxlan {
vni 100;
}
}
vlan-200 {
vlan-id 200;
interface ae0.200;
routing-interface irb.200;
vxlan {
vni 200;
}
}
}
route-distinguisher 2.2.2.2:4001;
vrf-target target:100:4001;
}
PE3:
regress@PE3# show routing-instances
evpn {
protocols {
evpn {
encapsulation vxlan;
extended-vni-list [ 100 200 ];
multicast-mode ingress-replication;
}
}
vtep-source-interface lo0.0;
instance-type virtual-switch;
bridge-domains {
vlan-100 {
vlan-id 100;
interface ge-0/0/0.100;
vxlan {
vni 100;
}
}
vlan-200 {
vlan-id 200;
interface ge-0/0/0.200;
vxlan {
vni 200;
}
}
}
route-distinguisher 3.3.3.3:4001;
vrf-target target:100:4001;
}
regress@PE3# show interfaces
ge-0/0/0 {
vlan-tagging;
unit 100 {
vlan-id 100;
}
unit 200 {
vlan-id 200;
}
}
lo0 {
unit 0 {
family inet {
address 3.3.3.3/32;
}
PE123 igp,bgp,mpls
Local MAC learning:
regress@PE2# run show bridge mac-table vlan-id 100
MAC flags       (S -static MAC, D -dynamic MAC, L -locally learned, C -Control MAC
O -OVSDB MAC, SE -Statistics enabled, NM -Non configured MAC, R -Remote PE MAC, P -Pinned MAC)
Routing instance : evpn
Bridging domain : vlan-100, VLAN : 100
MAC                 MAC      Logical                Active
address             flags    interface              source
2c:6b:f5:41:3c:c0   DL       ae0.100        ...<<<<...  from local interface
regress@PE2# run show evpn database l2-domain-id 100 instance evpn
Instance: evpn
VLAN  DomainId  MAC address        Active source                  Timestamp        IP address
100        00:00:5e:00:01:01  05:00:00:fd:e9:00:00:00:64:00  Sep 22 20:07:44  100.100.100.254
100        2c:6b:f5:41:3c:c0  00:11:11:11:11:11:11:11:11:11  Sep 22 23:32:08  100.100.100.1 <<<
100        2c:6b:f5:48:5a:f0  irb.100                        Sep 22 20:07:44  100.100.100.251
Advertise MAC/IP route to remote Pes:
regress@PE1# run show route table evpn.evpn.0 match-prefix *2c:6b:f5:f5*
evpn.evpn.0: 27 destinations, 27 routes (27 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both
2:1.1.1.1:4001::100::2c:6b:f5:f5:ff:f0/304 MAC/IP
*[EVPN/170] 00:19:09
Indirect
2:1.1.1.1:4001::200::2c:6b:f5:f5:ff:f0/304 MAC/IP
*[EVPN/170] 00:19:09
Indirect
regress@PE1# run show route advertising-protocol bgp 2.2.2.2 table evpn.evpn.0 match-prefix *2c:6b:f5:f5*
evpn.evpn.0: 27 destinations, 27 routes (27 active, 0 holddown, 0 hidden)
Prefix                  Nexthop              MED     Lclpref    AS path
2:1.1.1.1:4001::100::2c:6b:f5:f5:ff:f0/304 MAC/IP
*                         Self                         100        I
2:1.1.1.1:4001::200::2c:6b:f5:f5:ff:f0/304 MAC/IP
*                         Self                         100        I
2:1.1.1.1:4001::100::2c:6b:f5:f5:ff:f0::100.100.100.251/304 MAC/IP
*                         Self                         100        I
2:1.1.1.1:4001::200::2c:6b:f5:f5:ff:f0::200.200.200.251/304 MAC/IP
*                         Self                         100        I
EVPN-VXLAN MX Packet Walk
Pod8
vmm config /vmm/data/user_disks/annaw/lab/evpn/vmm.conf  -g vmm-default
deactivate groups global interfaces lo0
deactivate groups global routing-options router-id
Configuration
PE1
regress@PE1# show interfaces
ge-0/0/0 {
gigether-options {
802.3ad ae0;
}
}
ge-0/0/1 {
vlan-tagging;
unit 100 {
vlan-id 100;
}
unit 200 {
vlan-id 200;
}
}
ae0 {
flexible-vlan-tagging;
encapsulation flexible-ethernet-services;
esi {
00:11:11:11:11:11:11:11:11:11;
all-active;
}
aggregated-ether-options {
lacp {
active;
system-id 00:00:00:00:00:01;
}
}
unit 100 {
encapsulation vlan-bridge;
vlan-id 100;
family bridge;
}
unit 200 {
encapsulation vlan-bridge;
vlan-id 200;
family bridge;
}
}
irb {   <<< ...同一个...ESI...的两台设备的...irb ...的...ip...地址和...virtaul ...地址都是一样的
unit 100 {
virtual-gateway-accept-data;
family inet {
address 100.100.100.251/24 {
virtual-gateway-address 100.100.100.254;
}
}
}
unit 200 {
virtual-gateway-accept-data;
family inet {
address 200.200.200.251/24 {
virtual-gateway-address 200.200.200.254;
}
}
}
}
lo0 {
unit 0 {
family inet {
address 2.2.2.2/32;
}
}
}
regress@PE2# show routing-instances
evpn {
protocols {
evpn {
encapsulation vxlan;
extended-vni-list [ 100 200 ];
multicast-mode ingress-replication;
default-gateway no-gateway-community; <<<...只有...ESI...的...routers...才需要配这个
}
}
vtep-source-interface lo0.0;
instance-type virtual-switch;
bridge-domains {
vlan-100 {
vlan-id 100;
interface ae0.100;
routing-interface irb.100;
vxlan {
vni 100;
}
}
vlan-200 {
vlan-id 200;
interface ae0.200;
routing-interface irb.200;
vxlan {
vni 200;
}
}
}
route-distinguisher 2.2.2.2:4001;
vrf-target target:100:4001;
}
PE3:
regress@PE3# show routing-instances
evpn {
protocols {
evpn {
encapsulation vxlan;
extended-vni-list [ 100 200 ];
multicast-mode ingress-replication;
}
}
vtep-source-interface lo0.0;
instance-type virtual-switch;
bridge-domains {
vlan-100 {
vlan-id 100;
interface ge-0/0/0.100;
vxlan {
vni 100;
}
}
vlan-200 {
vlan-id 200;
interface ge-0/0/0.200;
vxlan {
vni 200;
}
}
}
route-distinguisher 3.3.3.3:4001;
vrf-target target:100:4001;
}
regress@PE3# show interfaces
ge-0/0/0 {
vlan-tagging;
unit 100 {
vlan-id 100;
}
unit 200 {
vlan-id 200;
}
}
lo0 {
unit 0 {
family inet {
address 3.3.3.3/32;
}
PE123 igp,bgp,mpls
Local MAC learning:
regress@PE2# run show bridge mac-table vlan-id 100
MAC flags       (S -static MAC, D -dynamic MAC, L -locally learned, C -Control MAC
O -OVSDB MAC, SE -Statistics enabled, NM -Non configured MAC, R -Remote PE MAC, P -Pinned MAC)
Routing instance : evpn
Bridging domain : vlan-100, VLAN : 100
MAC                 MAC      Logical                Active
address             flags    interface              source
2c:6b:f5:41:3c:c0   DL       ae0.100        ...<<<<...  from local interface
regress@PE2# run show evpn database l2-domain-id 100 instance evpn
Instance: evpn
VLAN  DomainId  MAC address        Active source                  Timestamp        IP address
100        00:00:5e:00:01:01  05:00:00:fd:e9:00:00:00:64:00  Sep 22 20:07:44  100.100.100.254
100        2c:6b:f5:41:3c:c0  00:11:11:11:11:11:11:11:11:11  Sep 22 23:32:08  100.100.100.1 <<<
100        2c:6b:f5:48:5a:f0  irb.100                        Sep 22 20:07:44  100.100.100.251
Advertise MAC/IP route to remote Pes:
regress@PE1# run show route table evpn.evpn.0 match-prefix *2c:6b:f5:f5*
evpn.evpn.0: 27 destinations, 27 routes (27 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both
2:1.1.1.1:4001::100::2c:6b:f5:f5:ff:f0/304 MAC/IP
*[EVPN/170] 00:19:09
Indirect
2:1.1.1.1:4001::200::2c:6b:f5:f5:ff:f0/304 MAC/IP
*[EVPN/170] 00:19:09
Indirect
regress@PE1# run show route advertising-protocol bgp 2.2.2.2 table evpn.evpn.0 match-prefix *2c:6b:f5:f5*
evpn.evpn.0: 27 destinations, 27 routes (27 active, 0 holddown, 0 hidden)
Prefix                  Nexthop              MED     Lclpref    AS path
2:1.1.1.1:4001::100::2c:6b:f5:f5:ff:f0/304 MAC/IP
*                         Self                         100        I
2:1.1.1.1:4001::200::2c:6b:f5:f5:ff:f0/304 MAC/IP
*                         Self                         100        I
2:1.1.1.1:4001::100::2c:6b:f5:f5:ff:f0::100.100.100.251/304 MAC/IP
*                         Self                         100        I
2:1.1.1.1:4001::200::2c:6b:f5:f5:ff:f0::200.200.200.251/304 MAC/IP
*                         Self                         100        I
