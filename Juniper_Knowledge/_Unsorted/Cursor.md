# Cursor

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

Cursor
Cursor documentation learning:
https://cursor.com/docs/get-started/quickstart
必读章节： * Quickstart: 学习最核心的两个快捷键：Cmd + K（行内编辑）和 Cmd + L（侧边栏聊天）。
Composer (Cmd + I): 这是 Cursor 的“大招”，可以跨文件生成整个功能模块（比如同时修改 HTML, CSS 和 JS）。
cmd+点击文件...：在terminal里dir下点击打开文件
先ask模式提出自己的plan
然后agent模式生成代码
# Cursor AI编程零基础全套教程-b站 #
https://www.bilibili.com/video/BV1ffmqBWErp/?spm_id_from=333.337.search-card.all.click&vd_source=9a41d251e1b4b29be7296cf7acdcf3fe
Cursor的免费替-字节的trae
ctrl...+k...：选定区域对话
ctrl...+L...：全文档对话
@: 引用,注记
rules: give AI a manual
许 多 Al 工 具 支 持 模 板 、 规 则 或 快 捷 命 令 , 例 如 “/generate" 、 "/optimize" 等 。 熟 悉 这 些 命 令 可 以 节 
省 时 间 。 为 常 用 任 务 创 建 自 定 义 模 板 , 以 便 快 速 调 用 。 
除 此 之 外 ,Cursor 的 AI 功 能 是 其 核 心 亮 点 , 以 下 常 用 的 快 捷 键 可 帮 助 您 更 高 效 地 使 用 AI 辅 助 功 能 : 
· Tab: 接 受 Al 提 供 的 代 码 建 议 或 自 动 补 全 。 
· Ctrl/Cmd + B: 显 示 或 隐 藏 资 源 管 理 器 面 板 。 
· Ctrl/Cmd + K: 打 开 AI 提 示 栏 , 用 于 生 成 新 代 码 或 编 辑 现 有 代 码 。 
· Ctrl/Cmd +L: 打 开 AI 聊 天 面 板 , 可 用 于 提 问 、 编 辑 代 码 或 对 整 个 项 目 进 行 操 作 。 
· Ctrl/Cmd + 1: 打 开 AI 代 理 , 用 于 跨 文 件 编 辑 代 码 或 对 整 个 项 目 进 行 开 发 。 
· Ctrl/Cmd+/: 在 AI 模 型 之 间 切 换 , 例 如 GPT-4 和 Claude 3.5 。 
· Ctrl/Cmd + Alt + L: 打 开 聊 天 历 史 记 录 。 
· Ctrl/Cmd + Shift + P: 打 开 命 令 面 板 , 可 用 于 访 问 VS Code 设 置 或 Cursor 特 定 功 能 。 ...许 多 Al 工 具 支 持 模 板 、 规 则 或 快 捷 命 令 , 例 如 “/generate" 、 "/optimize" 等 。 熟 悉 这 些 命 令 可 以 节 
省 时 间 。 为 常 用 任 务 创 建 自 定 义 模 板 , 以 便 快 速 调 用 。 
除 此 之 外 ,Cursor 的 AI 功 能 是 其 核 心 亮 点 , 以 下 常 用 的 快 捷 键 可 帮 助 您 更 高 效 地 使 用 AI 辅 助 功 能 : 
· Tab: 接 受 Al 提 供 的 代 码 建 议 或 自 动 补 全 。 
· Ctrl/Cmd + B: 显 示 或 隐 藏 资 源 管 理 器 面 板 。 
· Ctrl/Cmd + K: 打 开 AI 提 示 栏 , 用 于 生 成 新 代 码 或 编 辑 现 有 代 码 。 
· Ctrl/Cmd +L: 打 开 AI 聊 天 面 板 , 可 用 于 提 问 、 编 辑 代 码 或 对 整 个 项 目 进 行 操 作 。 
· Ctrl/Cmd + 1: 打 开 AI 代 理 , 用 于 跨 文 件 编 辑 代 码 或 对 整 个 项 目 进 行 开 发 。 
· Ctrl/Cmd+/: 在 AI 模 型 之 间 切 换 , 例 如 GPT-4 和 Claude 3.5 。 
· Ctrl/Cmd + Alt + L: 打 开 聊 天 历 史 记 录 。 
· Ctrl/Cmd + Shift + P: 打 开 命 令 面 板 , 可 用 于 访 问 VS Code 设 置 或 Cursor 特 定 功 能 。
readme.md 项目需求
plan.md 开发计划
right click->run python file in Terminal
bubble.py > ... 
Plan, @ for context, / for commands 
1 
def bubble_sort (arr) : 
2 
n = len (arr) 
3 
for i in range(n) : 
4 
# 每 次 遍 历 将 最 大 的 元 素 移 到 最 后 
5 
for j in range(0, n - i - 1): 
6 
if arr[j] > arr[j + 1]: 
Go to Definition 
F12 
00 V 
Auto v 
a 
7 
arr[j], arr[j + 1] = ar 
8 
Go to Declaration 
9 
# 测 试 代 码 
Go to Type Definition 
Local v 
10 
if 
_name 
== " main_": 
Go to References 
4 F12 
11 
test_cases = [ 
[], 
Add Symbol to Current Chat 
12 
13 
[1], 
Add Symbol to New Chat 
14 
[5, 4, 3, 2, 1], 
Create Rule 
15 
[1, 2, 3, 4, 5], 
Peek 
> 
16 
[3, 1, 4, 1, 5, 9, 2, 6], 
17 
[2, 2, 2, 2] 
Find All References 
14 F12 
18 
] 
Show Call Hierarchy 
14H 
19 
for idx, test in enumerate [Any] (tes 
20 
arr = test. copy() 
Rename Symbol 
F2 
21 
bubble_sort (arr) 
Change All Occurrences 
96 F2 
22 
print(f"Test case {idx+1}: 原 始 : 
Refactor ... 
^4R 
23 
24 
Source Action ... 
25 
26 
Cut 
27 
Copy 
28 
Paste 
28 V 
29 
Run in Interactive window 
> 
Organize Imports 
Run Python 
> 
Run Python File in Terminal 
Run Selection/Line in Python Terminal 
Command Palette ... 
Cursor Tab In 14 Col 25 Spaces . 4 UT ...bubble.py > ... 
Plan, @ for context, / for commands 
1 
def bubble_sort (arr) : 
2 
n = len (arr) 
3 
for i in range(n) : 
4 
# 每 次 遍 历 将 最 大 的 元 素 移 到 最 后 
5 
for j in range(0, n - i - 1): 
6 
if arr[j] > arr[j + 1]: 
Go to Definition 
F12 
00 V 
Auto v 
a 
7 
arr[j], arr[j + 1] = ar 
8 
Go to Declaration 
9 
# 测 试 代 码 
Go to Type Definition 
Local v 
10 
if 
_name 
== " main_": 
Go to References 
4 F12 
11 
test_cases = [ 
[], 
Add Symbol to Current Chat 
12 
13 
[1], 
Add Symbol to New Chat 
14 
[5, 4, 3, 2, 1], 
Create Rule 
15 
[1, 2, 3, 4, 5], 
Peek 
> 
16 
[3, 1, 4, 1, 5, 9, 2, 6], 
17 
[2, 2, 2, 2] 
Find All References 
14 F12 
18 
] 
Show Call Hierarchy 
14H 
19 
for idx, test in enumerate [Any] (tes 
20 
arr = test. copy() 
Rename Symbol 
F2 
21 
bubble_sort (arr) 
Change All Occurrences 
96 F2 
22 
print(f"Test case {idx+1}: 原 始 : 
Refactor ... 
^4R 
23 
24 
Source Action ... 
25 
26 
Cut 
27 
Copy 
28 
Paste 
28 V 
29 
Run in Interactive window 
> 
Organize Imports 
Run Python 
> 
Run Python File in Terminal 
Run Selection/Line in Python Terminal 
Command Palette ... 
Cursor Tab In 14 Col 25 Spaces . 4 UT
