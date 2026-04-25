# My-Topology

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

My-Topology
/vmm/data/user_disks/annaw/sample <<< my vmm topologies
/vmm/data/user_disks/annaw <<< images that doesnt exist in  /vmm/data/base_disks
/vmm/data/user_disks/annaw/vmm-config <<< base configuration
/vmm/data/base_disks:
centos  debian          freebsd  junos  old_images  sanity_test_images  staging  vmm-configs-devel
dbng    default_images  ixia     ligen  paragon     spirent             ubuntu   vmm-configs-vmm3
Base config:
/vmm/data/base_disks/vmm-configs-vmm3/vmm-configs/bangalore/junos
q-pod05-vmm.englab.juniper.net
q-pod08-vmm.englab.juniper.net   <<< ...VMX-VPTX(evo)-VMX
ssh sjiang@q-pod13-vmm.englab.juniper.net <<< 6 routers
ssh sjiang@q-pod21-vmm.englab.juniper.net
ssh sjiang@q-pod22-vmm.englab.juniper.net
ssh sjiang@q-pod23-vmm.englab.juniper.net
ssh sjiang@q-pod25-vmm.englab.juniper.net
ssh sjiang@q-pod26-vmm.englab.juniper.net
ssh sjiang@q-pod27-vmm.englab.juniper.net
ssh sjiang@q-pod29-vmm.englab.juniper.net
ssh sjiang@q-pod30-vmm.englab.juniper.net
ssh sjiang@q-pod32-vmm.englab.juniper.net
ssh sjiang@q-pod36-vmm.englab.juniper.net
ssh sjiang@q-pod38-vmm.englab.juniper.net
ssh sjiang@q-pod39-vmm.englab.juniper.net
一个月不动，topology会被自动撤销
在 VMM 3.0 架构中，系统提供的默认 MPC（也称为 FPC）镜像是针对较新版本的 Junos 优化的。
• 默认兼容性： VMM 3.0 的默认 MPC 镜像仅与 Junos OS 18.4 及更高版本兼容
使用 #define VIRTUAL_MPC_DISK VMPC_WRL6_IMAGE
# ...VMX-VPTX(evo)-VMX... #. pod8
3 connections 
ae0 
0 
vmxDS1 
vbrackla 
vmxDS2 
vMX960 
vPTX 
vMX960 ...3 connections 
ae0 
0 
vmxDS1 
vbrackla 
vmxDS2 
vMX960 
vPTX 
vMX960
EVOVPTX_CONNECT(IF_ET(1, 0, 0), private20)  (GE(0, 0, 0) vmxDS1
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
# Tester #.pod13
/homes/rwei/subscribers$ cat simple.bras.pnj.vmm
vmm config /vmm/data/user_disks/annaw/sample/tester/tester.conf -g vmm-default
vmm config /homes/rwei/subscribers/simple.bras.pnj.vmm -g vmm-default
em2 
VM1 
VM2 
em1 
em1 
009 
009 
021 
021 
000 
VIO2 
R1 
R2 
IXIA 
031 
031 
030 
020 
vio1 
030 
020 
009 
Q1 
000 
001 
em1 
SPIRENT ...em2 
VM1 
VM2 
em1 
em1 
009 
009 
021 
021 
000 
VIO2 
R1 
R2 
IXIA 
031 
031 
030 
020 
vio1 
030 
020 
009 
Q1 
000 
001 
em1 
SPIRENT
r1
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
# 6routers-tester #
Segment Routing - 2025-1030-922101
et-1/1/0 
r1 
ge-0/0/32 
xe-1/0/5:0 
ex4300-32f-01 
xe-0/1/6 
xe-0/1/7 
101.234.16.19/31 
singtel-mx10003 
et-1/1/1 101.234.3.136 
R3 
24.4R2-$1 
xe-0/0/32 
xe-0/0/33 
xe-0/1/3 
xe-1/0/5:1 
xe-1/0/1 
101.234.16.7/31 
xe-0/1/4 
101.234.16.21/31 
101.234.16.18/31 
mx204-01 
ge-5/1/4 
xe-0/0/7 
xe-0/0/8 
101.234.3.131 
xe-1/0/9 
ER 
xe-1/1/14 (ae12) 
24.2R2 
xe-1/1/18 
xe-1/1/1 
et-0/0/0 
xe-0/1/0 
2/1 
RR1 
101.234.16.6/31 
101.234.16.0/31 
101.234.16.4/31 
QSFP28-100G-CU1M 
Spirent 4U 
× 
X 
X 
x\ 
X 
xe-1/1/17 
Yge-5/1/5 
LR 
xe-1/1/15 (ae13) 
mx480-3d-02 
101.234.16.8/31 
et-0/0/0 
xe-0/1/0 
xe-1/1/19 
x 
101.234.3.130 
X 
24.2R2-S1 
SR 
101.234.16.1/31 
101.234.16.5/31 
212 
1X 
X 
xe-0/1/3 
R4 
xe-1/1/0 
xe-0/0/1 
101.234.16.9/31 
xe-1/1/7 101.234.16.12/31 
xe-1/0/0 
101.234.16.14/31 
xe-1/0/2 
mx204-02 
101.234.16.20/31 
xe-1/0/0 
xe-1/1/6 
101.234.3.132 
xe-1/1/10 101.234.16.13/31 
24.2R2 
r2 
xe-0/1/6 
xe-0/1/7 
xe-1/0/10 
xe-1/1/5 
xe-1/1/14 
xe-1/0/1 
101.234.16.16/31 
xe/1/1/16 
xe-1/1/0 
mx240-3d-01 
101.234.16.15/31 
101.234.3.135 
RR 2 
xe-1/0/11 
24.2R2-S1 
xe-1/1/15 
xe-1/1/1 
xe/1/1/17 
101.234.16.17/31 
ge-1/2/0 
mx240-3d-02 
101.234.3.128 
ge-1/3/8 
24.2R2-S1 
ge-1/3/8 ...et-1/1/0 
r1 
ge-0/0/32 
xe-1/0/5:0 
ex4300-32f-01 
xe-0/1/6 
xe-0/1/7 
101.234.16.19/31 
singtel-mx10003 
et-1/1/1 101.234.3.136 
R3 
24.4R2-$1 
xe-0/0/32 
xe-0/0/33 
xe-0/1/3 
xe-1/0/5:1 
xe-1/0/1 
101.234.16.7/31 
xe-0/1/4 
101.234.16.21/31 
101.234.16.18/31 
mx204-01 
ge-5/1/4 
xe-0/0/7 
xe-0/0/8 
101.234.3.131 
xe-1/0/9 
ER 
xe-1/1/14 (ae12) 
24.2R2 
xe-1/1/18 
xe-1/1/1 
et-0/0/0 
xe-0/1/0 
2/1 
RR1 
101.234.16.6/31 
101.234.16.0/31 
101.234.16.4/31 
QSFP28-100G-CU1M 
Spirent 4U 
× 
X 
X 
x\ 
X 
xe-1/1/17 
Yge-5/1/5 
LR 
xe-1/1/15 (ae13) 
mx480-3d-02 
101.234.16.8/31 
et-0/0/0 
xe-0/1/0 
xe-1/1/19 
x 
101.234.3.130 
X 
24.2R2-S1 
SR 
101.234.16.1/31 
101.234.16.5/31 
212 
1X 
X 
xe-0/1/3 
R4 
xe-1/1/0 
xe-0/0/1 
101.234.16.9/31 
xe-1/1/7 101.234.16.12/31 
xe-1/0/0 
101.234.16.14/31 
xe-1/0/2 
mx204-02 
101.234.16.20/31 
xe-1/0/0 
xe-1/1/6 
101.234.3.132 
xe-1/1/10 101.234.16.13/31 
24.2R2 
r2 
xe-0/1/6 
xe-0/1/7 
xe-1/0/10 
xe-1/1/5 
xe-1/1/14 
xe-1/0/1 
101.234.16.16/31 
xe/1/1/16 
xe-1/1/0 
mx240-3d-01 
101.234.16.15/31 
101.234.3.135 
RR 2 
xe-1/0/11 
24.2R2-S1 
xe-1/1/15 
xe-1/1/1 
xe/1/1/17 
101.234.16.17/31 
ge-1/2/0 
mx240-3d-02 
101.234.3.128 
ge-1/3/8 
24.2R2-S1 
ge-1/3/8
vmm config /vmm/data/user_disks/annaw/sample/6routers-tester/6routers.conf -g vmm-default
MX10003 
EX4300 
et-1/0/0 
xe-0/2/1 
MX240-01 
et-1/0/1 
xe-0/2/0 
MX480-02-3d 
xe-0/2/1 
RR1 
Text 
MX240-01-3d 
MX240-02 
xe-0/2/0 
xe-0/2/1 
xe-0/3/1 
MX240-02-3d 
RR2 ...MX10003 
EX4300 
et-1/0/0 
xe-0/2/1 
MX240-01 
et-1/0/1 
xe-0/2/0 
MX480-02-3d 
xe-0/2/1 
RR1 
Text 
MX240-01-3d 
MX240-02 
xe-0/2/0 
xe-0/2/1 
xe-0/3/1 
MX240-02-3d 
RR2
