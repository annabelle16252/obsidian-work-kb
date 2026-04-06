![[Pasted image 20260331081018.png]]

# Drawio
我是juniper的网络工程师-TAC，在工作中会经常需要用vmm来作lab test，也需要绘制topology。
cursor给我做了个vmm生成文件，file:///Users/annaw/Annabelle/AI/Cursor/202501/index.html
这个工具里有一个功能，‘generate topology diagram’，可以根据我选的vmm配置的设备，端口和连线来绘制topology图。我让cursor尝试了很多方式实现，画出的图都不符合网络topology要求和逻辑。
下面这个是github上一个开源的画网络topology图的工具：
https://github.com/amrelhusseiny/drawio_network_plot
你帮我把他clone到我的本地并安装，生成一个我自己的工具。并且需要结合我上面这个vmm工具来定制更改，让他更适合放到我的vmm工具里来生成topology。


1. Clone & 安装 drawio_network_plot
克隆到：[/Users/annaw/Annabelle/AI/Claud Code/drawio_network_plot/](../Annabelle/AI/Claud Code/drawio_network_plot/)
用虚拟环境安装：.venv/bin/pip install -e .
2. 定制 Python Topology 生成器
新文件： [juniper_vmm_topology.py](../Annabelle/AI/Claud Code/drawio_network_plot/juniper_vmm_topology.py)

针对Juniper VMM的设备类型映射（VMX→router, vPTX→router, vQFX→l3_switch, vEX→l2_switch, vUbuntu/vCentOS/WindR→server, VIXIA/Spirent→server）
三层自动布局：Routers（顶层）→ Switches（中层）→ Testers/Servers（底层）
端口标签用draw.io原生edge label（附着在连线上，移动节点时跟随）
CLI用法：python juniper_vmm_topology.py topology.json [-o output.drawio]
3. 改进 VMM 工具的 JS topology 生成
修改： script.js

改进项	旧实现	新实现
布局方向	RIGHT（左→右）	DOWN（上→下，层级结构）
端口标签	浮动文本框（位置固定，移动节点后标签脱离）	draw.io edge label（附着连线，自动跟随）
连线弯折	忽略ELK弯折点	包含完整bend points，正交路由
设备分层	无	自动识别 router/ptx/switch/server/tester 排序
PTX支持	归类为router但无区分	单独ptx类型，正确图标
新增按钮	—	"Export topology JSON" — 下载JSON供Python工具使用
工作流程
方式A（浏览器内，即时）：点击 Generate topology diagram → 下载 .drawio 文件（布局已改善）

方式B（Python精细控制）：

点击 Export topology JSON → 下载 vmm-topology-xxx.json
运行：

cd "/Users/annaw/Annabelle/AI/Claud Code/drawio_network_plot"
.venv/bin/python3 juniper_vmm_topology.py ~/Downloads/vmm-topology-2026-03-25-0230.json
