# All in one_sean

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

All in one_sean
#define VMX_PHASE 3
#include "/vmm/data/user_disks/vmxc/common.vmx.p3.defs"
#include "/vmm/bin/common.defs"
// do not touch above 3 lines
#define VMX_DISK basedisk "/vmm/data/user_disks/sjiang/image/junos-x86-64-21.1R1.11.vmdk";
#define VMX_DISK_2 basedisk "/vmm/data/user_disks/sjiang/image/junos-x86-64-19.1R3.9.vmdk";
//#define TG1_DISK basedisk "/vmm/data/user_disks/sjiang/image/Ixia_Virtual_Test_Appliance_8.31_EA_KVM_0.10.qcow2";
//#define TG1_DISK basedisk "/vmm/data/user_disks/sjiang/image/spirent-5.04.8556.img";
// define the images will be used in this topo as above
// note the name "VMX_DISK" and "TG1_DISK", it will be used in below
// if there are different versions used, please define different names for them
#define VMX_CONFIG install "/vmm/data/user_disks/sjiang/vmx.base.conf" "/root/junos.base.conf";
// define the start-up config for the device
// this specified config will overwride the default start-up config on device as "/root/junos.base.conf"
// use "VMX_CONFIG" to enable the start-up config
#define VMX1_FILES install "/vmm/data/user_disks/sjiang/startfiles"  "/var/home/startfiles/";
// define the files loading to the device during boot up
TOPOLOGY_START(vmx_topology)
//#define VMX_CHASSIS_I2CID 48  //MX240
//#define VMX_CHASSIS_I2CID 33  //MX480
//#define VMX_CHASSIS_I2CID 21  //MX960
//#define VMX_CHASSIS_I2CID 88  //MX80
//#define VMX_CHASSIS_I2CID 89  //MX80_48T
//#define VMX_CHASSIS_I2CID 137  //MX2020
//#define VMX_CHASSIS_I2CID 138  //MX2010
//#define VMX_CHASSIS_I2CID 215  //MX2008
//#define VMX_CHASSIS_I2CID 217  //MX10001
//#define VMX_CHASSIS_I2CID 218  //MX10002
//#define VMX_CHASSIS_I2CID 39  //EX3200_48_P
//#define VMX_CHASSIS_I2CID 44  //EX4200_48_P
//#define VMX_CHASSIS_I2CID 149  //EX9204
//#define VMX_CHASSIS_I2CID 161  //VMX
// this line indicates the beginning of topology
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
VMX_CONNECT(GE(0,0,0), private1) // to R2
VMX_MPC_END
VMX_CHASSIS_END
#undef VMX_CHASSIS_I2CID
#undef VMX_CHASSIS_NAME
// **************** END of R1 ****************
// **************** R2 ****************
// normal router for example
#define VMX_CHASSIS_I2CID 21 //MX960
#define VMX_CHASSIS_NAME r2
VMX_CHASSIS_START()
VMX_RE_START(r2_re, 0)
VMX_RE_INSTANCE(r2_re, VMX_DISK_2, VMX_RE_I2CID, 0)
VMX_CONFIG  // labroot/lab123
VMX_RE_END
VMX_MPC_START(r2_MPC0, 0)
VMX_MPC_INSTANCE(r2_MPC0, VMX_DISK, VMX_MPC_I2CID, 0)
VMX_CONNECT(GE(0,0,0), private1) // to R1
VMX_MPC_END
VMX_CHASSIS_END
#undef VMX_CHASSIS_I2CID
#undef VMX_CHASSIS_NAME
// **************** END of R2 ****************
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
VMX_CONNECT(GE(0,0,1), dead)
VMX_CONNECT(GE(0,0,2), dead)
VMX_CONNECT(GE(0,0,3), dead)
VMX_CONNECT(GE(0,0,4), dead)
VMX_CONNECT(GE(0,0,5), dead)
VMX_CONNECT(GE(0,0,6), dead)
VMX_CONNECT(GE(0,0,7), private5)
VMX_MPC_END
VMX_CHASSIS_END
#undef VMX_CHASSIS_I2CID
#undef VMX_CHASSIS_NAME
// **************** END of R3 ****************
// **************** R4 ****************
// example for EA chip MPC and 10G/100G links
#define VMX_CHASSIS_I2CID 33 //MX480
#define VMX_CHASSIS_NAME R4
VMX_CHASSIS_START()
VMX_RE_START(r4_re, 0)
VMX_RE_INSTANCE(r4_re, VMX_DISK, VMX_RE_I2CID, 0)
VMX_RE_END
VMX_MPC_START(r4_MPC0, 0)
VMX_MPC_INSTANCE(r4_MPC0, VMX_DISK, VMX_MPC_I2CID, 0)
VMX_CONNECT(GE(0,0,0), private3) // 1g connection //
VMX_CONNECT(GE(0,0,1), private3) // 1g connection //
VMX_MPC_END
// define MPC0 as normal FPC, link speed as 1G
// note the MACRO is "VMX_MPC_I2CID" for LU chip set
VMX_MPC_START(r4_MPC1, 1)
VMX_MPC_INSTANCE(r4_MPC1, VMX_DISK, VMX_EA_MPC_I2CID, 1)
VMX_CONNECT(ET(1, 0, 0, 0), private4)   // 100g connection //
VMX_CONNECT(ET(1, 0, 1, 0), private4)   // 100g connection //
VMX_CONNECT(ET(1, 0, 2, 0), private5)   // 100g connection //
VMX_CONNECT(ET(1, 0, 3, 0), private5)   // 100g connection //
VMX_MPC_END
// define MPC1 as EA based MPC
// note the MARCO is "VMX_EA_MPC_I2CID" for EA chip set
// port number is set in the third column for 100G port
VMX_MPC_START(r4_MPC2, 2)
VMX_MPC_INSTANCE(r4_MPC2, VMX_DISK, VMX_EA_MPC_I2CID, 2)
VMX_CONNECT(XE_CHAN(2, 0, 0, 0), private6)   // 10g CONNECTION //
VMX_CONNECT(XE_CHAN(2, 0, 0, 1), private6)   // 10g CONNECTION //
VMX_CONNECT(XE_CHAN(2, 0, 0, 2), private7)   // 10g CONNECTION //
VMX_CONNECT(XE_CHAN(2, 0, 0, 3), private7)   // 10g CONNECTION //
VMX_MPC_END
// 10g link could be used on EA based as well
// note the link name changed, and the fourth column is used
VMX_CHASSIS_END
#undef VMX_CHASSIS_I2CID
#undef VMX_CHASSIS_NAME
// **************** END of R4 ****************
// **************** IXIA ****************
vm "IXIA_1" {
hostname "IXIA_1";
TG1_DISK
ncpus 8;
memory 2048;
interface "vio0" {EXTERNAL;};
interface "vio1" {bridge "private10";};
};
vm "IXIA_2" {
hostname "IXIA_2";
TG1_DISK
ncpus 8;
memory 2048;
interface "vio0" {EXTERNAL;};
interface "vio1" {bridge "private11";};
};
// **************** END of IXIA ****************
PRIVATE_BRIDGES
TOPOLOGY_END
