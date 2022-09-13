# Rclone 使用教程 - 挂载 OneDrive、Google Drive 等网盘(Linux) - P3TERX ZONE
前言
--

Rclone 是一个的命令行工具，支持在不同对象存储、网盘间同步、上传、下载数据。并且通过一些设置可以实现离线下载、服务器备份等非常实用的功能。Rclone 有很多种使用方式，挂载是其中的一种。

> **友情提示：**  挂载这个操作并不是必须的，作为一个实验性功能它有很多局限性和问题。挂载后并不能当做一个真正的磁盘来使用，在进行文件操作时会使用本地磁盘进行缓存，即占用本地磁盘空间。使用不当还可能造成磁盘写满、VPS卡死等问题。在 Google 上搜索“Rclone”，与之相关的最多的关键词就是“挂载”，这在一定程度上对很多刚接触的小伙伴造成了误导。要稳定的进行上传、下载、同步等操作建议使用 Rclone 的原生命令功能，使用方法参见《[Rclone 进阶使用教程 - 常用命令参数](https://p3terx.com/archives/rclone-advanced-user-manual-common-command-parameters.html)》。

安装和配置 Rclone
------------

官方提供了[一键安装脚本](https://p3terx.com/go/aHR0cHM6Ly9yY2xvbmUub3JnL2luc3RhbGwvI3NjcmlwdC1pbnN0YWxsYXRpb24)：

```
curl https://rclone.org/install.sh | sudo bash
```

安装完后，输入 `rclone config` 命令进入交互式配置选项，按照提示一步一步来进行操作即可。如果你一脸懵逼，可以去看《[Rclone 安装配置教程](https://p3terx.com/archives/rclone-installation-and-configuration-tutorial.html)》来了解配置的详细过程。

安装 fuse
-------

挂载需要安装 fuse，根据自己的系统来选择安装命令：

```
 apt-get update && apt-get install -y fuse

yum install -y fuse
```

挂载网盘
----

挂载网盘分为手动挂载和开机自动挂载，根据自己的需求来选择。

### 手动挂载

```
 rclone mount <网盘名称:网盘路径> <本地路径> [参数] --daemon


fusermount -qzu <本地路径>
```

`网盘名称`为配置时填的 `name`，`网盘路径`为网盘里的文件夹，留空为整个网盘，`本地路径`为 VPS 上的本地文件夹。`参数`可以查看[官方文档](https://p3terx.com/go/aHR0cHM6Ly9yY2xvbmUub3JnL2NvbW1hbmRzL3JjbG9uZV9tb3VudC8)根据需求进行选择。实际输入时不要有括号，这里只是为了更清楚的区分。`--daemon` 为进程守护参数，可后台运行。

#### 使用示例

输入命令进行挂载操作：

```
rclone mount Onedrive:/ /Onedrive --copy-links --allow-other --allow-non-empty --umask 000 --daemon
```

然后输入 `df -h` 命令查看挂载情况。

```
root@P3TERX:~
Filesystem      Size  Used Avail Use% Mounted on
udev            286M     0  286M   0% /dev
tmpfs            60M  7.8M   52M  14% /run
/dev/sda1        99G   25G   71G  26% /
tmpfs           297M   24K  297M   1% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs           297M     0  297M   0% /sys/fs/cgroup
Onedrive:       5.0T  216G  4.8T   5% /Onedrive 
```

取消挂载：

```
fusermount -qzu /Onedrive
```

### 开机自动挂载

*   下载并编辑自启脚本

```
wget -N git.io/rcloned && nano rcloned
```

*   修改内容：

```
NAME="Onedrive" #Rclone配置时填写的name
REMOTE=''  #远程文件夹，网盘里的挂载的一个文件夹，留空为整个网盘
LOCAL='/Onedrive'  #挂载地址，VPS本地挂载目录
```

*   设置开机自启

```
mv rcloned /etc/init.d/rcloned
chmod +x /etc/init.d/rcloned
update-rc.d -f rcloned defaults 
chkconfig rcloned on 
bash /etc/init.d/rcloned start
```

看到 `[信息] rclone 启动成功 !` 即可。

#### 管理

开始挂载 `bash /etc/init.d/rcloned start`

停止挂载 `bash /etc/init.d/rcloned stop`

重新挂载 `bash /etc/init.d/rcloned restart`

查看日志 `tail -f /$HOME/.rclone/rcloned.log`

#### 卸载自启挂载

```
bash /etc/init.d/rcloned stop
update-rc.d -f rcloned remove # Debian/Ubuntu
chkconfig rcloned off # CentOS
rm -f /etc/init.d/rcloned
```

参考资料
----

[rclone 官方文档](https://p3terx.com/go/aHR0cHM6Ly9yY2xvbmUub3JnL2NvbW1hbmRzL3JjbG9uZV9tb3VudC8)

[在 Debian/Ubuntu 上使用 rclone 挂载 OneDrive 网盘](https://p3terx.com/go/aHR0cHM6Ly93d3cubW9lcmF0cy5jb20vYXJjaGl2ZXMvNDkxLw)

> **本文作者：** [P3TERX](https://p3terx.com/)
> 
> **本文链接：** [https://p3terx.com/archives/linux-vps-uses-rclone-to-mount-network-drives-such-as-onedrive-and-google-drive.html](https://p3terx.com/archives/linux-vps-uses-rclone-to-mount-network-drives-such-as-onedrive-and-google-drive.html)
> 
> **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0](https://p3terx.com/go/aHR0cHM6Ly9jcmVhdGl2ZWNvbW1vbnMub3JnL2xpY2Vuc2VzL2J5LW5jLXNhLzQuMC9kZWVkLnpo) 许可协议。非商业转载及引用请注明出处（作者、原文链接），商业转载请联系作者获得授权。

[Linux](https://p3terx.com/tag/Linux/)[VPS](https://p3terx.com/tag/VPS/)[Rclone](https://p3terx.com/tag/rclone/)[OneDrive](https://p3terx.com/tag/OneDrive/)[Google Drive](https://p3terx.com/tag/Google-Drive/)