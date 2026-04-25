# Switch

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

Switch
============================
# EX VLAN #
ge-0/0/12 {
unit 0 {
family ethernet-switching {
interface-mode trunk;
vlan {
members vlan100;
}
<<<...连接...router...，...router...测配...vlan-tagging...可以，但...switch...测要这样配，否则...ping...不通
============================
# ...传输... #
传统模型：三层模式
现代模型：...spine-leaf
# VLAN #
一个...lan...是一个广播域，...switch...隔离广播域，但多个...switch...不现实。所以用...vlan...，...virtually...隔离...LAN...，从而隔离广播域。
Types of VLAN:
-default VLAN
-data VLAN: user vlan, divide switch to couple of vlans
-native VLAN: traffic entering a trunk port with vlan untagged will be assigned a native vlan id (pre-configured)
-management VLAN
