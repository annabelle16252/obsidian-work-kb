# start-up config/files

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

start-up config/files
WAY 1
VMX_RE_INSTANCE(r2_re, VMX_DISK_1, VMX_RE_I2CID, 0)
install "/vmm/data/user_disks/sjiang/vmx.base.conf" "/root/junos.base.conf";
VMX_RE_END
WAY 2
#define VMX1_CONFIG install "/vmm/data/user_disks/sjiang/vmx.base.conf" "/root/junos.base.conf";
VMX_CHASSIS_START()
VMX_RE_START(r1_re, 0)
VMX_RE_INSTANCE(r1_re, VMX_DISK, VMX_RE_I2CID, 0)
VMX1_CONFIG  // labroot/lab123
VMX_RE_END
WAY 3... *
#undef BASECONFIG
#define BASECONFIG "/vmm/data/user_disks/sjiang/vmx.base.conf"
sjiang@q-pod21-vmm:~$ cat vmx.base.conf
groups {
protect: default {
system {
root-authentication {
encrypted-password "$6$ns3peN3i$40WVep0Gl/6W0ih7KBWxPviH3/E0BYllTACng4aV/3/asGdQx1XacDgXEX0kDD3ZzkfVGxLpXOItQ8dp2FRCU1"; ## SECRET-DATA
}
login {
user labroot {
uid 2002;
class super-user;
authentication {
encrypted-password "$6$ClhIbgP8$EryiehcOwjbu8RrrtVxHMTiLvCBT1J25jatKHwIU4h.FO9PW508hOQZZQCaewQgYp6XqUNku4oqq75XSAeR4D/"; ## SECRET-DATA
}
}
}
services {
ftp;
ssh;
telnet;
netconf {
ssh;
}
web-management {
http;
}
}
time-zone Asia/Shanghai;
syslog {
archive size 10m files 10;
user * {
any emergency;
}
file messages {
any any;
authorization info;
}
file cli-commands {
interactive-commands any;
}
time-format millisecond;
}
}
}
}
system {
apply-groups default;
}
#define VMX1_FILES install "/homes/jhseo/vmm-dc/cfg/vmx1_re/"  "/var/home/jncie/";
VMX_RE_START(vmx1_re,0)
VMX_RE_INSTANCE(vmx1_re0, VMX_DISK, VMX_RE_I2CID,0)
VMX1_FILES
VMX_RE_END
//files on VMM POD server will be loaded to router during boot up
