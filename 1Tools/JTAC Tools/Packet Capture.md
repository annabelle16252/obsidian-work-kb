RE traffic:
monitor traffic interface = Junos CLI 层面的接口抓包工具，适合常规接口抓包。
tcpdump = OS/shell 层面的底层抓包工具，适合抓 RE 内部接口和更特殊的系统路径。

因为 monitor traffic 是 Junos 软件通过指令去读取内核数据并转义的；而 tcpdump 是直接挂载在底层的 BPF 网络接口上。如果需要分析报文问题之类的要用tcpdump

Transit traffic:
Port mirror
# tcpdump
https://juniper.lightning.force.com/lightning/r/Knowledge__kav/ka03c0000009SOMAA2/view

start shell
su
tcpdump -i ge-1/2/1 -w /var/tmp/test.log
# Monitor traffic interface
monitor traffic interface <interface-name> no-resolve size 1500 matching "tcp port 179" write-file /var/tmp/bgp.pcap



