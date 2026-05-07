![[Pasted image 20260507091955.png]]


# Script
ge-5/2/0(traffic interface backbone)
ge-5/3/0(spirent traffic) 

Run script on MX960:
1.deactivate protocols oam ethernet link-fault-management interface ge-6/0/1-> wait for 5s -> rollback 1 -> wait for 15s
2.ssh to peer device:10.219.22.245 -> show interfaces ge-5/3/0 extensive | match pps | refresh 5:
if output first two lines are not 0 -> run one more round script
if output first two lines are 0 -> stop script