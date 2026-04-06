24.X以降はvFPC-lts19.vmdkを使わないと起動しない <<< vmx only

# VMX_MPC_I2CID

VMX_MPC_I2CID      0xBAA
VMX_EA_MPC_I2CID   0xC4A
VMX_XL_MPC_I2CID   0xB8A
VMX_XL_ETH_MPC_I2CID 0xC06
VMX_XL_100G_MPC_I2CID 0xB8C
VMX_ZT_MPC_I2CID   0xCD8

VREDBULL_I2CID      0x0CE7 /* I2C_ID_REDBULL_4000G_MPC */ MPC11

VALFAROMEO_FPC_I2CID  0x0D7A /* I2C_ID_ALFAROMEO_LC */
VMX10008_FPC_CHASSIS_MODEL 263 /* JNX_PRODUCT_MODEL_MX10008 */

I2C_ID_JNP10K_36QDD_LC  // 0x0D7A  JNP10K LC1201
FPC_I2CID  0x0CE7 /* I2C_ID_JNP10K_36QDD_LC */


# EA Chipset 
**MPC7/8/9 Stout***
6xQSFP MRATE PIC0 - port 0-5, Available interfaces: xe-0/0/x:0-5(10G). Once vmm started, et-0/0/0-5(40G) or et-0/0/2&et-0/2/5 (100G) can be configured under chassis level.
```
#undef VMX_CHASSIS_I2CID
#define VMX_CHASSIS_I2CID 21
#undef VMX_CHASSIS_NAME
#define VMX_CHASSIS_NAME r2

   VMX_CHASSIS_START()
      VMX_RE_START(r2_RE, 0)
      VMX_RE_INSTANCE(r2_RE, VMX_DISK2, VMX_RE_I2CID, 0)
      install "/vmm/data/base_disks/vmm-configs-vmm3/vmm-configs/bangalore/junos/vmx.base.systest.conf" "/root/junos.base.conf";
      VMX_RE_END
      VMX_MPC_START(r2_MPC, 0)
         VMX_MPC_INSTANCE(r2_MPC, VMX_MPC_DISK, VMX_EA_MPC_I2CID, 0)
            VMX_CONNECT(XE_CHAN(0, 0, 0, 0), private1)
      VMX_MPC_END
   VMX_CHASSIS_END

```

regress@r2_RE# run show chassis hardware 
Hardware inventory:
Item             Version  Part number  Serial number     Description
Chassis                                VM69AFAF3801      MX960
Midplane        
Routing Engine 0                       8fcd39c6-1c       RE-VMX
CB 0                                                     VMX SCB
FPC 0                     BUILTIN      BUILTIN           VMX EAGLE MPC
  CPU            Rev. 1.0 RIOT-LITE    BUILTIN          
  MIC 0                                                  MRATE-6xQSFPP-XGE-XLGE-CGE
    PIC 0                 BUILTIN      BUILTIN           MRATE-6xQSFPP-XGE-XLGE-CGE
# Trio Chipset
```
VMX_MPC_START(q1_mpc, 0)
VMX_MPC_INSTANCE(q1_mpc, VMX_MPC_DISK, VMX_MPC_I2CID, 0)
VMX_CONNECT(GE(0, 0, 0), private6)
VMX_CONNECT(GE(0, 0, 1), private37)
VMX_CONNECT(GE(0, 0, 9), private10)
VMX_CONNECT(XE(0, 2, 0), private1)
VMX_CONNECT(XE(0, 3, 0), private2)
VMX_MPC_END
VMX_CHASSIS_END

```

FPC 0                     BUILTIN      BUILTIN           Virtual FPC
  CPU            Rev. 1.0 RIOT-LITE    BUILTIN          
  MIC 0                                                               Virtual 20x 1GE(LAN) SFP
    PIC 0                 BUILTIN      BUILTIN           Virtual 10x 1GE(LAN) SFP
    PIC 1                 BUILTIN      BUILTIN           Virtual 10x 1GE(LAN) SFP
  MIC 1                                                               Virtual 4x 10GE(LAN) XFP
    PIC 2                 BUILTIN      BUILTIN           Virtual 2x 10GE(LAN) XFP
    PIC 3                 BUILTIN      BUILTIN           Virtual 2x 10GE(LAN) XFP

