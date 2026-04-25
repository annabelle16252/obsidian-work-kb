# RPKI SERVER

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

RPKI SERVER
https://routinator.docs.nlnetlabs.nl/en/stable/installation.html
https://www.shellop.com/archives/97
https://labs.ripe.net/author/tashi_phuntsho_3/how-to-install-an-rpki-validator/
Ste...p 1 configure server interface connection with vmx
SERVER eth2 192.168.0.1 ------ 192.168.0.2 ge-0/0/3 vmx
Router side
set interfaces ge-0/0/3 unit 0 family inet address 192.168.0.2/24
set routing-options validation group test session 192.168.0.1 refresh-time 300
set routing-options validation group test session 192.168.0.1 hold-time 600
set routing-options validation group test session 192.168.0.1 record-lifetime 360
set routing-options validation group test session 192.168.0.1 port 3323
set routing-options validation group test session 192.168.0.1 local-address 192.168.0.2
Server side
Root
Embe1mpls​
nmcli c delete "Wired connection 1"
nmcli c add con-name link1 type ethernet ifname eth2 ip4 1.0.0.100/24 gw4 1.0.0.1 ip6 2001::100/64 gw6 2001::1
nmcli c add con-name link1 type ethernet ifname eth2 ip6 2001::100/64 gw6 2001::1
nmcli c up link1
sudo sed -i -e "s|mirrorlist=|#mirrorlist=|g" /etc/yum.repos.d/CentOS-*
sudo sed -i -e "s|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g" /etc/yum.repos.d/CentOS-*
yum clean all && yum makecache
yum -y install vim
vim /etc/yum.repos.d/nlnetlabs.repo
Add below:
[nlnetlabs]name=NLnet Labsbaseurl=https://packages.nlnetlabs.nl/linux/centos/$releasever/main/$basearchenabled=1
sudo rpm --import https://packages.nlnetlabs.nl/aptkey.asc
sudo yum install -y routinator
Confirm the status:
sudo systemctl status routinator
Enable validation session:
routinator server --rtr 192.168.0.1:3323
Need wait for 20 minutes for the record to be synced
[edi t] 
Sessi on 
192.168.@.1 
run 
show 
validation 
sessi on 
State 
Up 
Flaps 
Uptime #1Pv4/IPv6 records 
425317/103111 ...[edi t] 
Sessi on 
192.168.@.1 
run 
show 
validation 
sessi on 
State 
Up 
Flaps 
Uptime #1Pv4/IPv6 records 
425317/103111
To install a Routinator package, you need Red Hat Enterprise Linux (RHEL) 7, 8 or 9, or compatible operating system such as Rocky Linux. Packages are available for the... ...amd64/...x86_64... architecture only.
First create a file named... .../etc/yum.repos.d/nlnetlabs.repo, enter this configuration and save it:
[nlnetlabs]name=NLnet Labsbaseurl=https://packages.nlnetlabs.nl/linux/centos/$releasever/main/$basearchenabled=1
Add the GPG key from NLnet Labs:
sudo rpm --import https://packages.nlnetlabs.nl/aptkey.asc
You can now install Routinator with:
sudo yum install -y routinator
After installation Routinator will run immediately as the user... ...routinator... ...and be configured to start at boot. By default, it will run the RTR server on port 3323 and the HTTP server on port 8323. These, and other values can be changed in the... ...configuration file located in... .../etc/routinator/routinator.conf.
You can check the status of Routinator with:
sudo systemctl status routinator
You can view the logs with:
sudo journalctl --unit=routinator
From <https://routinator.docs.nlnetlabs.nl/en/stable/installation.html>
Cedi t] 
labroot@rl_re# 
Sessi on 
192.168.@.1 
run 
show 
validation 
session 
State 
Up 
Flaps 
2 
Uptime #1Pv4/IPv6 records ...Cedi t] 
labroot@rl_re# 
Sessi on 
192.168.@.1 
run 
show 
validation 
session 
State 
Up 
Flaps 
2 
Uptime #1Pv4/IPv6 records
routinator --extra-tals-dir /root/tals server --rtr 192.168.0.1:3323
routinator server --rtr 192.168.0.1:3323
Enable BGP route validation on router
=======================================================================
policy-options {
policy-statement validation {
term valid {
from {
protocol bgp;
validation-database valid;
}
then {
validation-state valid;
community add origin-validation-state-valid;
accept;
}
}
}
community origin-validation-state-valid members 0x4300:0.0.0.0:0;
}
protocols {
bgp {
import validation;
}
}
=======================================================================
2.16.122.0/24-24           21342 192.168.0.1                             valid
labroot@r1_re# run show route 2.16.122.0
inet.0: 11 destinations, 11 routes (11 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both
2.16.122.0/24      *[BGP/170] 00:29:44, localpref 100
AS path: 21342 I, validation-state: valid
>  to 12.0.0.2 via ge-0/0/0.0
[edit]
labroot@r1_re# run show bgp validation statistics
Policy origin-validation re-evaluation statistics: 2
Attempts resulting Valid: 2
Attempts resulting Invalid: 0
Attempts resulting Unknown: 0
inet.0, 0
inet6.0, 0
BGP import policy reevaluation notifications: 0
1.179.0.0/20-20             9723 192.168.0.1                             valid
[edit]
labroot@r1_re# run show route
inet.0: 5 destinations, 5 routes (5 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both
10.0.0.0/8         *[Static/5] 19:52:16
>  to 10.49.31.254 via fxp0.0
10.49.0.0/19       *[Direct/0] 19:52:16
>  via fxp0.0
10.49.22.132/32    *[Local/0] 19:52:16
Local via fxp0.0
192.168.0.0/24     *[Direct/0] 19:50:00
>  via ge-0/0/3.0
192.168.0.2/32     *[Local/0] 19:50:00
Local via ge-0/0/3.0
RPKI-INSTANCE.inet.0: 3 destinations, 3 routes (3 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both
11.22.11.22/32     *[BGP/170] 00:00:07, localpref 100
AS path: 12345 I, validation-state: unknown
>  to 12.0.0.2 via ge-0/0/0.0
12.0.0.0/24        *[Direct/0] 19:30:39
>  via ge-0/0/0.0
12.0.0.1/32        *[Local/0] 19:30:39
Local via ge-0/0/0.0
inet6.0: 1 destinations, 1 routes (1 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both
ff02::2/128        *[INET6/0] 19:52:16
MultiRecv
RPKI-INSTANCE.inet6.0: 1 destinations, 1 routes (1 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both
ff02::2/128        *[INET6/0] 19:30:39
MultiRecv
[edit]
labroot@r1_re# save init
Wrote 177 lines of configuration to 'init'
[edit]
labroot@r1_re#
[edit]
labroot@r1_re#
[edit]
labroot@r1_re# set routing-options validation static record 11.22.11.22 maximum-length 32 origin-autonomous-system 12345 validation-state valid
[edit]
labroot@r1_re# commit
commit complete
[edit]
labroot@r1_re# run show route 11.22.11.22
RPKI-INSTANCE.inet.0: 3 destinations, 3 routes (3 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both
11.22.11.22/32     *[BGP/170] 00:00:05, localpref 100
AS path: 12345 I, validation-state: valid
>  to 12.0.0.2 via ge-0/0/0.0
[edit]
labroot@r1_re# run show validation database
RV database: default
Prefix                 Origin-AS Session                                 State   Mismatch
11.22.11.22/32-32          12345 internal                                valid
IPv4 records: 1
IPv6 records: 0
[edit]
labroot@r1_re# delete rouo
^
syntax error.
labroot@r1_re# delete rouo
^
syntax error.
labroot@r1_re# delete routing-options validation static
[edit]
labroot@r1_re# set rouo
^
syntax error.
labroot@r1_re# set routing-options validation static record 11.22.11.22 maximum-length 32 origin-autonomous-system 54321 validation-state valid
[edit]
labroot@r1_re# commit
commit complete
[edit]
labroot@r1_re# run show validation database
RV database: default
Prefix                 Origin-AS Session                                 State   Mismatch
11.22.11.22/32-32          54321 internal                                valid
IPv4 records: 1
IPv6 records: 0
[edit]
labroot@r1_re# run show route 11.22.11.22
RPKI-INSTANCE.inet.0: 3 destinations, 3 routes (3 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both
11.22.11.22/32     *[BGP/170] 00:00:56, localpref 100
AS path: 12345 I, validation-state: valid
>  to 12.0.0.2 via ge-0/0/0.0
[edit]
labroot@r1_re# set rouo
^
syntax error.
labroot@r1_re# set routing-options validation ni
^
syntax error.
labroot@r1_re# set routing-options validation notification-rib RPKI-INSTANCE.inet.0
[edit]
labroot@r1_re# commit
commit complete
[edit]
labroot@r1_re# run show route 11.22.11.22
RPKI-INSTANCE.inet.0: 3 destinations, 3 routes (3 active, 0 holddown, 0 hidden)
+ = Active Route, - = Last Active, * = Both
11.22.11.22/32     *[BGP/170] 00:00:08, localpref 100
AS path: 12345 I, validation-state: invalid
>  to 12.0.0.2 via ge-0/0/0.0
[edit]
labroot@r1_re#
