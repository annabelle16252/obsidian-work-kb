# JUNOS

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

JUNOS
vmhost用下面命令在两个set之间切换版本...：
request vmhost software rollback
request vmhost reboot
<<<<
labroot@jtac-mx960-r2015-re0# run show vmhost version
Current root details,           Device sda, Label: jrootb_P, Partition: sda4
Current boot disk: Primary
Current root set: b
UEFI    Version: NGRE_v00.53.00.01
Primary Disk, Upgrade Time: Fri Nov 21 06:48:01 IST 2025
Version: set p
VMHost Version: 7.2752
VMHost Root: vmhost-x86_64-23.2R2-S3-20241205_0251_builder
VMHost Core: vmhost-core-x86-64-23.2R2-S3.8
kernel: 5.2.60-rt15-LTS19
Junos Disk: junos-install-mx-x86-64-23.2R2-S3.8
Version: set b
VMHost Version: 5.2276
VMHost Root: vmhost-x86_64-20.2R3-20210224_0019_builder
VMHost Core: vmhost-core-x86-64-20.2R3.9
kernel: 4.8.28-rt10-WR9.0.0.24_ovp
Junos Disk: junos-install-mx-x86-64-20.2R3.9
<<<<
# Snapshot #
Take a snapshot of the files currently used to run the device, and copy the files onto the alternate solid-state drive (SSD). The snapshot includes the complete contents of the /soft, /config, and /root directories, copies of user data, and content from the /var directory (except the /var/core, /var/external, /var/log, and /var/tmp directories).
# ZTP #
KB97458... ...Configuration wiped out after reboot
INFO: ZTP: Unable to lock config db retry config commit
Stop ZTP:
start shell
systemctl stop ztp
Config removal for ZTP:
delete system commit factory-settings
delete system phone-home
delete chassis auto-image-upgrade
JUNOS
# Snapshot #
Take a snapshot of the files currently used to run the device, and copy the files onto the alternate solid-state drive (SSD). The snapshot includes the complete contents of the /soft, /config, and /root directories, copies of user data, and content from the /var directory (except the /var/core, /var/external, /var/log, and /var/tmp directories).
# ZTP #
KB97458... ...Configuration wiped out after reboot
INFO: ZTP: Unable to lock config db retry config commit
Stop ZTP:
start shell
systemctl stop ztp
Config removal for ZTP:
delete system commit factory-settings
delete system phone-home
delete chassis auto-image-upgrade
