
# vSpirient
**Manul:
https://junipernetworks.sharepoint.com/:p:/r/sites/Projects1/vmx/_layouts/15/Doc.aspx?sourcedoc=%7B46CF8460-95CF-4446-8BCF-F710C70002B5%7D&file=Module%202-%20Virtual%20Traffic%20Generator%20(vSpirent%20%26%20IxVM).pptx&action=edit&mobileredirect=true&DefaultItemOpen=1



```
# Wind River Linux
```
  vm "WindR_VM1" {
     hostname "WindR_VM1";
     WindR_VM1
     memory 8192;
     ncpus 4;
     interface "em0" { bridge "external"; };
     interface "em1" { bridge "private7"; };
     interface "em2" { bridge "private10"; };
  }; 


# vCentos

#include "/vmm/data/user_disks/vmxc/common.vmx.p3.defs"
#include "/vmm/bin/common.defs"
#define VCENTOS_DISK basedisk "/vmm/data/base_disks/centos/centos8.2.qcow2"

TOPOLOGY_START(topology)

vm "vcentos" {
    hostname "vcentos" ;
    VCENTOS_DISK;
    memory 16384;
    ncpus 8;

    setvar "+qemu_args" "-cpu host -enable-kvm";

    interface "em0" { bridge "external"; };
    interface "em1" { bridge "private11"; };
    interface "em2" { bridge "private12"; };
};

PRIVATE_BRIDGES
TOPOLOGY_END


```
bootdisk
```
#include "/vmm/data/user_disks/vmxc/common.vmx.p3.defs"
#include "/vmm/bin/common.defs"

TOPOLOGY_START(topology)

vm "srv" {
    hostname "srv" ;
    bootdisk "ENV(HOME)/vmm/centos9.qcow2";
    memory 16384;
    ncpus 8;

    setvar "+qemu_args" "-cpu host -enable-kvm";

    interface "em0" { bridge "external"; };
    interface "em1" { bridge "private11"; };
    interface "em2" { bridge "private12"; };
};

PRIVATE_BRIDGES
TOPOLOGY_END


# vUbuntu
```
#include "/vmm/data/user_disks/vmxc/common.vmx.p3.defs"
#include "/vmm/bin/common.defs"
#define UBUNTU_DISK basedisk "/vmm/data/base_disks/ubuntu/ubuntu-20.04.2.qcow2"

TOPOLOGY_START(topology)

vm "vubuntu" {
    hostname "vubuntu" ;
    UBUNTU_DISK;
    memory 16384;
    ncpus 8;

    setvar "+qemu_args" "-cpu host -enable-kvm";

    interface "em0" { bridge "external"; };
    interface "em1" { bridge "private11"; };
    interface "em2" { bridge "private12"; };
};

PRIVATE_BRIDGES
TOPOLOGY_END

```

bootdisk:
```
annaw@q-pod13-vmm:/net/10.51.248.230/vmm/user_disks/alexchen/vmm$ cat vubuntu.topo
#include "/vmm/data/user_disks/vmxc/common.vmx.p3.defs"
#include "/vmm/bin/common.defs"

TOPOLOGY_START(topology)

vm "srv" {
    hostname "srv" ;
    bootdisk "ENV(HOME)/vmm/ubuntu18.qcow2";
    // bootdisk_rw "ENV(HOME)/vmm/ubuntu18.qcow2";
    memory 16384;
    ncpus 8;

    setvar "+qemu_args" "-cpu host -enable-kvm";

    interface "em0" { bridge "external"; };
    interface "em1" { bridge "private11"; };
    interface "em2" { bridge "private12"; };
};

PRIVATE_BRIDGES
TOPOLOGY_END


```


# vIXIA

**Manul:
https://trollwiki.englab.juniper.net/mediawiki/index.php/Vixia-Linux-APP-Server
Vixia 9.3 Image :/vmm/data/base_disks/ixia/ixia-vm93-ch.qcow2
IXIA License Server: "q-pod-ixlic1.englab.juniper.net"
"Vixia Linux App Server" and "Vixia" login credential: admin / admin


https://junipernetworks.sharepoint.com/:w:/r/sites/routing_l2_collboration/_layouts/15/Doc.aspx?sourcedoc=%7B3F2283F0-346B-4D51-AB11-0CDCD1090037%7D&file=Accessing%20Virtual%20IXIA84%20Updated.docx&action=default&mobileredirect=true&DefaultItemOpen=1

***Access method
```
1.
ssh -N -L localhost:5281:q-pod-ixas34.englab.juniper.net:3389 10.32.192.46
<<<login with Windows PW and stay in stuck
2.
Start RDP(Windows APP) to 127.0.0.1:5281 
JNPRLAB PW:  Gemini@chatgpt.0217
3.Add license
q-pod21-vmm:~> vmm serial -t ixia-vm1  (admin/admin) Gemini@chatgpt.0217
# set license-server 10.49.32.221 '
# restart-service ixServer '
4.Once the license is set , login into vIXIA APP IxNetwork 
File --> Preference--> License Setting, set the license Server to 10.49.32.221. 




1.
ssh -N -L localhost:5281:q-pod-ixas34.englab.juniper.net:3389 10.32.192.46
<<<login with Windows PW and stay in stuck
2.Add license in WebUI:ANNAW01LNXAPPSVR 10.52.104.200
new Ixnetwork-> setting->add  license-server 10.49.32.221
3.Start RDP(Windows APP) to 127.0.0.1:5281 
JNPRLAB PW:  Gemini@chatgpt.0217
Open Ixnetwork 


Ixia download link:
https://support.ixiacom.com/version/ixnetwork-1000?utm_source=chatgpt.com
```

