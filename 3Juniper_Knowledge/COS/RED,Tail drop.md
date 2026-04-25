# RED drop
RED drop 是主动队列管理（AQM）机制触发的拥塞丢包，表明出口队列已超过配置的丢弃阈值，设备在队列满之前主动丢弃包。

tail drop是带宽没了开始丢，RED是在带宽没了之前提前丢。

引起RED drop可能原因：burst(buffer过小)或者drop profile(scheduler 配置里)

若 tail-drop 多 → 真实拥塞，需扩容或流量shaping。
若 只有 red-drop，tail-drop 很少 → 大概率 drop-profile 阈值太低，调整曲线即可

查看drop-profile：
show class-of-service interface ge-6/0/0
show class-of-service drop-profile

set class-of-service interfaces ge-6/0/0 scheduler-map TRUNK-scheduler-6COS

set class-of-service schedulers TRUNK-standard-scheduler-6COS drop-profile-map loss-priority low protocol any drop-profile standard