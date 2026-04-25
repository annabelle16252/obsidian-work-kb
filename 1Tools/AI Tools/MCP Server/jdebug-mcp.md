/volume/buildtools/bin/jdebug

https://compiler-sa-wiki.juniper.net/index.php/Visual_Studio_Code#Jdebug_MCP

Use JDDebug MCP and server to root cause this core file:
/volume/CSdata/nsivakumaran/2026-0127-074359/escalations/30Jan/RE0-Mgd-core-Oct9-13/mgd.core.0.gz

ttsv-shell001 


# 手动执行
/volume/buildtools/bin/jaas --qnc /volume/CSdata/nsivakumaran/2026-0127-074359/escalations/30Jan/RE0-Mgd-core-Oct9-13/mgd.core.0.gz

![[Pasted image 20260421113823.png]]

Provide:
1. Exact backtrace with frame numbers
2. The crashing function and line number
3. Root cause (memory initialization issue, null pointer, buffer overflow, etc.)
4. Recommended fix

```
{
  "servers": {
    "jdebug-mcp": {
      "type": "stdio",
      "command": "/volume/buildtools/bin/jdebug-mcp",
      "args": ["--debug"],
      "env": {
        "LOGPATH": "/b/workspace/",
        "JDEBUG_PATH": "/volume/buildtools/bin/jdebug",
        "DEBUG_INSTRUCTIONS_FILE": "/volume/hab/Linux/Ubuntu-22.04/x86_64/vscode/mcp/conf/HowToDebug.txt"
      }
    }
  }
}

```


三个工具可以独立用，也可以串联——比如：jdebug 找到根因代码行 → compile-mcp 修完再确认无新 warning → coverity-mcp 再扫一遍确认无新缺陷。

✅ 如果输出指向一个明确的代码缺陷 → 后续用 compile-mcp + coverity-mcp
✅ 如果输出是配置问题或第三方库问题 → 可能不需要 compile-mcp/coverity-mcp