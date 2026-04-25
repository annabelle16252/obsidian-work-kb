# Duel RE

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

Duel RE
#define VMX_PHASE 3
#include "/vmm/data/user_disks/vmxc/common.vmx.p3.defs"
#include "/vmm/bin/common.defs"
#define VMX_DISK basedisk "/vmm/data/user_disks/sjiang/image/junos-x86-64-21.1R1.11.vmdk";
#define VMX1_CONFIG install "/vmm/data/user_disks/sjiang/vmx.base.conf" "/root/junos.base.conf";
TOPOLOGY_START(vmx_topology)
// **************** R3 ****************
// example for dual RE installed
#define VMX_CHASSIS_I2CID 48  //MX240
#define VMX_CHASSIS_NAME r3
VMX_CHASSIS_START()
VMX_RE_START(r3_re, 0)
VMX_RE_INSTANCE(r3_re, VMX_DISK, VMX_RE_I2CID, 0)
VMX_OTHER_RE_PRESENT
VMX1_CONFIG  // labroot/lab123
VMX_RE_END
VMX_RE_START(r3_re1, 1)
VMX_RE_INSTANCE(r3_re1, VMX_DISK, VMX_RE_I2CID, 1)
VMX_OTHER_RE_PRESENT
VMX1_CONFIG  // labroot/lab123
VMX_RE_END
VMX_MPC_START(r3_MPC0, 0)
VMX_MPC_INSTANCE(r3_MPC0, VMX_DISK, VMX_MPC_I2CID, 0)
VMX_CONNECT(GE(0,0,0), private2)
VMX_CONNECT(GE(0,0,1), private2)
VMX_MPC_END
VMX_CHASSIS_END
#undef VMX_CHASSIS_I2CID
#undef VMX_CHASSIS_NAME
// **************** END of R3 ****************
PRIVATE_BRIDGES
TOPOLOGY_END
