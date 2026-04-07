# RoCE

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

RoCE
# ...传统以太网直接用于AI训练的缺点... #
1.AI...训练的特殊性会导致...ECMP...带宽负载不均衡
2....网络拥塞控制不佳
3....无法提供足够的带宽和低延迟，因此...Spectrum-X...成为更优的选择
Al优化的网纟各有哪些独特的特征
高带宽
大象流(ElephantFlow)
同步的和突发的流0
主要是ROMA流量
训练工作会持续很长的时间（以刁、时/天计算）
长尾延迟会严重影响工作完成的时间
All-to-AllSchematicTrafficPattern
所以传统以太网需要作相应的...AI...优化
# PCIe #
带宽低，即使...PCIe 5.0...也只有...64GB...，无法满足...AI ...需求。而...GPU...的内存带宽是...T...级别的
NVLINK...解决了...GPU...与...GPU...或其他部件的带宽，点对点连接低延迟。
# RDMA #
相比...TCP/IP...提高了数据传输速度。技术实现方式：
1.IB ...网络：需要专门的...switch...和...IB...适配器
CPU
GPU
CONNECTX
PCIeSwitch
CPU
CONNECTX
GPUDirectROMA
SystemMemory
0
GPI-J
IB...是...NVDIA...的一套全新的架构，从链路层到传输层都无法与现有的以太网兼容，如果想用...IB...，要购买全套...IB...设备
2.RoCE...：在以太网上实现...RDMA
优势：用户从以太网切换到...RoCE...只需要购买支持...RoCE...的网卡...-ConnectX...就可以了，其他都兼容以太网现有设备。
CCYinoctX—525GbFDt旧《．伴舅t
P28，PCIeGen3．0黑8
ConnectX．．6IOOGbE《．凶rt
QSFPS6.PCIo4．06
ConnectX-74伽GS．以
OSFP.PCIO5．016
3.iWARP...：基于...TCP/IP...实现...RDMA
