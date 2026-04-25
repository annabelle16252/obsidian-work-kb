MCP helps to merge the capabilities of LLM with Juniper servers.

/volume/pfetools/mcp/config$
# Juniper MCP servers
**Mediawiki**
![[Pasted image 20260404105000.png]]
can search for commands
https://aftwiki.juniper.net
```
{
  "defaultWiki": "aftwiki.juniper.net",
  "wikis": {
    "aftwiki.juniper.net": {
      "sitename": "AFT Wiki",
      "server": "https://aftwiki.juniper.net",
      "articlepath": "/index.php",
      "scriptpath": "",
      "token": null,
      "private": false
    }
  }
```

mediawiki_server.py — MCP Server 主体，注册了 4 个工具：

Tool	功能
search_wiki	搜索 Wiki，返回匹配文章列表
get_article	获取文章简介（intro 段落）
get_article_full	获取文章完整内容（截断 12000 字）
get_article_sections	列出文章所有章节标题及索引


**junos**
connect and configure juniper devices

**gnats**

**jdebug**
![[Pasted image 20260404105923.png]]
**vmm**



# Document
/Users/annaw/Annabelle/Technical/MCP/MCP_Server_introduction.pdf
![[Pasted image 20260402142145.png]]



VS Code 里的 Copilot Chat 作为统一入口，下面接多个 MCP server，例如 Junos MCP、Wiki MCP、Qdrant MCP、VMX MCP、BSDT MCP、SharePoint MCP 等，形成一个“工具池”。
![[Pasted image 20260402171509.png]]
STDIO - no token needed
HTTP - token needed

![[Pasted image 20260404085724.png]]
