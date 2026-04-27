# Layer 2

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

Layer 2
# ...二层透传... #
mx --- qfx --- mx
当两个...mx...接口是三层，通过...qfx ...透传就是二层透传，需要...qfx...的口配...trunk...或...bridge...：
trunk...：
labroot@jtac-qfx5110-48s-4c-r2028# show
xe-0/0/0 {
unit 0 {
family ethernet-switching {
interface-mode trunk;
vlan {
members vlan600;
}
labroot@jtac-qfx5110-48s-4c-r2028# show vlans
vlan600 {
vlan-id 600;
}
Bridge:
labroot@jtac-mx480-r2050-re0#
ge-1/0/9 {
flexible-vlan-tagging;
encapsulation flexible-ethernet-services;
unit 10 {
encapsulation vlan-bridge;
vlan-id 10;
}
[edit]
labroot@jtac-mx480-r2050-re0# show bridge-domains
vlan10 {
vlan-id 10;
interface ge-1/0/9.10;
}
# ...三层透传... #
mx --- mx,...互联接口有...ip...，... ...两边只要打上...vlan-tagging...就行。
# LLDP #
如果有二层传输问题，...LLDP...可以检测出来，会...down...。所以可以通过这个看看。
BFD...只能检测出三层问题
# bridge-domain #
https://www.juniper.net/documentation/us/en/software/junos/bridging-learning/topics/task/layer-2-services-bridge-domains-configuring.html
Includes a set of logical interfaces that participate in Layer 2 learning and forwarding. You can optionally configure a VLAN identifier and a routing interface for the bridge domain to also support Layer 3 IP routing.
# ifl mode #...
Access Mode:
It's ifl's default mode, ...可以接收...untag(no vlan)...或者...tag...的包....
但是会在设备内部打上...vlan...以便从相应...vlan interface...出去。在出口出去的时候，...vlan tag...被剥掉。
Trunk Mode:
default...只能接收带...vlan...的包...,...出去设备的时候也带着...vlan(native-vlan)...出去。
如果想接收不带...vlan...的包需要...ifd...下配一个...native-vlan-id...：
------------
[edit interfaces ge-fpc/pic/port]
flexible-vlan-tagging;
native-vlan-id number;... <<<<
----------------
# ...family ethernet-switching... #
vlan-tagging...是一个...unit...只允许带配置的...vlan...的包过。而...flexible-vlan-tagging...可以让...1...个...unit...可以让带不同...vlan...的包过。所以要加...family ethernet-switch+vlan members...，当然...interface mode...就要是...trunk...。
~~...不同...vlan...之间通讯需要...3...层路由或...irb...口
# router ...跑纯二层流量... #
https://www.juniper.net/documentation/us/en/software/junos/mc-lag/topics/topic-map/examples-mc-lag.html
把所有口放到一个...bridge domain...里面，口封装...vlan-bridge
# Vlan Normalization #
https://www.juniper.net/documentation/en_US/junos-space16.1/topics/concept/layer2-provisioning-vlan-manipulation-understanding.html
# Q-in-Q #
[EX] Understanding and configuring 802.1Q (Q-in-Q) dot1q tunneling
https://kb.juniper.net/InfoCenter/index?page=content&id=KB12259
ge-0/0/1 and ge-0/0/3 are access ports.
ge-0/04 is the trunk port.
root# show ethernet-switching-options
dot1q-tunneling {
    ether-type 0x8100;
}
root# show vlans
qinqvlan {
    vlan-id 4001;
    interface {
        ge-0/0/1.0;
        ge-0/0/3.0
    }
    dot1q-tunneling {
        customer-vlans 1-4094;
    }
}
root# show interfaces ge-0/0/4
unit 0 {
    family ethernet-switching {
        port-mode trunk;
        vlan {
            members 4001;
        }
    }
}
root# show interfaces ge-0/0/1
unit 0 {
    family ethernet-switching;
}
root# show interfaces ge-0/0/3
unit 0 {
    family ethernet-switching;
}
流量...c-vlan...是...100-200...的可以进来，然后...input...的话是...push...一个...s-vlan 10...，... output...是...pop...掉...s-vlan 10.
# Native VLAN #
[EX] Native VLAN ID and tagged behavior in EX-series switches
From <https://kb.juniper.net/InfoCenter/index?page=content&id=KB17419>
Here is how it works:
Transmit = untagged (pass)
Receive = untagged (pass)
Receive = tagged to MGMT (drop)
MGMT is NOT a member of trunk, but it is a member of native VLAN:
Transmit = tagged (pass)
Receive = untagged (pass - mapped to MGMT)
Receive = tagged (pass)
MGMT IS a member of the trunk and native VLANs:
# Dynamic VLAN #
https://www.juniper.net/documentation/en_US/junos/topics/concept/interfaces-802-1q-vlans-overview.html
VLAN ID 0 is reserved for tagging the priority of frames. VLAN IDs 1 through 511 are reserved for normal VLANs. VLAN IDs 512 and above are reserved for VLAN circuit cross-connect (CCCs).
Configure a dynamic profile for dynamic VLAN or dynamic stacked VLAN creation. ...就是可以根据比如进来包的...ifl...来自动给这个包一个...vlan id...或...stacked vlan...。
Singal vlan
Vlan configuration...：
[edit interfaces interface-name]
Dual vlan
user@host# vlan-tagging;
[edit interfaces interface-name]
Mixed
user@host# stacked-vlan-tagging;
[edit interfaces ge-fpc/pic/port]
user@host#flexible-vlan-tagging;
[edit interfaces ge-fpc/pic/port unit logival-unit-number]
user@host# vlan-id number;
family family {
address address;
}
user@host# vlan-tags inner tpid.vlan-id outer tpid.vlan-id;
family family {
address address;
}
# STP #
https://www.juniper.net/documentation/us/en/software/junos/stp-l2/topics/topic-map/spanning-tree-configuring-stp.html
Spanning Tree Protocol (STP), defined in IEEE 802.1D, creates a tree of links in the Ethernet switched network. Links that cause loops in the network are disabled, thereby providing a single active link between any two devices.
Spanning tree protocols work by creating bridges. A root bridge (switch) is a bridge at the top of a Spanning Tree. Ethernet connections branch out from the root switch, connecting to other switches in the Local Area Network (LAN).
The default spanning-tree protocol for EX Series switches is Rapid Spanning Tree Protocol (RSTP). RSTP provides faster convergence times than the original Spanning Tree Protocol (STP). However, some legacy networks require the slower convergence times of basic STP that work with 802.1D 1998 bridges.
# RSTP #
<<<...如果不满足以上条件，就需要正常经过两个...delay timer 30s...，再切换
https://www.juniper.net/documentation/us/en/software/junos/stp-l2/topics/topic-map/spanning-tree-configuring-rstp.html
RSTP identifies certain links as point to point. When a point-to-point link fails, the alternate link can transition to the forwarding state, which speeds up convergence.
Benefits of Using RSTP:
RSTP is faster than STP.
Voice and video work better with RSTP than they do with STP.
RSTP supports more ports than MSTP or VSTP.
RSTP is backward compatible with STP; therefore, switches do not all have to run RSTP.
On MX and ACX routers, you can configure RSTP, MSTP, and VSTP instance interfaces as edge ports.
RSTP responds to changes within the timeframe of three hello BPDUs (bridge protocol data units), or 6 seconds.
Configuring nonstop bridging (NSB) on a switch with redundant Routing Engines keeps RSTP synchronized on both Routing Engines. This way, RSTP remains active immediately after a switchover because it is already synchronized to the backup Routing Engine. RSTP does not have to reconverge after a Routing Engine switchover when NSB is enabled because the neighbor devices do not detect an RSTP change on the switch.
The root port is responsible for forwarding data to the root bridge.
The alternate port is a standby port for the root port. When a root port goes down, the alternate port becomes the active root port.
The designated port forwards data to the downstream network segment or device.
The backup port is a backup port for the designated port. When a designated port goes down, the backup port becomes the active designated port and starts forwarding data.
An RSTP topology contains ports that have specific roles:
Configuring RSTP and Nonstop Bridging on Switch 1:
If Switch 1 includes dual Routing Engines, configure NSB.
set chassis redundancy graceful-switchover
set system commit synchronize
set protocols layer2-control nonstop-bridging <<<
vmm config /vmm/data/user_disks/annaw/lab/rstp/rstp.conf  -g vmm-default
regress /MaRtInI ; root/Embe1mpls  root123
regress@Switch-1# run show spanning-tree interface
Spanning tree interface parameters for instance 0
Interface                  Port ID    Designated         Designated         Port    State  Role
port ID           bridge ID          Cost
xe-0/0/0                   128:490      128:490   8192.020586711302         1000    FWD    ROOT
xe-0/0/1                   128:491      128:491  16384.020586712502         1000    FWD    DESG
xe-0/0/2                   128:492      128:492  16384.020586712502         1000    FWD    DESG
# MSTP #
<<<...如果出现...master ...端口，就说明处于不同域里，查看以上三项哪项不同
# VSTP #
https://www.juniper.net/documentation/us/en/software/junos/stp-l2/topics/topic-map/spanning-tree-configuring-vstp.html
VSTP has the following benefits:
Connects devices that are not part of the network
Compatible with Cisco PVST+
VSTP and RSTP are the only spanning-tree protocols that can be configured concurrently on a device.
You can use Juniper Networks switches with VSTP and Cisco switches with PVST+ and Rapid-PVST+ in the same network. Cisco supports a proprietary Per-VLAN Spanning Tree (PVST) protocol, which maintains a separate spanning tree instance per each VLAN.
