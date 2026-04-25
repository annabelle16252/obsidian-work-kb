# IXIA and Spirent (in validate)

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

IXIA and Spirent (in validate)
#define TG1_DISK basedisk "/vmm/data/user_disks/sjiang/image/Ixia_Virtual_Test_Appliance_8.31_EA_KVM_0.10.qcow2";
#define TG1_DISK basedisk "/vmm/data/user_disks/sjiang/image/spirent-5.04.8556.img";
vm "IXIA_1" {
hostname "IXIA_1";
TG1_DISK
ncpus 8;
memory 2048;
interface "vio0" {EXTERNAL;};              // default port for login
interface "vio1" {bridge "private1";};     // traffic port for device
};
vm "IXIA_2" {
hostname "IXIA_2";
TG1_DISK
ncpus 8;
memory 2048;
interface "vio0" {EXTERNAL;};             // default port for login
interface "vio1" {bridge "private1";};    // traffic port for device
};
https://junipernetworks.sharepoint.com/sites/tide/cybersecurity/SitePages/Lab%20Jump%20Host%20Instructions.aspx
#### How to Login vmm IXIA
https://junipernetworks.sharepoint.com/sites/tide/cybersecurity/SitePages/Lab%20Jump%20Host%20Instructions.aspx
1. install vnc viewer
https://www.realvnc.com/en/connect/download/viewer/
2. connect to server
VNC hosts:
qclab-evnc-01
qclab-evnc-02
qclab-evnc-03:5999
3. change resolution
RandR=1920x1080,2560x1600,1024x768
~/.vnc/config.d/Xvnc
xrandr -s
4. rdp to server
rdesktop -g 90% -d jnpr -u harryqian -p - sv8-stc-win01.englab.juniper.net
q-pod-stc-win01.englab.juniper.net
q-pod-stc-win02.englab.juniper.net
sv8-stc-win01.englab.juniper.net
sv8-stc-win02.englab.juniper.net
rdesktop -g 95% -d jnpr -u harryqian -p - q-pod-ixas34.englab.juniper.net
sv8-pod1-ixlic1.englab.juniper.net
