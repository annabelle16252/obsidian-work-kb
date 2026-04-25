# Annabelle

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

Annabelle
https://junipernetworks.sharepoint.com/teams/ps/core-edge/Wiki/VMM%20Topologies.aspx
JNPRLAB PW: Monkey@1999nicetry Gemini@chatgpt.0217
Reset pw link:
easylink.juniper.net/jnprlabnet-reset-password
regress MaRtInI
root Embe1mpls
ssh annaw@q-pod22-vmm.englab.juniper.net
/vmm/data/base_disks/junos
/vmm/data/user_disks/annaw
/vmm/data/user_disks/annaw/sample/bbe$
q-pod05-vmm.englab.juniper.net
q-pod08-vmm.englab.juniper.net
​q-pod13-vmm.englab.juniper.net​
​q-pod21-vmm.englab.juniper.net
​sv8-pod1-vmm.englab.juniper.net
sv8-pod3-vmm.englab.juniper.net
Image:
annaw@q-pod08-vmm:/vmm/data/base_disks/junos$ ls
cbng  cmgd  cmx  cptx  crpd  csrx  vevo  vex  vmx  vptx  vqfx  vrr  vsrx
**vmm location和jtac tool是不相通的...，image不能存到jtactool上
Base config:
/volume/CSdata/annaw/vmm-config
nullconf.conf  vmx.base.systest.conf
启动VMM：
vmm capacity -g vmm-default   查看pod容量
vmm config config_file.vmm -g vmm-default
vmm bind
vmm start
vmm stop
vmm unbind
vmm ip
vmm list
vmm serial -t device_name -g vmm-default
Console: $ vmm serial -t vbrackla_RE0
我这个
Base config:
/vmm/data/base_disks/vmm-configs-vmm3/vmm-configs/bangalore/junos
# Ref. #
/homes/sseki/vm_cfg$
# Open case #
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
License
On MX router (root/Embe1mpls)
request system license add terminal
Paste licenses
Ctrl + D
------
E400185416 aeaqib qfdyps aijca4 ucj6zk asphea cetyor pnftdh lyf5zc o25zlj zqzuat deubib m65kso pbu6qa 6vs36c sci
E400907452 aeaqib qcsaea okbzxu yyz25s 54f54g 7n4lsf z4pdne ynlhky 4qikev 4virt2 7is56j 3fkcdq 7txmkt t35m
E401171420 aeaqib qfemsc kjrha4 uaqhw3 zx3f2n q7kllg e4bz7m m4pz4a 4tdoeh 3lc6jq ps3qdr oe4tlq 7hlkpl qvoout j3i
E401014968 aeaqib qbgids rusa2x vpkuhp fsrhtb ooozpe ovy24l 3eopgl mho4tg dn3zig dzw46t f6fswn rlvlnq mo
E403226704 aeaqib qbpqds rppgex rbngzc xzt7pg 6gohb3 hch5ri kpucef 6lxnrc a7ckd7 6vqkwz bsjljp yqr7pc 6z
E402027465 aeaqib qboyds qtrc36 dte5c7 mypddn djx4bd vn5rlh isaksj y364qw 2fzbcg zzmayk wo3nor yxnf4s zf
----
show system license to check
