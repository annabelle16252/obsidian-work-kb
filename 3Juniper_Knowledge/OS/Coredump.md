# Coredump

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

Coredump
/volume/case_2023
/volume/CSdata/annaw/case/
# ...解...core...方法... #
1. BAM  ...自动解core...
annaw@svl-jtac-lnx05:/volume/case_2023/2023-1108-006365$
~jtac/bin/bam core-NPC3.gz.core.0
~jtac/bin/bam /volume/ftp/pub/incoming/2014-1112-1111/xxxxx.gz
2. ...BUG Searcher
https://www-jtac.jnpr.net/bugsearcher/
这个有个好处，解出来的结果里可以点...BT...的字段，选“...get bug...”搜索包含相同...bt...结果的...PR
"Chmod 755"
3. Jdebug
/volume/buildtools/bin/jdebug
# ...用...jdebug tool ...解...core...方法：
annaw@svl-jtac-lnx05:/volume/CSdata/annaw/case/2022-0307-430519$
/volume/buildtools/bin/jdebug rpd.core-tarball.4.tgz
4. JRCA
只用于...PFE...的...core...，但...MS-DPC...不支持。这个工具可以模拟...FPC ...crash之前的状态。我们可以用各种shell下的show命令去尝试找到更多...FPC ...crash的trigger。
annaw@svl-jtac-lnx01:~$ cd /volume/wf-pfe-tools/bin...
annaw@svl-jtac-lnx01:/volume/wf-pfe-tools/bin$  ./jrca~ /homes/annaw/core-NPC4.gz.core.0
jrca> shell   <<<<...用这个就进入了...fpc shell mode
---------------
MX104-ABB-0(JPTYOATT1002E jrca uart)# show ?
show
access-security       Display Access Security information
adspmb                show ADS7830 Vout/Vin DC-DC converter
ael1010               show ael1010 phy information
nvram                 system NVRAM information
syslog                current syslog details
<snip..>
用...?...可以知道都有哪些available的命令
------------------
下面是看在...FPC ...crash前... ...jnh... ...memory的状态，从这个core看memory的使用率是高的。
NPC4(CE0000-BASD11 jrca uart)# show syslog message  <<< ...看之前的log
NPC4(CE0000-BASD11 jrca uart)# show nvram
<snip..>
Vmcore...能解出来，...Kernel core...可能解不出来报错
#  比较size #
如果...core...解不出来，用这个看...size...跟现网用这个命令试出来的...size...是否一样
> file checksum md5 + file name
# 产生 Coredump #
1. generate a live daemon coredump without restart
root@Router% gcore -s -c /var/tmp/<process_name.core.pid>  <pid>
generate a daemon coredump with restart
user@Router> request system core-dump <process name>
root@Router% kill -6 <PID>
2. generate a live kernel coredump without reboot
Way 1: kernel live core
% sysctl -a kern.live_core_dump
way2: vmcore live core
------------
> start shell user root
% sysctl -w kern.live_core_dump=1
kern.live_core_dump: 0 -> 1
root@jtac-mx104-r2007-re0%   chunk 0: 4182016 bytes ... ok
chunk 1: 2138112 bytes ... ok
chunk 2: 196608 bytes ... ok
chunk 3: 83222528 bytes /
<snip..>
chunk 231: 16384 bytes ... ok
chunk 232: 16384 bytes ... ok
Live Dump complete
------------
generate a kernel coredump (RE will reboot)
root@Router% sysctl -w debug.kdb.panic=1
3. generate a FPC/PFE/IQ2 coredump without reboot
FPC0(Router vty)# write coredump
generate a FPC/PFE/IQ2 coredump(CPU board will reboot)
FPC0(Router vty)# set parser security 15
Security level set to 15
FPC0(Router vty)# test panic
4. generate a MS/AS-PIC coredump(PIC will reboot)
way1:
sp43(atr0)# set parser security 15
Security level set to 15
sp43(atr0)# test panic
way2:
sp43(atr0)# write coredump
panic: cpu 0: coredump_manual_handler
notice:"write coredump" will cause MS/AS-PIC reset
5. generate rpd live core  (might trigger protocol flap)
gcore -s -c /var/tmp/rpd.livecore `cat /var/run/rpd.pid`
gcore  -c /var/tmp/rpd.core.5075 -s /usr/libexec64/rpd 5075
-rw-rw-rw-  1 root  wheel    8817191 Nov 10 07:36 /var/tmp/rpdlive.core.0.gz
6. generate a chassid live core during the problem state:
abroot@jtac-mx480-r2041-re0> start shell user root
Password:
root@jtac-mx480-r2041-re0:/var/home/labroot # ps -aux |egrep chassisd
root    29725   7.1  0.1  471368   69100  -  S    Wed17     206:28.96 /usr/sbin/chassisd -N
root    65319   0.0  0.0   11192    2516 u0  S+   14:16       0:00.00 egrep chassisd (grep)
root@jtac-mx480-r2041-re0:/var/home/labroot # gcore -c /var/tmp/chassisd.core.100 /usr/sbin/chassisd 29725
labroot@jtac-mx480-r2041-re0> show system core-dumps
/var/crash/*core*: No such file or directory
-rw-r--r--  1 root  wheel  102014976 Jul 8  14:21 /var/tmp/chassisd.core.100
