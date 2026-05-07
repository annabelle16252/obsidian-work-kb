![[Pasted image 20260507091955.png]]


# Script
ge-5/2/0(traffic interface backbone)
ge-5/3/0(spirent traffic) 

start shell
sh /var/tmp/mx960_oam_final.sh

```

The script runs on MX960 and loops indefinitely:

Disables 802.3ah OAM on ge-6/0/1 (configure private + commit), waits 5s.
Restores OAM via rollback 1 + commit, waits 15s.
Polls MX480 ge-5/3/0 pps every 5s via passwordless SSH:
5 consecutive all-zero samples → stops (traffic loss detected).
5 consecutive non-zero samples → proceeds to next round.
Repeats until traffic loss is confirmed. Ctrl-C to stop manually.



```
