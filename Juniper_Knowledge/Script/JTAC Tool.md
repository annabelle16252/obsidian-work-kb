# JTAC Tool

> [!note] Recovered from OneNote
> This note was recovered from OneNote backup (2026-03-25).
> Content from Obsidian modifications after OneNote import may be missing.

JTAC Tool
/volume/case_2023
/volume/CSdata/annaw/case/
# ...解压文件... #
1. ...把...tgz...文件拷到目录下，...mkdir...一个同名...folder
2....把...tgz...文件...mv...到这个新...folder...下
3....进入这个...foler
> tar -zxvf    + xxx.tgz...文件
> unrar + xxx.rar / unrar e xxx.rar
> ...unzip KHILS-ER2 log.zip
> ...7za x KHILS-ER2 log.7z
# ...压缩成...tgz...文件... #
$ tar -czvf test.tgz /homes/annaw/test/test-1
<<<...把...test/test-1...这个目录压缩成名为...test.tgz...的压缩文件
# ...给文件读写权限permission... #
chmod -R a+rx * ... 给所有文件加权限
annaw@svl-jtac-lnx05:/volume/CSdata/annaw/case$ chmod 777 2020-0927-0317
$ chmod -R 777 /volume/CSdata/annaw/case/2021-1018-343196/
# 给文件建立md5 #
md5sum /volume/build/junos/15.1/release/15.1F6-S4.2/ship/junos-vmhost-install-mx-x86-64-15.1F6-S4.2.tgz
sha1sum /volume/build/junos/15.1/release/15.1F6-S4.2/ship/junos-vmhost-install-mx-x86-64-15.1F6-S4.2.tgz
