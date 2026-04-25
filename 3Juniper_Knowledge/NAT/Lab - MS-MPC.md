# Lab - MS-MPC

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

Lab - MS-MPC
https://junipernetworks.sharepoint.com/sites/UltraLabServiceCatalogue/SitePages/CGNAT-Using-IXIA-and-Mx.aspx
2025-0908-849017
jtac-mx480dc-r2017(xe-5/0/0)------------(xe-0/0/0)jtac-mx5-t-r2008
jtac-mx480dc-r2017(xe-5/0/1)------------(xe-0/0/0)jtac-mx5-t-r2007
NH based NAT
ISP A 提供公网地址段 ...203.0.113.0/24...，下一跳是 ...ISP-A-gw...。
ISP B 提供公网地址段 ...198.51.100.0/24...，下一跳是 ...ISP-B-gw...。
使用 next-hop based NAT，可以让发往不同 ISP 的流量自动选对 NAT 地址段。
