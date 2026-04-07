

15:20:23 -- test chassis fan all speed 100  
  15:20:28 -- test chassis fan all speed 10
  你先做好event 然后直接0


blocking air flow` 或 `removing fan tray for a while`（短时间挡风/拔风扇托盘来制造过温）


1. 在目标 FPC 的进/出风区域做局部挡风（小面积、短时间），不要挡整机风道。
2. 同时在目标 FPC 端口打高流量，让它自身发热更快。


3. 风扇位置：图最下方整块黑色栅格区域（前面板底部）就是风扇/进风区域。
4. 主要散热区域：风从底部进风后经过中间 FPC/MPC 板卡区（你插 MPC 的区域）再被机框风道带走。
5. 你做过温实验时，重点关注中间板卡区对应槽位（比如 FPC0）和底部风扇区气流是否受阻。


https://www.juniper.net/documentation/us/en/hardware/mx960/index.html
![[Pasted image 20260407094408.png]]

0-5， re ， 7-11
机框风道和该槽位附近气流
# Topo
FPC 0 REV 07 750-063741 CAGZ2064 MPCE Type 2 3D Q

Dear Team,

Could you help to arrange a MX960 for FPC temperature test?

Need one MX960 with two MPCE Type 2 3D Q cards in slot 6 & slot 9.  Please help to block the slot 6 FPC's air flow to make its temperature going high.

Please contact me via Teams so that we can do the test together.

Thanks
Annabelle


dont touch fpc， too hot

show log messages | match "CHASSISD_TEMP_HOT_NOTICE|CHASSISD_OVER_TEMP_CONDITION|CHASSISD_OVER_TEMP_SHUTDOWN_TIME"