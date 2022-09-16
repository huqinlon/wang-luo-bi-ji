# 备份armbian/linux/ubuntu系统到img镜像 - 简书
[![](https://upload.jianshu.io/users/upload_avatars/23859079/f73e792a-ccfd-4e83-9def-2745517eaf82.png?imageMogr2/auto-orient/strip|imageView2/1/w/96/h/96/format/webp)
](https://www.jianshu.com/u/e3dc5c8207e8)

2020.11.28 23:21:34字数 57阅读 3,500

以优盘备份armbian20.10为例

```
mount /dev/sda /mnt #假如优盘被识别为/dev/sda
cd /mnt
nano back_cmd.sh 
```

将以下代码复制进去

```
#!/bin/sh
#backup for Armbian_20.10_buster
echo "create img file..."
fallocate -l $(( 3584 * 1024 *1024 )) Backup_system.img
echo "create fdisk.cmd"
cat > fdisk.cmd <<-EOF
o
n
p
1

+512MB
t
c
n
p
2


w
EOF
echo "fdisk.cmd done!"
echo "disk partition..."
fdisk Backup_system.img < fdisk.cmd
echo "Done!"
echo "losetup loop0..."
#losetup -f -P --show Backup_system.img
losetup -d /dev/loop0
losetup -P loop0 Backup_system.img
echo "mount and format..."
mkfs.vfat -n "BOOT" /dev/loop0p1
mke2fs -F -q -t ext4 -L ROOTFS -m 0 /dev/loop0p2
mkdir /img -p
mount /dev/loop0p2 /img
mkdir /img/boot
mount /dev/loop0p1 /img/boot

echo "Begin backup system..."
cd /
DIR_INSTALL=/img
cp -r /boot/* /img/boot/
mkdir -p $DIR_INSTALL/dev
mkdir -p $DIR_INSTALL/media
mkdir -p $DIR_INSTALL/mnt
mkdir -p $DIR_INSTALL/proc
mkdir -p $DIR_INSTALL/run
mkdir -p $DIR_INSTALL/sys
mkdir -p $DIR_INSTALL/tmp
echo "bin"
tar -cf - bin | (cd $DIR_INSTALL; tar -xpf -)
echo "boot"
tar -cf - boot | (cd $DIR_INSTALL; tar -xpf -)
echo "etc"
tar -cf - etc | (cd $DIR_INSTALL; tar -xpf -)
echo "home"
tar -cf - home | (cd $DIR_INSTALL; tar -xpf -)
echo "lib64 and lib"
tar -cf - lib64 | (cd $DIR_INSTALL; tar -xpf -)
tar -cf - lib | (cd $DIR_INSTALL; tar -xpf -)
echo "opt root sbin selinux"
tar -cf - opt | (cd $DIR_INSTALL; tar -xpf -)
tar -cf - root | (cd $DIR_INSTALL; tar -xpf -)
tar -cf - sbin | (cd $DIR_INSTALL; tar -xpf -)
tar -cf - selinux | (cd $DIR_INSTALL; tar -xpf -)
echo "srv"
tar -cf - srv | (cd $DIR_INSTALL; tar -xpf -)
echo "usr"
tar -cf - usr | (cd $DIR_INSTALL; tar -xpf -)
echo "var"
tar -cf - var | (cd $DIR_INSTALL; tar -xpf -)
echo "Done!"
sync
echo "解除挂载的备份镜像文件"
umount /img/boot
umount /img/
losetup -d /dev/loop0
echo "All done!" 
```

ctrl+s保存，ctrl+x退出

```
chmod a+x back_cmd.sh
./back_cmd.sh 
```

可以对镜像进行压缩

```
xz -z Backup_system.img #无进度，压缩后文件小很多，所以比较慢，压缩后原来的img会被删除。 
```

参看  
[N1备份armbian/linux/ubuntu系统到img镜像](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.haiyun.me%2Fcategory%2Fn1%2F)