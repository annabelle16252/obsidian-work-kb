# SHELL Script

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

SHELL Script
是一个命令行解释器，负责接收用户输入的命令，然后调用...kernel...去执行这些命令，再把执行的结果返回给客户。在...Linux...系统中默认安装的...shell...都是...bash...。...MAC电脑的Terminal程序就是一个linux环境...。
脚本编辑...app：vscode
# Script #
% vi hello.sh
#!/bin/bash  <<< 第一行这个格式，可以把bash换成其他解释器
echo "hello shell"
date...
whoami
esc :wq! / :q!  ...<<<保存退出
% chmod a+x hello.sh
% ./hello.sh   ...<<<...执行脚本 ./ or bash or sh
% chmod +x test.sh... ...<<<附加权限
if [ $# -ne 1 ]; then...
$#：变量个数
-eq（等于）
-lt（小于）
-gt（大于）
-ne ...（不等于）
if ! [[ "$num" =~ ^[0-9]+$ ]]; then
=~... ...：是
^：匹配字符串的开头
if [ $((num % 2)) -eq 0 ]; then
% ： 取模运算符（求余数），计算 $num 除以 2 的余数。
||...:...或者
if [[ $choice = "y" ]] || [[ $choice = "Y" ]];then
continue
echo "Hello, World!"           # 输出到终端echo "Hello" > file.txt        # 覆盖写入文件echo "World" >> file.txt       # 追加到文件echo $PATH                     # 打印环境变量
# 变量 #
#!/bin/bash
#read name
name=$1
channel=$2
echo "Hello, $name, Welcome to $channel"
% sh test.sh Annabelle Wonderful-World
Hello, Annabelle, Welcome to Wonderful-World
number=$(shuf -i 1-10 -n 1)
echo $number
全局变量(默认)
局部变量
is_prime() {...     <<<<函数
local num=$1...        <<<<
if [ $num -lt 2 ]; then
return 1
fi
# if 语句 #
if [[ $guess -eq $number ]]; then
echo "Bingo"
elif [[ $guess -lt $number ]]; then
echo "bigger"
else
echo "less"
fi
# ...循环... #
for...
遍历文件列表
for file in *.txt; do    echo "处理文件: $file"done
固定次数的循环
for ((i=1; i<=10; i++)); do    echo "计数: $i"done
遍历数组
colors=("红" "绿" "蓝")for color in "${colors[@]}"; do    echo "颜色: $color"done
for ((i=2; i*i<=num; i++));do
if [ $((num % i)) -eq 0 ]; then
return 1
fi
done
while
条件满足时持续执行
count=0while [ $count -lt 5 ]; do    echo "计数: $count"    ((count++))done
读取文件内容
while read line; do    echo "行内容: $line"done < file.txt
创建无限循环
while true; do    echo "持续运行..."    sleep 1done
等待某个条件满足
while [ ! -f /tmp/ready.flag ]; do    echo "等待文件出现..."    sleep 1done
在条件成立下一直执行：
number=$(shuf -i 1-10 -n 1)
while [[ $guess -ne $number ]]
do
echo "Please enter a random unmber between 1-10"
read guess
if [[ $guess -eq $number ]]; then
echo "Bingo"
elif [[ $guess -lt $number ]]; then
echo "bigger"
else
echo "less"
fi
done
无限循环直到break或手动终止：
number=$(shuf -i 1-10 -n 1)
while true
do
echo "Please enter a random unmber between 1-10"
read guess
if [[ $guess -eq $number ]]; then
echo "Bingo! Would you want to continue?(y/n)"
read choice
if [[ $choice = "y" ]];then
continue
else
break
fi
elif [[ $guess -lt $number ]]; then
echo "bigger"
else
echo "less"
fi
done
