# Script

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

Script
# ...后台跑脚本... #
#... s...h xxx.sh &
% ps -aux | grep anna
labroot 48252   0.0  0.0   11168   2548  0  S+   02:22      0:00.00 grep anna
labroot 47875   0.0  0.0   11356   2780  1- S    02:19      0:00.01 sh anna.sh
# kill -9 66423
# shell ...脚本运行命令... #
执行命令...：
cli -c "config;set apply-groups SQT-POLICER-AE;set apply-groups SQT-MPC10E-COS-CBF; commit and-quit"
cli -c 'show log messages | match "CosSchedNode::create failed ifl:" | last 10'
命令输出到文本...：
cli -c "set cli timestamp" >> cos.txt
执行pfe level命令...：
cprod -A fpc3 -c "show class-of-service interface scheduler hierarchy ifl 371 index 162"
显示文本：
echo "this is the round "$i". >> cos.txt
echo "this is the first parameter $1" ...>> cos.txt
循环执行...：
------
#!/bin/sh
i=1
while [ "$i" -le 10 ]
do
<正文>
i=`expr $i + 1`
done
------
# Shell Script Recording #
在...crt...里面...start recording...，然后输入你要的命令注意不要回车，... ...然后...stop recording...，就出来下面的文件：
#$language = "VBScript"
#$interface = "1.0"
crt.Screen.Synchronous = True
' This automatically generated script may need to be
' edited in order to work correctly.
crt.Screen.Send "show system processes extensive " & chr(124) & " match " & chr(34) & "smihelperd" & chr(124) & "idle" & chr(34) & " "
Sub Main
End Sub
绿色部分就是这个命令，到...shell script...里面怎么写
------------------------------
# Python #
Python 函数调用
https://www.jianshu.com/p/51c9990058c1
Python
https://www.jianshu.com/p/4711af1ca12a
https://www.vandyke.com/support/securecrt/scripts/getdata.py.txt
