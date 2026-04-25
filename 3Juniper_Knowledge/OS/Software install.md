# Software install

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

Software install
> request system software add junos-evo-install-ptx-fixed-x86-64-20.1-202005231046.0-EVO.iso restart
For rollback only reboot option available：
>request system software rollback junos-evo-install-ptx-fixed-x86-64-21.1R1-202102080159.0-EVO
Can install multiple images and rollback between them
regress@vbrackla_RE0> show system software list 
Feb 13 16:17:06 
node: re0 
Active boot device is primary: /dev/vda 
List of installed version (s) : 
'-' running version 
'>' next boot version after upgrade/downgrade 
'<' rollback boot version 
junos-evo-install-ptx-fixed-x86-64-20.1-202005231046.0-EVO - [2021-02-12 17:25:46] 
junos-evo-install-ptx-fixed-x86-64-20.1-202005222056.0-EVO - [2021-02-12 16:53:16] 
1 1 
> 
junos-evo-install-ptx-fixed-x86-64-20.1-202005211346.0-EVO - [2021-02-13 15:59:32] ...regress@vbrackla_RE0> show system software list 
Feb 13 16:17:06 
node: re0 
Active boot device is primary: /dev/vda 
List of installed version (s) : 
'-' running version 
'>' next boot version after upgrade/downgrade 
'<' rollback boot version 
junos-evo-install-ptx-fixed-x86-64-20.1-202005231046.0-EVO - [2021-02-12 17:25:46] 
junos-evo-install-ptx-fixed-x86-64-20.1-202005222056.0-EVO - [2021-02-12 16:53:16] 
1 1 
> 
junos-evo-install-ptx-fixed-x86-64-20.1-202005211346.0-EVO - [2021-02-13 15:59:32]
root@vbrackla_RE0> request system software rollback 
node: re0 
Validating current config for rollback version junos-linux-install-ptx-x86-64-18.4-20180819.3 ...root@vbrackla_RE0> request system software rollback 
node: re0 
Validating current config for rollback version junos-linux-install-ptx-x86-64-18.4-20180819.3
Need to restart device to take this into effect
We could also "sync" the image to the other RE node - don't need to download the 
image again when we need to replace RE node. (sync is hidden on brackla) 
root@vbrackla-re0> request system software ? 
Possible completions : 
add 
Add extension or upgrade package 
delete 
Remove extension or upgrade package 
rollback 
Attempt to roll back to previous set of packages 
sync 
Sync software from master node to other node 
validate 
Verify package compatibility with current configuration ...We could also "sync" the image to the other RE node - don't need to download the 
image again when we need to replace RE node. (sync is hidden on brackla) 
root@vbrackla-re0> request system software ? 
Possible completions : 
add 
Add extension or upgrade package 
delete 
Remove extension or upgrade package 
rollback 
Attempt to roll back to previous set of packages 
sync 
Sync software from master node to other node 
validate 
Verify package compatibility with current configuration
Restart does not work and requires reboot if there are fundamental changes in YOCTO/JWRL/uswitchd/igmd
Software
Junos® OS Evolved Software Installation and Upgrade Guide
junos-evo-install-ptx-x86-64-20.4R2.13-EVO.iso
junos-evo-install-qfx-ms-x86-64-22.1R1-S1.2-EVO.iso
Evo on Nautilus (QFX5200)
{junos-linux-install}-{qfx-ms-fixed}-{x86-64}-{version}.iso
Evo on Brackla (QFX)
{junos-linux-install}-{qfx-fixed}-{x86-64}-{version}.iso
Evo on Brackla (PTX)
{junos-linux-install}-{ptx-fixed}-{x86-64}-{version}.iso
Evo on PTX5K
{junos-linux-install}-{ptx}-{x86-64}-{version}.iso