![[Pasted image 20260406173848.png]]

***VXM--VIXIA
```
#include "/vmm/data/user_disks/vmxc/common.vmx.p3.defs"
#include "/vmm/bin/common.defs"
#include "/vmm/data/vmm-configs/common/ixia/common.vixia.defs"

#define VMX_DISK1 basedisk "/vmm/data/user_disks/annaw/junos-virtual-x86-64-23.2R2-S1.3.vmdk";
#define VIXIA_AS_DISK basedisk "/vmm/data/base_disks/ixia/ixia-vm92-as.qcow2";
#define VIXIA_DISK basedisk "/vmm/data/base_disks/ixia/ixia-vm92-ch.qcow2";
#define VMX_PHASE 3
#if VMX_PHASE == 3
#include "/vmm/data/user_disks/vmxc/common.vmx.p3.defs"
#elif VMX_PHASE == 2
#include "/vmm/data/user_disks/vmxc/common.vmx.p2.defs"
#else
#include "/vmm/data/user_disks/vmxc/common.vmx.p1.defs"
#endif

//TOPOLOGY_START(vmx_topology)
config "config" {
display "NULL";


#undef VMX_CHASSIS_I2CID
#define VMX_CHASSIS_I2CID 33
#undef VMX_CHASSIS_NAME
#define VMX_CHASSIS_NAME r1

   VMX_CHASSIS_START()
      VMX_RE_START(r1_RE, 0)
      VMX_RE_INSTANCE(r1_RE, VMX_DISK1, VMX_RE_I2CID, 0)
      install "/vmm/data/base_disks/vmm-configs-vmm3/vmm-configs/bangalore/junos/vmx.base.systest.conf" "/root/junos.base.conf";
      VMX_RE_END
      VMX_MPC_START(r1_MPC, 0)
         VMX_MPC_INSTANCE(r1_MPC, VMX_MPC_DISK, VMX_MPC_I2CID, 0)
            VMX_CONNECT(GE(0, 0, 0), private1)
      VMX_MPC_END
   VMX_CHASSIS_END

//config "config" {
//display "NULL";
#undef VIXIA_AS_MEMORY
#define VIXIA_AS_MEMORY 8192
#undef VIXIA_AS_NCPU
#define VIXIA_AS_NCPU 4
#undef VIXIA_AS_DISK
#define VIXIA_AS_DISK /vmm/data/base_disks/ixia/ixia-vm92-as.qcow2
  VIXIA_APPSERVER_START(APP0, VIXIA_AS_DISK )
  VIXIA_APPSERVER_END
#undef VIXIA_MEMORY
#define VIXIA_MEMORY 4096
#undef VIXIA_NCPU
#define VIXIA_NCPU 4
#undef VIXIA_DISK
#define VIXIA_DISK /vmm/data/base_disks/ixia/ixia-vm92-ch.qcow2
  VIXIA_CHASSIS_START ( ixia, VIXIA_DISK )
     //Interface VIXIA_CONNECT VIO_1 (net-name private1)
     VIXIA_CONNECT(VIO(1), private1)
  VIXIA_CHASSIS_END
  PRIVATE_BRIDGES
};
//   PRIVATE_BRIDGES
//TOPOLOGY_END
```





***单独一台VIXIA
```

#include "/vmm/bin/common.defs"
#include "/vmm/data/vmm-configs/common/ixia/common.vixia.defs"
config "config" {
display "NULL";
#undef VIXIA_AS_MEMORY
#define VIXIA_AS_MEMORY 8192
#undef VIXIA_AS_NCPU
#define VIXIA_AS_NCPU 4
#undef VIXIA_AS_DISK
#define VIXIA_AS_DISK /vmm/data/base_disks/ixia/ixia-vm92-as.qcow2
  VIXIA_APPSERVER_START(APP0, VIXIA_AS_DISK )
  VIXIA_APPSERVER_END
#undef VIXIA_MEMORY
#define VIXIA_MEMORY 4096
#undef VIXIA_NCPU
#define VIXIA_NCPU 4
#undef VIXIA_DISK
#define VIXIA_DISK /vmm/data/base_disks/ixia/ixia-vm92-ch.qcow2
  VIXIA_CHASSIS_START ( RT0, VIXIA_DISK )
     //Interface connect1 (net-name connect1)
     VIXIA_CONNECT(VIO(1), private1)
     //Interface connect2 (net-name connect2)
     VIXIA_CONNECT(VIO(2), private2)
     //Interface connect3 (net-name connect3)
     VIXIA_CONNECT(VIO(3), private3)
     //Interface connect4 (net-name connect4)
     VIXIA_CONNECT(VIO(4), private4)
  VIXIA_CHASSIS_END
#undef VIXIA_MEMORY
#define VIXIA_MEMORY 4096
#undef VIXIA_NCPU
#define VIXIA_NCPU 4
#undef VIXIA_DISK
#define VIXIA_DISK /vmm/data/base_disks/ixia/ixia-vm92-ch.qcow2
  VIXIA_CHASSIS_START ( RT1, VIXIA_DISK )
     //Interface connect1 (net-name connect1)
     VIXIA_CONNECT(VIO(1), private1)
     //Interface connect2 (net-name connect2)
     VIXIA_CONNECT(VIO(2), private2)
     //Interface connect3 (net-name connect3)
     VIXIA_CONNECT(VIO(3), private3)
     //Interface connect4 (net-name connect4)
     VIXIA_CONNECT(VIO(4), private4)
  VIXIA_CHASSIS_END
  PRIVATE_BRIDGES
};
```











