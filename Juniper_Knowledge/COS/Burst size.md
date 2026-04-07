# Burst size

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

Burst size
Methods to calculate the burst size rate limit
https://juniper.lightning.force.com/lightning/r/Knowledge__kav/ka0Dp000000jgWVIAY/view
if the rate limit is 10 Mbps, then the burst size should be 1.25k.
Burst Time = 
burst-size-limit 
shaping-rate ...Burst Time = 
burst-size-limit 
shaping-rate
shaping...-rate如果没配...，就默认是端口带宽
:~ Add - 
Generate Stream Block >< Delete 
Edit ... 
Ej Copy Wizard. 
0.89999 % 
Scheduling Mode 
Bandwidth Utilization (%): 0.9 
Port Based 
Burst Size: 
24 
Duration Mode: 
Seconds 
V 
Load per Stream Block 
Rate Based 
Inter Frame Gap: 
12 
Second(S): 
0.005 
Advanced Interleaving 
Group ID will be set in the stream block grid. 
Inter Frame Gap Unit: 
bytes 
Advanced ... 
) Manual Based 
Schedule ... 
Scheduling mode graphical example 
Traffic 
Strean 
Frame Length 
Fixed Frame 
Status 
Active 
Name 
Tags 
Index 
Controlled By 
Traffic Pattern 
Type 
x Port 
Rx Port 
State 
Count 
Path Count 
Load 
Load Unit 
Mode 
MIX Distribution 
Group 
Length 
Le 
IStreamBlo .. 
Click to ad. 
1400 
10 
generator 
Port 
Port //3 3/4 
Ready 
90 
IMbps 
|Fixed ...:~ Add - 
Generate Stream Block >< Delete 
Edit ... 
Ej Copy Wizard. 
0.89999 % 
Scheduling Mode 
Bandwidth Utilization (%): 0.9 
Port Based 
Burst Size: 
24 
Duration Mode: 
Seconds 
V 
Load per Stream Block 
Rate Based 
Inter Frame Gap: 
12 
Second(S): 
0.005 
Advanced Interleaving 
Group ID will be set in the stream block grid. 
Inter Frame Gap Unit: 
bytes 
Advanced ... 
) Manual Based 
Schedule ... 
Scheduling mode graphical example 
Traffic 
Strean 
Frame Length 
Fixed Frame 
Status 
Active 
Name 
Tags 
Index 
Controlled By 
Traffic Pattern 
Type 
x Port 
Rx Port 
State 
Count 
Path Count 
Load 
Load Unit 
Mode 
MIX Distribution 
Group 
Length 
Le 
IStreamBlo .. 
Click to ad. 
1400 
10 
generator 
Port 
Port //3 3/4 
Ready 
90 
IMbps 
|Fixed
Burst Size（一次突发多少帧）
Seconds: interval多少秒
Frame Size = 512 Bytes
Burst Size = 1000 frames
Burst Interval = 10 ms
每个 burst 发出的数据量是：
1000 x 512/8=4.096Mb
平均速率计算：
4.096Mb/0.01ms=409.6Mbps
https://juniper.lightning.force.com/lightning/r/Knowledge__kav/ka0Dp000000jgWVIAY/view
Burst (bytes) = (rate [bps]) * 0.000125 [sec/interval]) or (maximum packet size [bits])
