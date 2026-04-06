https://github.com/Juniper/junos-mcp-server/blob/main/README.md


A Model Context Protocol (MCP) server for Juniper Junos devices that enables LLM interactions with network equipment.

using SSH key-based authentication

VS Code Copilot → mcp.json → 本机 junos-mcp-server → devices.json → 两台 Junos 设备
SSH KEY/ Token based

# Command
***mcp server needs to be restarted everytime when MAC reboot***
快捷方式：
~/junos-mcp-server/start-mcp.sh
普通命令：
cd ~/junos-mcp-server && source .venv/bin/activate
python jmcp.py -f devices.json -t streamable-http

# Add new device to MCP
方法一：Copilot 帮你来做
设备名, IP地址, 用户名, 密码, 是否需要跳板, 跳板信息 
方法二：按照以下manul来做：
/Users/annaw/junos-mcp-server/docs/mcp-device-template.md

# Topo
jtac-mx480-r2033 10.219.35.20
jtac-mx480-r2607 10.219.34.198

labroot@jtac-mx480-r2033-re0> show lldp neighbors   
Local Interface    Parent Interface    Chassis Id          Port info          System Name
ge-0/2/0           -                   10:0e:7e:3f:b9:c0   651                jtac-mx480-r2607-re0.ultralab.juniper.net
ge-0/2/1           -                   10:0e:7e:3f:b9:c0   653                jtac-mx480-r2607-re0.ultralab.juniper.net

labroot@jtac-mx480-r2607-re0> show lldp neighbors    
Local Interface    Parent Interface    Chassis Id          Port info          System Name
ge-0/2/0           -                   30:b6:4f:e6:7a:c0   533                jtac-mx480-r2033-re0.ultralab.juniper.net
ge-0/2/1           -                   30:b6:4f:e6:7a:c0   537                jtac-mx480-r2033-re0.ultralab.juniper.net




# Installation
1.mcp server installation
cd ~/junos-mcp-server
source .venv/bin/activate <<<pip要在虚拟环境中才能安装，mac本地安装不了
pip install -r requirements.txt
(.venv) annaw@JNPR-MAC-DL6D54 junos-mcp-server % 
2. edit devices.json 
设备名, IP地址, 用户名, 密码, 是否需要跳板, 跳板信息
jtac-mx480-r2033 10.219.35.20 labroot lab123
jtac-mx480-r2607 10.219.34.198 labroot lab123
3. 启动 MCP server
4. 配置 VS Code
 {
     "mcp": {
         "servers": {
             "junos-mcp": {
                 "url": "http://127.0.0.1:30030/mcp/"
             }
         }
     }
 }



/Users/UserID/Library/Application Support/Code/User/mcp.json


# Document
![[Pasted image 20260402142145.png]]

STDIO 模式（单机）
   MCP Client  --stdin-->  MCP Server
   MCP Client  <-stdout--  MCP Server
- 同一台机器上运行
 - Client（如 Claude Desktop/Copilot）直接启动 Server 进程
 - 通过标准输入/输出管道通信
 - 一对一：一个 Client 对应一个 Server 进程
 - 简单、安全，无需网络端口
 
 Streamable HTTP 模式（网络环境）
 MCP Client 1  --HTTP POST-->   MCP Server
 MCP Client 1  <--SSE流式响应--  MCP Server
 
 MCP Client 2  --HTTP POST-->   MCP Server  
 MCP Client 2  <--SSE流式响应--  MCP Server
 - Server 是一个独立运行的 HTTP 服务
 - 多个 Client 可以同时连接同一个 Server
 - Client 发 HTTP POST 请求，Server 用 SSE（Server-Sent Events） 流式返回响应
 - 适合跨机器、多人共用