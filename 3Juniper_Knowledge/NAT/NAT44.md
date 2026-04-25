# NAT44

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

NAT44
# address-pooling-paired #
同一个内部源 IP，总是会被映射到 同一个公网地址（保持绑定关系）。
即使发起多个不同的会话，也不会切换公网地址，只会在这个公网地址上分配不同的端口。
