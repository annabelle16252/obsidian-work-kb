/volume/case_2026/2026-0323-648596
/volume/CSdata/annaw/case/2026-0323-648596

> tar -zxvf    + xxx.tgz文件
> tar -xvf  xxx.tar文件
> unrar + xxx.rar / unrar e xxx.rar
> unzip KHILS-ER2 log.zip
> 7za x KHILS-ER2 log.7z
```


# 压缩成tgz文件 #
$ tar -czvf test.tgz /homes/annaw/test/test-1
<<<把test/test-1这个目录压缩成名为test.tgz的压缩文件

# SCP #
scp user@remote_host:/path/to/file.txt /local/path/
把远程主机上的 file.txt 拷到本地目录 /local/path/
复制整个目录（加 -r）
$ scp PTX10004.log annaw@10.104.8.127:/volume/CSdata/annaw/case/

# PR
Search Methods:
1.Jtac tool
```
p-jtac-lnx02:/$ query-pr -q -e 'product~"acx"&description~"PermissionError: \[Errno 13\] Permission Denied"' |grep '\-1 '
*** \ 是转义符，[ ] -等特殊符号前需要加上\ ，才能表示这个符号

query-pr -q -e 'synopsis~"cisco"'|grep '\-1 '

query-pr -q -e 'product~"mx304"&description~"PICD_REMOTE_FAULT"& description~"down"'|grep '\-1 '

query-pr -q -e 'description~"MIC-MACSEC"& description~"ARP"'|grep '\-1 '
query-pr -q -e 'synopsis~"cisco"&synopsis~"sfp"'|grep '\-1 '

```

2.Gnats 
Advance->Using Gnats query-pr expression
![[Pasted image 20260419153736.png]]
product~"acx"&description~"PermissionError: \[Errno 13\] Permission Denied"
product~"acx" 
& description~"PermissionError: \[Errno 13\] Permission Denied" 
& description~"error:32"
![[Pasted image 20260419153820.png]]

![[Pasted image 20260419153827.png]]
product~"mx"&description~"QSFP56-DD-400GBASE-LR4-10"

3.nexlify
https://nexlify.juniper.net/
可以直接search error：PermissionError: [Errno 13] Permission denied

# Open service release 
Singtel的话，会先开一个case，cfts在pr里开scope，标上SR blocker
Allan会在pathfinder上开这个release （有的客户是需要cfts操作这一步）
https://apps-int.juniper.net/jdm/db/prMonitorForm.html?relVersion=24.2R2-S4
# 给文件建立md5 #
md5sum /volume/build/junos/15.1/release/15.1F6-S4.2/ship/junos-vmhost-install-mx-x86-64-15.1F6-S4.2.tgz
sha1sum /volume/build/junos/15.1/release/15.1F6-S4.2/ship/junos-vmhost-install-mx-x86-64-15.1F6-S4.2.tgz

# bass
qnc-baas-shell.juniper.net
https://netseg.juniper.net/
baas, junos/evo repository, code access, build commands 《〈《 alex session
