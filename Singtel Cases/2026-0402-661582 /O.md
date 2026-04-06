
/volume/case_2026/2026-0402-661582
/volume/CSdata/annaw/case/

Mitigation when CHASSISD_OVER_TEMP_CONDITION happens
这个case，我们想知道如果把高温的FPC offline了，shutdown process是不是会停了

Junos: 23.2R2-S3.8



# Analysis

We did not find code evidence showing that offlining/removing the overheated FRU within the 240-second window will explicitly cancel the pending chassis shutdown. The available code confirms the presence of thermal thresholds, chassis over-temperature shutdown logic, and FRU/FPC offline handling, but no direct logic was found stating that an offline/remove action aborts the timer once started.

The most reasonable interpretation is that such an action may help only if it clears the chassis over-temperature condition before timer expiry. However, this is not guaranteed, since thermal sensing/state refresh and cooldown are not instantaneous, and the issue may already have become chassis-wide.

The Juniper KB provided is consistent with this conclusion: it does not describe FRU offline as a supported mitigation, and instead shows that impaired cooling can rapidly escalate into a chassis-wide over-temperature event leading to shutdown of all FRUs. Its recommended practice is to complete fan tray work very quickly, or halt the chassis beforehand if more maintenance time is required.


There is no confirmed software rule stating that offlining the overheated FRU within the 240-second window will stop the chassis shutdown. The shutdown appears to depend on whether the over-temperature condition persists. Since sensor update timing and thermal cooldown are not instantaneous, offlining the FRU should not be treated as a guaranteed mitigation.


我把相关的source code都查了一圈，没有找到code级别能证明fru offline可以阻止chassis shutdown的内容。

chassisd:Jul 30 17:44:17 CHASSISD_OVER_TEMP_CONDITION: Chassis temperature over 90 degrees C (but no fan/impeller failure detected); routing platform will shutdown in 240 seconds if condition persists
这个log写着如果chassis温度高持续超过240s就会shutdown，理论上是有机会的不shutdown的。但温度高是扩散到整个chassis的，多快offline，chassis整体温度才能降下来这个也取决于实际环境和设备负载情况。还有不通设备，fpc不通slot位置等。

kb/pr没找到啥，就有一个ptx的但跟这个情况也不一样。如果你实在想测，我明天可以找找有没有命令能做出来fpc温度高，然后你门在lab里试试看多快offline fru能有帮助。但这个也只是个参考数值，真实情况取决于实际环境。


但仍然缺的关键证据

你真正想找的是类似下面这种语义：

if overtemp condition clears, cancel shutdown timer
if hot FRU removed, reevaluate chassis thermal state
if no FRU exceeds red threshold, abort chassis shutdown
在这两个文件里都没看到。

也就是说，到现在为止，证据链还是这样：

日志证明：if condition persists 才会 240 秒后整机掉电
schema/ODL 证明：系统确实有正式的温度阈值、red alarm、fire-shutdown、FRU 温度状态、FPC offline 状态模型
但还没有直接实现证据证明：offline/remove hot FRU 会显式取消 pending shutdown
所以目前最稳的判断仍然是

如果在 240 秒内把过热 FRU offline 或取出，并且这样做让 chassis over-temperature condition 清除，那么从逻辑上“有机会”阻止整机 shutdown。
但这是基于“condition persists”这句话和温度状态模型做出的推断，不是从这两个文件里直接看到的明确代码分支。


# PR
https://gnats.juniper.net/web/default/1133952#scope_tab
https://gnats.juniper.net/web/default/1329660#external_tab
not fixed ptx