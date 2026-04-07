# ZT topology (MPC10)

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

ZT topology (MPC10)
#define VMX_PHASE 3
#if VMX_PHASE == 3
#include "/vmm/data/user_disks/vmxc/common.vmx.p3.defs"
#elif VMX_PHASE == 2
#include"/vmm/data/user_disks/vmxc/common.vmx.p2.defs"
#else
#include "/vmm/data/user_disks/vmxc/common.vmx.p1.defs"
#endif
TOPOLOGY_START(vZT_topology)
#define VMX_ZT_MPC_I2CID   0xCD8
#define VMX_DISK basedisk "/vmm/data/user_disks/sjiang/image/junos-x86-64-23.2R1.14.vmdk";
#define VMX_CONFIG install "/vmm/data/user_disks/sjiang/Startup_conf/vmx.default-base.conf" "/root/junos.base.conf";
#undef VMX_MPC_DISK
#define VMX_MPC_DISK VMPC_LTS22_IMAGE
#undef VIRTUAL_RE_NCPU
#define VIRTUAL_RE_NCPU 4
#undef VIRTUAL_RE_MEMORY
#define VIRTUAL_RE_MEMORY 8192
#undef VMX_CHASSIS_I2CID
#define VMX_CHASSIS_I2CID 33
#undef VMX_CHASSIS_NAME
#define VMX_CHASSIS_NAME r0
VMX_CHASSIS_START()
VMX_RE_START(r0_re, 0)
VMX_RE_INSTANCE(r0_re, VMX_DISK, VMX_RE_I2CID, 0)
...VMX_CONFIG  // labroot/lab123
VMX_RE_END
VMX_MPC_START(r0_mpc, 0)
VMX_MPC_INSTANCE(r0_mpc, VMX_MPC_DISK, VMX_ZT_MPC_I2CID, 0)
setvar "use_virtio_disk" "1";
memory 8192;
VMX_CONNECT(XE_CHAN(0, 0, 0, 0), private2)
VMX_CONNECT(XE_CHAN(0, 0, 0, 1), private4)
VMX_CONNECT(XE_CHAN(0, 0, 0, 2), private4)
VMX_MPC_END
VMX_CHASSIS_END
PRIVATE_BRIDGES
TOPOLOGY_END
