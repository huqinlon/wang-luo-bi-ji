# emby 破解版完全食用方法
最新教程更新于 2020/09/13，添加 IOS破解方案

小广告
---

是否在为 Emby 无法刮削片子而烦恼？**Emby 国内加速服务**加速所有刮削器访问下载，拒绝手动刮削，直接勾选 TheMovieDb、The Open Movie Database、TheTVDB 来刮削电影、剧集、动画，让海报布满你的 Emby，让刮削不再成问题！旨在为了提高中国用户的 Emby 使用体验，[了解一下](https://neko.re/archives/192.html)  
[![](https://yukino.nos-eastchina1.126.net/QQ20200912-202411.png)
](https://yukino.nos-eastchina1.126.net/QQ20200912-202411.png)

前言
--

我从 [dotnet CoreFx](https://github.com/dotnet/corefx) 重新编译了 `System.Net.Http.dll` ，将认证服务器指向我的 api，达到通过认证的目的。

除了破解认证以外，我还修改了默认插件源，用我的服务器反代了官方的插件源，以加速国内用户下载插件。（其实官方插件源直接打不开

项目的开源地址在 [GitHub](https://github.com/YukiCoco/EmbyCrack)，希望能够留下你的小星星。

服务端破解
-----

**注意**：如果以前使用其他破解方案，必须将系统的 hosts 的 `mb3admin.com` `www.mb3admin.com` 条目删除，否则破解**不会**生效

**简要方法**：只需要将 [破解程序集](https://github.com/YukiCoco/EmbyCrack/tree/master/assembly) 替换原有文件即完成破解，原有文件地址为 `system/System.Net.Http.dll`，或使用 docker 直接安装破解版。

### 具体方法：

#### 群晖套件版

直接替换无效，请直接使用 Docker 替换或者用 Docker 的封装版本

#### 威联通 华芸等套件版（Linux）

Emby所在目录：

威联通：/share/CACHEDEV1\_DATA/.qpkg/EmbyServer

华芸：/volume1/.@plugins/AppCentral/emby-server/seystem/EmbyServer

如果目录不存在，使用 `find / -name EmbyServer` 直接查找，需要等待一段时间.  
关闭 Emby 后使用 `wget -O Emby所在目录/system/System.Net.Http.dll 'https://github.com/YukiCoco/EmbyCrack/raw/master/assembly/unix-x64/System.Net.Http.dll' --no-check-certificate` 下载破解程序集替换原有程序

或者你直接使用 Putty 替换也可以

#### Linux

1.  使用 `systemctl stop emby-server.service` 结束 emby 进程
2.  使用`ps -aux | grep "emby"` 找到 emby ，比如这里我的 emby 所在目录为`/opt/emby-server/system/`
3.  如果上面的指令无效，使用 `find / -name EmbyServer` 直接查找，需要等待一段时间
4.  使用 `wget -O /opt/emby-server/system/System.Net.Http.dll 'https://github.com/YukiCoco/EmbyCrack/raw/master/assembly/unix-x64/System.Net.Http.dll' --no-check-certificate` 下载破解程序集替换原有程序，注意这里要把路径改成你 Emby 所在目录
5.  启动 Emby 进程 `systemctl start emby-server.service`

如下操作找到 emby 位置

#### Windows

下载程序集后进入 Emby 目录替换原来的文件即可。

#### Docker

##### 替换安装

输入 `docker ps` 得到 emby docker 的 `CONTAINER ID`  
输入 `docker exec -it CONTAINER ID /bin/sh` 进入 docker 终端  
输入 `wget https://github.com/YukiCoco/EmbyCrack/raw/master/assembly/unix-x64/System.Net.Http.dll` 下载 dll  
输入 `cp System.Net.Http.dll system/` 替换原有 dll ，然后重启 emby docker 即可。

##### 全新安装

安装 Docker 源使用  
– `yukinococo/emby_crack:unix-x64` （Unix）  
– `yukinococo/emby_crack:windows-x64` （Windows）

至此，服务端破解就已经生效了，接下来还需要在浏览器做一些小修改才能在 PC 上完全使用，坐和放宽 :）

客户端破解
-----

**注意**：如果以前使用其他破解方案，必须将系统的 hosts 的 `mb3admin.com` `www.mb3admin.com` 条目删除，否则破解**不会**生效

*   **PC 浏览器**：安装 URLRedirector([Chrome,](https://chrome.google.com/webstore/detail/maolmdhneopinciaokgohljhpdedekee) [Firefox](https://addons.mozilla.org/zh-CN/firefox/addon/urlredirector/?src=search)) 插件，添加用户规则

原始地址 `https://mb3admin.com` ，目标地址 `https://crackemby.neko.re` ，然后确认并保存，别忘了勾选重定向。

[![](https://neko.re/wp-content/uploads/2020/07/emby1-1024x623.png)
](https://neko.re/wp-content/uploads/2020/07/emby1-1024x623.png)

别忘了勾选这里  
[![](https://neko.re/wp-content/uploads/2020/07/emby2.png)
](https://neko.re/wp-content/uploads/2020/07/emby2.png)

[![](https://yukino.nos-eastchina1.126.net/QQ20200915-202914.png)
](https://yukino.nos-eastchina1.126.net/QQ20200915-202914.png)

输入任意字符后这样显示，就是破解完成了。

*   Android & TV：使用Emby 破解版本，转自[nas2X](https://www.nas2x.com/threads/emby-for-androidtv-1-7-92g.1469/): [蓝奏云下载](https://wwa.lanzous.com/b0f1vlhri) ,密码:i06m
*   IOS：[破解教程](https://neko.re/archives/208.html)  
    至此，破解工作全部结束，**Enjoy**~

高级应用
----

服务端获取插件源已经替换为我的，但是后台显示插件图片采用的是 `https://raw.githubusercontent.com` 这个节点，国内访问速度很慢，建议加入到自己的代理白名单。

[![](https://neko.re/wp-content/uploads/2020/07/QQ20200724-121206-1024x608.png)
](https://neko.re/wp-content/uploads/2020/07/QQ20200724-121206-1024x608.png)