ge-x/0/0-9 ; ge-x/1/0-9 ; xe-x/2/0-1 ; xe-x/3/0-1
# YT Chipset MPC (valfromeo)
/vmm/data/user_disks/dhahm/valfaromeo:
/vmm/data/user_disks/sbairy/valfaromeo:
/vmm/data/user_disks/valfaromeo:
```
VMX10008 - JNP10K-LC9600
    VALFAROMEO_FPC_START_ (VMX10008_CHASSIS_NAME, VALFAROMEO_FPC0)

        #undef FPC_BOOT_ARGS
        #define FPC_BOOT_ARGS   " " /* Unused; for future expansions. */

        // These I2CIDs come from i2cid_def_shared.h
        #define VALFAROMEO_FPC_I2CID  0x0D7A /* I2C_ID_ALFAROMEO_LC */
        #define VMX10008_FPC_CHASSIS_MODEL 263 /* JNX_PRODUCT_MODEL_MX10008 */

        setvar "qemu_args" "-drive file=/vmm/data/user_disks/vredbull/boot/vREDBULL_OVMF_CODE.fd,if=pflash,format=raw,unit=0,readonly=on -drive file=/vmm/data/user_disks/vredbull/boot/vREDBULL_OVMF_VARS.fd,if=pflash,format=raw,unit=1,readonly=on";

        VALFAROMEO_FPC_INSTANCE_DISKLESS (VMX10008_CHASSIS_NAME, VMX10008_FPC_CHASSIS_MODEL, 
                                          VALFAROMEO_RE0, VALFAROMEO_FPC0, VALFAROMEO_FPC_I2CID, FPC_BOOT_ARGS)

        VALFAROMEO_CONNECT (IF_ET_CHAN (0, 0, 0, 0),  private1)
        VALFAROMEO_CONNECT (IF_ET_CHAN (0, 0, 0, 1),  private2)
        VALFAROMEO_CONNECT (IF_ET_CHAN (0, 0, 0, 2),  private3)
        VALFAROMEO_CONNECT (IF_ET_CHAN (0, 0, 0, 3),  private4)

        VALFAROMEO_CONNECT (IF_ET_CHAN (0, 0, 1, 0),  private5)
        VALFAROMEO_CONNECT (IF_ET_CHAN (0, 0, 1, 1),  private6)
        VALFAROMEO_CONNECT (IF_ET_CHAN (0, 0, 1, 2),  private7)
        VALFAROMEO_CONNECT (IF_ET_CHAN (0, 0, 1, 3),  private8)

        VALFAROMEO_CONNECT (IF_ET_CHAN (0, 0, 2, 0),  private9)
        VALFAROMEO_CONNECT (IF_ET_CHAN (0, 0, 2, 1),  private10)
        VALFAROMEO_CONNECT (IF_ET_CHAN (0, 0, 2, 2),  private11)
        VALFAROMEO_CONNECT (IF_ET_CHAN (0, 0, 2, 3),  private12)

        VALFAROMEO_CONNECT (IF_ET_CHAN (0, 0, 3, 0),  private13)
        VALFAROMEO_CONNECT (IF_ET_CHAN (0, 0, 3, 1),  private14)
        VALFAROMEO_CONNECT (IF_ET_CHAN (0, 0, 3, 2),  private15)
        VALFAROMEO_CONNECT (IF_ET_CHAN (0, 0, 3, 3),  private16)

    VALFAROMEO_FPC_END_
```


# ZT Chipset MPC
```
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
#define VMX_DISK basedisk "/vmm/data/user_disks/annaw/junos-virtual-x86-64-21.4R3.16.vmdk";

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
```

regress@r0_re> show chassis hardware 
Hardware inventory:
Item             Version  Part number  Serial number     Description
Chassis                                VM69B0C34D2F      MX480
Midplane        
Routing Engine 0                       00ed8684-1c       RE-VMX
CB 0                                                     VMX SCB
FPC 0                     BUILTIN      BUILTIN           VMX ZT MPC
  CPU            Rev. 1.0 RIOT-LITE    BUILTIN          
  MIC 0                                                  MRATE-5xQSFPP
    PIC 0                 BUILTIN      BUILTIN           MRATE-5xQSFPP


# Redbull - MPC11 (zt chip)

/vmm/data/user_disks/roselynl/q-pod13-vmm/redbull:

