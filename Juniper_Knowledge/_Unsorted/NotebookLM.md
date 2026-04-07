# NotebookLM

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

NotebookLM
-免费版每个笔记最多添加50个知识来源
-Notebooklm -websync full site importer: 可以把整个网站抓取到一个笔记本里
-可以在notebooklm里生成一个笔记...，去gemini里引用这个笔记，gemini就能帮你做出一个基于这个笔记的report.../ppt...甚至是做一个网页网站...，无需自己写任何代码
非敏感培训资料：使用 Firecrawl 抓取成 Markdown -> 上传至 NotebookLM（享受它最强的总结和音频播客功能）。
涉及公司机密/SR Case 资料：使用 AnythingLLM 挂载本地文件夹 -> 配合 Ollama (Llama-3-8B) 运行。
如果 NotebookLM 对这两个站解析不好（结构复杂、JS 多、内容不完整），可以用 Firecrawl 或 Jina 先把网页转成 Markdown，再导入 NotebookLM。
维度
Jina Reader
Firecrawl
用法
在 URL 前加 https://r.jina.ai/
需 API、有免费额度
适用场景
单页、少量页面
整站爬取、深度抓取
免费额度
有
有
导出格式
Markdown
Markdown / JSON 等
上手难度
很低
稍高
因为内容在登录后的 SharePoint 里，所有在线抓取工具（NotebookLM、Jina、Firecrawl）都拿不到真实内容。
只能在你已登录的浏览器里，用复制、打印为 PDF 或扩展保存的方式把内容导出，再上传到 NotebookLM。
