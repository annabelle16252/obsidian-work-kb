# YT Chipset

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

YT Chipset
Command
MX304_Life_of_a_Packet training
Life of a packet inside YT chipset
Junos shell
LNX-FPC0(vty)# show mqss 1 wi stats port-group 1
AFT Cli
root@inet-pue-fuertes-97> start shell pfe network fpc0
root@inet-pue-fuertes-97-fpc0:pfe>
The MQSS’s CPQF block acts as an interface between fabric queueing subsystem and the fabric subsystem.
The MQSS’s DSTAT block is responsible for making the packet drop decisions and keeping track of the queue statistics.  It also maintains the total number of packets enqueued for the WAN and fabric schedulers.
From ingress PFE:
From egress PFE:
Ttrace
root@jtac-mx304-r2012-re0-fpc0:pfe> test jnh packet-via-dmem full-packet yes inst 1 enable
root@jtac-mx304-r2012-re0-fpc0:pfe> test jnh packet-via-dmem-capture inst 1 parcel-type-mask 0x3 match-string "0x0b0b0b03" offset 32
root@jtac-mx304-r2012-re0-fpc0:pfe> test jnh packet-via-dmem-capture inst 1 parcel-type-mask 0x0
root@jtac-mx304-r2012-re0-fpc0:pfe> test jnh packet-via-dmem-dump inst 1
