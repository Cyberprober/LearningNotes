# Linux 磁盘管理

## df
df命令参数功能：检查文件系统的磁盘空间占用情况
```shell
-h 以人们较易阅读的 GBytes, MBytes, KBytes 等格式自行显示；
-T ：显示文件系统类型, 连同该 partition 的 filesystem 名称 (例如 ext3) 也列出；

# example
df -h /etc
```

## du
Linux du 命令也是查看使用空间的，但是与 df 命令不同的是 Linux du 命令是对文件和目录磁盘使用的空间的查看
```shell
-h ：以人们较易读的容量格式 (G/M) 显示；
-s ：列出总量而已，而不列出每个各别的目录占用容量；
```

## fdisk
fdisk 是 Linux 的磁盘分区表操作工具。
```shell
fdisk -l
df -h /
fdisk /dev/hdc
```


## 磁盘格式化
磁盘分割完毕后自然就是要进行文件系统的格式化，格式化的命令非常的简单，使用 mkfs（make filesystem） 命令。

mkfs -t ext3 /dev/hdc6

## 磁盘检验
sck（file system check）用来检查和维护不一致的文件系统。
若系统掉电或磁盘发生问题，可利用fsck命令对文件系统进行检查。

fsck [-t 文件系统] [-ACay] 装置名称

## 磁盘挂载与卸除
mount [-t 文件系统] [-L Label名] [-o 额外选项] [-n]  装置文件名  挂载点
mount /dev/hdc6 /mnt/hdc6

## 参考资料
[Linux 磁盘管理](https://www.runoob.com/linux/linux-filesystem.html)


