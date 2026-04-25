# Packet Capture

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

Packet Capture
Transit packet capture:
analyzer...：镜像端口上所有包，不能精确匹配包类型，一般用于swtich等二层设备上
port... mirror...：通过firewall... filter来过滤抓包...，可以精确匹配包类型
～...LAG的话port mirror没法抓member link的包，必须用analyzer
Host-bound packet capture:
tcpdump: A standard Linux tool that captures packets visible to the kernel. It is limited to capturing traffic that the kernel can see.
monitor traffic interface: A Junos CLI command that utilizes tcpdump to capture exception traffic or traffic going to and from the Routing Engine. It is specifically designed for Junos devices.
fpc-shell> tcpdump -i xe-1/0/0.0 -w twamp-clientxe100.pcap -s 50000
