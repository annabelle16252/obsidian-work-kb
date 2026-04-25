# Firewall

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

Firewall
# 转发表filter#
forwarding-options {
family inet {
filter {
output CHINA-NON_CHINA-OUTGOING;
}
-和接口上的filter是组合关系，都生效。所以有可能发生冲突。
-只针对该router上的转发流量生效，“host / control-plane traffic”这些流量不受影响。
