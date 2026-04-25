KB MD file:
最早发布	2022-04-11
最晚发布	2026-01-31
统计文章数	78,021 篇（含重复字段）

日常使用不需要。现在的架构是：

kb_search → 查本地 SQLite，不碰 OneDrive
kb_read_article → 读 DB 里的 body 字段，不碰 OneDrive
唯一需要 OneDrive 同步的场景：你往 OneDrive 上传了新的 KB md 文件，想让它被索引进来。这时需要 OneDrive 先把文件同步到本地，然后重启 kb-cli-mcp server，增量扫描会自动把新文件加进 DB。

平时 OneDrive 不在线/没同步，也完全不影响搜索和读文章。