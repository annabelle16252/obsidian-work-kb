# kb-annaw-mcp-server

kb-annaw-mcp is a local MCP tool built for Annaw's Juniper markdown knowledge base.

## KB source folder

- /Users/annaw/Annabelle/AI/Cursor/202501/ollama-rag/knowledge/web-scraper/juniper-docs
- Contains 11k+ markdown files collected from Juniper docs scraping

## Project path

- /Users/annaw/Annabelle/AI/Cursor/202501/Workbook/Knowledge/MCP Server/kb-annaw-mcp

## Provided tools

- kb_root: show which KB folder is in use
- kb_index_stats: count markdown files and top directories
- kb_find_by_filename(keyword): quick filename lookup
- kb_search(query): full-text search with snippets
- kb_read_article(relative_path): open one markdown document

## Run locally

cd "/Users/annaw/Annabelle/AI/Cursor/202501/Workbook/Knowledge/MCP Server/kb-annaw-mcp"
python3 -m venv .venv
source .venv/bin/activate
python server.py

## VS Code MCP config

{
  "mcp": {
    "servers": {
      "kb-annaw-mcp": {
        "type": "stdio",
        "command": "/Users/annaw/Annabelle/AI/Cursor/202501/Workbook/Knowledge/MCP Server/kb-annaw-mcp/.venv/bin/python",
        "args": [
          "/Users/annaw/Annabelle/AI/Cursor/202501/Workbook/Knowledge/MCP Server/kb-annaw-mcp/server.py"
        ],
        "env": {
          "KB_ANNAW_ROOT": "/Users/annaw/Annabelle/AI/Cursor/202501/ollama-rag/knowledge/web-scraper/juniper-docs"
        }
      }
    }
  }
}

## Notes

- This server is read-only and only reads markdown files.
- If you want a narrower corpus, point KB_ANNAW_ROOT to a smaller subfolder.
- The implementation is dependency-free Python stdio MCP, no pip package needed.
