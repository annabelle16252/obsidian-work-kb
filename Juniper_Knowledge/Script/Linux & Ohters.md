# Linux & Ohters

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

Linux & Ohters
# ...打印... #
***...列的定义是以一行中空格为间隔算的
annaw@svl-jtac-lnx05:~$ cat temp.txt
1. aaa 111:100 aaa
2. bbb 222:200 bbb
3. ccc 333:300 ccc
annaw@svl-jtac-lnx05:~$ cat temp.txt | awk '{print $3}' <<<...取值第三列
111:100
222:200
333:300
annaw@svl-jtac-lnx05:~$ cat temp.txt | awk '{print $3 $4}'<<<...取值第三和四列
111:100aaa
222:200bbb
333:300ccc
annaw@svl-jtac-lnx05:~$ cat temp.txt | awk '{print $3}' | awk -F ":" '{print $2}' <<<...取值第三列以...":"...为分隔符的第二列
100
200
300
# sort...命令... #
把内容按照字段相同的排序出来：
annaw@svl-jtac-lnx05:~$ cat aaa.txt | sort
A 1
B 3
C 5
D 2
F 8
# uniq -c ...命令... #
找出有重复的，... -c...是...count...，并显示重复次数
$ cat 20220309-MX-OC-03-sockmon.txt| egrep -i route | grep inet | awk '{ print $7 }'| sort | uniq -c
58 42.61.91.188
51 42.61.91.189
43 42.61.91.190
44 58.185.15.164
43 58.185.235.11
53 58.185.235.12
53 58.185.235.13
53 58.185.235.14
36 58.185.39.51
55 58.185.39.52
49 58.185.39.53
45 58.185.39.54
目录下找文件夹：
annaw@svl-jtac-lnx02:~$ find -name onerouter
./lab/onerouter
./.snapshot/hourly.2020-06-24_2000/lab/onerouter
# head/tail #
annaw@svl-jtac-lnx02:~$ head -2 test.txt
111
222 event
annaw@svl-jtac-lnx02:~$ tail -2 test.txt
888
999
annaw@svl-jtac-lnx02:~$ cat  test.txt | head -5 | tail -2
278  log
256  error
<<<...前...5...行内容的后两行，就是第...4-5...行
- stop ...命令
HUP     1    终端断线
INT     2    中断（同 Ctrl + C）
QUIT    3    退出（同 Ctrl + ）
TERM    15    终止
KILL    9    强迫终止
CONT    18    持续（与STOP相反， fg/bg号令）
STOP    19    暂停（同 Ctrl + Z）
<<<  'kill -STOP <pid>;sleep 361; kill -CONT <pid>' : ...这里的...kill...不是杀掉的意思，只是单纯的运行...stop...和...contiue...的命令
# VI #
ctrl+f :...往后翻页
ctrl + b:...往前翻页
/xxxx ...回车...: ...查找相关内容，再按...n...就是...next...，匹配的下一条
?xxxx ...回车...: ...查找相关内容，再按...n...就是...up...，匹配的上一条
:set nu ：... ...显示行数
600 Feb 19 03:01:13   init: link-management (PID 2023) terminate signal 15 sent
601 Feb 19 03:01:13   dfwd[2060]: dfwdlib_process_client_disconnect:10591 num_id_list[0] = 22
602 Feb 19 03:01:13   init: routing-socket-proxy (PID 2024) terminate signal 15 sent
...:...6...00  ：显示第...600...行
Archive...并压缩文件保存到一个路径：
> file archive compress source /var/log/* destination /var/home/ipcore/<imse-name>-varlog-m0-r0
让文件可以用...less...读：
iconv -f UTF-16 -t UTF-8 "imse03.klg_klj_2020-09-21_23-52-45 - during.txt" > "imse03.klg_klj_2020-09-21_23-52-45 - during.txt.less"
查看磁盘空间：
- ...df [选项] [文件]
“df -h”这条命令再熟悉不过。以更易读的方式显示目前磁盘空间和使用情况。
“df -i” 以inode模式来显示磁盘使用情况。
显示文件：
- ls -lt ...：... ...新的在上面
ls -ltr : ...旧的在上面
-more /zmore + file name
查看...folder...大小：
% pwd
/var/log
% du -sh  <<<
18M    .
# Linux Shell ...命令... #
...查找:
-vi...这个文件
-...用... :/...查找内容...  , press "n" ...可以到下一个位置
编辑文件...:
[root@centos-cfts-china-vm2 raddb]# vi users
- i    <<<insert ...开始编辑模式，开始编辑
- esc <<<...退出编辑模式
- ...:wq!... <<<...保存退出
- ...:q!... <<<...不保存退出
==: ...判断是否相等
=...：赋值
shell...下开启...timestamp...：
labroot@jtac-mx480-r2041-re0> start shell
%  cli -c "set cli timestamp"
Feb 04 11:08:10
CLI timestamp set to: %b %d %T
shell...下执行...pfe shell...命令：
% cprod -A member0-fpc1 -c "show luchip 0 rate"
查看文件夹大小...:
root@lab-mx960-3d-02-re1:/var/home/nera # du -sh /packages/db/
5.4G    /packages/db/
# Calculation #
Binary...：...2 进制
Octal:8进制
Decimal: 10 进制 1234…10
Hex: 16 进制  0123456789abcdef
# L...inux 基本命令... #
more : 类似 cat ，不过会以一页一页的形式显示
Source: shell 里执行这个路径的指定文件
- cp -R source_dir destination_dir   :  copy...整个...folder...的文件
- cp -r
给文件读写权限permission：
chmod -R a+rx *
-rwxr-xr-x  1 annaw support1 1939168 Nov  5 18:34 ospf-re0.log
-rwxr-xr-x  1 annaw support1 2472295 Nov  5 18:40 ospf-re1.log
-rwxr-xr-x  1 annaw support1    4219 Nov  5 18:42 re0.log
-rwxr-xr-x  1 annaw support1     139 Nov  5 18:42 re1.log
查看...CPU:
administrator@server-6d:~$ cat /proc/cpuinfo | grep MHz
# sysctl #
是一个 Linux/Unix 系统命令，用于在运行时动态查看或修改内核参数。这些参数通常位于 /proc/sys/ 目录下，涉及系统的各种配置，如网络、内存、文件系统、安全等。
sysctl -a  # 显示所有可用的内核参数
sysctl -w <parameter>=<value>  # 临时修改，重启后失效
sysctl -p  # 重新加载配置（永久生效）
root@jtac-mx304-r2013-re0-fpc0:/# sysctl -a|grep -i martian
net.ipv4.conf.all.log_martians = 1
# Socekt #
Sockets are Linux file descriptors that serve as the communication end-points for processes running on that device. Each Linux socket consists of the device's IP address and a selected port number.
A socket connection is a bidirectional communication pipe that allows two processes to exchange information within a network.
The typical sockets use case is the client-server network interaction model. In this model, the server process socket listens and waits for clients' requests.
# Thread #
https://www.baeldung.com/linux/process-vs-thread
A thread is a lightweight process. A process can do more than one unit of work concurrently by creating one or more threads. These threads, being lightweight, can be spawned quickly.
There are two types of process: single threaded & multi threaded process
PID: Unique process identifier
LWP: Unique thread identifier inside a process
NLWP: Number of threads for a given process
[user@fedora ~]$ ps -eLf UID          PID    PPID     LWP  C NLWP STIME TTY          TIME CMDroot           1       0       1  0    1 Jun28 ?        00:00:16 /usr/lib/systemd/systemd --switched-root --system --deserialize 31root           2       0       2  0    1 Jun28 ?        00:00:00 [kthreadd]root           3       2       3  0    1 Jun28 ?        00:00:00 [rcu_gp]root           4       2       4  0    1 Jun28 ?        00:00:00 [rcu_par_gp]root           6       2       6  0    1 Jun28 ?        00:00:05 [kworker/0:0H-acpi_thermal_pm]root           8       2       8  0    1 Jun28 ?        00:00:00 [mm_percpu_wq]root          12       2      12  0    1 Jun28 ?        00:00:11 [ksoftirqd/0]root          13       2      13  0    1 Jun28 ?        00:01:30 [rcu_sched]root          14       2      14  0    1 Jun28 ?        00:00:00 [migration/0]root         690       1     690  0    2 Jun28 ?        00:00:00 /sbin/auditdroot         690       1     691  0    2 Jun28 ?        00:00:00 /sbin/auditdroot         709       1     709  0    4 Jun28 ?        00:00:00 /usr/sbin/ModemManagerroot         709       1     728  0    4 Jun28 ?        00:00:00 /usr/sbin/ModemManagerroot         709       1     729  0    4 Jun28 ?        00:00:00 /usr/sbin/ModemManagerroot         709       1     742  0    4 Jun28 ?        00:00:00 /usr/sbin/ModemManager
Copy
# Free bsd #
6.1  legcy
10.1 occam
Freebsd website
https://www.freebsd.org/cgi/man.cgi?query=traceroute&apropos=0&sektion=0&manpath=FreeBSD+8.0-RELEASE&arch=default&format=html
How to find out junos freebsd version?
1.from shell
lab@HamburgerKid> start shell
% sysctl kern.osreldate
kern.osreldate: 601000  <<< start with 6, this is FreeBSD6 base (6.1)
% sysctl kern.osreldate
kern.osreldate: 1001510  <<< start with 10, FreeBSD10 base.
2.
root@rollpanna> show version | match kernel
JUNOS Kernel Software Suite [16.1X70-D10.3]  <<<< this is legacy output
lab@kurodai-re0> show version | match kernel
JUNOS OS Kernel 64-bit  [20160616.329709_builder_stable_10]  <<<< occam shows 32 or 64 bit
#  ...匹配条件... #
/*补充：其它表达式*/
ip[9:1]  代表跳过0-8 9个字节的下一个，就是第10个   用于抓协议
ip[9]    代表跳过0-8 9的字节的  只抓这一个字节  不如上面可以字节规定
dst net net
src net net
net net
net net mask netmask
net net/len
匹配IPv4/v6地址为net网络的数据报。其中net可以为192.168.0.0或192.168这两种形式。如net 192.168 或net 192.168.0.0
net net mask netmask仅对IPv4数据包有效，如net 192.168.0.0 mask 255.255.0.0
net net/len同样只对IPv4数据包有效，如net 192.168.0.0/16
dst portrange port1-port2
src portrange port1-port2
portrange port1-port2
匹配端口在port1-port2范围内的ip/tcp，ip/upd，ip6/tcp和ip6/udp数据包。dst, src分别指明源或目的。没有则表示src or dst
less length 匹配长度少于等于length的报文。
greater length 匹配长度大于等于length的报文。
ip protochain protocol 匹配ip报文中protocol字段值为protocol的报文
ip6 protochain protocol 匹配ipv6报文中protocol字段值为protocol的报文
如”ip protochain 6’”匹配ipv4网络中的TCP报文，与”ip && tcp”用法一样，6是TCP协议在IP报文中的编号。
ether broadcast  匹配以太网广播报文
ether multicast  匹配以太网多播报文
ip broadcast  匹配IPv4的广播报文。也即IP地址中主机号为全0或全1的IPv4报文。
ip multicast  匹配IPv4多播报文，也就是IP地址为多播地址的报文。
ip6 multicast  匹配IPv6多播报文，即IP地址为多播地址的报文。
vlan vlan_id  匹配vlan报文，且vlan号为vlan_id的报文
还可以通过逻辑运算符组成复杂的表达式，逻辑运算符有以下三个：
“&&” 或“and”
“||” 或“or”
“!” 或“not”
并且可通过()进行复杂的连接运算。
如”host 192.168.240.3 &&( tcp port 80 || tcp port 443)”
# grep #
精确查找：
$ cat 20201214-server-logs.log | grep -w "6889" | more
# ...删除所有文件... #
rm -R *
commit full <<<...需要加一个这个
# Linux Folders #
/boot folder for all booting related information.
/dev folder for all hardware devices attached to machine.
/etc folder for all configuration files saved here
====================================
# Smartd #
FreeBsd manual gives us some hint about the smartd disk check interval(30 min) .
https://www.freebsd.org/cgi/man.cgi?query=smartd&manpath=FreeBSD+9.0-RELEASE+and+Ports&format=html
-i N, --interval=N
Sets the interval between disk checks to N seconds, where N is a
decimal integer. The minimum allowed value is ten and the maxi-
mum is the largest positive integer that can be  represented  on
your system (often 2^31-1).  The default is 1800 seconds.
^^^^^^^^^^^^^^^^^^^^^^^^^^^^
And, we can check the smartd status at any time with the following command.
Note  that the superuser can make smartd check the status of the
disks at any time by sending it the SIGUSR1 signal, for example
with the command:
kill -SIGUSR1 <pid>
where  <pid>  is the process id number of smartd.  One may also
use:
killall -USR1 smartd
for the same purpose.
root@kurodai-re0:/etc # ps auxw | grep smartd
root  7727   0.0  0.0   32332    2920  -  I    Tue05PM     0:00.39 /usr/sbin/smartd -n
root 28336   0.0  0.0   22680    2240  0  S+    4:10PM     0:00.00 grep smartd
root@kurodai-re0:/etc # date; kill -USR1 7727
Mon Jun 13 16:10:29 JST 2016
Jun 13 16:10:29.833  kurodai-re0 smartd[7727]: Signal USR1 - checking devices now rather than in 1203 seconds.
Jun 13 16:10:29.843  kurodai-re0 smartd[7727]: Device: /dev/ada0, 26677 Offline uncorrectable sectors
Jun 13 16:10:29.843  kurodai-re0 smartd[7727]: Device: /dev/ada0, SMART Usage Attribute: 199 UDMA_CRC_Error_Count changed from 118 to 54
Jun 13 16:10:29.843  kurodai-re0 smartd[7727]: Device: /dev/ada0, SMART Usage Attribute: 201 Unknown_SSD_Attribute changed from 168 to 171
KB15241 tells us about the knob to stop it.
http://kb.juniper.net/InfoCenter/index?page=content&id=KB15241&smlogin=true&actp=search
set system processes disk-monitoring disable
======================================================
# Signup #
root@jtac-mx960-r2039-re0-node:/etc#
chrisli对所有人说： 11:48 AM
SIGHUP	1	Terminal line hangup
SIGINT	2	Interrupt program
SIGQUIT	3	Quit program
SIGILL	4	Illegal instruction
SIGTRAP	5	Trace trap
SIGABRT	6	Abort
SIGEMT	7	Emulate instruction executed
SIGFPE	8	Floating-point exception
SIGKILL	9	Kill program
SIGBUS	10	Bus error
SIGSEGV	11	Segmentation violation
SIGSYS	12	Bad argument to system call
SIGPIPE	13	Write on a pipe with no one to read it
<<< singal hang up = kill process?
====================
# Crontab #
https://man7.org/linux/man-pages/man5/crontab.5.html
A crontab file contains instructions for the cron(8) daemon in the following simplified manner: "run this command at this time on this date".  Each user can define their own crontab.  Commands defined in any given crontab are executed under the user who owns that particular crontab.
====================
# jlock #
lock...是...OS operating...里为了写的时候不冲突，有一个程序写的时候，...lock...上，另一个程序就不能写了
====================================================
# ...设备免密码登录... #
From EX login to VC without PW:
1, get public key of the VC:
% ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/var/home/labroot/.ssh/id_rsa):
/var/home/labroot/.ssh/id_rsa already exists.
Overwrite (y/n)? y
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /var/home/labroot/.ssh/id_rsa.
Your public key has been saved in /var/home/labroot/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:Nnbuz2PTPcVNr8odjIFO859LyT9rUDN1xZ8BvU1yciY labroot@jtac-ex4550-32f-r2042
The key's randomart image is:
+---[RSA 2048]----+
|             .o.o|
|              EoO|
|               OO|
|           .   *+|
|        S = . .o=|
|       o * o * .=|
|          o o.Bo.|
|         . o++oB.|
|          .o=+*o=|
+----[SHA256]-----+
Generated SSH key file /var/home/labroot/.ssh/id_rsa.pub with fingerprint SHA256:Nnbuz2PTPcVNr8odjIFO859LyT9rUDN1xZ8BvU1yciY
% cat /var/home/labroot/.ssh/id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDcyLnzS171iOCWckxyFYwqJlQVVaNapYeFbEjyP8uqeQk8GdFZ4hByjzjxFK1dv7/aqnC4kFfOx6KByeVNZ4t8Uo9+UrQLRrW/ocTiUBWRIHCT1HUMK8rrP4RrT8nX8aYlatdOPtWmj2dTDNdR8oce+dkC9sSaz169hDEvaDdlmAc2stfPmPJFevoxvtotb6eZD8cV/BWjVZLz5aVDBUe9yty1EvBxkN2ZjyUl4z2goqiB/5QfAbjKGGxyD1CP4rMrSaVJtsR9GKnpBw6LQYGnRO5ionCwPZnVv58osF8MTOloYxgRDRv8sVdVOdqZ6Cq1nRNHRcYf/l5i2tZpD4e9 labroot@jtac-ex4550-32f-r2042
2, put the VC public key on your EX:
# set system login user labroot authentication ssh-rsa ""
commit
<<<...引号里放入黄色的...publish-key
注意如果...default group...被...protect...了，要先...unprotect
================================================================
# Kill deamon process #
1. show system process
Find the pid for deamon
2.start shell
Su
Pw
Kill pid#
lab@m10i-re1> show system processes | grep ksyncd
1588  ??  S      0:13.11 /usr/sbin/ksyncd –N
lab@m10i-re1> show system processes | grep ksyncd
6988  ??  S      0:00.05 /usr/sbin/ksyncd -N
===================================
# kill process #
<<<RPD-- RPD(KRT --> KRT queue)-- KERNEL
他们之间的链接都用...routing-socket
======================================================================
# KRT Queue #
KRT Q
KRT(Kernel Route Table) queue is a module within Routing Protocol Daemon (RPD).
It synchronizes routing tables in RPD and forwarding tables in Kernel in an optimized fashion, and manages multiple object types – route, routing table, interface, nexthop.
------------------------
syslog indicate "DAEMON-3-RPD_KRT_Q_RETRIES: Indirect Next Hop Update: No buffer space available", service was affected
-----------------------------
每一条这种log ：kernel: rt_pfe_veto: Too many delayed route/nexthop unrefs. Op 8 err 55, rtsm_id 0:-1, msg type 10
只是表示发生过一次RPD下发的速度比PFE消化的速度快，当PFE未消化的数目达到阈值（40k）时，便会出现这种log。出现这种情况时RPD会等待0.5ms左右后继续下发这些未消化的条目。 所以有多少条这种log表示发生过多少次，并不是说一直在发生。
RESOLUTION DETAIL: Expected behavior when too many unicast nhs need flush enable composite nexthop
These "rt_pfe_veto" and "RPD_KRT_Q_RETRIES" mean that the krt queue is temporally busy for installing large number of routing updates from the RPD to the forwarding table in the kernel due to the resource competition... ...between RPD and sampling. In such condition, the routing convergence takes longer time to complete.
=================================================
# Kernel #
同步发生在两步：
1. RE...上的...routing-table...和...forwarding-table...的同步
2.forwarding-table...通过...kernel...下放到...PFE...上，两个...table...的同步
Kernel ...卡的最好解决方法是...switch over
Kernel...是...RPD...的一个功能，...ifsmon...是...kernel...的一个工具看什么被送给...kenenl...处理下放。
KRT...是...RE-PFE...的...queue...。
=================================================
# KRT #    ...连接...RE...和...PFE...的...queue
Krt slow...是可能由于有...task...，但...stuck...就有问题了。
When GRES is enabled on a system, in order to make sure the information (or the ...“...state...”...) is in-sync between the master, backup Routing Engines and the PFEs, an ifstate-based Asynchronous routing socket model is implemented for tracking the changes.
The ...“...ifstate...”... is the state associated with the data structures for enqueuing on a temporal chain and the clients of the ifstate module can consume the data structure changes.
While the states are being replicated to the ifstate peers, a per-peer basis COMMIT_MARKER (CM) and COMMIT_PROPOSAL (CP) would be used to remember where is the last sync-up location which helps when a re-sync is required after a failover.
KRT Queue Stuck
-------------------
Aug 31 12:54:53 emperor-re0-r11 rpd[1132]: %DAEMON-3-RPD_KRT_Q_RETRIES: kqp 0x3b2df370:
op change queue high-change table inet.0 attempts 10 1.9.6.9/32 -> {10.11.21.1, as0.0,
10.11.31.1}=>{10.11.21.1, 10.11.41.1, as0.0, 10.11.31.1} RPD_KRT_Q_RETRIES: Route Update:
No buffer space available
----------------
<<<...这个未必就代表...krt stuck...了，可能只是短暂的...congestion...。
Other than the temporary congestion situation like this, the kernel will return “No buffer space available” when it’s running
into one of these situations:
 M_IFSTATE usage is greater than 87.5% of its per-type limit and the number of dead ifstate is greater than 12.5% of
