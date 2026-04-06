/volume/case_2026/
/volume/CSdata/annaw/case/
/volume/ftp/case/

# Sync case log to local PC#
MAC terminal run:
./sync_case.sh 2026-0127-072424 <<< new case SR

# 给文件读写权限permission
chmod -R a+rx *  给所有文件加权限
annaw@svl-jtac-lnx05:/volume/CSdata/annaw/case$  chmod -R a+rx 2020-0927-0317
$ chmod -R 777 /volume/CSdata/annaw/case/2021-1018-343196/

# 解压文件 #
1.把tgz文件拷到目录**下，mkdir一个同名folder
2.把tgz文件mv到这个新folder下
3.进入这个foler
```
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


# 给文件建立md5 #
md5sum /volume/build/junos/15.1/release/15.1F6-S4.2/ship/junos-vmhost-install-mx-x86-64-15.1F6-S4.2.tgz
sha1sum /volume/build/junos/15.1/release/15.1F6-S4.2/ship/junos-vmhost-install-mx-x86-64-15.1F6-S4.2.tgz
