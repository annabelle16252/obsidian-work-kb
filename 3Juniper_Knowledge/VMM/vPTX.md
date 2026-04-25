# vPTX 

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

v...PTX
vBrackla
#include "/vmm/bin/common.defs"
#include "/vmm/data/user_disks/vmxc/common.vmx.p3.defs"
#include "/vmm/data/user_disks/vptxc/common.brackla.defs"
#include "/vmm/data/user_disks/vptxc/common.evovptx.defs"
#define VMX_PHASE 3
#define EVO_DEV 1
#define EVOVPTX_DISK1 "/vmm/data/base_disks/sanity_test_images/junos-evo-install-ptx-fixed-x86-64-20.4R2.14-
EVO.iso"
#define EVOVPTX_FPC_CSPP_IMG "/vmm/data/base_disks/junos/vevo/ubuntu_vm_evo.qcow2"
#define VMM_ENV_CSPP_CFG_DIR /vmm/data/base_disks/junos/vevo/COSIMPP/
#define VMX_DISK1 basedisk "/vmm/data/base_disks/default_images/default_image_vmx_phase3.img";
TOPOLOGY_START(vbrackla_vmx_topology)
#undef PTX_CHAS_NAME
#define PTX_CHAS_NAME evovbrackla
EVOVPTX_CHASSIS_START_ (PTX_CHAS_NAME)
EVOvBracklaRE(PTX_CHAS_NAME,EVOVPTX_DISK1)  <<< RE
EVOvBrackla_CSPP_START(PTX_CHAS_NAME,EVOVPTX_FPC_CSPP_IMG)  <<< FPC
EVOVPTX_CONNECT(IF_ET(1, 0, 0), private10)
EVOVPTX_CONNECT(IF_ET(1, 0, 1), private11)
EVOvBrackla_CSPP_END
EVOVPTX_CHASSIS_END_
vScapa
#include "/vmm/data/user_disks/vptxc/common.evovscapa.defs"
#define EVOVPTX1_IMG \
“/vmm/data/base_disks/junos/vevo/junos-evo-install-ptx-x86-64-21.1-202011042049.0-EVO.iso“
#define EVOVPTX_FPC_CSPP_IMG \
"/vmm/data/base_disks/junos/vevo/ubuntu_vm_evo.qcow2“
#define VMM_ENV_CSPP_CFG_DIR /vmm/data/base_disks/junos/vevo/COSIMPP/
EVOVPTX_TOPOLOGY_START_ (Evo_vScapa_Testbed)
#undef PTX_CHAS_NAME
#define PTX_CHAS_NAME EVOvSCAPA1
#undef PTX_CHAS_I2CID
#define PTX_CHAS_I2CID 240 /* JNX_PRODUCT_PTX10008 = 240 */
#define EVO_DEVELOPER_FILES \
install "ENV(CONFIG_BASE)/evo-mount/yp.conf" "/etc/yp.conf"; \
install "ENV(CONFIG_BASE)/evo-mount/auto.evo" "/etc/auto.evo"; \
install "ENV(CONFIG_BASE)/evo-mount/auto.evofiler" "/etc/auto.evofiler"; \
install "ENV(CONFIG_BASE)/evo-mount/auto.master" "/etc/auto.master"; \
install "ENV(CONFIG_BASE)/evo-mount/nisdomainname" "/etc/nisdomainname"; \
install "ENV(CONFIG_BASE)/evo-mount/nsswitch.conf" "/etc/nsswitch.conf"; \
install "/vmm/data/vmm-configs/common/vptxc/rc.vmm" "/var/vmguest/rc.vmm"; \
install "ENV(CONFIG_BASE)/evo-mount/ssh_host_rsa_key" "/home/root/.ssh/ssh_host_rsa_key"; \
install "ENV(CONFIG_BASE)/evo-mount/ssh_host_rsa_key.pub" "/home/root/.ssh/ssh_host_rsa_key.pub";
EVOVPTX_CHASSIS_START_ (PTX_CHAS_NAME)
EVOVPTX_RE_START_ (PTX_CHAS_NAME, EVOVPTX_RE0)
#undef RE_BOOT_ARGS
#define RE_BOOT_ARGS " " /* Unused; for future expansion. */
#define RE_I2CID 0x0CA3 /* Doon RCB: I2C_ID_JNP10K_RCB */
EVOVPTX_RE0_INSTANCE_ISO_AUTODISK (PTX_CHAS_NAME, PTX_CHAS_I2CID, EVOVPTX1_IMG,
EVOVPTX_RE0, RE_I2CID, RE_BOOT_ARGS)
EVO_DEVELOPER_FILES
EVOVPTX_RE_END_
EVOVPTX_FPC_START_ (PTX_CHAS_NAME, EVOVPTX_FPC0)
#undef FPC_BOOT_ARGS
#define FPC_BOOT_ARGS " " /* Unused; for future expansions. */
#undef FPC_I2CID
#define FPC_I2CID 0x0D07 /* I2C_ID_JNP10K_36QDD_LC */
EVOVPTX_FPC_INSTANCE_DISKLESS (PTX_CHAS_NAME, PTX_CHAS_I2CID,
EVOVPTX_RE0, EVOVPTX_FPC0, FPC_I2CID, FPC_BOOT_ARGS)
EVOVPTX_FPC_END_
EVOVPTX_FPC_CSPP_START_ (PTX_CHAS_NAME, EVOVPTX_FPC0)
EVOVPTX_FPC_CSPP_INSTANCE (PTX_CHAS_NAME, EVOVPTX_FPC0,
EVOVPTX_FPC_CSPP_IMG)
EVOVPTX_INSTALL_FPC_CSPP_BUNDLE_SCAPA (VMM_ENV_CSPP_CFG_DIR)
EVOVPTX_CONNECT (IF_ET (0, 0, 0), private10)
EVOVPTX_CONNECT (IF_ET (0, 0, 1), private11)
EVOVPTX_FPC_CSPP_END_
PRIVATE_BRIDGES TOPOLOGY_END
