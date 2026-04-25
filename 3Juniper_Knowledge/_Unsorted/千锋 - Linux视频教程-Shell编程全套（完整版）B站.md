# 千锋 - Linux视频教程-Shell编程全套（完整版）B站

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

千锋 - Linux视频教程-Shell编程全套（完整版）B...站
https://www.bilibili.com/video/BV1d7411p7xR?from=search&seid=16719883399085667596
========================
# P1 ...变量... #
变量类型：
1....自定义变量：只在当前...shell...里可用，退出来了再进去就没了
d = $1
d:...变量...name  1...：变量值
d = $(date)
代表变量是从否个地方取值的。... ...设定...date=yes ...有什么行为，...date=no...有什么行为
2....环境变量：一直都可用
操作系统自带的变量，可以通过...env...来看当前用户的所有的环境变量：
---------------
[root@CFTS-Server-QNC ~]# ...env
[root@CFTS-Server-QNC ~]# export
---------------
也可以定义环境变量：
---------------
[root@CFTS-Server-QNC ~]# a=1
[root@CFTS-Server-QNC ~]# export a
[root@CFTS-Server-QNC ~]# echo $a
1
---------------
取消环境变量：
---------------
[root@CFTS-Server-QNC ~]# unset a
[root@CFTS-Server-QNC ~]# echo $a
[root@CFTS-Server-QNC ~]#
---------------
========================
# P2 ...变量... #
[root@CFTS-Server-QNC ~]# bash <<< ...打开一个子进程
[root@CFTS-Server-QNC ~]#
[root@CFTS-Server-QNC ~]# vi test
echo $anna1
echo "----->$anna1"
~
~
[root@CFTS-Server-QNC ~]# bash test
1234
----->1234
[root@CFTS-Server-QNC ~]# bash test
----->
[root@CFTS-Server-QNC ~]# anna1=1234
[root@CFTS-Server-QNC ~]# bash test
----->
[root@CFTS-Server-QNC ~]# export  anna1
[root@CFTS-Server-QNC ~]# bash test
1234
----->1234
[root@CFTS-Server-QNC ~]#
========================
# P4 #
[root@CFTS-Server-QNC ~]# vi test
dat=$(date +%F)
echo $dat
echo "----->$anna1"
~
[root@CFTS-Server-QNC ~]# sh test
2020-05-28
----->
内置的位置参数：
[root@CFTS-Server-QNC ~]# cat position.sh
echo "this is script name $0" <<<...运行的脚本名称
echo "this is the first parameter $1" <<< ...跟着的第一个字符
echo "this is the second parameter $2"
[root@CFTS-Server-QNC ~]# sh position.sh hello world
this is script name position.sh
this is the first parameter hello
this is the second parameter world
[root@CFTS-Server-QNC ~]# name=shark
[root@CFTS-Server-QNC ~]# echo ${#name}
5
<<< echo {#...}...统计字符串个数
看进程号：
[root@CFTS-Server-QNC ~]# vi test &
[1] 11679
[root@CFTS-Server-QNC ~]# echo $!
11679
[1]+  Stopped                 vi test
[root@CFTS-Server-QNC ~]#
赋予变量值：
[root@CFTS-Server-QNC ~]# read ip port
1.1.1.1 2.2.2.2
[root@CFTS-Server-QNC ~]# echo ip
ip
[root@CFTS-Server-QNC ~]# echo $ip
1.1.1.1
[root@CFTS-Server-QNC ~]# echo $port
2.2.2.2
[root@CFTS-Server-QNC ~]#
输入显示脚本：
[root@CFTS-Server-QNC ~]# cat pw.sh
read -p "please enter user name & password " username password
echo "your user name is: $username"
echo "your password is: $password"
[root@CFTS-Server-QNC ~]# sh pw.sh
please enter user name & password anna test
your user name is: anna
your password is: test
