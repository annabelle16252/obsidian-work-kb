
/volume/case_2026/2026-0402-661582
/volume/CSdata/annaw/case/2026-0402-661582

Mitigation when CHASSISD_OVER_TEMP_CONDITION happens
这个case，我们想知道如果把高温的FPC offline了，shutdown process是不是会停了

Junos: 23.2R2-S3.8


OT = Over Temperature
FT = Fire Temperature
超过 OT 且持续 240 秒，shutdown FRU
到了 FT，系统会采取更激烈的保护动作
文档里写的是：**超过 FT 持续 4 秒就触发保护**
    - `MX2010/MX2020`：shutdown 该 FRU
    - `MX240/MX480/MX960`：shutdown 整个 chassis
放到你这个 case 里，就是：
- 如果只是到 **OT**，文档支持先处理单个 FRU
- 如果已经到 **FT**，对 `MX960/480/240`，文档定义的行为就是**整机关机**，不是只关单板
```
labroot@jtac-mx480-r2033-re0> show chassis temperature-thresholds 
                                        Fan speed      Yellow alarm      Red alarm      Fire Shutdown
                                       (degrees C)      (degrees C)     (degrees C)      (degrees C)
Item                                  Normal  High   Normal  Bad fan   Normal  Bad fan     Normal
Chassis default                           48    54       65       55       80       65        100
Routing Engine 0                          70    80       95       95      110      110        112
FPC 0                                     55    60       75       65      105       80        110


Red alarm -> fire shutdown : 240s
1. 看到 CHASSISD_OVER_TEMP_CONDITION 就表示已进入倒计时。
2. 日志里会显示 remaining seconds（例如 shutdown in 240 seconds / 120 seconds / 4 seconds）。
3. 如果最终看到 CHASSISD_OVER_TEMP_SHUTDOWN_TIME，说明倒计时走完，系统已执行全 FRU 下电

```

# Doc
https://junipernetworks.sharepoint.com/:w:/r/sites/plm/_layouts/15/Doc.aspx?sourcedoc=%7BF37B4B07-F7C0-40AC-A5A8-68CCBAB77C64%7D&file=MX_Power_Management.docx&action=default&mobileredirect=true&DefaultItemOpen=1

# PR
https://gnats.juniper.net/web/default/1133952#scope_tab
https://gnats.juniper.net/web/default/1329660#external_tab
not fixed ptx