live ifstates
 non RTM_ADD at 96.875% of available vm.kmem memory
 RTM_ADD at 93.75% of available vm.kmem memory
 RTM_ADD/RTM_CHANGE with the unrefs exceeds the rt_nh_max_delayed_unrefs limit
<<<这些situation也会出no buffer的log
The following could help us to find out if we are running out of kernel virtual memory.
root@Jocelyn_RE1% sysctl -a | grep vm.kmem
vm.kmem_size: 655360000
vm.kmem_size_max: 655360000
vm.kmem_size_scale: 3
vm.kmem_map_free: 596692992
vm.kmem_mt_used_max_percent: 1
root@Jocelyn_RE1%
To determine if we are having KRT queue stuck, the main point is to check if the following “route substructs” and “rnh... ...unref substructs” counters are moving. If they are not and we keep seeing entries got stuck in the “show krt queue” output,... ...most likely, we have run into a problem. By default, the rt_nh_max_delayed_unrefs limit is set to 10000.
root@Jocelyn_RE1% ifsmon -I
Basic ifstate statistics
Total number of live ifstates : 3693
Total number of dead ifstates : 0
#ele in nexthop resolve chain : 0
route substructs allocated : 389710
route substructs deleted : 389710
rnh unref substructs alloc-ed : 0
rnh unref substructs del-ed : 0
veto_retry = 0, veto_recover = 0 veto_nh_delayed_unref = 0
root@Jocelyn_RE1% sysctl -a | grep -i unref
net.rt_nh_max_delayed_unrefs: 10000... <<<<
root@Jocelyn_RE1%
For example, the following is captured when we run into a KRT queue stuck problem where the delay unrefs has reached
the limit.
What should we collect?
Once we have found a KRT queue stuck but the system is not running into any memory issue, it’s either a bug or someone not sending the “ack” back to the master routing engine which avoids the ifstate chain from moving forward. There are a few ways for us to check who it is.
Before we perform any trouble shooting, we should collect the followings just in case if we need help from Engineering or a system crashes while we are working on it. Please capture them a few times so that we can compare the delta values of the counters.
All Routing Engines
CLI:
# show log messages | no-more
# show log ksyncd | no-more
# show system connection tp extensive | no-more
# show system connection extensive | no-more
# show system virtual-memory | no-more
# show system buffer
# show krt queue | no-more
# show krt state
# show route summary
# request support information
SHELL:
# ifsmon –I –d
# ifsmon –p
# ifsmon –P
# ifsmon –g
# ifsmon –d –k
# ifsmon -c
# sysctl –a | grep vm.kmem
Trace ifstate debug messages...:
1. Configure the following syslog from both Master and Backup REs:
# show system syslog
file gres-log {
...kernel any;
...archive size 10m files 10;
}
2. Enable the following traces on both Master and Backup REs (with root
access right) :
# sysctl –w net.rts_ifstate_trace_enable=0xff
# sysctl –w net.rts_ifstate_tracerec_max=500000
# sysctl –w net.rts_ifstate_log_level=0xff
3. Capture the gres-log and ksyncd log from both Master and Backup REs.
Live kernel core dump...:
1. Capture kernel core from both Master and Backup REs REs (with root
access right):
# sysctl –w kern.live_core_dump=1
All PFEs (FPC/FEB..etc) and... ...Intelligent Service PICs... ...(IQ2/IQ2E/ASP/MS..etc)
# show pfe manager session statistics
# show pfe manager statistics
# show heap
# show heap 1
# show packet
# show packet statistics
# show device local
# show syslog messages
# show socket
# show socket id <#ID> (#ID of the PPM, chassis and PFE connections)
ifsmon is a very useful tool for ifstate debugging.
a & b dump routes in a route table
a, b & D rn_debug_walk a route table for sanity
a, b & B pre-order traversal dump of nodes in radix route table
d detailed output
R Peer registration information
c dump percentage of client progress in the ifstate chain
f dump nexthop index tracking entries
g get global rtsock statistics
h print help
I dump Basic ifstate statistics(rts_ifs_stats)
k use /dev/kmem
M use argument file instead of /dev/kmem
n name of the peer for -P ex:FEB0
N read debug symbols from argument file
p dump pfelistner array
P dump pfe peer management statitics
r print in reverse order
s dump pfe peer management statistics changes periodically
S dump ifstate chain from this address
t print ifstate trace records
w print ifstate profiling data
T get time from the core
v extra detailed output when used with -d
C dump jnx_cap_t
o write raw dump ifstates to output file
1 dump the ifstate object from this address
G dump gres state var history
For example, it could tell us who is the slowest ifstate peer, the rtsock msg statistics in the last 10 mins interval and also
the more recent GRES state history.
root@Jocelyn_RE1% ifsmon -c
root@Jocelyn_RE1% ifsmon -g
root@Jocelyn_RE1% ifsmon -G
Who's blocking the ifstate chain?
root@emperor-re0-r11% ifsmon -P
PFE Type : (7)RE
PFE Class : 1, PFE Index : 1, PFE Slot : 1
PFE Type : (12)LCC
PFE Class : 1, PFE Index : 0, PFE Slot : 0
Next, we will try to figure out if any of the ifstate peers is causing us problem. The following is an example from a TX system. However, the same methodology can be used on another system with GRES enabled.
First, we have to check the CM for all the ifstate peers and slave from the master RE. It can be extracted from the ifstate chain dump with the ifsmon command.
SCC RE0 (master)
root@emperor-re0-r11% ifsmon -kd | egrep -i "comm"
Here are the PFE types defined.
const char *pfetype_strings[PFE_PEER_TYPE_MAX + 1] = {
"Unknown",
"SCB",
"SFM",
"FPC",
"FPC",
"GFPC",
"SFPC",
"RE",
"MINT",
"ASP",
"FWDD",
"FPC",
"LCC",
"RFPC",
"RFEB",
"OSE",
"GGSN",
"XDPC",
"IQ2",
"ASQ",
"LSQ",
"GGSNI",
"MSP",
"HCM",
"SFI",
"PFEM",
"Invalid"
};
# UNIX time convert #
https://codebeautify.org/unix-time-stamp-converter
Mar 20 02:11:28.263896 radius-acct-stop: Acct-Session-Id added: 121051:121064-1584636866 => Fri Mar 20 2020 00:54:26
