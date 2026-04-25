通过jtac tool用jaas解coredump

以后你只要说一句“帮我解这个 coredump”，我就默认做这套流程，不再等你补充格式要求：
直接用 JAAS/JAAS-MCP 起 core
按你同事那种专业 RCA 格式输出
自动去 gnats.juniper.net:1529 搜最相关 PR
只返回最高匹配的 PR#、Synopsis 和简短匹配原因
如果没命中，直接写：Relevant PR: 找不到PR