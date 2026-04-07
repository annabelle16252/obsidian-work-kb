# IXIA network

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

IXIA network
vm "IxiaLnxAppSvr" {
hostname "IxiaLnxAppSvr";
basedisk ".../vmm/data/user_disks/sjiang/image.../ixia-vm90-as.qcow2";
memory 8192;
ncpus 4;
interface "vio0" { bridge "external"; };
setvar "use_virtio_disk" "1";
setvar "disk_format" "qcow2";
setvar "use_dhcp" "1";
};
vm "IxiaLnxChSvr" {
hostname "IxiaLnxChSvr";
basedisk "/vmm/data/user_disks/sjiang/image/ixia-vm90-ch.qcow2";
memory 4096;
ncpus 4;
interface "vio0" { bridge "external"; };        //default
interface "vio1" { bridge "private8"; };        //to EX2
interface "vio2" { bridge "private9"; };        //to EX3
setvar "use_virtio_disk" "1";
setvar "disk_format" "qcow2";
setvar "use_dhcp" "1";
};
上面两个...vm...都是必须的
第一个不要做修改... ...直接用... ...是用来提供...IXIA...管理页面的...server
第二个是拓扑中使用的... IXIA port...，按需求指定接口
拓扑开机后，会有下面两个...IP...：
IxiaLnxAppSvr 10.53.84.7
IxiaLnxChSvr 10.53.89.156
通过任意一个...VM-shared desktop ...在浏览器直接打开... IxiaLnxAppSvr ...的地址
登录用户名.../...密码是...  admin/admin
要新建拓扑，选择左下方的... new session
session...打开后，需要手动指定... license server
打开右上角的设置图标
选择... SETTING
在...License ...输入下面的...server
sv8-pod1-ixlic1.englab.juniper.net
License 
Licensing Mode 
License Servers 
Tier Level 
M lied 
sv8-podI -ixlic I .englab.juniper.net 
Tier 3 ...License 
Licensing Mode 
License Servers 
Tier Level 
M lied 
sv8-podI -ixlic I .englab.juniper.net 
Tier 3
The new license server is located in Quincy,... ... q-pod-ixlic1 (10.49.32.221) and is currently serving license
q-pod-ixlic1.englab.juniper.net
Another way to connect to vIXIA chassis
在本地打开...CMD...，输入下面命令（需要...VPN...连接）：
ssh -N -L 13389:q-pod-ixas34.englab.juniper.net:3389 10.32.192.47 -l sjiang -N
将用户名改成自己的...Juniper...用户名
C: -N -L 13389:q-pod-ixas34.eng1ab.juniper.net:3389 le. 32.192.46 -1 sjiang -N 
sjiang@łe.32.192.46's password: ...C: -N -L 13389:q-pod-ixas34.eng1ab.juniper.net:3389 le. 32.192.46 -1 sjiang -N 
sjiang@łe.32.192.46's password:
出现密码提示后，输入...Juniper...的登陆密码（旧），界面停留在这个状态，不需要关闭
打开远程连接，连接到... localhost:13389
用户名... ...：... JNPRLAB\username
这里的...username...换成自己的...Juniper...用户名
密码是...JNPRLAB...密码（新）
Remote Desktop Connection 
Remote Desktop 
Connection 
Computer 
user name 
localhost: 13388 
JNPRLAB\sjiang 
You will be asked for credentials when you connect 
Show Qptions 
Help ...Remote Desktop Connection 
Remote Desktop 
Connection 
Computer 
user name 
localhost: 13388 
JNPRLAB\sjiang 
You will be asked for credentials when you connect 
Show Qptions 
Help
登陆后直接使用...IXIA ...软件... ...添加...chassis...就可以用
（仍然需要更改...license server...）
q-pod-ixlic1.englab.juniper.net
8 , 50 , 1 501 “ 
IxNetwork 
9 , 10287 
9 02112.6 
。 丨 k 
930 12.7 ...8 , 50 , 1 501 “ 
IxNetwork 
9 , 10287 
9 02112.6 
。 丨 k 
930 12.7
