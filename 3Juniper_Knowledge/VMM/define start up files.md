# define start up files

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

define start up files
#define VMX1_FILES install "/homes/jhseo/vmm-dc/cfg/vmx1_re/"  "/var/home/jncie/";
#define VMX1_CONFIG install "/homes/jhseo/vmm-dc/cfg/vmx1.cfg"  "/root/junos.base.conf";
VMX_RE_START(vmx1_re,0)
VMX_RE_INSTANCE(vmx1_re0, VMX_DISK, VMX_RE_I2CID,0)
VMX1_CONFIG
VMX1_FILES
VMX_RE_END
The VMX1_CONFIG will load the start-up config for a device
The VMX1_FILE will copy the files from VMM server to VMX router during boot up
