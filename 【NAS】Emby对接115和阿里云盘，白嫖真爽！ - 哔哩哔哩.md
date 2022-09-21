# 【NAS】Emby对接115和阿里云盘，白嫖真爽！ - 哔哩哔哩
今天是白嫖教程

理论上，只要打通网络，emby是可以直接对接云盘的，就不需要自己NAS上加硬盘了。

前不久看有人出了专门针对阿里云的webdav的服务，算是其中的一种，这次的主角是**clouddrive**

它支持：

*   阿里云盘
    
*   115网盘
    
*   沃云盘
    
*   webdav
    
*   等等
    

其实，有看到115和阿里云盘我就已经兴奋起来了

重点看看是否能实现**不用自己买硬盘**

先说结论：能！

它的原理是把阿里云盘的内容挂载到你的NAS上，之后会把网盘上的封面什么的下载到硬盘上，其实就是个缓存。

但是前提是你一定要关闭emby建立媒体库时候，所有的，所有的，所有的，所有的刮削、扫描功能，全部取消勾选才行，否则一旦建立，你就会发现，你的NAS在疯狂下载云盘里的所有视频，后果很严重。

![](https://i0.hdslb.com/bfs/article/b9116b3b5dcc5f1f2b9f08041bddc7842e839509.png@942w_176h_progressive.webp)

如上图，他会疯狂下载网盘中的所有东西，下载到你的挂载点上

当你取消挂载的时候，东西一下子就都没了，仿佛从未发生过，回收站都找不到，但我们知道，数据写入是实打实的消耗了硬盘的

![](https://i0.hdslb.com/bfs/article/69a03ac2941492ec4a6e5a0df81e3484ac33d66a.png@942w_255h_progressive.webp)

这个工具的真正用法是：

本地刮削好以后再上传，之后通过挂载到NAS上看片，最后确认视频正常播放，且没被封杀，再删除或者不删除本地被占用的空间。

闹归闹，教程还是要上一下的

我这里是威联通系统，给出模板如下，大家可以根据自己的情况更改

```
version: '3'

services:
  cloudnas:
    image: cloudnas/clouddrive
    container_name: clouddrive
    volumes:
      - /share/CACHEDEV2_DATA/Container/clouddrive/cloud:/CloudNAS:shared
      - /share/CACHEDEV2_DATA/Container/clouddrive/config:/Config
    environment:
      - FuseUID=1000
      - FuseGID=100
    devices:
      - /dev/fuse:/dev/fuse
    restart: unless-stopped
    privileged: true
    ports:
       - 19798:9798
```

安装完成后，通过ip+端口号，就能看到下面的界面

![](https://i0.hdslb.com/bfs/article/18a64f0674c9721b051319ba24d63e7bc0dd6e83.png@942w_318h_progressive.webp)

如图即可挂载，好的一点是直接扫码就行，不用找扫码freshtoken这种东西了，也不容易失效

![](https://i0.hdslb.com/bfs/article/d24dd91d2866ac9841c1e9ee138998784aa61101.png@942w_557h_progressive.webp)

阿里云盘是不可能让你白嫖的，这么多年了互联网逻辑就是先白嫖，再伸手要钱，再限速，会员和超级会员走起，所以建议各位老老实实买硬盘  

后续会出一个完整的教程