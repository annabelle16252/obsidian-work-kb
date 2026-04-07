# Linux Server (in validate)

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

Linux Server (in validate)
#define VMX_PHASE 3
#include "/vmm/bin/common.defs"
#include "/vmm/data/user_disks/vmxc/common.vmx.p3.defs"
#define Ubuntu_VM bootdisk_rw "/vmm/data/user_disks/sjiang/ubuntu-18.04.3.img";
TOPOLOGY_START(vmx_topology)
vm "Ubuntu_VM" {
hostname "Ubuntu_VM";
Ubuntu_VM
memory 8192;
ncpus 4;
interface "em0" { bridge "external"; };
};
PRIVATE_BRIDGES
TOPOLOGY_END
#define VMX_PHASE 3
#include "/vmm/bin/common.defs"
#include "/vmm/data/user_disks/vmxc/common.vmx.p3.defs"
#define CENTOS bootdisk_rw "/vmm/data/base_disks/centos/centos7.0.img";
TOPOLOGY_START(vmx_topology)
vm "Centos_VM" {
hostname "Centos_VM";
CENTOS
memory 4096;
ncpus 4;
interface "em0" { bridge "external"; };
interface "em1" { bridge "private1"; };
interface "em2" { bridge "private2"; };
};
PRIVATE_BRIDGES
TOPOLOGY_END
root
Embe1mpls
#define VMX_PHASE 3
#include "/vmm/data/user_disks/vmxc/common.vmx.p3.defs"
#include "/vmm/bin/common.defs"
#define VMX_DISK basedisk "/vmm/data/user_disks/sjiang/image/junos-x86-64-21.1R1.11.vmdk";
#define VMX_CONFIG install "/vmm/data/user_disks/sjiang/vmx.base.conf" "/root/junos.base.conf";
TOPOLOGY_START(vmx_topology)
// **************** R1 ****************
// normal router for example
#define VMX_CHASSIS_I2CID 33 //MX480
#define VMX_CHASSIS_NAME r1
VMX_CHASSIS_START()
VMX_RE_START(r1_re, 0)
VMX_RE_INSTANCE(r1_re, VMX_DISK, VMX_RE_I2CID, 0)
VMX_CONFIG  // labroot/lab123
VMX_RE_END
VMX_MPC_START(r1_MPC0, 0)
VMX_MPC_INSTANCE(r1_MPC0, VMX_DISK, VMX_MPC_I2CID, 0)
VMX_CONNECT(GE(0,0,0), private1) // to server
VMX_MPC_END
VMX_CHASSIS_END
#undef VMX_CHASSIS_I2CID
#undef VMX_CHASSIS_NAME
// **************** END of R1 ****************
// **************** centos server****************
vm "centos" {
hostname "centos";
basedisk "/vmm/data/user_disks/sjiang/image/centos8.2.qcow2";
install "/dev/null" "/usr/bin/cloud-init";
install "/vmm/data/user_disks/sjiang/Linux/rc.local" "/etc/rc.d/rc.local";
install "/vmm/data/user_disks/sjiang/Linux/shadow" "/etc/shadow";
interface "em0" { bridge "external"; };
interface "em1" { bridge "private1"; };
};
// **************** END of centos server****************
PRIVATE_BRIDGES
TOPOLOGY_END
#include "/vmm/bin/common.defs"
config "myconfig" {
vm "centos" {
hostname "centos";
basedisk "/vmm/data/user_disks/sjiang/image/centos8.2.qcow2";
install "/dev/null" "/usr/bin/cloud-init";
install "/vmm/data/user_disks/sjiang/Linux/rc.local" "/etc/rc.d/rc.local";
install "/vmm/data/user_disks/sjiang/Linux/shadow" "/etc/shadow";
interface "em0" { bridge "external"; };
interface "em1" { bridge "private2"; };
interface "em2" { bridge "private3"; };
interface "em3" { bridge "private4"; };
};
PRIVATE_BRIDGES
};
