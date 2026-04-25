

# MediaWiki
这个mcp工作原理是调 MediaWiki API。
AFT Wiki 本身是一个 MediaWiki 站点（aftwiki.juniper.net），MediaWiki 自带搜索 API。aft-mcp 的做法是：
搜索时 → 调 MediaWiki 的 action=query&list=search API
这是 MediaWiki 内置的全文搜索引擎（基于数据库索引或 CirrusSearch/Elasticsearch）
不是 MCP 自己读全站，是 Wiki 服务器端做的搜索，返回匹配结果列表
读页面时 → 调 action=query&prop=revisions API 拿原始 wikitext，然后本地用正则把 wikitext 转成纯文本（去掉模板、表格、HTML 标签等），截断到 4000 字符
读章节列表时 → 调 action=parse&prop=sections
读完整页面时 → 同读页面，但截断到 12000 字符

# evo-wiki
https://evowiki.juniper.net/index.php/Main_Page

# aft-wiki
Advanced Forwarding Toolkit_ represents the modern PFE software architecture that runs on the line cards.
https://aftwiki.juniper.net

MX 里属于 AFT 架构的 FPC/LC 主要是这几类：
1. MPC10E (Ferrari)  
    AFT ZT based  
    机框：MX960 / MX480 / MX240
2. MPC11E (Redbull)  
    AFT ZT based  
    机框：MX2020 / MX2010
3. LC9600 (Alfa Romeo)  
    AFT YT based  
    机框：MX10008
4. MX304 (Bugatti)  
    AFT YT based  
    机框：MX304

同页给的对照（非 AFT）也有：
1. MPC1 到 MPC9（all variants）是 Non-AFT（Ukern, EA and before）
2. MX204、MX10K3 Summit 是 Non-AFT
3. LC2101 (Indus)、LC480 (Renault) 是 Non-AFT EA based

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