只有这个image可以正常显示interfaces： "/vmm/data/user_disks/annaw/junos-virtual-x86-64-21.2I-20210304_dev_common.0.0857.vmdk"
```

#include "/vmm/bin/common.defs"
#include "/vmm/data/user_disks/vredbull/common/common.vredbull.defs"
#define VMX_PHASE 3

#if VMX_PHASE == 3
#include "/vmm/data/user_disks/vmxc/common.vmx.p3.defs"
#elif VMX_PHASE == 2
#include "/vmm/data/user_disks/vmxc/common.vmx.p2.defs"
#else
#include "/vmm/data/user_disks/vmxc/common.vmx.p1.defs"
#endif

#define VMX_DISK1 basedisk "/vmm/data/user_disks/annaw/junos-virtual-x86-64-21.2I-20210304_dev_common.0.0857.vmdk";

TOPOLOGY_START(vmx_topology)

#undef VMX_CHASSIS_I2CID
#define VMX_CHASSIS_I2CID 21
#undef VMX_CHASSIS_NAME
#define VMX_CHASSIS_NAME dut


   VMX_CHASSIS_START()
      VMX_RE_START(dut_re0, 0)
         VMX_RE_INSTANCE(dut_re0, VMX_DISK1, VMX_RE_I2CID, 0)
         //install "/vmm/data/automation/fusion/vmm_config/20190725-14074-i3xgha.dut.tunnel" "/root/olive.conf";
         install "/vmm/data/user_disks/vredbull/conf/services" "/etc/services";
         install "/vmm/data/user_disks/vredbull/conf/inetd.conf" "/etc/inetd.conf";
         install "/vmm/data/user_disks/vredbull/bin/mpcsd_evo" "/usr/share/pfe/mpcsd_evo";
         install "/vmm/data/user_disks/vredbull/conf/pxe_slot_attrib.conf" "/usr/share/pfe/pxe_slot_attrib.conf";
         install "/vmm/data/user_disks/vredbull/conf/imgargs" "/root/imgargs";
         install "/volume/distfiles/cosim/ztcosim-20181213.tgz" "/var/tmp/zt_cosim.tgz";
         install "/vmm/data/user_disks/vredbull/conf/rpio_zt_lnx.conf" "/usr/share/pfe/rpio_zt_lnx.conf";
         install "/vmm/data/user_disks/vredbull/workaround/libcrypto.so.1.0.0" "/var/tmp/libcrypto.so.1.0.0";
         setvar "hgcomm_vector_idx" "3";
      VMX_RE_END
      VREDBULL_FPC_START_ (VMX_CHASSIS_NAME, VREDBULL_FPC0)
           #undef FPC_BOOT_ARGS
           #define FPC_BOOT_ARGS   " "

           #define FPC_I2CID  0x0CE7
           setvar "qemu_args" "-drive file=/vmm/data/user_disks/vredbull/boot/vREDBULL_OVMF_CODE.fd,if=pflash,format=raw,unit=0,readonly=on -drive file=/vmm/data/user_disks/vredbull/boot/vREDBULL_OVMF_VARS.fd,if=pflash,format=raw,unit=1,readonly=on";
           //setvar "delayed_start" "900";
           VREDBULL_FPC_INSTANCE_DISKLESS (VMX_CHASSIS_NAME, VMX_CHASSIS_I2CID,  VREDBULL_RE0, VREDBULL_FPC0, FPC_I2CID, FPC_BOOT_ARGS)
        //Interface dut-r1-0 (net-name connect1)
        //Interface dut-r1-1 (net-name connect2)
           VREDBULL_CONNECT (IF_ET (0, 0, 0), private1)
           VREDBULL_CONNECT (IF_ET (0, 0, 1), private2)
      VREDBULL_FPC_END_

      VREDBULL_FPC_START_ (VMX_CHASSIS_NAME, VREDBULL_FPC1)
           #undef FPC_BOOT_ARGS
           #define FPC_BOOT_ARGS   " "

           #define FPC_I2CID  0x0CE7
           setvar "qemu_args" "-drive file=/vmm/data/user_disks/vredbull/boot/vREDBULL_OVMF_CODE.fd,if=pflash,format=raw,unit=0,readonly=on -drive file=/vmm/data/user_disks/vredbull/boot/vREDBULL_OVMF_VARS.fd,if=pflash,format=raw,unit=1,readonly=on";
           //setvar "delayed_start" "900";
           VREDBULL_FPC_INSTANCE_DISKLESS (VMX_CHASSIS_NAME, VMX_CHASSIS_I2CID,  VREDBULL_RE0, VREDBULL_FPC1, FPC_I2CID, FPC_BOOT_ARGS)
        //Interface dut-r1-0 (net-name connect1)
        //Interface dut-r1-1 (net-name connect2)
           VREDBULL_CONNECT (IF_ET (1, 0, 0), private3)
           VREDBULL_CONNECT (IF_ET (1, 0, 1), private4)
      VREDBULL_FPC_END_
   VMX_CHASSIS_END

   PRIVATE_BRIDGES



regress@dut_re0> show chassis hardware 
Hardware inventory:
Item             Version  Part number  Serial number     Description
Chassis                                VM69ABFB0100      MX960
Midplane        
Routing Engine 0                       1f9566c4-1a       RE-VMX
CB 0                                                     VMX SCB
FPC 0                     BUILTIN      BUILTIN           MPC11E 3D MRATE-40xQSFPP
  CPU           
  MIC 0                   BUILTIN      BUILTIN           MRATE-5xQSFPP
    PIC 0                 BUILTIN      BUILTIN           MRATE-5xQSFPP
      Xcvr 0     REV 01   740-058732   1DJQA042004       QSFP-100GBASE-LR4
      Xcvr 1     REV 01   740-058732   1DJQA042004       QSFP-100GBASE-LR4
      Xcvr 2     REV 01   740-058732   1DJQA042004       QSFP-100GBASE-LR4
      Xcvr 3     REV 01   740-058732   1DJQA042004       QSFP-100GBASE-LR4
      Xcvr 4     REV 01   740-058732   1DJQA042004       QSFP-100GBASE-LR4
    
```
