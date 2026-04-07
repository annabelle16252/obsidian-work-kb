# Latency

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

Latency
一般正常链路ping te st 30ms
# 链路 latency产生的原因 #
比如FTP等...，会因为以下因素...导致重传...：
链路crc/phycial issue...：与带宽无关，与传输介质，距离有关
链路拥塞...：...COS congestion, burst,
链路抖动:
处理延时：路由不对称...，
接口流量图无法直接看到 latency，但可以看到是否拥塞/丢包
# Verify latency #
KB79131 [Junos] How to troubleshoot/isolate latency issue
https://juniper.lightning.force.com/lightning/r/Knowledge__kav/ka0Dp000000j1XrIAI/view
We cannot rely on the traceroute output to confirm the latency.
1.switch to the 2nd path to see if latency persistent
2.Access to each of the intermediate router and check for any ingress/egress drops on the interfaces.
3.go to nh to test latency and do same thing via the path till find the node with ping latency
<<<
root@n05-44# show services | display set
set services rpm probe probe1 test test1 target address 1.1.1.2
set services rpm probe probe1 test test1 probe-count 5
set services rpm probe probe1 test test1 probe-interval 10
set services rpm probe probe1 test test1 thresholds rtt 20
> show services rpm probe-results
<<<<
KB28157 ICMP Ping Showing Latency for Host Inbound and Outbound traffic
https://juniper.lightning.force.com/lightning/r/Knowledge__kav/ka0Dp000000jkCnIAI/view
ping is not the best way to test latency on juniper devices, RPM is recommended.
<<<
root@n05-44# show services | display set
set services rpm probe probe1 test test1 target address 1.1.1.2
set services rpm probe probe1 test test1 probe-count 5
set services rpm probe probe1 test test1 probe-interval 10
set services rpm probe probe1 test test1 thresholds rtt 20
> show services rpm probe-results
<<<<
