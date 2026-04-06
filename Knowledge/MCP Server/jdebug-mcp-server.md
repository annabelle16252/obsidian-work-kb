/volume/buildtools/bin/jdebug

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

![[Pasted image 20260405125205.png]]
三个工具可以独立用，也可以串联——比如：jdebug 找到根因代码行 → compile-mcp 修完再确认无新 warning → coverity-mcp 再扫一遍确认无新缺陷。视频中演讲者演示时也是分三段独立演示，每个都能单独产出结果。