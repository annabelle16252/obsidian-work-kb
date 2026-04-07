# Snmp trapd

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

Snmp trapd
https://www.cnblogs.com/cenima/p/17836877.html
install net-snmp.x86_64
install vim
modify /etc/snmp/snmptrapd.conf
add below line:
disableAuthorization yes
authCommunity log,execute,net test
add MIB for the target OID under below directory:
/usr/share/snmp/mibs
## all MIB files for the target version need to be added
configrue GRE and GRE KA on both router
configure SNMP on R1, specify community and trap-group
start receive trap on server:
snmptrapd -m /usr/share/snmp/mibs/mib-jnx-oam.txt -C -c /etc/snmp/snmptrapd.conf -df -Lo
snmptrapd -C -c /etc/snmp/snmptrapd.conf -df -Lo
snmptrapd -m /usr/share/snmp/mibs/mib-jnx-oam.txt -df -Lo
snmptrapd  -df -Lo
===========
-m specify the MIB for the target OID
snmptrapd -m /root/JuniperMibs/mib-jnx-oam.txt -df -Lo
mib-ospf2trap.txt
[root@centos ~]# snmptrapd -C -c /etc/snmp/snmptrapd.conf -df -Lo
Cannot bind for clientaddr: Address already in use
couldn't open udp:162 -- errno 98 ("Address already in use")
netstat -lnp| grep 162
kill -9 id
2024-04-22 01:33:23 _gateway [UDP: [192.168.0.2]:53730->[192.168.0.1]:162]:
DISMAN-EVENT-MIB::sysUpTimeInstance = Timeticks: (24327538) 2 days, 19:34:35.38 SNMPv2-MIB::snmpTrapOID.0 = OID: SNMPv2-SMI::transmission.166.4.0.3
SNMPv2-SMI::transmission.166.4.1.3.3.1.2.97.98.99.100.101.102.2147483648.97.98.99.100.101.102 = INTEGER: 5
SNMPv2-SMI::transmission.166.4.1.3.3.1.8.97.98.99.100.101.102.2147483648.97.98.99.100.101.102 = Timeticks: (24327538) 2 days, 19:34:35.38
SNMPv2-SMI::transmission.166.4.1.3.4.1.1.97.98.99.100.101.102.2147483648.97.98.99.100.101.102 = Counter32: 2147483647
SNMPv2-SMI::transmission.166.4.1.3.4.1.2.97.98.99.100.101.102.2147483648.97.98.99.100.101.102 = Counter32: 2147483647
SNMPv2-MIB::snmpTrapEnterprise.0 = OID: SNMPv2-SMI::enterprises.2636.1.1.1.2.99
2024-03-28 07:05:08 _gateway [UDP: [192.168.0.2]:63580->[192.168.0.1]:162]:
SNMPv2-MIB::sysUpTime.0 = Timeticks: (19257701) 2 days, 5:29:37.01   SNMPv2-MIB::snmpTrapOID.0 = OID: JUNIPER-OAM-MIB::jnxOamGreKeepAliveAdjacencyChangeNotif
JUNIPER-OAM-MIB::jnxOamGreKeepAliveInterfaceName.0 = STRING: gr-0/0/0.0
JUNIPER-OAM-MIB::jnxOamGreKeepAliveAdjacencyState.0 = INTEGER: down(0)
SNMPv2-MIB::snmpTrapEnterprise.0 = OID: JUNIPER-SMI::jnxProducts.1.1.2.99
2024-04-22 02:03:08 _gateway [UDP: [192.168.0.2]:53730->[192.168.0.1]:162]:
DISMAN-EVENT-MIB::sysUpTimeInstance = Timeticks: (24506012) 2 days, 20:04:20.12 SNMPv2-MIB::snmpTrapOID.0 = OID: IF-MIB::linkDown
IF-MIB::ifIndex.528 = INTEGER: 528
IF-MIB::ifAdminStatus.528 = INTEGER: down(2)
IF-MIB::ifOperStatus.528 = INTEGER: down(2)
IF-MIB::ifName.528 = STRING: ge-0/0/1
snmptrapd  -df -Lo
2024-04-22 02:11:23 _gateway [UDP: [192.168.0.2]:53730->[192.168.0.1]:162]:
DISMAN-EVENT-MIB::sysUpTimeInstance = Timeticks: (24555559) 2 days, 20:12:35.59
SNMPv2-MIB::snmpTrapOID.0 = OID: SNMPv2-SMI::enterprises.2636.3.83.81.1.2.1
SNMPv2-SMI::enterprises.2636.3.83.81.1.3.1.0 = STRING: "gr-0/0/0.0"
SNMPv2-SMI::enterprises.2636.3.83.81.1.3.2.0 = INTEGER: 1
SNMPv2-MIB::snmpTrapEnterprise.0 = OID: SNMPv2-SMI::enterprises.2636.1.1.1.2.99
snmptrapd -m /root/JuniperMibs/mib-jnx-oam.txt -df -Lo
2024-04-22 08:54:56 _gateway [UDP: [192.168.0.2]:53730->[192.168.0.1]:162]:
SNMPv2-MIB::sysUpTime.0 = Timeticks: (26976807) 3 days, 2:56:08.07
SNMPv2-MIB::snmpTrapOID.0 = OID: JUNIPER-OAM-MIB::jnxOamGreKeepAliveAdjacencyChangeNotif
JUNIPER-OAM-MIB::jnxOamGreKeepAliveInterfaceName.0 = STRING: gr-0/0/0.0
JUNIPER-OAM-MIB::jnxOamGreKeepAliveAdjacencyState.0 = INTEGER: up(1)
SNMPv2-MIB::snmpTrapEnterprise.0 = OID: JUNIPER-SMI::jnxProducts.1.1.2.99
