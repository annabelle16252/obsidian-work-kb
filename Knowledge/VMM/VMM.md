https://junipernetworks.sharepoint.com/teams/ps/core-edge/Wiki/VMM%20Topologies.aspx

**Running VMM Topologies for annaw:
https://vmm.englab.juniper.net/user/annaw

JNPRLAB PW:  Gemini@chatgpt.0217
Reset pw link:
easylink.juniper.net/jnprlabnet-reset-password

regress MaRtInI
root Embe1mpls


**Base Disk
 /vmm/data/base_disks
 Base config:
 /vmm/data/vmm-configs/bangalore/junos
 
/vmm/data/user_disks/annaw/sample

ssh annaw@q-pod22-vmm.englab.juniper.net

q-pod05-vmm.englab.juniper.net
q-pod08-vmm.englab.juniper.net
​q-pod13-vmm.englab.juniper.net​
​q-pod21-vmm.englab.juniper.net
​sv8-pod1-vmm.englab.juniper.net
sv8-pod3-vmm.englab.juniper.net

# VMM Command
vmm capacity -g vmm-default   <<< 查看pod容量
vmm config config_file.vmm -g vmm-default
vmm bind
vmm start
vmm stop
vmm unbind
vmm ip
vmm list

vmm serial -t device_name -g vmm-default
Console: $ vmm serial -t vbrackla_RE0

# Image 
annaw@q-pod08-vmm:/vmm/data/base_disks/junos$ ls
cbng  cmgd  cmx  cptx  crpd  csrx  vevo  vex  vmx  vptx  vqfx  vrr  vsrx
vmm location和jtac tool是不相通的，image不能存到jtactool上


# Base Config
/vmm/data/base_disks/vmm-configs-vmm3/vmm-configs/bangalore/junos
/volume/CSdata/annaw/vmm-config

install "/homes/sseki/vmx.base.systest.conf" "/root/junos.base.conf";

Junosで使うconfigは/config/juniper.conf.gz, junos.base.confは起動時にVMMサーバーがスクリプトで読み込んでデフォルトコンフィグをアップデートする


# Open Case
coretech-jira-support@juniper.net;GLO-all@juniper.net
JIRA
<<<
1. Login to https://coretech-jnpr.atlassian.net/jira/for-you?visitedUserSeg=true  using your Windows credentials. 
2. From the top menu bar, select Create. The Create Issue pop-up window appears. 
3. In the Create Issue pop-up window, select the following options: 
• Project–Select GLO Lab Support from the drop-down menu. 
• Issue Typpe–Select Virtual JUNOS Pod support from the drop-down menu. 
• Name of POD–Specify the name of the POD your VM topology exists. 
• VMM UUID–Include the output of the vmm uuid command. 
• Environment–Secify the method of deployment used to bring up the VM topology: 
For Pbuilder - Include params and environmet settings. 
For Fusion - Include fusion job ID and URL. 
• Keyword–Breifly describe your issue. 
• Summary–Provide a short description of your issue. 
• Description–Provide a detailed description of the issue. Ensure you add all relevant information needed to 
resolve the issue, such as the path to the VMM configuration file in use and / or steps to reproduce (if this is 
clearly reproducible). 
• Attachment–Attach any error messages or screen shot of errors being reported. Include the output of the vmm 
args command. 
• Watchers–You can optionally add usernames of people who would like to be notified on the ticket resolution 
process.37 
4. Select the Create option to submit the issue. A GLO ID is assigned to your issue and you will receive an email from 
Engineering Support Request System on successful submission.
<<<


# VMM3.0
在 VMM 3.0 架构中，系统提供的默认 MPC（也称为 FPC）镜像是针对较新版本的 Junos 优化的。
默认兼容性： VMM 3.0 的默认 MPC 镜像仅与 Junos OS 18.4 及更高版本兼容
使用 # define VIRTUAL_MPC_DISK VMPC_WRL6_IMAGE



# Full list of supported models:
```
Device model [ MX10003 ] not supported. Supported models :[SUPPORTEDPIC, VMX, VBGPROUTESERVER, VMX-BASE, VMX86, VMX960, VMX480, VMX240, VMX2010C, VMX2008, VMX2010, VMX2020, VMX10008, VMX10004, VMX304, VMX301, VMX9608, VJPG, VJX, VEX, VEX9214, VEX9208, VEX9204, VJUNOS-MX, VJUNOS-EX, VJUNOS-EVO, CJUNOS-EVO, VJUNOS-EVO-BX, CJUNOSEVO-CLAB, VJUNOS-EVO-BX, VPTX, EVOVPTX, EVOVPTX1SNG, EVOVPTX2SNG, EVOVPTX3SNG, EVOVPTX4SNG, EVOVPTX1HEN, EVOVPTX2HEN, EVOVPTX3HEN, EVOVPTX4HEN, EVOVPTX1POL, EVOVPTX2POL, EVOVPTX3POL, EVOVPTX4POL, EVOVPTX1OME, EVOVPTX2OME, EVOVPTX3OME, EVOVPTX4OME, VSCAPA, VLIGENPLUSEVO, VSCAPA16, VBOWMORE, VKX, PTX-VSONIC, VQFX10, VQFXTVP, VQFX12, VVALE, VPTX10008, VQFX5200, VBRACKLA, VSRXHA, VSRXVIO, OLIVE, VRR, LINUX, LIGEN, B4-CONTROLLER, NORTHSTAR, APSTRA, ODL, PARAGON-K8-CONTROLLER, PARAGON-WORKER, PARAGON-MASTER, BCS-JUMPHOST, BCS-NODE, UI-NODE-LINUX, ATOM-MASTER, ATOM-WORKER, PAA-CONTROL-CENTER, PAA-TEST-AGENT, IXAPPSERVER, VSPIRENT, VIXIA, IXVMCHASSISSERVER, IXVM, IXNEWVM, FREEBSD, VEVO, VATTELLA, VSAPPHIRE, VARDBEG, VBALERION, VBALERION-2BX, VWHISTLER, VSPECTROLITE, UBUNTU, CRPD, CBNG, CSRX, NCPTX, NCPTX-SETUP, CPTX, CIXIA-KNE, CPTX-SETUP, CIXIA-DOCKER, CMGD, CPTX, CMX, VOCTOMORE, PCMGD, SDV44, VNFX, ]
```


![[Pasted image 20260310152738.png]]
