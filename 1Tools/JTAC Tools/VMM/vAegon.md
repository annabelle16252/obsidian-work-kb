
VScapa BX(Aegon LC) EVOwiki:
https://evowiki.juniper.net/index.php/VScapa_BX

Ref:
/vmm/data/user_disks/svignesh/vAegon


LC1301目前pbuilder无法自动跑，把一下prompt给copilot，让他手写vmm config：

-----
Topology：几台路由器，每台几个 FPC，类型（LC1301=AEGON/BX，LC1201=SCAPA/BT）
Pod：q-pod13-vmm
参考文件：/vmm/data/user_disks/svignesh/vAegon/pbuild.conf 和 /vmm/data/user_disks/vptxc/common.evovscapabx.compat.defs
ISO：/vmm/data/user_disks/svignesh/vAegon/cspp-integration-fpc/phase2/junos-evo-install-ptx-x86-64-25.3-202503290457.0-EVO.iso

------

# vmm
annaw@q-pod13-vmm:~/sample/vaegon_vscapa_mixed$ 
r1(LC1301+LC1301)--r2(LC1301+LC1201)--r3(LC1301+LC1201)
r1_RE0 10.49.237.1
r1_FPC0_CSPP 10.49.236.233
r1_FPC1_CSPP 10.49.236.247
r2_RE0 10.49.240.211
r2_FPC0_CSPP 10.49.237.49
r2_FPC1_CSPP 10.49.243.199
r3_RE0 10.49.247.37
r3_FPC0_CSPP 10.49.224.236
r3_FPC1_CSPP 10.49.232.85

![[Pasted image 20260418172924.png]]
