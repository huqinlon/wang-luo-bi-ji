# Armbian bullseye 系统OMV 6.x安装分享_jackboy6的博客-CSDN博客_armbian系统能干什么
OMV 5.x网上教程很多, 6.x的官方有方法，但是因为墙的原因，要换源, 对初学者来说并没有一份完全照抄的教程参考, 经过一番摸索, 总结了下OMV 6.x的安装过程如下:

第一步当然是Armbian系统烧录, 这步网上教程很多，基本没啥问题，安装完成后需要替换国内源, 我这边是用的清华源，详细如下:

先su，登录root用户, 这点很重要, 后面的OMV安装一定用root用户，之前我一直用sudo，安装了许多遍都没能成功, 会跳出 php 的依赖关系并无法解决的问题, 用root 就没这个问题.

su

然后输入密码，即可登录root用户

接下来

nano /etc/apt/sources.list

将里面的内容替换为:

deb https://mirrors.tuna.tsinghua.edu.cn/[debian](https://so.csdn.net/so/search?q=debian&spm=1001.2101.3001.7020) bullseye main contrib non-free  
deb https://mirrors.tuna.tsinghua.edu.cn/debian bullseye-updates main contrib non-free  
deb https://mirrors.tuna.tsinghua.edu.cn/debian bullseye-backports main contrib non-free  
deb https://mirrors.tuna.tsinghua.edu.cn/debian-security bullseye-security main contrib non-free  
保存退出,  然后替换Armbian 源

nano /etc/apt/sources.list.d/armbian.list

将内容替换为

deb https://mirrors.tuna.tsinghua.edu.cn/armbian bullseye main bullseye-utils bullseye-desktop

保存退出

最后生效

apt-get update

到此Armbian系统更换源完成, 接下来就是参考OMV官方+清华源的提示安装OMV

首先

apt-get install --yes gnupg  
wget -O "/etc/apt/trusted.gpg.d/openmediavault-archive-keyring.asc" https://packages.openmediavault.org/public/archive.key  
apt-key add "/etc/apt/trusted.gpg.d/openmediavault-archive-keyring.asc"

接下来: 

cat <<EOF > /etc/apt/sources.list.d/openmediavault.list  
deb https://mirrors.tuna.tsinghua.edu.cn/OpenMediaVault/public shaitan main  
deb https://mirrors.tuna.tsinghua.edu.cn/OpenMediaVault/packages shaitan main  
\## Uncomment the following line to add software from the proposed repository.  
\# deb https://mirrors.tuna.tsinghua.edu.cn/OpenMediaVault/public shaitan-proposed main  
\# deb https://mirrors.tuna.tsinghua.edu.cn/OpenMediaVault/packages shaitan-proposed main  
\## This software is not part of OpenMediaVault, but is offered by third-party  
\## developers as a service to OpenMediaVault users.  
\# deb https://mirrors.tuna.tsinghua.edu.cn/OpenMediaVault/public shaitan partner  
\# deb https://mirrors.tuna.tsinghua.edu.cn/OpenMediaVault/packages shaitan partner  
EOF

生效后, 输入如下指令

export LANG=C.UTF-8  
export DEBIAN\_FRONTEND=noninteractive  
export APT\_LISTCHANGES\_FRONTEND=none  
apt-get update  
apt-get --yes --auto-remove --show-upgraded \\  
    --allow-downgrades --allow-change-held-packages \\  
    --no-install-recommends \\  
    --option DPkg::Options::="--force-confdef" \\  
    --option DPkg::Options::="--force-confold" \\  
    install openmediavault-keyring openmediavault

官方文件是还有一句 omv-confdbadm populate，这一句不能直接执行，会报错

需要按如下操作

export PATH=$PATH:/usr/sbin

然后再

omv-confdbadm populate

至此, OMV及安装完成并运行了

在浏览器输入Armbian系统的IP地址即可看到OMV的登陆界面, 后面的OMV设定网上有教程可参考

(IP地址不知道的话，在terminal里输入ifconfig可以查询到)