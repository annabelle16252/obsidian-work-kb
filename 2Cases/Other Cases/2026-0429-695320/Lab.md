![[Pasted image 20260507091955.png]]


# Script
ge-5/2/0(traffic interface backbone)
ge-5/3/0(spirent traffic) 

sh /var/tmp/mx960_oam_toggle.sh

```
The script is designed to run from the MX960 local shell and automate repeated OAM flap testing against the peer MX480.

It performs these actions in each round:

1. It starts on the MX960 and establishes an SSH control session to the peer device labroot@10.219.22.245. You enter the peer password once at the beginning, and the script reuses that session for all later peer checks.
2. It locally deactivates protocols oam ethernet link-fault-management interface ge-6/0/1 on the MX960 and commits that change.
3. It waits 5 seconds after the OAM deactivation commit.
4. It performs rollback 1 on the MX960 to restore the previous OAM configuration and commits that rollback.
5. It waits 15 seconds after the rollback commit.
6. It connects to the peer MX480 and runs show interfaces ge-5/3/0 extensive | match pps | no-more.
7. It extracts all pps values from that command output, not just the first two lines.
8. It keeps polling the same peer command every 5 seconds until it gets at least 3 consecutive samples where the full set of pps values is exactly identical. That is the script’s definition of a stable output.
9. If every pps value in that stable sample is 0, the script stops.
10. If any pps value in that stable sample is non-zero, the script starts another round and repeats the full OAM deactivate/restore sequence.

By default, it will keep doing this until all stable peer pps values become zero, or until it hits the configured maximum round limit.
```
