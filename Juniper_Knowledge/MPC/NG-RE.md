# NG-RE

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

NG-RE
The NG-RE has a virtualized architecture where Junos OS runs as a virtual machine over a Linux-based host (VMHost).
Junos OS VM
Linux-based host (VMHost)
RE-S-1800x4 <<< normal RE, junos
RE-S-2X00x6 <<< NGRE, vmhost
On normal RE’s, the VMHost commands will not work:
> show vmhost           ^
如果RE varlog里找不到问题...，去vmhost的varlog里看看
KB31910 [MX/PTX] How to collect "/var/log" files from Next Generation Routing Engine (NG-RE)
To collect /var/log from Junos OS VM, use the following command to zip all files under /var/log and dump it as /var/tmp/re0-var-log-20240125.tgz .
user@router-re0> file archive compress source /var/log/* destination /var/tmp/re0-var-log-20240125.tgz
Steps to collect /var/log/ from Linux-based host (VMHost):
1. Log in to Junos OS shell.
user@router-re0> start shell% suPassword:root@router-re0:~#
2. Log in to VMhost.
root@router-re0:/var/log # vhclient -sLast login: Mon Jan 25 22:26:19 2024 from 192.168.1.2
3. Tar and zip the content.
root@router-re0:~# cd /var/logroot@router-re0-node:~# tar -zcvf re0-vmhost-var-log-20240125.tgz /
