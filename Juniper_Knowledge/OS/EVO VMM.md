# EVO VMM

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

EVO VMM
Below is the Topology diagram of the VMM testbed we use. 
ae0 
ge-0/0/0 
et-1/0/0:0 
et-1/0/0:1 
ge-0/0/0 
vmxDS1 
vBrackla 
vmxDS2 
10.10.10.10 
ge-0/0/2 
et-1/0/2:0 
11.11.11.11 
et-1/0/1:1 
ge-0/0/1 
12.12.12.12 
. We have one side towards vmxDS2 an aggregate interface 
. To the other side vmxDS1 we use ECMP path 
LDP and RSVP tunnel is established for MPLS data path forwarding. ...Below is the Topology diagram of the VMM testbed we use. 
ae0 
ge-0/0/0 
et-1/0/0:0 
et-1/0/0:1 
ge-0/0/0 
vmxDS1 
vBrackla 
vmxDS2 
10.10.10.10 
ge-0/0/2 
et-1/0/2:0 
11.11.11.11 
et-1/0/1:1 
ge-0/0/1 
12.12.12.12 
. We have one side towards vmxDS2 an aggregate interface 
. To the other side vmxDS1 we use ECMP path 
LDP and RSVP tunnel is established for MPLS data path forwarding.
/vmm/data/user_disks/annaw/sample/evo$
vmm config vbrackla_vmm_1.cfg -g vmm-default
$ cd /vmm/data/user_disks/evo_test/EVOvPTX/pbuilder_reference
/vmm/data/base_disks/junos/vevo/junos-evo-install-ptx-fixed-x86-64-23.4R2-S1.7-EVO.iso
/vmm/data/user_disks/evo_test/EVOvBRACKLA/COSIMPP
Hostbound traffic via two different paths – network stack under Management interface and network stack under WAN interfaces.
mgmt_junos :
FIBD which listens to all Route and Nexthop related objects distributed throughout the system
