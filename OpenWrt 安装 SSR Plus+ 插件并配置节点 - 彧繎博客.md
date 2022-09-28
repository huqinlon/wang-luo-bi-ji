# OpenWrt 安装 SSR Plus+ 插件并配置节点 - 彧繎博客
前面有说过在 OpenWrt 安装配置 PassWall 的教程，下面分享一下在R2S上的 OpenWrt 安装 ShadowSocksR Plus+ 插件并配置订阅节点，目前SSR Plus+（酸酸乳 Plus+）支持 SS、SSR、V2RAY、TROJAN、SOCKS5、TUN，想要更好的使用 酸酸乳 Plus+ 还需要配合使用 Turbo ACC 网络加速设置 和 ChinaDNS，本站有相关教程，可查阅后配合使用。

**插件下载**
--------

本插件为 aarch64\_generic 架构固件使用的 ShadowSocksR Plus+ 插件，优先R2S，R4S，N1等 arm64 架构使用，其他架构的可以参考插件库，如出现无法安装或者提示错误，可以查看《[OpenWrt 正确编译 SSRplus 与 Passwall 的方法](https://opssh.cn/luyou/188.html)》，本教程的前提是必须拥有 ShadowSocksR Plus+ 所有的依赖才可以直接安装使用。

**插件库：** [https://op.supes.top/packages/](https://op.supes.top/packages/)

**主插件：** [https://cloud.opssh.cn/chajian/luci-app-ssr-plus\_178-6\_all.ipk](https://cloud.opssh.cn/chajian/luci-app-ssr-plus_178-6_all.ipk)

**中文补丁：** [https://cloud.opssh.cn/chajian/luci-i18n-ssr-plus-zh-cn\_178-6\_all.ipk](https://cloud.opssh.cn/chajian/luci-i18n-ssr-plus-zh-cn_178-6_all.ipk)

**插件安装**
--------

下载好 ShadowSocksR Plus+ 插件，打开 OpenWrt 管理界面，进入系统列表页找到文件传输，选择上传 ShadowSocksR Plus+ 插件，并在上传文件列表进行安装，如下图：

**个人建议：** 将主插件和中文一起上传，先安装主插件，然后安装中文版。

![](https://oss.opssh.cn/zb_users/upload/2021/11/202111017639_880.png)

**插件配置**
--------

进入ShadowSocksR Plus+ 插件，选择 服务器节点，先填写 订阅节点 链接，然后关闭自动更新，更新订阅URL列表，如果更新订阅URL列表没有更新成功，可以选择更新所有订阅服务器节点进行更新，因为部分固件经常会无法使用URL更新列表。

![](https://oss.opssh.cn/zb_users/upload/2021/11/202111011604_982.png)

进入高级设置，打开服务器节点故障切换，不建议开启广告屏蔽，开启后经常无法打开网页。

![](https://oss.opssh.cn/zb_users/upload/2021/11/202111013985_883.png)

进入客户端选项，主服务器选择较低延迟的节点，这里在说明一下，延迟高低不能完全代表节点服务器的快慢，有些专线都延迟就会很高，但下载很快，游戏和奈飞在需要的时候可以选择开启，日常浏览谷歌和YouTube无需开启，DNS服务器可以配合 ChinaDNS 插件使用。

![](https://oss.opssh.cn/zb_users/upload/2021/11/202111014158_699.png)

如果有不想走代理的网站，进入访问控制，选择不走代理的域名，将域名复制进入即可。

![](https://oss.opssh.cn/zb_users/upload/2021/11/202111016043_705.png)

以上完成后，进入状态选项内，【谷歌】连通性检查 和 【百度】连通性检查，如果正常，那么就可以浏览谷歌和YouTube了，如果不正常，先检查服务器端口，然后更新一下【GFW列表】数据库 和 【国内IP段】数据库。

![](https://oss.opssh.cn/zb_users/upload/2021/11/202111011664_591.png)

**节点测速**
--------

为保证测速效果，测试环境为最低配置，移动100M + 普通节点，YouTube 4K 跑分为 38638Kbps，本次测速节点由 [Gsou 云加速](https://gsoust.xyz/auth/register?code=Oerw) 提供，全站专线，游戏加速，流媒体4K秒开，年付新用户全场六折。

![](https://oss.opssh.cn/zb_users/upload/2022/01/202201294798_337.png)

**最后说明**
--------

在 100M 的移动宽带下测试 YouTube 4K视频播放下载结果，YouTube 视频下载速度为 38638Kbps，如果订阅节点感觉整体不太理想，可以配合下载安装 [Turbo ACC 网络加速插件](https://opssh.cn/luyou/15.html)，开启 转发加速 和 BBR加速，DNS建议使用 阿里云 和 DNSpod 的免费DNS服务。