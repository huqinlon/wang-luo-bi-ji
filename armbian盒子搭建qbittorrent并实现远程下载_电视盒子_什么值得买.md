# armbian盒子搭建qbittorrent并实现远程下载_电视盒子_什么值得买
2022-05-15 20:17:52 12点赞 87收藏 13评论

缘起
--

前几天入坑了PT，选择了qbittorrent来挂，用了一台老主机挂了大概一周，感觉投入收入比不划算。于是看了旁边下跑docker有点闲的小盒子，决定给它加点负载，毕竟小盒子耗电量不高且能省一台主机的地方，不用再搭一套环境加一堆[网线](https://www.smzdm.com/fenlei/wangxian/)电源线。上一张岁月静好的完成图。

[![](https://qnam.smzdm.com/202205/15/6280e654dc49c3978.jpg_e1080.jpg)
](https://post.smzdm.com/p/a5oe60kl/pic_2/)

折腾
--

### 安装qbittorrent

在armbian环境下安装qbittorrent还是比较简单的，ssh进盒子，在命令行中输入输入apt-get install qbittorrent-nox一键安装即可。安装完成后输入qbittorrent-nox -d命令，qbittorrent就运行起来了，这个时候在浏览器输入局域网内盒子的IP地址加上端口8080就可以进入qbittorrent的后台了，默认的密码用户名是admin，密码是adminadmin，如下图所示。

[![](https://qnam.smzdm.com/202205/15/6280e654df7685494.jpg_e1080.jpg)
](https://post.smzdm.com/p/a5oe60kl/pic_3/)

进入qbittorrent网页管理后需要设置一些基本参数，先点击“设置”按钮，在选中“web ui”选项卡，在这里设置语言，修改用户名和密码，到此步qbittorrent基本上就搭建好了，先将其放到一边。

[![](https://qnam.smzdm.com/202205/15/6280e654de848594.jpg_e1080.jpg)
](https://post.smzdm.com/p/a5oe60kl/pic_4/)

[![](https://qnam.smzdm.com/202205/15/6280e654e15971526.jpg_e1080.jpg)
](https://post.smzdm.com/p/a5oe60kl/pic_5/)

### 利用内网穿透远程管理qbittorrent

内网穿透有很多种方法，对于我们这种无关痛痒的服务，选择白嫖免费的穿透就可以了。在免费穿透的选择上是走了些弯路，大多免费穿透都有些限制，比如Sakurafrp不支持http，sunny-ngrok需要实名认证要认证费，natapp无法让穿透服务在后台运行，钉钉穿透需要很多环境的支持....看我这浏览记录就知道我白嫖得有多努力了。

[![](https://qnam.smzdm.com/202205/15/6280e654e17572179.jpg_e1080.jpg)
](https://post.smzdm.com/p/a5oe60kl/pic_6/)

最终选择了闪库内网穿透，虽然他的免费线路网速低不是太稳定但对于我们这种简单的服务是足够的了。进入闪库内网穿透的官网www.ipyingshe.com注册一个账号，完成后跳转到它的控制台。先下载客户端，小盒子是linux arm架构的所以选择“Linux ARM下载”。

[![](https://qnam.smzdm.com/202205/15/6280e654e2d733179.jpg_e1080.jpg)
](https://post.smzdm.com/p/a5oe60kl/pic_7/)

下载完成后会[得到](https://pinpai.smzdm.com/271508/)一个名为“sk\_linux\_arm”的文件，放着备用，我们要先建立一个隧道才能使用。按照图示操作，点击“开通/购买隧道”，隧道名称随意，内网端口填写qbittorrent web ui的端口8080，内网地址默认，选择免费版套餐。创建好后，复制下隧道令牌备用。

[![](https://qnam.smzdm.com/202205/15/6280e6560949f8535.jpg_e1080.jpg)
](https://post.smzdm.com/p/a5oe60kl/pic_8/)

[![](https://qnam.smzdm.com/202205/15/6280e656288926881.jpg_e1080.jpg)
](https://post.smzdm.com/p/a5oe60kl/pic_9/)

[![](https://qnam.smzdm.com/202205/15/6280e656387906066.jpg_e1080.jpg)
](https://post.smzdm.com/p/a5oe60kl/pic_10/)

ssh进小盒子，将刚才下载的“sk\_linux\_arm”文件上传到盒子，我直接上传到了根目录，上传工具可以用“winscp”或者“finalshell”等就不再赘述了。我这里用的finalshell在根目录新建一个脚本文件1.sh，编辑1.sh输入这两行代码cd /root nohup ./sk\_linux\_arm -token=yourtoken &将yourtoken替换成你的隧道令牌，要注意格式跟图上一样，编辑完后保存。文件管理器右键“文件权限”给予“sk\_linux\_arm”和“1.sh”文件777权限。可以先测试一下内网穿透成功没有，直接在命令行输入nohup ./sk\_linux\_arm -token=yourtoken &（记得替换令牌）命令，在浏览器上输入刚才创建的隧道的域名，如果成功了的话会打开qbittorrent的登录[界面](https://pinpai.smzdm.com/288313/)。

[![](https://qnam.smzdm.com/202205/15/6280e6562d4f9762.jpg_e1080.jpg)
](https://post.smzdm.com/p/a5oe60kl/pic_11/)

[![](https://qnam.smzdm.com/202205/15/6280e6563c89d3267.jpg_e1080.jpg)
](https://post.smzdm.com/p/a5oe60kl/pic_12/)

[![](https://qnam.smzdm.com/202205/15/6280e656e23b79138.jpg_e1080.jpg)
](https://post.smzdm.com/p/a5oe60kl/pic_13/)

[![](https://qnam.smzdm.com/202205/15/6280e6575e3ff1796.jpg_e1080.jpg)
](https://post.smzdm.com/p/a5oe60kl/pic_14/)

### 设置服务开机自启

打开/etc目录下的“rc.local”文件在exit 0这一行的上面加上qbittorrent-nox -d和./root/1.sh两行，如图示，然后保存退出。不出意外的话就设置好了内网穿透和qbittorrent的开机自启，先不要着急测试，还有一项重要操作没有做。

[![](https://qnam.smzdm.com/202205/15/6280e6576cbae1105.jpg_e1080.jpg)
](https://post.smzdm.com/p/a5oe60kl/pic_15/)

### 为qbittorrent挂载下载盘

作为下载器，最重要的当然是放资源的硬盘了，我不是收集狂，所以选择了一块720g的固态来做下载盘，linux是需要将硬盘挂载到想要的目录才可以正常使用的。有临时挂载和永久挂载两种方式，作为下载盘，我们基本上不会随意拔插硬盘，为了让其重启后能自动挂载，我们就将其永久挂载。

*   首先将硬盘连接到盒子上，输入命令fdisk -l查看是否系统能正确识别到，一般会在输出内容的最下面，如果只挂载了一块硬盘的话会被挂载成sda1，记住这个路径。
    

[![](https://qnam.smzdm.com/202205/15/6280e6575aa0f6882.jpg_e1080.jpg)
](https://post.smzdm.com/p/a5oe60kl/pic_16/)

*   将硬盘格式化成适用于linux的文件系统，输入命令mkfs.ext4 /dev/sad1上一步查询出的是哪个硬盘就输入哪个硬盘，别格错了，数据无价....
    

[![](https://qnam.smzdm.com/202205/15/6280e6578cc032990.jpg_e1080.jpg)
](https://post.smzdm.com/p/a5oe60kl/pic_17/)

*   输入blkid /dev/sda1查询硬盘的UUID，将其记录下来。
    

[![](https://qnam.smzdm.com/202205/15/6280e6584a8644793.jpg_e1080.jpg)
](https://post.smzdm.com/p/a5oe60kl/pic_18/)

*   编辑 /etc/fstab文件，添上一行UUID=12333-4321-567-4242-17557575 /mnt/yingpan/ ext4 defaults 0 0，其中“12333-4321-567-4242-17557575”是上一步的UUID，“/mnt/yingpan/”是你要挂载的目录也就是下载目录，一般讲[文件夹](https://www.smzdm.com/fenlei/wenjianjia/)挂载mnt目录下，新建目录的话要记得给权限。
    

[![](https://qnam.smzdm.com/202205/15/6280e6586d39c3623.jpg_e1080.jpg)
](https://post.smzdm.com/p/a5oe60kl/pic_19/)

*   输入mount -a不出意外的话就能看到挂载的硬盘了。
    

[![](https://qnam.smzdm.com/202205/15/6280e65867e0b287.jpg_e1080.jpg)
](https://post.smzdm.com/p/a5oe60kl/pic_20/)

*   挂在好硬盘后就可以进入qbittorrent管理界面设置下下载目录，设置成刚才挂载的目录，设置好后可以下载个小文件测试下。
    

[![](https://qnam.smzdm.com/202205/15/6280e658a223c6902.jpg_e1080.jpg)
](https://post.smzdm.com/p/a5oe60kl/pic_21/)

### 最后的一点设置

做完这一切可以重启下盒子检验劳动成果，不出意外的话就可以用穿透的域名远程访问qbittorrent了，这里推荐一个远程管理qbittorrent的小程序，叫PT管理宝。小程序里就能搜到，简单的设置就可以方便的远程管理qbittorrent了。

**端口号就是80**  

[![](https://qnam.smzdm.com/202205/15/6280e65891bb57314.jpg_e1080.jpg)
](https://post.smzdm.com/p/a5oe60kl/pic_22/)

[![](https://qnam.smzdm.com/202205/15/6280e658e83542634.jpg_e1080.jpg)
](https://post.smzdm.com/p/a5oe60kl/pic_23/)

总结
--

这次折腾耗时都耗在了寻找免费内网穿透上了，果然免费的才是最贵的...小盒子的超低功耗和超低价格是安装这类下载器的不二之选，不过对于需要储存大容量资料就不是太适合了，如果硬要上的话就得加[硬盘盒](https://www.smzdm.com/fenlei/yidongyingpanhe/)，就不如上x86啦，不过它适合我这种资源量小的，不过应该是可以挂载nas的目录，不过手头暂时没搭建nas没法测试，且我入PT才不久，大部分骚操作都不会，还得潜行修炼才行。

![](https://res.smzdm.com/pc/pc_shequ/dist/img/the-end.png)

![](https://qny.smzdm.com/202206/20/62b067564830b3731.jpg_a200.jpg)

绿联无线投屏器同屏hdmi传输转换器笔记本电脑到电视机高清点对点 【套装】发射器\*1+接收器\*1

绿联无线投屏器同屏hdmi传输转换器笔记本电脑到电视机高清点对点 【套装】发射器\*1+接收器\*1

_659元起_

![](https://qny.smzdm.com/202104/21/607f8c86d790a7974.jpg_a200.jpg)

Apple 苹果 4K电视盒子 黑色

Apple 苹果 4K电视盒子 黑色

_1399元起_

![](https://qny.smzdm.com/202108/02/6107bfa7d1d748150.jpg_a200.jpg)

WeBox 泰捷盒子 WE60C2 4K电视盒子 2GB+8GB 黑色

WeBox 泰捷盒子 WE60C2 4K电视盒子 2GB+8GB 黑色

_189元起_

![](https://y.zdmimg.com/202105/06/60939edd1e7da8929.jpg_a200.jpg)

当贝 H1 4K电视盒子 白色

当贝 H1 4K电视盒子 白色

_299元起_