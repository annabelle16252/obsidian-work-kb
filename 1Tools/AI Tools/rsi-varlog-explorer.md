https://github.hpe.com/michael-tucker1/rsi-explorer
https://github.hpe.com/ying-wang/rsi-varlog-explorer
https://pypi.org/project/rsi-varlog-explorer/1.5.2/

```
% ssh annaw@p-jtac-lnx02.juniper.net
Ctrl+L 快捷键清屏。

chmod -R u+w /volume/CSdata/<your_username>/case/

# 全部自动发现并解压到当前目录（目录需要读写权限且只包含一台设备的rsi&varlog）
rsi-varlog /volume/CSdata/annaw/case/2026-0323-648596

# 指定读取和解压的文件
--rsi-file 指定rsi file
--var-log-dir 指定解压文件

# 指定解压路径 （目录需要读写权限）
--extract-dir

# install new version
ssh annaw@p-jtac-lnx02.juniper.net
python3 -m pip install --user --upgrade --no-cache-dir rsi-varlog-explorer==1.6.3
hash -r
python -m pip show rsi-varlog-explorer | grep -i Version
rsi-varlog 1.6.3


rsi-varlog /volume/case_2026/2026-0413-672364 --rsi-file 20260413-FFTEQ-BB1-RSI.log --var-log-dir 2026-04-13-FFTEQ-BB1-re0.tgz 2026-04-13-FFTEQ-BB1-re1.tgz --extract-dir /volume/CSdata/annaw/case/2026-0323-648596_test
```

```

python3 -m pip install --user --upgrade --no-cache-dir rsi-varlog-explorer==1.5.10
hash -r
```

![[Pasted image 20260414140411.png]]

# auto-more
```
按键	功能
Space / f / ^F / Right-Arrow	下一页
Enter / j / ^N / ^X / ^Z / Down-Arrow	下一行
TAB / d / ^D	下半页
b / ^B / Left-Arrow	上一页
u / ^U	上半页
k / Delete / Backspace / ^P / Up-Arrow	上一行
g / ^A	跳到顶部
G / ^E	跳到底部（不退出，显示 100%）
搜索

按键	功能
/	正向搜索（提示 Search for:）
?	反向搜索（提示 Reverse search for:）
n	重复上次搜索，跳到下一条匹配
过滤

按键	功能
m / M	只显示匹配行（提示 Match for:）
e / E	排除匹配行（提示 Match except:）
c / C	清除所有过滤
其他

按键	功能
s / S	保存输出到文件（提示 Filename:）
h	显示完整 help（和真机一致的格式）
H	到底部后保持停留（不自动退出）
N	到底部后不保持停留
^L / ^R	重绘屏幕
q / Q / ^K / ^C	退出 automore
提示行格式（和真机一致）

滚动中：---(more 40%)---
到底部：---(more 100%)---
过滤激活时：---(more[filtered] 60%)---
触发时机

show log <filename> → 默认强制进入 automore
任意命令 | more → 强制分页
任意命令 | no-more → 禁用分页
输出超过终端高度 80% → 自动分页
```



In Figure 1, when using the / search function in the tool’s show log messages view, pressing / a second time shows Search for: but clears the previous search string.
In Figure 2, on the lab MX960 device, pressing / a second time shows Search for (AMD): and retains the previous search string.
Please change the tool’s behavior to match the MX960 behavior.


![[Pasted image 20260416165131.png]]

![[Pasted image 20260416165615.png]]
![[Pasted image 20260416165647.png]]
![[Pasted image 20260416165828.png]]![[Pasted image 20260416182847.png]]
![[Pasted image 20260416182954.png]]