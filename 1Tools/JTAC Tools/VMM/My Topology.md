# VMX-VPTX(evo)-VMX  - pod8
![[Pasted image 20260224140541.png]]EVOVPTX_CONNECT(IF_ET(1, 0, 0), private20)  (GE(0, 0, 0) vmxDS1
EVOVPTX_CONNECT(IF_ET(1, 0, 1), private10)  (GE(0, 0, 1) vmxDS1
EVOVPTX_CONNECT(IF_ET(1, 0, 2), private11)  (GE(0, 0, 2) vmxDS1

EVOVPTX_CONNECT(IF_ET(1, 0, 3), private12)  (GE(0, 0, 0) vmxDS2 ae0
EVOVPTX_CONNECT(IF_ET(1, 0, 4), private13)  (GE(0, 0, 1) vmxDS2 ae0

VMX_CONNECT(GE(0, 0, 3), private42) (GE(0, 0, 2) vmxDS2
VMX_CONNECT(GE(0, 0, 4), private43) (GE(0, 0, 3) vmxDS2
VMX_CONNECT(GE(0, 0, 5), private44) (GE(0, 0, 4) vmxDS2

q-pod08-vmm.englab.juniper.net
/vmm/data/user_disks/annaw/sample/evo$
vmm config vbrackla_vmm_1.cfg 

Past config:
vbrackla_workshop.cfg
vmxDS1_workshop.cfg
vmxDS2_workshop.cfg


# Tester .pod13
/homes/rwei/subscribers$ cat simple.bras.pnj.vmm
vmm config /vmm/data/user_disks/annaw/sample/tester/tester.conf -g vmm-default
vmm config /homes/rwei/subscribers/simple.bras.pnj.vmm -g vmm-default
![[Pasted image 20260224140609.png]]r1
         VMX_CONNECT(GE(0, 0, 9), private7) --- VM1 em1
         VMX_CONNECT(XE(0, 2, 1), private3) --- r2 XE(0, 2, 1)
         VMX_CONNECT(XE(0, 3, 1), private4) --- r2 XE(0, 3, 1)
         VMX_CONNECT(XE(0, 3, 0), private2) --- q1 XE(0, 3, 0)
         VMX_CONNECT(XE(0, 2, 0), private1) --- q1 XE(0, 2, 0)

r2
         VMX_MPC_INSTANCE(r2_mpc, VMX_MPC_DISK, VMX_MPC_I2CID, 0)
         VMX_CONNECT(GE(0, 0, 0), private9) --- "IxiaLnxChSvr" "vio2"
         VMX_CONNECT(GE(0, 0, 9), private8) --- vm2 em1
q1
         VMX_MPC_INSTANCE(q1_mpc, VMX_MPC_DISK, VMX_MPC_I2CID, 0)
         VMX_CONNECT(GE(0, 0, 0), private6) --- IxiaLnxChSvr "vio1"
         VMX_CONNECT(GE(0, 0, 1), private37) --- vspirent em1
         VMX_CONNECT(GE(0, 0, 9), private10) --- vm1 em2
      

  vm "IxiaLnxAppSvr" {
     interface "vio0" { bridge "external"; };


  vm "IxiaLnxChSvr" {
     interface "vio0" { bridge "external"; };

   vm "vspirent" {
      interface "em0" { bridge "external"; };
   };
  vm "WindR_VM1" {
     interface "em0" { bridge "external"; };
  
  vm "WindR_VM2" {
     interface "em0" { bridge "external"; };
  };



# 6routers-tester
Segment Routing - 2025-1030-922101
![[Pasted image 20260224140714.png]]
vmm config /vmm/data/user_disks/annaw/sample/6routers-tester/6routers.conf -g vmm-default
![[Pasted image 20260224140835.png]]