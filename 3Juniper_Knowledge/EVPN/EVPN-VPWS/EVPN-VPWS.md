# Config
```
routing-instances {
    ASER-205-VPWS-SYS {
        instance-type evpn-vpws;
        interface ae244.305;             ← AC 接口，VLAN 305
        route-distinguisher 10149:11333;
        vrf-import VPWS-ASER-205-VPWS-SYS-IMPORT;
        vrf-export VPWS-ASER-205-VPWS-SYS-EXPORT;
        protocols {
            evpn {
                interface ae244.305 {
                    vpws-service-id {
                        local 11109;     ← 本端 EVI label/service ID
                        remote 11108;    ← 对端 PE 的 service ID
                    }
                }
            }
        }
    }
}

AC 接口配置模式（ae244.305）：
unit 305 {
    encapsulation vlan-ccc;
    vlan-id 305;
    input-vlan-map  { swap; vlan-id 1000; }   ← 入方向 VLAN swap：305→1000
    output-vlan-map swap;                       ← 出方向 VLAN swap：1000→305
    esi {
        00:00:0a:13:00:07:a3:05:00:00;         ← ESI（以太网段标识符）
        single-active;                          ← 主备保护模式（非 all-active）
        df-election-type preference { value 3000; }
    }
    family ccc {
        policer { input Limit-100M; }          ← 入向限速 100M
    }
}


```

# 工作原理

![[Pasted image 20260413082001.png]]
![[Pasted image 20260413082030.png]]
物理上没有一根线从 CE-A 拉到 CE-B，但 VPWS 让两端 CE 感知不到中间的 IP/MPLS 网络，行为和专线一模一样 —— 所以叫"伪"线，即仿真出来的线。

![[Pasted image 20260413080550.png]]
控制平面（BGP EVPN）：
1.MTSINARE-PE01 通过 iBGP（AS 10149）连接两台 RR（101.234.3.128 / 101.234.3.130），family evpn signaling
2.每个 EVPN-VPWS 实例通告 EVPN Type-1 Auto-Discovery Route（AD Route），携带：
- ESI（如 00:00:0a:13:00:07:a3:05:00:00）
- vpws-service-id local 11109（本端 label）
- Route Target target:10149:10669
3.对端 PE 通过匹配相同 RT + remote service-id 11108 来识别这条电路对应的另一端
4.双端 AD Route 都存在时，CCC（Circuit Cross Connect）电路状态变为 ccc up

数据平面：
1.CE → PE：以太帧携带 VLAN 305 进入 ae244.305，VLAN swap 305→1000，封装 MPLS 隧道头（标签来自 BGP EVPN 协商的 VPWS label）发往对端 PE
2.PE → CE：从 MPLS 隧道解封，VLAN swap 1000→305，从 ae244.305 发出
3.完全透明的点到点 L2 电路，客户看到的是两端 VLAN 305 直连

ESI + Single-Active：
每个 AC 接口配置了 ESI，表明接入侧支持多归（multi-homing）
single-active 模式：同一时刻只有一条 AC 路径在转发（另一台 PE 作为备份）
DF（Designated Forwarder）选举用 preference value 3000 控制

# VPWS DF
```
esi {
    00:00:0a:13:00:07:a3:05:00:00;
    single-active;
    df-election-type preference { value 3000; }
}
```
VPWS 是点到点 L2 电路，没有 BUM 流量问题，DF 的意义在这里是：
决定哪台 PE 的 AC 接口处于 Active 状态（另一台 PE 的 AC 为 Standby，ccc 状态为 Down）
preference value 3000 —— 值越大越优先成为 DF，客户用这个来手动控制主备路径
当 DF 的 AC 链路故障时，EVPN 控制平面重新选举，备份 PE 接管变为新 DF，服务倒换

# Command
![[Pasted image 20260413102707.png]]
r1_RE	10.52.130.151	MX10008
r2_RE	10.52.130.110	MX10008
r3_RE	10.52.148.183	MX10008
r4_RE	10.52.141.161	MX10008
r5_RE	10.52.139.92	MX10008
r6_RE	10.52.145.255	MX10008
ce1_RE0	10.52.148.33	MX480
ce2_RE0	10.52.137.149	MX960

一、BGP 邻居及 EVPN 路由验证
```
# 1. 检查 BGP 邻居状态（应显示 Established）
show bgp summary

# 2. 检查 EVPN 路由表
show route table bgp.evpn.0
```

