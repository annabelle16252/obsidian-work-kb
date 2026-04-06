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
