# 怎么获取网页logo图标的URL链接 - 简书
怎么获取网页logo图标的URL链接
==================

[![](https://upload.jianshu.io/users/upload_avatars/26312444/598d2e35-8d0b-42c1-9dc9-9dcc25dc9f70.png?imageMogr2/auto-orient/strip|imageView2/1/w/96/h/96/format/webp)
](/u/1262cf1b9984)

[乐吐槽](/u/1262cf1b9984)关注IP属地: 广东

0.1142021.05.18 22:20:44字数 492阅读 7,084

网页logo图标一般是指favicon图标，作为缩略的网站标志，一般显示于浏览器的地址栏或者在标签上。

如图中红圈的位置， 即是favicon图标。

![](https://upload-images.jianshu.io/upload_images/26312444-47fe04132dce433d)

网易云logo

favicon的格式不一定是ico格式，它可以是png，jpg甚至是gif，不过ico格式是所有浏览器都支持的。

最近在做自己的网址导航，需要提取和显示网址的favicon图标，使导航链接除了文字名称，前面还有更明显的logo标志，使选择和查看都更加便捷。

![](https://upload-images.jianshu.io/upload_images/26312444-86913934b28120b3)

自建网址导航需要l链接ogo

![](https://upload-images.jianshu.io/upload_images/26312444-fd08bc387e993347)

乐吐槽网址导航

#### 那么怎么获取网页logo图标的URL链接呢？有三种方法。

*   **最常用的方法（适用于90%的站点）是，直接在访问网址首页链接后加上 _/favicon.ico_**，例如：
    
    [https://www.baidu.com/favicon.ico](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.baidu.com%2Ffavicon.ico)
    
    \[图片上传失败...(image-8d6714-1621347601936)\]
    
*   第二种获取方法，需要在浏览器界面**按F12键，进入开发者模式**（建议使用google内核浏览器）。在默认的“Elements”中**点开<head>...</head>**
    
    **找到<link 中含有favicon或ico的链接**，右键点选“Edit attribute“（即编辑属性）以复制该链接，粘贴到空白页面后回车后即显示该网站的logo图标。
    
    ![](https://upload-images.jianshu.io/upload_images/26312444-d4a86f8829df7433)
    
    F12开发者模式
    
    ![](https://upload-images.jianshu.io/upload_images/26312444-bc648e387b0e5296)
    
    找到标识
    
    ![](https://upload-images.jianshu.io/upload_images/26312444-d781c6e623370910)
    
    复制链接
    

说明：有的链接前缀格式是//开头的，建议使用作为URL链接时前面加上https:，

例如//s1.music.126.net/style/favicon.ico?v20180823

改成[https://s1.music.126.net/style/favicon.ico?v20180823](https://links.jianshu.com/go?to=https%3A%2F%2Fs1.music.126.net%2Fstyle%2Ffavicon.ico%3Fv20180823)

![](https://upload-images.jianshu.io/upload_images/26312444-d66db8e63409309c)

//开头的favicon链接

*   第三种方法，**使用第三方的矢量logo图标素材，如60logo、WorldVectorLogo**（不支持中文搜索）
    
    这些网站有很多，可以找到所需的logo或自定义logo，还是很方便的。
    

![](https://upload-images.jianshu.io/upload_images/26312444-835e63f8959b8b04)

image

![](https://upload-images.jianshu.io/upload_images/26312444-a90c1ad336a17fab)

image

以上，你学会了吗？

1人点赞

[建站及SEO](/nb/50183322)

更多精彩内容，就在简书APP

![](https://upload.jianshu.io/images/js-qrc.png)

"小礼物走一走，来简书关注我"

赞赏支持还没有人赞赏，支持一下

[![](https://upload.jianshu.io/users/upload_avatars/26312444/598d2e35-8d0b-42c1-9dc9-9dcc25dc9f70.png?imageMogr2/auto-orient/strip|imageView2/1/w/100/h/100/format/webp)
](/u/1262cf1b9984)

[乐吐槽](/u/1262cf1b9984 "乐吐槽")有文艺情怀的职场IT男 <br>弱电&amp;运维工程师<br>记录和分享有趣的文字和知识

总资产0.295共写了1.6W字获得3个赞共3个粉丝

关注