```
二、VPWS 伪线及连接验证
# 1. 查看所有 L2VPN 伪线状态（最关键，应显示 Up）
show l2vpn connection

# 2. 详细查看伪线状态（包含 MTU、流量计数）
show l2vpn connection detail

# 3. 查看伪线 EVPN 标记（Route Distinguisher、Export/Import community）
show l2vpn connection extensive | match "Route-Distinguisher|Community"

# 4. 查看 VPWS 伪线流量统计
show l2vpn connection statistics

# 5. 验证 Martini LSP 状态（底层 MPLS 隧道，应为 Up）
show l2circuit connections

三、CE 侧验证
在 CE（ce1、ce2）上执行：
# 1. 查看 CE 到 PE 的连接
show interfaces ge-0/0/0 | match "Physical|Protocol"

# 2. 查看 CE 的 MAC 地址表
show ethernet-switching table

# 3. 验证 CE 侧的 VLAN 配置（应与 VPWS 映射一致）
show configuration interfaces ge-0/0/0

四、流量和 MAC 学习验证
# 1. 在 PE 上查看学习到的 MAC（应包含远端 CE 的 MAC）
show ethernet-switching table

# 2. 查看 PE 上的 Bridge 域状态（VPWS 创建的隐式 BD）
show bridge-domains

# 3. 检查 EVPN MAC 学习（Type-2 MAC/IP 路由）
show route table bgp.evpn.0 detail | match "Type 2"

# 4. 验证伪线上的流量（应有双向数据包计数增加）
show l2vpn connection statistics | match "packets|octets"

五、环路防护及高级检查
# 1. 查看分割地平线（Split Horizon）状态
show l2vpn connection detail | match "Split Horizon"

# 2. 验证 BPDU 过滤器是否启用（防止 STP 风暴）
show configuration interfaces xe-0/0/0 | match bpdu
show bridge-domains | match "BPDU"

# 3. 检查异常流量计数（丢弃、错误）
show l2vpn connection statistics | match "drop|error"

# 4. 验证冗余 PE 配置（如有 VRRP/MC-LAG）
show configuration interfaces ae0

# 5. 查看所有 EVPN 社区属性（确保 RT 配置一致）
show route table bgp.evpn.0 extensive | match "Community"

六、端到端连通性测试
# 在 CE1 上 ping CE2 的 IP（需提前配置 IP 地址）
ping 192.168.1.2

# 或从 CE1 SSH 到 CE2
ssh regress@192.168.1.2

# 在 CE 上执行 traceroute（验证路径）
traceroute 192.168.1.2

七、配置一致性检查
# 1. 查看 VPWS 配置（Route Distinguisher、Target 社区）
show configuration routing-instances <vpws-instance-name>

# 2. 验证所有 PE 的导入/导出社区一致
show configuration routing-instances | grep -A 10 "target"

# 3. 查看接口到 VPWS 的映射
show configuration interfaces ge-0/0/0 | grep "family bridge"

快速验收检查单（重点）

项目	命令	预期结果
BGP 邻居	show bgp summary	所有邻居 Established
EVPN 路由	show route table bgp.evpn.0	有 Type 1/2/4 路由
VPWS 连接	show l2vpn connection	全部 Up
伪线流量	show l2vpn connection statistics	双向数据包递增
CE 连通	ping 跨 CE IP	100% 成功
```

ESI Verification
==show evpn vpws-instance VPWS_A==
在r1/r2/r5/r6 上
![[Pasted image 20260415174257.png]]

CCC / L2VPN 路径验证
```
# 在 r1 上查看 CCC 状态
show connections
show l2circuit connections

# 也可查
show mpls lsp
```

PE-PE 三层 IP 连通性
```
show ldp session



```


PE-CE 三层 IP 连通性
```
# r1 ae0.110 = 172.16.11.0/31
# ce1 的对端 IP 应为 172.16.11.1（视具体配置）
ping 172.16.11.1 routing-instance <vrf> count 5   # 从 r1 ping ce1

# r2 ae0.120 = 172.16.12.0/31
ping 172.16.12.1 routing-instance <vrf> count 5

# r5 ae0.210 = 172.16.21.0/31
ping 172.16.21.1 routing-instance <vrf> count 5

# r6 ae0.220 = 172.16.22.0/31
ping 172.16.22.1 routing-instance <vrf> count 5
```

CE 端到端流量验证
```
# ce1
show interfaces ae0 terse
show interfaces ae0.0 detail | match "inet"

# ce2
show interfaces ae0 terse
show interfaces ae0.0 detail | match "inet"

```
然后跨 EVPN/CCC 做端到端 ping（从 ce1 ping ce2）：
```
# 从 ce1
ping <ce2_ae0_ip> count 10 rapid
ping <ce2_ae0_ip> count 10 size 1400   # 大包测试

# 从 ce2
ping <ce1_ae0_ip> count 10 rapid

# traceroute 验证路径
traceroute <ce2_ae0_ip> no-resolve
<<<
regress@ce1_RE0# run traceroute 192.168.100.2 
traceroute to 192.168.100.2 (192.168.100.2), 30 hops max, 52 byte packets
 1  192.168.100.2 (192.168.100.2)  85.048 ms  80.482 ms  99.529 ms
 <<<
```
流量灰度验证 — 接口计数器
```
# 在 r1/r2/r5/r6 执行，确认 ae0 有流量增长
show interfaces ae0 | match "Input|Output|bps|pps"
show interfaces et-0/0/0:3 | match "Input|Output|bps|pps"

# 等 30 秒后重跑，确认计数器递增
clear interfaces statistics et-0/0/0:3
# (发起 ping 流量后)
show interfaces et-0/0/0:3 statistics
```
快速一键验收（批量）
```

```