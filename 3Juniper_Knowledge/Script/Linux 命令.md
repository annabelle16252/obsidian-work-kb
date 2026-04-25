# Linux 命令

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

Linux ...命令
# cat #
zcat:...解压并...show ...（...for .gz...文件）
$ zcat interactive-commands.*.gz | egrep -i "clear dhcp"
cat...：...show ...（...for .tgz ...文件）
$ cat messages | tail -n 10 :  ...从倒数第...10...行读取
# grep #
grep -i : ...不分大小写...grep
grep -v : 排除内容显示
grep -w: ...精确匹配...字符串
grep -C 5 foo file 显示file文件里匹配foo字串那行以及上下5行
grep -B 5 foo file 显示foo及前5行
grep -A 5 foo file 显示foo及后5行
grep xxx  > temp.txt : grep 结果输入到文件里
egrep "event|error" ...：并列，含有任何一个字符串就符合
egrep "show chassis" * : 显示内容所在的文件目录
grep -r -e "systest.conf" 递归当前目录下的所有文件夹里的文件包含指定内容的都给列出来...。...-rw是精确匹配
------
# ...d...iff #
diff -y:
------
annaw@svl-jtac-lnx05:~$ cat temp.txt
111
annaw@svl-jtac-lnx05:~$ cat compare1.txt
111
222
333
444
annaw@svl-jtac-lnx05:~$ diff -y compare1.txt temp.txt
111                                                             111
222                                                           <
333                                                           <
444                                                           <
-----
# awk #
1. awk '!/xxxx/'  <<< 输出排除xxxx的其他所有内容
$ cat compare1.txt | awk '!/222/'
------
annaw@svl-jtac-lnx05:~$ cat compare1.txt | awk '!/33/'
111
222
444
555
666
-----
# find #
find  ~~ -exec ~~ : 查找目录下文件并执行动作
$ find /Users/annabelle/test -size +100k -a -size -400k -exec mv {} /Users/annabelle \;
-a : and
-size : 文件大小， +100k：大于100k，-500k：小于500k
-exec ~~ \; ： 执行后边的动作    \; 终止符
{}：占位符，代表前面找到的所有匹配内容
# ...文件行数... #
$ cat vbf-flow-m0fpc0-jun26.log | wc -l
52461
# ps -aux #
·USERPID%CPU%MEMVSZRSSTTYSTATSTARTTIMECOMMAND
·vsz:显示讲程使用的虚拟内存量(KB)
·RSS：表示该进程占用的固定内存量(KB)
·驻留中页的数量
# dd #
dd 是 Linux 系统中一个强大的命令行工具，用于复制和转换文件数据。它的名字来源于“Data Duplicator”，但通常被称为“磁盘复制器”或“数据销毁器”，因为它可以以低级别的方式操作数据，功能非常强大且灵活。
$ dd if=11.txt of=22.txt...   <<< 用if的内容覆盖of的
if: 输出文件
of: ...输入文件
设置每次读写的块大小(BlockSize），例如bs=l表示每次读写1MBO
bs
指定复制的块数。
count
跳过输入文亻牛开头的指定块数。
skip
跳过输出文亻牛开头的指定块数。
seek
指定数据转换方式，例如conv=notrunc表示不截断输出文件。
conv
控制进度显示，例如status-progress显示实时进度。
status
# VI/VIM ...文本编辑器... #
[root@linux]# vi users <<< ...进入文本
：尾行模式，光标移到文本最后
...:wq!... <<<...保存退出...,
:q!... <<<...不保存退出
:/ <<<...查找内容...  , press "n" ...可以到下一个位置
插入模式...：...i ...在光标处插入，...a-...在光标后插入，...o-...在下一行插入
^...行首... $...行...尾
esc ...退出
# free #
是 Linux 系统中用于显示内存使用情况的命令行工具。它可以显示系统的物理内存、交换空间（Swap）以及缓冲区和缓存的使用情况
root
物理内存
十
交内存Swap“
ers
OSt～#
total
Cace：
524280
ree
used
388
160
free
1156
524120
shared
buffers
cached
u
# uname #
是 Linux 和 Unix 系统中用于显示系统信息的命令。它可以输出当前操作系统的内核名称、版本、主机名、硬件架构等信息。
