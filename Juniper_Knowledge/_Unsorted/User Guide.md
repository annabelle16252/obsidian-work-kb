# User Guide

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

User Guide
Orchestration Servers 
Compute Cluster 
· VMM Server, NFS 
· VHOST servers running VM's 
· Database Server 
· 256 GB memory, around 56 vCPUs 
· VDE Services 
· Dual Xeons 
VHOST Servers 
VM 1 
Virtual Topology 
VM 2 
VMM Server 
VM 3 
VMM Client 
NFS Server 
· Using interfacing services/scripts 
Virtual Switch 
-- 
VDE Server 
VMM Client 
User Request 
"Create Virtual Testbed" 
jn-000022 ...Orchestration Servers 
Compute Cluster 
· VMM Server, NFS 
· VHOST servers running VM's 
· Database Server 
· 256 GB memory, around 56 vCPUs 
· VDE Services 
· Dual Xeons 
VHOST Servers 
VM 1 
Virtual Topology 
VM 2 
VMM Server 
VM 3 
VMM Client 
NFS Server 
· Using interfacing services/scripts 
Virtual Switch 
-- 
VDE Server 
VMM Client 
User Request 
"Create Virtual Testbed" 
jn-000022
VMM是VM monitor, 也就是hypervisor,create,manage VMs...。类型包含比如...VMware ESXi...，...KVM等
Figure 5: Dual RE vMX Architecture 
TRIO 
(LU Emulator) 
MPC1 VM 
Junos 
RPIO Tunnel 
uKemel 
(with RPIO Client) 
... 
TRIO 
(LU Emulator) 
Hypervisor (Linux with KVM, Qemu) 
MPCn VM 
Junos 
RPIO Tunnel 
x86 Hardware 
uKemel 
(with RPIO Client) 
CHASSISD 
REO VM 
Junos 
RPD 
DCD 
CHASSISD 
RE1 VM 
Junos 
RPD 
DCD 
jn-000025 ...Figure 5: Dual RE vMX Architecture 
TRIO 
(LU Emulator) 
MPC1 VM 
Junos 
RPIO Tunnel 
uKemel 
(with RPIO Client) 
... 
TRIO 
(LU Emulator) 
Hypervisor (Linux with KVM, Qemu) 
MPCn VM 
Junos 
RPIO Tunnel 
x86 Hardware 
uKemel 
(with RPIO Client) 
CHASSISD 
REO VM 
Junos 
RPD 
DCD 
CHASSISD 
RE1 VM 
Junos 
RPD 
DCD 
jn-000025
• POD–Collection of VMM manager, VMM server, multiple VDE servers, VMM database server, and vhosts that
together constitute a single instance for running VMs.
/vmm/data/base_disks/ directory includes the Junos OS builds and non-Juniper images that you can use for deploying virtual and container Juniper platforms.
