# vIXIA

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

vIXIA
vm "IxiaLnxAppSvr" {
hostname "IxiaLnxAppSvr";
basedisk "/vmm/data/base_disks/ixia/ixia-vm90-as.qcow2";
memory 8192;
ncpus 4;
interface "vio0" { bridge "external"; };
setvar "use_virtio_disk" "1";
setvar "disk_format" "qcow2";
setvar "use_dhcp" "1";
};
vm "IxiaLnxChSvr" {
hostname "IxiaLnxChSvr";
basedisk "/vmm/data/base_disks/ixia/ixia-vm90-ch.qcow2";
memory 4096;
ncpus 4;
interface "vio0" { bridge "external"; };
//Interface connect1 (net-name r1connect1)
interface "vio1" { bridge "private6"; };
interface "vio2" { bridge "private9"; };
setvar "use_virtio_disk" "1";
setvar "disk_format" "qcow2";
setvar "use_dhcp" "1";
};
