# Calculation
https://juniper.lightning.force.com/lightning/r/Knowledge__kav/ka0Dp000000jgWVIAY/view
if the rate limit is 10 Mbps, then the burst size should be 1.25k.

![[Pasted image 20260419153131.png]]
shaping-rate如果没配，就默认是端口带宽


![[Pasted image 20260419153237.png]]
Burst Size（一次突发多少帧）
Seconds: interval多少秒

• Frame Size = 512 Bytes
• Burst Size = 1000 frames
• Burst Interval = 10 ms
每个 burst 发出的数据量是：
1000 x 512/8=4.096Mb
平均速率计算：
4.096Mb/0.01ms=409.6Mbps

https://juniper.lightning.force.com/lightning/r/Knowledge__kav/ka0Dp000000jgWVIAY/view
Burst (bytes) = (rate [bps]) * 0.000125 [sec/interval]) or (maximum packet size [bits])


