# 捡垃圾：50元包邮的我家云怎么样？教你如何挂载硬盘/共享文/smb和电脑访问，omv设置教程！_NAS存储_什么值得买
2019-10-19 15:18:40 1048点赞 3921收藏 761评论

**创作立场声明：** 此文为本人熬夜折腾连续一周的产物，欢迎点击收藏和点赞按钮，感谢大家的支持！

[![](https://qnam.smzdm.com/201910/19/5da9e30577a0f9342.jpg_e1080.jpg)
](https://post.smzdm.com/p/a3gwoe6k/pic_2/)

前言
--

大家好，俺又来了，这次带来一个50元还包邮的矿渣：我家云/联想粒子云 的OMV设置教程！![](https://res.smzdm.com/images/emotions/33.png)

**关于我家云：** 

> 联想粒子矿云 是 我家云 的马甲版本（主板相同），采用usb3.0转接sata方式内置 3.5寸4TB 硬盘；硬件方案为 RK3328，4核A53，1GB DDR内存，8GB emmc闪存，千兆网卡，usb2.0 和 usb3.0 接口各一个；闲鱼等平台上未解绑的4TB版本较便宜，刷入OMV等系统 后可作为轻量级NAS使用。

我家云和联想粒子云是同一种东西，就是logo不一样，内容都一模一样，下文都以我家云为主！![](https://res.smzdm.com/images/emotions/33.png)

**我家云和N1的配置还是要差一点点，但是内置3.5硬盘位置，而且颜值非常不错，金属外壳，用来当下载机，其速度还超过了N1只有USB2.0的速度，峰值ftp速度 读写能到100m/s，smb下也能达到60-80m/s。** 

整机功耗我测试过，我家云+3.5寸的4T硬盘，功耗只有8w，非常适合当一个轻量级的NAS和下载机，有了它，蜗牛都关机了！![](https://res.smzdm.com/images/emotions/33.png)

[![](https://am.zdmimg.com/201910/19/5da9e761373549809.jpg_e1080.jpg)
](https://post.smzdm.com/p/a3gwoe6k/pic_3/)

**外观展示：** 

这货的外观颜值真的非常的高，由于矿早就塌了，因为掉盘的原因，这家伙价格一路跌，999元跌到现在50元，也是没谁了（不含自带的硬盘），很多垃圾佬因为掉盘原因，放弃它，所以现在的价格算是冰点低价：![](https://res.smzdm.com/images/emotions/22.png)

[![](https://am.zdmimg.com/201910/19/5da9e41d41d523189.jpg_e1080.jpg)
](https://post.smzdm.com/p/a3gwoe6k/pic_4/)这一张图来自于网络

这是我买入的2台，正在测试，透明壳子的是小睿私人云（用来当播放器的，这个价位无敌，小睿以后有空给大家讲）：![](https://res.smzdm.com/images/emotions/22.png)

[![](https://qnam.smzdm.com/201910/19/5da9e71e5d2e73505.jpg_e1080.jpg)
](https://post.smzdm.com/p/a3gwoe6k/pic_5/)

我还购置了焊台和风枪，对这个我家云进行了硬改，只为确定它的稳定性，目前为止一切良好，等待我更多的结果，请持续关注我的动态和评论留言区：![](https://res.smzdm.com/images/emotions/22.png)

[![](https://qnam.smzdm.com/201910/19/5da9e71ed5ef22372.jpg_e1080.jpg)
](https://post.smzdm.com/p/a3gwoe6k/pic_6/)

**价格：**   

目前这家伙只要50元包邮，几乎99新，不带内置硬盘，闲鱼上大量有货，现在估计涨价幅度也在10块钱以内！

捡垃圾推荐，超值垃圾，值得下手！![](https://res.smzdm.com/images/emotions/34.png)

**购买原因：** 
----------

在半年之前，我就购买过我家云，那时候是买的带4T版本的，但是我家云刷入Omv（一个NAS系统）后，出现了严重的**掉盘情况**！![](https://res.smzdm.com/images/emotions/37.png)

没错，半个小时左右，硬盘丢失，完全无法正常使用，于是我就拿走了硬盘，放入别的NAS用了。我家云就一直吃灰的状态。![](https://res.smzdm.com/images/emotions/56.png)

**最近有朋友发现**，我家云已经有大佬研究出**不掉盘**的方法了，有用固件代码实现的，也有用硬件进行飞线短接实现的，目前我2种方法都在尝试和测试中，已经连续5天没出现掉盘。![](https://res.smzdm.com/images/emotions/64.png)

加上还有不少大佬分享了各种好玩，又有趣的固件，于是我家云的可玩性非常的高了！所以写篇文章推荐给各位值友！![](https://res.smzdm.com/images/emotions/43.png)

**刷机的教程，在网上大把大把，很好找到，找不到我以后可以写文章，为了篇幅问题，本篇不讲刷机教程。** 

**但是刷机之后，硬改的教程 以及 刷完之后如何设置omv的教程，发现没有人写，于是就有了这篇文章！![](https://res.smzdm.com/images/emotions/39.png)**

[![](https://qnam.smzdm.com/201910/19/5da9e5de5979a432.jpg_e1080.jpg)
](https://post.smzdm.com/p/a3gwoe6k/pic_7/)

**这里简单说明一下，刷机需准备一条双公头USB[数据线](https://www.smzdm.com/fenlei/shujuxian/)，不需要拆机，不需要换电源，只需要下载刷机工具，选择固件，连上我家云，3分钟即可完成，而且固件非常的成熟，放几个刷机以后的图：** 

1、刷机完成之后，进入这个页面，这个是基于armbian omv entware 的一套系统，大佬们整合的：

目前我刷的固件是这样的，还有些大佬分享的固件有更多功能，非常的全：![](https://res.smzdm.com/images/emotions/64.png)

[![](https://qnam.smzdm.com/201910/19/5da9e71cc4dc48739.jpg_e1080.jpg)
](https://post.smzdm.com/p/a3gwoe6k/pic_8/)

2、一个一个展示一下，omv 是一个类似[群晖](https://pinpai.smzdm.com/2315/)、[威联通](https://pinpai.smzdm.com/3063/)、万由NAS的系统，也可以当做一个nas系统使用，非常多的插件和功能，适合linux的玩家使用：![](https://res.smzdm.com/images/emotions/33.png)

[![](https://qnam.smzdm.com/201910/19/5da9e71cc353a4238.jpg_e1080.jpg)
](https://post.smzdm.com/p/a3gwoe6k/pic_9/)

3、这个armbian，linux[服务器](https://www.smzdm.com/fenlei/fuwuqi/)玩家最喜爱的系统之一，全是代码代码代码，但是这套东西稳定啊，而且可以长期开启运行某些程序，比如博客，各种网页，各种运行程序，用来学习linux也非常适合：![](https://res.smzdm.com/images/emotions/35.png)

其实我也是小白，但是接触多了，就慢慢了解了一点点，如果要捡好玩的垃圾，这套东西是必须要了解的！

[![](https://qnam.smzdm.com/201910/19/5da9e71cd2e984431.jpg_e1080.jpg)
](https://post.smzdm.com/p/a3gwoe6k/pic_10/)

4、可道云，如果说omv是NAS的框架，那可道云就是NAS和文件存储核心了，非常适合管理NAS上的文件，当我家云安装了硬盘以后，用这个可道云可以快速访问硬盘里面的文件，并且支持快速预览等功能：

您可以理解这货就是一个私人的百度网盘软件，而资料，放在我家云的硬盘中，随时读取： ![](https://res.smzdm.com/images/emotions/43.png)

[![](https://qnam.smzdm.com/201910/19/5da9e71d6aa926773.jpg_e1080.jpg)
](https://post.smzdm.com/p/a3gwoe6k/pic_11/)

5、可道云还有个高\*\*的桌面页面，可以运行主流的一些软件，配合端口转发，手机app，家里的这个小东西，真的能当作一个高性能的NAS使用，而且私有云，很牛的呢：![](https://res.smzdm.com/images/emotions/64.png)

[![](https://qnam.smzdm.com/201910/19/5da9e71d7e9fb8076.jpg_e1080.jpg)
](https://post.smzdm.com/p/a3gwoe6k/pic_12/)

6、再说另外的功能，aria2下载，可以下各种东西，类似离线下载，设置好了以后，还可以下载解锁某网盘的速度，可以说是非常强大的工具，以后有想听的朋友，可以细说，分享个设置教程等：![](https://res.smzdm.com/images/emotions/40.png)

[![](https://qnam.smzdm.com/201910/19/5da9e71d678919179.jpg_e1080.jpg)
](https://post.smzdm.com/p/a3gwoe6k/pic_13/)

7、最核心 硬核的功能，docker！这个东西是容器，看过我之前文章的不少小伙伴，都知道我在群晖NAS里面，利用docker容器，安装了各种各样非常厉害的程序。![](https://res.smzdm.com/images/emotions/109.png)

目前[我的小站](http://www.junwen.bid:1800/)已经半年多正常运行，全运行在家里的NAS里，但是NAS多费电啊，有了这货，就可以全套转移到这个里面，功耗大大降低，但是目前因为懒，实在不想替换，就给docker安装了一个QB下载器：

[![](https://qnam.smzdm.com/201910/19/5da9e71dcb4f21430.jpg_e1080.jpg)
](https://post.smzdm.com/p/a3gwoe6k/pic_14/)

8、QB下载器的教程，我之前有分享过，非常时候pt玩家使用，目前我这个是运行在我家云的docker上，非常的给力：![](https://res.smzdm.com/images/emotions/110.png)

[![](https://qnam.smzdm.com/201910/19/5da9eb38513771437.jpg_e1080.jpg)
](https://post.smzdm.com/p/a3gwoe6k/pic_15/)

9、除此之外，您也可以使用tr等东西，可以说linux常用的工具，这里都可以实现的，这里还有一个设备监视服务，可以看到设备的一些性能占用情况，我家云4核1G内存，完全没有压力：![](https://res.smzdm.com/images/emotions/33.png)

[![](https://qnam.smzdm.com/201910/19/5da9e71e157058572.jpg_e1080.jpg)
](https://post.smzdm.com/p/a3gwoe6k/pic_16/)

10、当然，还少不了大佬提供的PHP探针服务，这个可以直接了解这个作为服务器行不行，好不好用等情况：

目前wdmomo大佬的整个站都是放在我家云的这套服务器中，已经运行了很久，它的站比我的站强大太多了，我家云已经完全适合，没想到这么大的站，居然用在这50块钱的矿渣身上！![](https://res.smzdm.com/images/emotions/28.png)

[![](https://qnam.smzdm.com/201910/19/5da9e71e4156b4188.jpg_e1080.jpg)
](https://post.smzdm.com/p/a3gwoe6k/pic_17/)

到此，我家云的介绍已经非常清楚了。![](https://res.smzdm.com/images/emotions/111.png)

估计还是有朋友想问，它到底是做什么的？我怎么看不懂你说的！![](https://res.smzdm.com/images/emotions/58.png)
 ![](https://res.smzdm.com/images/emotions/57.png)
 ![](https://res.smzdm.com/images/emotions/135.png)

**我家云总结**
---------

我这里再总结一下，我家云，它适合当作一台下载机，和一个轻量的NAS，当作你家里的私有云使用。 。

原因有以下几点：![](https://res.smzdm.com/images/emotions/33.png)

1、**传输速度**，它比N1，章鱼星球、这种只有USB2.0的机器速度快，达到60-100m/s的内网传输速度。

2、**硬盘选择**，它内置3.5寸硬盘位，比猫盘这种放2.5寸硬盘位的机器更多的选择，3.5寸硬盘便宜。

3、**外观漂亮，**它和贝壳云是同一种CPU，但是它的外观比较漂亮，无需各种改造风道，全金属的外壳，非常结实。

4、**功耗较低，**目前它带一个硬盘的功耗只有8w，比蜗牛星际同等情况低了一倍功耗，每个月可以多省不少电费。

5、**价格便宜，**目前50块钱包邮，还要什么自行车呢。虽然速度无法跑满千兆，但是日常传个文件，下个电影直接进行播放，完全没有问题的。

它有什么缺点：![](https://res.smzdm.com/images/emotions/24.png)

1、它不适合当一个播放器使用，解码不如章鱼星球。

2、它有可能还会掉盘，目前还在测试阶段。

3、它相当于一个linux服务器，学习难度比群晖要高不少，还好大佬们都制作了很多优秀的固件。

4、会linux的朋友，会把这个玩得飞起，不会的朋友很容易敲错代码，导致各种不稳定情况。

建议多买2台，一台折腾，一台稳定挂机！

终于说完了！也许这是现在这个阶段，对我家云最中肯的一套总结了！欢迎大家评论区补充。

**我家云这个小东西，已经让我连续熬夜好几天了，希望大家能帮忙点下赞，点下收藏，谢谢大家的支持！**

OMV设置教程：
--------

最近很多买了我家云的朋友，不会用，最基础的OMV都不知道咋弄，这篇文章就来帮大家解决这个问题！![](https://res.smzdm.com/images/emotions/38.png)

我是阿文菌，感谢大家一路以来的支持，垃圾佬捡垃圾的路上，让我们一路随行！![](https://res.smzdm.com/images/emotions/118.png)

**刷机的时候，无需安装硬盘、刷好后，再安装硬盘，硬盘建议在电脑上格式化ext4格式，再使用！**

1、刷好我家云系统后，拔电源，安装好硬盘，开机！  
鼠标移动到每个功能上，都会提示账号和密码，先进入OMV：![](https://res.smzdm.com/images/emotions/33.png)

[![](https://qnam.smzdm.com/201910/19/5da9ef57b37302470.jpg_e1080.jpg)
](https://post.smzdm.com/p/a3gwoe6k/pic_18/)

2、输入账号：admin，密码：omv，登录：![](https://res.smzdm.com/images/emotions/114.png)

[![](https://qnam.smzdm.com/201910/19/5da9ef6459d8c7744.png_e1080.jpg)
](https://post.smzdm.com/p/a3gwoe6k/pic_19/)

3、登录以后，点击磁盘选项，看看你的磁盘正不正常：![](https://res.smzdm.com/images/emotions/110.png)

[![](https://qnam.smzdm.com/201910/19/5da9ef6a5cae03980.png_e1080.jpg)
](https://post.smzdm.com/p/a3gwoe6k/pic_20/)

4、如果这里没有读取到磁盘，直接重启一下omv：![](https://res.smzdm.com/images/emotions/34.png)

[![](https://qnam.smzdm.com/201910/19/5da9ef70286145477.png_e1080.jpg)
](https://post.smzdm.com/p/a3gwoe6k/pic_21/)

5、重启和关机选项在omv右上角的地方：![](https://res.smzdm.com/images/emotions/22.png)

[![](https://qnam.smzdm.com/201910/19/5da9ef7698222297.png_e1080.jpg)
](https://post.smzdm.com/p/a3gwoe6k/pic_22/)

6、重启以后，就可以看到硬盘了，这个固件，我使用了到现在快5天，无硬改我家云，都没掉盘。  
而且还用了docker挂了qb下载器进行下载，很稳！![](https://res.smzdm.com/images/emotions/60.png)

[![](https://qnam.smzdm.com/201910/19/5da9ef7fbebbf4473.png_e1080.jpg)
](https://post.smzdm.com/p/a3gwoe6k/pic_23/)

7、如果你的硬盘之前没有格式化成ext4格式，建议点下擦除，我这里仅演示，所以没点：  

[![](https://am.zdmimg.com/201910/19/5da9ef86334139762.png_e1080.jpg)
](https://post.smzdm.com/p/a3gwoe6k/pic_24/)

8、点击系统文件，选择sda1这个，点挂载：![](https://res.smzdm.com/images/emotions/99.png)

[![](https://qnam.smzdm.com/201910/19/5da9ef8b7ac3d1719.png_e1080.jpg)
](https://post.smzdm.com/p/a3gwoe6k/pic_25/)

9、挂载以后，就可以看到硬盘的容量了：![](https://res.smzdm.com/images/emotions/33.png)

[![](https://am.zdmimg.com/201910/19/5da9ef93650506070.png_e1080.jpg)
](https://post.smzdm.com/p/a3gwoe6k/pic_26/)

10、点下面的共享[文件夹](https://www.smzdm.com/fenlei/wenjianjia/)，添加，选择设备，输入文件夹名字，比如wjy，然后保存：![](https://res.smzdm.com/images/emotions/110.png)

[![](https://qnam.smzdm.com/201910/19/5da9ef992317f7569.png_e1080.jpg)
](https://post.smzdm.com/p/a3gwoe6k/pic_27/)

11、保存成功，这个路径并不是真实的硬盘路径，后文有讲：  

[![](https://qnam.smzdm.com/201910/19/5da9ef9f639ee4961.png_e1080.jpg)
](https://post.smzdm.com/p/a3gwoe6k/pic_28/)

12、点击下面的SMB，将SMB打开，每个选项完成后，都要应用一下：![](https://res.smzdm.com/images/emotions/22.png)

[![](https://qnam.smzdm.com/201910/19/5da9efa6c4a115205.png_e1080.jpg)
](https://post.smzdm.com/p/a3gwoe6k/pic_29/)

13、然后点击用户，添加一个用户，比如用户名aa,密码aa：![](https://res.smzdm.com/images/emotions/188.gif)

[![](https://qnam.smzdm.com/201910/19/5da9efadc01313501.png_e1080.jpg)
](https://post.smzdm.com/p/a3gwoe6k/pic_30/)

14、刷新一下页面，看到smb服务开启，就代表正常了：![](https://res.smzdm.com/images/emotions/189.gif)

[![](https://am.zdmimg.com/201910/19/5da9efb34c50c6496.png_e1080.jpg)
](https://post.smzdm.com/p/a3gwoe6k/pic_31/)

15、点击这个文件夹，可以设置一下权限，让这个账户能够读写：  

[![](https://qnam.smzdm.com/201910/19/5da9efb8511428379.png_e1080.jpg)
](https://post.smzdm.com/p/a3gwoe6k/pic_32/)

16、然后我们在电脑上输入这台我家云的ip，ip地址，输入刚刚建立的账户aa密码aa，找到了刚刚建立的共享文件夹：![](https://res.smzdm.com/images/emotions/67.png)

[![](https://qnam.smzdm.com/201910/19/5da9efbf3f3c85945.png_e1080.jpg)
](https://post.smzdm.com/p/a3gwoe6k/pic_33/)

17、然后我们右键，映射一下这个文件夹在电脑上：![](https://res.smzdm.com/images/emotions/110.png)

[![](https://qnam.smzdm.com/201910/19/5da9efc7a9eb01136.png_e1080.jpg)
](https://post.smzdm.com/p/a3gwoe6k/pic_34/)

18、这样，就能很方便的读写我家云上面的文件了，已经达到NAS的基本功能了 :![](https://res.smzdm.com/images/emotions/33.png)

[![](https://qnam.smzdm.com/201910/19/5da9efce839df8846.png_e1080.jpg)
](https://post.smzdm.com/p/a3gwoe6k/pic_35/)

19、除此之外，我家云的一个路径，还要说明一下，在使用tr下载工具的时候，这个路径需要设置一下。那么路径是多少呢？![](https://res.smzdm.com/images/emotions/33.png)

我用winscp工具，登录到我家云里面，查看里面的文件结构，发现这里有2个文件夹，都可以访问到硬盘的文件夹：

[![](https://qnam.smzdm.com/201910/19/5da9efe36d6864591.png_e1080.jpg)
](https://post.smzdm.com/p/a3gwoe6k/pic_36/)

20、使用这个路径作为下载地址，会比较的长，比较麻烦：![](https://res.smzdm.com/images/emotions/43.png)

[![](https://qnam.smzdm.com/201910/19/5da9eff04947d9548.png_e1080.jpg)
](https://post.smzdm.com/p/a3gwoe6k/pic_37/)

21、而share这个路径就比较方便了，以后下载路径就设置这个为硬盘路径：![](https://res.smzdm.com/images/emotions/99.png)

[![](https://qnam.smzdm.com/201910/19/5da9eff5214504006.png_e1080.jpg)
](https://post.smzdm.com/p/a3gwoe6k/pic_38/)

22、比如我在docker的QB里面就是这样设置的路径：![](https://res.smzdm.com/images/emotions/110.png)

[![](https://qnam.smzdm.com/201910/19/5da9effaaca0a5232.jpg_e1080.jpg)
](https://post.smzdm.com/p/a3gwoe6k/pic_39/)

以后添加硬盘和查找硬盘路径，就可以到 /sharedfolders里面查找！![](https://res.smzdm.com/images/emotions/33.png)

总结
--

好了。到此，我家云和OMV设置共享文件夹的教程就完成了！![](https://res.smzdm.com/images/emotions/43.png)

我家云50块钱还包邮的价格，已经能拥有一台轻量的NAS，实在是非常的不错的。

关于硬改，目前我还在测试阶段，等我测试好了后，可以出一篇关于硬改的文章，目前透露一下，需要短接和飞线，说难不难，说简单也不简单。![](https://res.smzdm.com/images/emotions/33.png)

大家可以愉快的玩耍了，出了任何问题，记得先重启，实在不行，重新刷机就好！![](https://res.smzdm.com/images/emotions/35.png)

我是阿文菌，写文章不易，希望大家多多支持！帮我点个赞，点个收藏再走行不行呢？

我们下篇文章再见！![](https://res.smzdm.com/images/emotions/33.png)

对了，如果有碎银子，也希望能打赏一点给俺，谢谢拉！![](https://res.smzdm.com/images/emotions/39.png)

![](https://res.smzdm.com/pc/pc_shequ/dist/img/the-end.png)