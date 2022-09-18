# linux同步时间命令有哪些-linux运维-PHP中文网
> linux同步时间命令：1、hwclock命令，可以让系统时间和硬件时间的同步，例“hwclock -w”或“hwclock -s”；2、ntpdate命令，可以让不同机器间同步时间。

![](https://img.php.cn/upload/article/202107/14/2021071411083445183.jpg)

本教程操作环境：Ubuntu 16.04系统、Dell G3电脑。

在Windwos中，系统时间的设置很简单，界面操作，通俗易懂，而且设置后，重启，关机都没关系。系统时间会自动保存在BIOS时钟里面，启动计算机的时候，系统会自动在BIOS里面取硬件时间，以保证时间的不间断。

但在Linux下，默认情况下，系统时间和硬件时间并不会自动同步。在Linux运行过程中，系统时间和硬件时间以异步的方式运行，互不干扰。硬件时间的运行，是靠BIOS电池来维持，而系统时间，是用CPU Tick来维持的。在系统开机的时候，会自动从BIOS中取得硬件时间，设置为系统时间。

**1\. Linux系统时间的设置**
--------------------

在Linux中设置系统时间，可以用date命令：

```
[root@node1 ~]# date
Tue Feb 25 20:15:18 CST 2014
[root@node1 ~]# date -s "20140225 20:16:00"  #yyyymmdd hh:mm:ss
Tue Feb 25 20:16:00 CST 2014
```

**2\. Linux硬件时间的设置**
--------------------

硬件时间的设置，可以用hwclock或者clock命令。两者基本相同，只用一个就行，只不过clock命令除了支持x86硬件体系外，还支持Alpha硬件体系。

```
[root@node1 ~]# hwclock --show
Tue 25 Feb 2014 08:21:14 PM CST -0.327068 seconds
[root@node1 ~]# hwclock --set --date "20140225 20:23:00"
[root@node1 ~]# hwclock
Tue 25 Feb 2014 08:23:04 PM CST -0.750440 seconds
```

**3\. 系统时间和硬件时间的同步**
--------------------

> 同步系统时间和硬件时间，可以使用hwclock命令。

```
[root@node1 ~]# hwclock --systohc <== sys（系统时间）to（写到）hc（Hard Clock）
[root@node1 ~]# hwclock -w
[root@node1 ~]# hwclock --hctosys
[root@node1 ~]# hwclock -s
```

**4\. 不同机器之间的时间同步**
-------------------

为了避免主机时间因为长期运行下所导致的时间偏差，进行时间同步（synchronize）的工作是非常必要的。Linux系统下，一般使用ntp服务器来同步不同机器的时间。一台机器，可以同时是ntp服务端和ntp客户端。在生产系统中，推荐使用像DNS服务器一样分层的时间服务器来同步时间。

不同机器间同步时间，可以使用ntpdate命令，也可以使用ntpd服务。

**4.1 ntpdate命令**

使用ntpdate比较简单。格式如下：

```
1 [root@node1 ~]# ntpdate [NTP IP/hostname]
2 [root@node1 ~]# ntpdate 192.168.0.1
3 [root@node1 ~]# ntpdate time.ntp.org
```

但这样的同步，只是强制性的将系统时间设置为ntp服务器时间。如果CPU Tick有问题，只是治标不治本。所以，一般配合cron命令，来进行定期同步设置。比如，在crontab中添加：

```
0 12 * * * /usr/sbin/ntpdate 192.168.0.1
```

这样，会在每天的12点整，同步一次时间。ntp服务器为192.168.0.1。

或者将下列脚本添加到/etc/cron.hourly/，这样就每小时会执行一次同步：

```
#!/bin/bash
#
# $Id: sync-clock,v 1.6 2009/12/23 15:41:29 jmates Exp $
#
# Use ntpdate to get rough clock sync with department of Genome Sciences
# time server.
NTPDATE=/usr/sbin/ntpdate
SERVER="192.168.0.1 "
# if running from cron (no tty available), sleep a bit to space
# out update requests to avoid slamming a server at a particular time
if ! test -t 0; then
  MYRAND=$RANDOM
  MYRAND=${MYRAND:=$$}
  if [ $MYRAND -gt 9 ]; then
    sleep `echo $MYRAND | sed 's/.*\(..\)$/\1/' | sed 's/^0//'`
  fi
fi
$NTPDATE -su $SERVER
# update hardware clock on Linux (RedHat?) systems
if [ -f /sbin/hwclock ]; then
  /sbin/hwclock --systohc
fi
```

**4.2 ntpd服务**

使用ntpd服务，要好于ntpdate加cron的组合。因为，ntpdate同步时间会造成时间的突变和跳跃，对一些依赖时间的程序和服务会造成影响。比如sleep，timer等。而且ntpd服务可以在修正时间的同时，修正CPU Tick。因此理想的做法为，在开机的时候，使用ntpdate强制同步时间，在其他时候使用ntpd服务来同步时间。

要注意的是，ntpd 有一个自我保护的机制：如果本机与上源时间相差太大，ntpd 不会运行时间同步操作，所以新设置的时间服务器一定要先 ntpdate 从上源取得时间初值, 然后启动 ntpd服务。ntpd服务运行后，先是每64秒与上源NTP服务器同步一次，根据每次同步时测得的误差值经复杂计算逐步调整自己的时间，随着误差减小，逐步增加同步的间隔。每次跳动，都会重复这个调整的过程。

**4.3. ntpd服务的设置**

**ntpd服务的相关设置文件如下：** 

（1）/etc/ntp.conf：这个是NTP daemon的主要设文件，也是 NTP 唯一的设定文件。

（2）/usr /share/zoneinfo/：在这个目录下的文件其实是规定了各主要时区的时间设定文件，例如北京地区的时区设定文件在 /usr/share/zoneinfo/Asia/Shanghai 就是了。这个目录里面的文件与底下要谈的两个文件(clock 与localtime)是有关系的。

（3）/etc/sysconfig/clock：这个文件其实也不包含在NTP 的 daemon 当中，因为这个是 Linux 的主要时区设定文件。每次开机后，Linux 会自动的读取这个文件来设定自己系统所默认要显示的时间。

（4）/etc /localtime：这个文件就是"本地端的时间配置文件"。刚刚那个clock 文件里面规定了使用的时间设置文件(ZONE) 为 /usr/share/zoneinfo/Asia/Shanghai ，所以说，这就是本地端的时间了，此时， Linux系统就会将Shanghai那个文件另存为一份 /etc/localtime文件，所以未来我们的时间显示就会以Beijing那个时间设定文件为准。

下面重点介绍 /etc/ntp.conf文件的设置。在 NTP Server 的设定上，建议不要对Internet 无限制的开放，尽量仅提供局域网内部的 Client 端联机进行网络校时。此外，NTP Server 总也是需要网络上面较为准确的主机来自行更新自己的时间啊，所以在我们的 NTP Server 上面也要找一部最靠近自己的 Time Server 来进行自我校正。事实上， NTP 这个服务也是 Server/Client 的一种模式。

```
[root@linux ~]# vi /etc/ntp.conf
# 1. 关于权限设定部分
#　　权限的设定主要以 restrict 这个参数来设定，主要的语法为：
# 　　restrict IP mask netmask_IP parameter
# 　　其中 IP 可以是软件地址，也可以是 default ，default 就类似 0.0.0.0
#　　至于 paramter 则有：
#　　　ignore　：关闭所有的 NTP 联机服务
#　　　nomodify：表示 Client 端不能更改 Server 端的时间参数，不过Client 端仍然可以透过 Server 端来进行网络校时。
#　　　notrust ：该 Client 除非通过认证，否则该 Client 来源将被视为不信任网域
#　　　noquery ：不提供 Client 端的时间查询
#　　　notrap ：不提供trap这个远程事件登入
#　　如果 paramter 完全没有设定，那就表示该 IP (或网域)"没有任何限制"
restrict default nomodify notrap noquery　# 关闭所有的 NTP 要求封包
restrict 127.0.0.1　　　 #这是允许本机查询
restrict 192.168.0.1 mask 255.255.255.0 nomodify
#在192.168.0.1/24网段内的服务器就可以通过这台NTP Server进行时间同步了
# 2. 上层主机的设定
#　　要设定上层主机主要以 server 这个参数来设定，语法为：
#　　server [IP|HOST Name] [prefer]
#　　Server 后面接的就是上层 Time Server，而如果 Server 参数
# 后面加上 perfer 的话，那表示我们的 NTP 主机主要以该部主机来
# 作为时间校正的对应。另外，为了解决更新时间封包的传送延迟动作，
#　　所以可以使用 driftfile 来规定我们的主机
#　　在与 Time Server 沟通时所花费的时间，可以记录在 driftfile 
#　　后面接的文件内，例如下面的范例中，我们的 NTP server 与 
#　　cn.pool.ntp.org联机时所花费的时间会记录在 /etc/ntp/drift文件内
server 0.pool.ntp.org
server 1.pool.ntp.org
server 2.pool.ntp.org
server cn.pool.ntp.org prefer
#其他设置值，以系统默认值即可
server  127.127.1.0     # local clock
fudge   127.127.1.0 stratum 10
driftfile /var/lib/ntp/drift
broadcastdelay  0.008
keys /etc/ntp/keys
```

总结一下，restrict用来设置访问权限，server用来设置上层时间服务器，driftfile用来设置保存漂移时间的文件。

**4.4 ntpd服务的启动与查询**

在启动NTP服务前，先对提供服务的这台主机手动的校正一次时间（因为启动服务器，端口会被服务端占用，就不能手动同步时间了）。

```
[root@node1 ~]# ntpdate cn.pool.ntp.org
25 Feb 21:10:52 ntpdate[9549]: adjust time server 202.112.31.197 offset 0.000101 sec
```

然后，**启动ntpd服务：** 

```
[root@node1 ~]# /etc/init.d/ntpd start
Starting ntpd: [ OK ]
[root@node1 ~]# date
Tue Feb 25 21:11:07 CST 2014
```

**查看端口（ntpd服务使用UDP的123端口）：** 

```
[root@node1 ~]# netstat -ln |grep :123
udp 0 0 12.12.12.100:123 0.0.0.0:*
udp 0 0 192.168.0.100:123 0.0.0.0:*
udp 0 0 172.18.226.174:123 0.0.0.0:*
udp 0 0 10.10.10.100:123 0.0.0.0:*
udp 0 0 127.0.0.1:123 0.0.0.0:*
udp 0 0 0.0.0.0:123 0.0.0.0:*
udp 0 0 fe80::225:90ff:fe98:61ff:123 :::*
udp 0 0 fe80::225:90ff:fe98:61fe:123 :::*
udp 0 0 fe80::202:c903:1b:afa1:123 :::*
udp 0 0 ::1:123 :::*
udp 0 0 :::123 :::*
```

**如何确认我们的NTP服务器已经更新了自己的时间呢？**

```
[root@node1 ~]# ntpstat
synchronised to NTP server (202.120.2.101) at stratum 4
time correct to within 557 ms
polling server every 64 s
# 该指令可列出NTP服务器是否与上层联机。由上述输出结果可知，时间校正约
# 为557*10(-6)秒，且每隔64秒会主动更新时间。
```

**常见的错误：** 

```
unsynchronized time server re-starting polling server every 64 s
25 Apr 15:30:17 ntpdate[11520]: no server suitable for synchronization found
```

其实，这不是一个错误。而是由于每次重启NTP服务器之后大约要3－5分钟客户端才能与server建立正常的通讯连接。当此时用客户端连接服务端就会报这样的信息。一般等待几分钟就可以了。

```
[root@node1 ~] # ntptrace –n
127.0.0.1:stratum 11, offset 0.000000，synch distance 0.950951
222.73.214.125：stratum 2，offset –0.000787，synch distance 0.108575
209.81.9.7:stratum 1，offset 0.000028，synch distance 0.00436，refid 'GPS'
# 这个指令可以列出目前NTP服务器（第一层）与上层NTP服务器（第二层）
# 彼此之间的关系，注意：该命令需要安装ntp-perl包
```

**ntpq命令：** 

![](https://img.php.cn/upload/article/000/000/024/7e891f3a17b2ff8a8f78a2b22e2aa732-0.png)

指令"ntpq -p"可以列出目前我们的NTP与相关的上层NTP的状态，以上的几个字段的意义如下：

remote：即NTP主机的IP或主机名称。注意最左边的符号，如果由"+“则代表目前正在作用钟的上层NTP，如果是”\*"则表示也有连上线，不过是作为次要联机的NTP主机。

```
refid：参考的上一层NTP主机的地址
st：即stratum阶层
when：几秒前曾做过时间同步更新的操作
poll：下次更新在几秒之后
reach：已经向上层NTP服务器要求更新的次数
delay：网络传输过程钟延迟的时间
offset：时间补偿的结果
jitter：Linux系统时间与BIOS硬件时间的差异时间
```

最后提及一点，ntp服务默认只会同步系统时间。如果想要让ntp同时同步硬件时间，可以设置/etc/sysconfig/ntpd 文件。

在/etc/sysconfig/ntpd文件中，添加 SYNC\_HWCLOCK=yes 这样，就可以让硬件时间与系统时间一起同步。

相关推荐：《[Linux视频教程](http://www.php.cn/course/list/33.html)》

> php入门到就业线上直播课：[查看学习](https://www.php.cn/jump/go.php?url=https%3A%2F%2Fwww.php.cn%2Fk.html&location=mob-article-content-php)

以上就是linux同步时间命令有哪些的详细内容，更多请关注php中文网其它相关文章！

声明：本文内容由网友自发贡献，版权归原作者所有，本站不承担相应法律责任。如您发现有涉嫌抄袭侵权的内容，请联系admin@php.cn核实处理。

[![](https://creatives-1301677708.file.myqcloud.com/images/placeholder/wwads-friendly-ads.png)
](https://wwads.cn/page/whitelist-wwads)