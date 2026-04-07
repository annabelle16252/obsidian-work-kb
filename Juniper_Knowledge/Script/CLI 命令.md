# CLI 命令

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

CLI 命令
# CLI show #
except:
> show log messages | except "ssh|mib|snmp"| match ddos
# Find & Match #
> show system resource-monitor summary |find "Slot # 3"   <<< ...把查找内容之后整个一个段落输出...
> show system resource-monitor summary |match "Slot # 3"  <<< ...只输出这一行
