# Static LSP

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

Static LSP
R1
[edit protocols mpls]
root# show
static-label-switched-path inet {
ingress {
next-hop 192.168.12.2;
to 192.168.67.6;
push 1000000;
}
}
interface all;
R2
[edit protocols mpls]
root# show
static-label-switched-path inet {
transit 1000000 {
next-hop 192.168.25.5;
swap 1000001;
}
}
interface all;
R5
[edit protocols mpls]
root# show
static-label-switched-path inet {
transit 1000001 {
next-hop 192.168.56.6;
swap 0;
}
}
interface all;
-...在...R1...，...R2...，...R5...上...configure...即可
-lable 0...是...router...需要弹出...label...，执行...ipv4...路由查找。
-...只要在接口上开启...mpls...，就会在...mpls0...表里建...3...个条目：
0-ipv4 explicit null
1-router alert label
2-ipv6 explicit null
