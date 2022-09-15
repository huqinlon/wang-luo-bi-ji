# 刷armbian后必要的安装和配置_地豆1213131的博客-CSDN博客_armbian
1.固定eth0 mac地址
--------------

每次重启armbian后会自动产生新的随机mac地址 对于局域网主机来说确实麻烦,配合 isc-[dhcp](https://so.csdn.net/so/search?q=dhcp&spm=1001.2101.3001.7020) ddns 根据固定的mac地址 可以找到主机名  
另外配置开机就激活eth0网卡 --虽然这样会在无网络的情况下陷入等待  
修改  
/etc/network/interfaces  
auto eth0  
hwaddress b6:fe:cc:2f:c5:f3  
注 b6:fe:cc:2f:c5:f3 是mac地址 随便设置一个  
重启网络  
systemctl restart NetworkManager

2.修改[主机名](https://so.csdn.net/so/search?q=%E4%B8%BB%E6%9C%BA%E5%90%8D&spm=1001.2101.3001.7020)和机器id
---------------------------------------------------------------------------------------------------

由于使用img包输入,这样会保留原始安装环境的一些配置,修改主机名和机器id可以区别开来

2.1 修改主机名  
查看主机名  
hostnamectl  
修改主机名  
如我的盒子设置为 s905  
sudo hostnamectl set-hostname s905  
替换为你想要为armbian盒子起的名字

sudo vim /etc/hosts  
将 127.0.0.1 旧主机名修改为新主机名

验证  
hostnamectl

2.2 修改机器id  
查看当前的id  
cat /etc/machine-id  
cat /var/lib/dbus/machine-id  
清空  
sudo cp /dev/null /etc/machine-id  
sudo cp /dev/null /var/lib/dbus/machine-id  
产生新的随机id  
sudo systemd-machine-id-setup  
重启设备

3.将普通账户加入sudoers , 移除ssh允许root登录 , 禁止非wheel组用户su root
-----------------------------------------------------

以root登录到系统

3.1 创建普通账户  
首先查看是否存在普通账户  
/etc/passwd|grep bash  
通常是 uid 1000开始账户  
如果没有就创建  
如创建用户 testuser  
useradd --shell /bin/bash --create-home testuser

3.2 将普通账户加入sudoers  
这样 该账户就使用sudo 而避免通过 su root 来完成一些特权操作  
编辑 /etc/sudoers 或 visudo  
在这一行下面 追加  
root ALL=(ALL:ALL) ALL  
testuser ALL=(ALL:ALL) ALL

当然如果你想要更加细致权限配置 可参阅  
man sudoers

3.3 移除ssh允许root登录  
修改 /etc/ssh/sshd\_config  
将 PermitRootLogin yes 修改为  
PermitRootLogin no

3.4 禁止非wheel组用户su root

文件 /etc/pam.d/su  
存在这2行  
auth sufficient pam\_rootok.so  
auth required pam\_wheel.so use\_uid

文件 /etc/login.defs  
SU\_WHEEL\_ONLY yes

创建 wheel组 debian通常没有默认创建该组  
sudo groupadd wheel

将用户 testuser 加入 wheel组 如果你向其他账户也能su root 也需要将那些账户 加入该组  
sudo usermod -G wheel testuser

4.修改软件源
-------

首先使 apt 支持 https 传输 ,如果只用http 可忽略

sudo apt install apt-transport-https ca-certificates

修改源  
sudo vim /etc/apt/sources.list  
#例如 清华源  
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ buster main contrib non-free  
#deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ buster main contrib non-free  
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ buster-updates main contrib non-free  
#deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ buster-updates main contrib non-free  
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ buster-backports main contrib non-free  
#deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ buster-backports main contrib non-free  
deb https://mirrors.tuna.tsinghua.edu.cn/debian-security/ buster/updates main contrib non-free  
#deb-src https://mirrors.tuna.tsinghua.edu.cn/debian-security/ buster/updates main contrib non-free

另外  
sudo vim /etc/apt/sources.list.d/armbian.list  
替换为  
deb https://mirrors.tuna.tsinghua.edu.cn/armbian buster main buster-utils buster-desktop

然后更新  
sudo apt update  
sudo apt upgrade -y

5.设置时区 时间
---------

创建当前时间和时区  
timedatectl 或 date -R  
修改时区  
tzselect 选择 asia china beijing yes  
sudo cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime  
再次查看  
timedatectl 或 date -R

armbian 自带 chronyd 客户端 用于同步ntp时间

测试时间源  
chronyc sources -v

如果想要换为国内的ntp服务器 修改 /etc/chrony/chrony.conf  
对于时间池 使用 pool 单个服务器的 使用 server  
如 使用aliyun的ntp  
server ntp1.aliyun.com prefer iburst  
然后重启 chrony  
sudo systemctl restart chronyd

6.ipv6配置
--------

启动配置ipv6 (其实默认也是启动的)

sudo vim /etc/sysctl.conf  
#开启  
net.ipv6.conf.all.disable\_ipv6 = 0  
net.ipv6.conf.default.disable\_ipv6 = 0

#通常无需转发 但如果使用 docker wireguard之类的或是路由器 需要开启  
net.ipv6.conf.default.forwarding = 0  
net.ipv6.conf.all.forwarding = 0

#ipv6自动配置配置 而不是通过dhcp分配  
net.ipv6.conf.default.autoconf = 1  
#0 使用EUI64  
#1 不产生链路本地地址 autoconf地址使用EUI64产生  
#为2 产生 stable privacy地址 指定stable\_secret作为密钥  
#为3 产生stable privacy 地址 使用随机密钥产生  
net.ipv6.conf.default.addr\_gen\_mode = 0

#是否使用临时地址  
#0不使用  
#为1 启动 路由表优先级 公网地址优先于临时地址  
#大于1 启动 路由表优先级 临时地址优先于公网地址  
#-1 用于回环地址和p2p网卡  
net.ipv6.conf.default.use\_tempaddr = 0

生效  
sudo sysctl -p

ipv6 从路由通告获取 dns地址  
sudo apt install rdnssd  
对应的服务  
sudo systemctl status rdnssd.service

7.dash 改为 bash
--------------

sudo dpkg-reconfigure bash

8.配置防火墙
-------

安装 防火墙保存工具  
sudo apt install iptables-persistent

配置防火墙

#ipv4的  
iptables -P INPUT DROP  
iptables -P FORWARD ACCEPT  
iptables -P OUTPUT ACCEPT

iptables -t filter -A INPUT -i lo -j ACCEPT  
iptables -t filter -A INPUT -m conntrack --ctstate ESTABLISH,RELATED -j ACCEPT  
iptables -A INPUT -t filter -m conntrack --ctstate INVALID -j DROP

#icmp  
iptables -t filter -N IN\_PKT\_ICMP  
iptables -t filter -A INPUT -p icmp -j IN\_PKT\_ICMP  
iptables -t filter -A IN\_PKT\_ICMP -p icmp --icmp-type 8/0 -j ACCEPT  
iptables -t filter -A IN\_PKT\_ICMP -j DROP

#SSH  
iptables -t filter -A INPUT -m conntrack --ctstate NEW -p tcp --dport 22 -j ACCEPT

#ipv6的  
ip6tables -P INPUT DROP  
ip6tables -P FORWARD ACCEPT  
ip6tables -P OUTPUT ACCEPT

ip6tables -A INPUT -t filter -i lo -j ACCEPT  
ip6tables -A INPUT -t filter -m conntrack --ctstate ESTABLISHED,RELATED -j ACCEPT  
ip6tables -A INPUT -t filter -m conntrack --ctstate INVALID -j DROP

ip6tables -t filter -A INPUT -p ipv6-icmp -j ACCEPT

#dhcpv6  
ip6tables -t filter -A INPUT -p udp --src fc00::/6 --dport 546 --sport 547 -j ACCEPT

#sshd  
ip6tables -t filter -A INPUT -p tcp --src fc00::/6 --dest fc00::/6 --dport 22 -m conntrack --ctstate NEW -j ACCEPT

保存防火墙规则  
sudo netfilter-persistent save