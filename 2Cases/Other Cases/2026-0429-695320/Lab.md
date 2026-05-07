![[Pasted image 20260507091955.png]]


# Script
ge-5/2/0(traffic interface backbone)
ge-5/3/0(spirent traffic) 

start shell
sh /var/tmp/mx960_oam_final.sh

```

The script runs on the MX960 and loops indefinitely until all peer pps values drop to 0:

1. Disable OAM — Deactivates 802.3ah link-fault-management on ge-6/0/1 via "configure private" + commit.
2. Wait 5 seconds for the OAM adjacency to drop.
3. Restore OAM — Runs "rollback 1" + commit to re-activate the original OAM config.
4. Wait 15 seconds for OAM to re-establish and traffic to reconverge.
5. Poll peer MX480 pps — SSHs (passwordless, using an embedded RSA key) to MX480 and runs "show interfaces ge-5/3/0 extensive | match pps" to extract all pps values (input/output at physical and logical levels).
6. Stability check — Repeats the pps query every 5 seconds until 3 consecutive identical samples are observed (up to 30 attempts).
7. Zero check — If all stable pps values are 0, the script stops immediately (traffic loss detected). Otherwise, it proceeds to the next round.

The script runs until pps drops to 0 — there is no round limit. Use Ctrl-C to stop manually if needed.

```
