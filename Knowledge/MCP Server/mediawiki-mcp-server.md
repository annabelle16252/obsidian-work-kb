https://aftwiki.juniper.net
**AFT** - _Advanced Forwarding Toolkit_ represents the modern PFE software architecture that runs on the line cards.


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

mediawiki_server.py — MCP Server 主体，注册了 4 个工具：

Tool	功能
search_wiki	搜索 Wiki，返回匹配文章列表
get_article	获取文章简介（intro 段落）
get_article_full	获取文章完整内容（截断 12000 字）
get_article_sections	列出文章所有章节标题及索引
