# Aria2 Pro - 更好用的 Aria2 Docker 容器镜像 - P3TERX ZONE
前言
--

Aria2 是目前最强大的全能型下载工具，它支持 BT、磁力、HTTP、FTP 等下载协议，常用做离线下载的服务端。目前有非常多的 Aria2 Docker 方案，大多都整合了 We­bUI 和文件管理功能，看似很好很强大，实际上都只是做了简单的打包的工作，完全没有考虑到核心的下载体验和资源占用等问题。这也导致很多人在初次使用 Aria2 时会遇到 BT 下载无速度、文件残留占用空间、任务丢失等问题，所以会觉得 Aria2 并不好用，但事实并非如此。[Aria2 完美配置](https://p3terx.com/go/aHR0cHM6Ly9naXRodWIuY29tL1AzVEVSWC9hcmlhMi5jb25m)是博主经过长时间使用和研究官方文档后总结出来的一套配置方案，其最初目的是为了解决这些问题，而且为 Aria2 添加了额外的一些功能，经过一年多时间的打磨已经积累了大量的使用者和良好的口碑，其中不乏一些知名开源项目开发者、影视字幕组、科技视频 UP 主。之前一直使用[一键脚本](https://p3terx.com/go/aHR0cHM6Ly9naXRodWIuY29tL1AzVEVSWC9hcmlhMi5zaA)作为部署方案，为了满足小伙伴们使用 Docker 部署的需求，博主特意制作了基于 Aria2 完美配置和特殊定制优化的 Aria2 Docker ，为了和一般的 Aria2 Docker 方案做区分所以将其取名为 [Aria2 Pro](https://p3terx.com/go/aHR0cHM6Ly9naXRodWIuY29tL1AzVEVSWC9kb2NrZXItYXJpYTItcHJv)。

镜像特点
----

*   使用 [Aria2 完美配置](https://p3terx.com/go/aHR0cHM6Ly9naXRodWIuY29tL1AzVEVSWC9hcmlhMi5jb25m)方案
    
    *   BT 下载率高、速度快
    *   重启后不丢失任务进度、不重复下载
    *   删除正在下载的任务自动删除未完成的文件
    *   下载错误自动删除未完成的文件
    *   下载完成自动删除控制文件(`.aria2`后缀名文件)
    *   下载完成自动删除种子文件(`.torrent`后缀名文件)
    *   下载完成自动删除空目录
    *   BT 下载完成自动清除垃圾文件(文件类型过滤功能)
    *   BT 下载完成自动清除小文件(文件大小过滤功能)
    *   有一定的防版权投诉、防迅雷吸血效果
    *   更好的 PT 下载支持
*   使用 [Aria2 Pro Core](https://p3terx.com/go/aHR0cHM6Ly9naXRodWIuY29tL1AzVEVSWC9hcmlhMi1idWlsZGVy) 项目最新静态编译二进制文件
    
    *   多平台：`amd64`, `i386`, `arm64`, `armhf`（VPS、群辉、树莓派等常见平台完美支持）
    *   全功能：`Async DNS`, `BitTorrent`, `Firefox3 Cookie`, `GZip`, `HTTPS`, `Message Digest`, `Metalink`, `XML-RPC`, `SFTP`
    *   单服务器线程数最大值无上限（已破解线程数限制）
    *   防掉线程优化
    *   内存消耗优化
    *   读写性能优化
    *   最新依赖库，下载更安全、稳定、快速
    *   持续更新最新版本
*   支持与 [RCLONE](https://p3terx.com/go/aHR0cHM6Ly9yY2xvbmUub3JnLw) 联动
    
    *   自动上传 OneDrive 、Google Drive 等网盘
    *   百度网盘转存到其它网盘
    *   多网盘自由选择
*   支持新一代互联网协议 IPv6
*   下载完成自动移动文件到指定目录（文件自动归档/分类）
*   定时自动更新 BT tracker 列表（无感知、无重启），保持 BT 下载高速率
*   用户文件权限自动配置功能
*   配置文件持久化，支持使用 [watchtower](https://p3terx.com/archives/docker-watchtower.html) 更新容器。
*   极简设计，专注下载，简单易用，少即是多。

项目地址
----

GitHub: [https://github.com/P3TERX/docker-aria2-pro](https://p3terx.com/go/aHR0cHM6Ly9naXRodWIuY29tL1AzVEVSWC9kb2NrZXItYXJpYTItcHJv)

Docker Hub: [https://hub.docker.com/r/p3terx/aria2-pro](https://p3terx.com/go/aHR0cHM6Ly9odWIuZG9ja2VyLmNvbS9yL3AzdGVyeC9hcmlhMi1wcm8)

支持本项目欢迎随手点个 `star`，可以让更多的人发现、使用并受益。你的支持是我持续开发维护的动力。

基础使用
----

*   最基本的启动命令如下，你只需要完整替换`<TOKEN>`字段(RPC密钥)即可启动。更强大的功能请阅读后文。

```
docker run -d \
    --name aria2-pro \
    --restart unless-stopped \
    --log-opt max-size=1m \
    --network host \
    -e PUID=$UID \
    -e PGID=$GID \
    -e RPC_SECRET=<TOKEN> \
    -e RPC_PORT=6800 \
    -e LISTEN_PORT=6888 \
    -v $PWD/aria2-config:/config \
    -v $PWD/aria2-downloads:/downloads \
    p3terx/aria2-pro
```

*   配置本机防火墙开放必要的入站端口，内网机器在路由器设置端口转发到相同端口。
*   使用你喜欢的 WebUI 或 App 进行连接，强烈推荐 [AriaNg](https://p3terx.com/archives/aria2-frontend-ariang-tutorial.html)。
*   体验高速远程离线下载的乐趣。

> **TIPS:**
> 
> *   如果你习惯使用 Docker Compose 进行部署，[这里提供了一个模版](https://p3terx.com/go/aHR0cHM6Ly9naXRodWIuY29tL1AzVEVSWC9Eb2NrZXItQXJpYTItUHJvL2Jsb2IvbWFzdGVyL2RvY2tlci1jb21wb3NlLnltbA)。
> *   群晖用户如果对以上文字描述看得一脸懵逼，那么可参考[这篇教程](https://p3terx.com/archives/synology-nas-docker-advanced-tutorial-deploy-aria2-pro.html)。
> *   unRaid 用户可直接添加[这个模版仓库](https://p3terx.com/go/aHR0cHM6Ly9naXRodWIuY29tL1AzVEVSWC91bnJhaWQtZG9ja2VyLXRlbXBsYXRlcw)。

选项参数说明
------

### Docker 基本选项

  
点击查看

*   `--name aria2-pro` - 容器名称，可自定义以示区分。
*   `--restart unless-stopped` - 设置容器重启策略，详情参见 [Docker 官方文档](https://p3terx.com/go/aHR0cHM6Ly9kb2NzLmRvY2tlci5jb20vZW5naW5lL3JlZmVyZW5jZS9jb21tYW5kbGluZS9ydW4vI3Jlc3RhcnQtcG9saWNpZXMtLS1yZXN0YXJ0)。
*   `--log-driver json-file` - 设置日志记录格式为 json 格式。这是 Docker 的默认值，某些特殊情况可能需要设置。
*   `--log-opt max-size=1m` - 日志大小限制为`1MB`，防止 Aria2 持续下载产生大量的日志占用磁盘空间。某些 GUI 可能没有相关选项。~所以说有什么理由不用 CLI 一把梭？~
*   `--network host` - 使用 host 网络模式。直接使用宿主机网络，免去端口映射导致的部分性能损失，且灵活性更高，可更方便的配置使用 IPv6 网络。host 网络模式仅适用于 Docker 17.06+ ，如果你的 Docker 版本低于此，请先升级。
    
    > ⚠️ ma­cOS 和 Win­dows 上的 Docker 目前暂时无法使用 host 网络模式，依然需要进行端口映射。方法参见后面的 **bridge 网络模式**章节。
    

### 容器目录映射/挂载

  
点击查看

*   `-v $PWD/aria2-config:/config` - 配置目录映射，配置文件持久化。左边为宿主机路径供自定义，**不要有中文、不要混用配置文件，首次使用请确保目录为空。** 
*   `-v $PWD/aria2-downloads:/downloads` - 下载目录映射。左边为宿主机路径供自定义，**不要有中文**。

> **TIPS:** 注意确认目录是否存在、权限是否正确。

### 用户文件权限设置（容器用户映射）

  
点击查看

对以上映射的目录进行用户权限设置，`aria2c` 核心进程会以所设定的用户运行。当使用非 root 用户进行管理时非常重要，这关乎到安全性和文件是否能正常访问。你不应该错过这个细节，否则可能导致不必要的麻烦。

*   `-e PUID=$UID` - 用户映射。设置文件管理账户的`UID`(用户 ID)。忽略则默认为`nobady`用户，并权限最大化。
*   `-e PGID=$GID` - 用户组映射。设置文件管理账户的`GID`(用户组 ID)。忽略则默认为`nogroup`用户组，并权限最大化。

> **科普：**  在常规的 Linux 发行版中`$UID`与`$GID`这两个环境变量分别为当前登录账户的`UID`与`GID`值，所以通过 CLI 启动容器可以直接使用这两个变量。但需要注意可能有部分系统`$GID`没有被定义。

**如果管理文件的账户不是当前登录的账户或者使用 GUI 创建容器请务必执行 `id` 命令手动获取并填写**。比如我的账户为 `p3terx`，那么就执行 `id p3terx`：

```
$ id p3terx
uid=1000(p3terx) gid=100(users) ...
```

设置后容器会自动去配置文件权限，配置目录权限为 `755`，配置文件为 `600`，附加功能脚本为 `700`，下载目录为设定用户 `rwx` 权限，以提升安全性，且每次重启容器会进行修正，防止权限问题。

在不进行任何设置的情况下，即 `PUID` 或 `PGID` 其中任意一个变量留空。容器默认会定义用户为 `nobady`、用户组为 `nogroup`，并且设置权限最大化（`777`），这样理论上任何用户都可以读写配置文件、附加功能脚本等文件和访问下载目录。如果你对文件权限不敏感可以这么做，但并不推荐。

### Aria2 配置选项环境变量

  
点击查看

用于设置一些可能需要自定义的 Aria2 配置选项，方便一键部署。

> **TIPS:** 以下环境变量定义后将直接写入配置文件(`aria2.conf`)，通过变量定义后无法通过配置文件修改，因为每次容器重启会自动修正为环境变量定义的值。你也可以选择忽略它们，直接在容器创建后修改配置文件。

*   `-e RPC_SECRET=<TOKEN>` - RPC 密钥设置，即 WebUI 连接时需要填写的密码，只建议使用字母和数字。如果没有设置，配置文件中的默认密码为`P3TERX`。
*   `-e RPC_PORT=6800` - RPC 端口设置。
*   `-e LISTEN_PORT=6888` - BT 监听端口（TCP）、DHT 监听端口（UDP）设置，即 Aria2 配置中`listen-port`与`dht-listen-port`选项定义的端口。如果没有设置，配置文件中的默认值为`6888`。
*   `-e DISK_CACHE=<SIZE>` - 磁盘缓存设置，默认值`64M`。建议在有足够的内存空闲情况下设置为适当增加大小，以减少磁盘 I/O ，提升读写性能，延长硬盘寿命。比如`128M`、`256M`等。此项值仅决定上限，实际对内存的占用取决于网速(带宽)和设备性能等其它因素。当下载文件超过这个大小且网速足够快时最多会占用所设置大小的内存，所以不宜过大，设置不当轻则进程终结、重则宕机。
*   `-e IPV6_MODE=true` - 开启 IPv6 模式。此变量等同于设定配置文件中的选项`disable-ipv6=false`与`enable-dht6=true`。可间接提升 BT 下载速率，但需要网络完整支持 IPv6 ，否则会导致部分功能异常，甚至无法下载。

### 特殊模式环境变量

  
点击查看

*   `-e SPECIAL_MODE=move` - 开启**文件自动归档/分类**功能，即在文件下载完成后把文件移动到指定目录。默认移动到下载目录下的`completed`子目录。有关详情在后面的**进阶玩法**章节。
*   `-e SPECIAL_MODE=rclone` - 开启 **RCLONE 联动**功能。首次启动容器会在容器内自动安装 RCLONE，且每次重启会自动更新。有关详情在后面的**进阶玩法**章节。

### 其它环境变量

  
点击查看

*   `-e UPDATE_TRACKERS=false` - 禁用自动更新 BT tracker 。PT 下载和想手动填写设置 BT tracker 需求必须禁用。
*   `-e CUSTOM_TRACKER_URL=<URL>`：自定义 BT tracker 列表获取链接，多个链接可以用半角逗号(`,`)进行分隔。如果没有指定则默认从`https://trackerslist.com/all_aria2.txt`进行获取。
*   `-e UMASK_SET=022` - umask 设置，默认值`022`。决定你下载下来的文件的权限，对权限不敏感或不理解直接填写`000`。
*   `-e TZ=<TIME_ZONE>` - 时区设置，默认时区为`Asia/Shanghai`，若无特殊需求无需自定义。

### bridge 网络模式

> **TIPS:** ma­cOS 和 Win­dows 必须要使用 bridge 网络模式。

  
点击查看

bridge 网络模式下需要把容器内部的端口映射到宿主机，它是 Docker 默认的网络模式，所以很多 Docker 镜像的默认使用说明都包含端口映射的参数，也包括在 2020 年 3 月 28 日之前的本项目。bridge 网络模式主要是用于网络隔离，对于一般用户几乎无用，而且多了一层 NAT ，会有一定的网络性能损失。如果要使用 IPv6 网络还需要进行一些列复杂的设置。对于全新部署且没有特殊需求不会用到下面这些参数。

*   `-p 6800:6800` - RPC 通讯端口映射。
*   `-p 6888:6888` - BT 监听端口（TCP）映射，即 Aria2 配置中`listen-port`选项定义的端口。
*   `-p 6888:6888/udp` - DHT 监听端口（UDP）映射，即 Aria2 配置中`dht-listen-port`选项定义的端口。

bridge 网络模式启动命令示例：

```
docker run -d \
    --name aria2-pro \
    --restart unless-stopped \
    --log-opt max-size=1m \
    -e PUID=$UID \
    -e PGID=$GID \
    -e RPC_SECRET=<TOKEN> \
    -e RPC_PORT=6800 \
    -p 6800:6800 \
    -e LISTEN_PORT=6888 \
    -p 6888:6888 \
    -p 6888:6888/udp \
    -v ~/aria2-config:/config \
    -v ~/aria2-downloads:/downloads \
    p3terx/aria2-pro
```

> **TIPS:** bridge 网络模式下如果需要自定义端口，建议映射到宿主机相同的端口，避免混淆和功能异常。

注意事项
----

*   作者不会对使用此项目造成的损失承担任何责任，使用前请务必详细阅读整个文档再考虑是否使用。
*   容器启动命令有关路径与端口参数中`:`(冒号)右边的值为容器内部的固定值（常识），不要去修改，否则可能导致无法正常工作。
*   Aria2 配置文件中某些没必要修改的选项参数和已通过环境变量设定的选项参数默认情况下修改无效，重启后会自动修复为正确的值。（为了防止错误修改后导致容器工作异常所做的自我修复功能，比如可以防止把容器内的路径改成容器外的路径之类的迷惑行为）
*   由于 Aria2 暂时没有 UPnP 功能，所以必须配置防火墙开放监听端口，内网设备在路由器设置端口转发到相同端口，这对 BT 下载尤为重要，否则 Aria2 将无法与外界进行数据交换，影响下载率和速度。方法可参考**内网端口转发设置**章节。有关原理参见《[Aria2 无法下载磁力链接、BT种子和速度慢的解决方案](https://p3terx.com/archives/solved-aria2-cant-download-magnetic-link-bt-seed-and-slow-speed.html)》。
*   某些 NAS 系统比如 OpenMediaVault 由于挂载盘默认使用了`noexec`特征，如果配置文件目录映射到了挂载盘下可能会导致附加功能脚本没有执行权限，解决方法可参考《[OpenMediaVault 使用中遇到的问题和解决方案 #1 - permission denied](https://p3terx.com/archives/problems-and-solutions-encountered-in-using-openmediavault-1.html)》。
*   在中国大陆地区使用可能需要处理网络问题。已做针对性优化，但国情都懂的。
*   其它有关使用的注意事项因为精力有限暂未做整理，可查看本博客[其它 Aria2 文章](https://p3terx.com/tag/aria2/)（不完全适用于本项目，仅供参考）。

其它说明
----

### 配置文件与脚本说明

  
点击查看

*   Aria2 Pro 镜像中没有内置配置文件和脚本，而是在容器启动时从 [P3TERX/aria2.conf](https://p3terx.com/go/aHR0cHM6Ly9naXRodWIuY29tL1AzVEVSWC9hcmlhMi5jb25m) 项目进行下载（配置目录没有相应文件的情况下）并进行转换。目的是方便维护与更新，在不更新镜像的情况下可对配置与脚本中的问题进行快速修复。更新最新配置方案或者想恢复初始配置文件只需删除配置文件目录中的所有文件并重启容器即可。
*   默认开启 BT 强制加密，理论上有一定的防版权投诉和防迅雷吸血的效果，在测试使用中没有发现对下载速度有影响，但是否具有普适性还有待观察和研究。如果你在使用中遇到了问题，可以手动修改配置文件去关闭。
*   出于安全性的考虑，配置目录中配置文件的权限为`600`、脚本为`700`，如需编辑需要使用设定的账户（没有设置用户 ID 除外）。
*   配置文件和脚本无法下载一般是容器的网络没有通导致的，这个问题一般出现在 OpenWrt 系统中。

### 高级自定义功能

  
点击查看

容器完成初始化后配置目录下有个 `script.conf` 文件，通过它可以自定义**文件删除功能**、**文件清理功能**、**文件过滤功能**。**文件上传设置**和**文件移动设置**为特殊模式选项，有关详情请参见**进阶玩法**章节。

### 重启防任务丢失

  
点击查看

重启后下载进度不会丢失，不会重复下载，但这个功能的目的是为了防止意外，并不是为了没事瞎重启而做的。目前经过成百上千次测试，有极小概率会导致部分正在进行的任务在重启后会下载失败，这与所下载文件的服务器的某些策略有关，所以不管是重启设备还是 Aria2 Pro 本身都建议先暂停任务再重启。

### 单服务器线程数最大值无上限

  
点击查看

Aria2 官方对单服务器线程数进行限制必然是有他们自己的考虑，但我个人认为自由软件就是要自由，所以解除了这个限制，让所有人可以自由选择。不过无脑的增加线程数并不会让下载速度飞起来，有时会起到反作用，甚至导致无限重启。合理的设置才是正道。

此外 Aria2 Pro 还有特殊的防掉线程优化以及增强配置选项，这是其他项目所不具备的，有关详情配置文件中有注释说明。

### 开启 IPv6 功能

  
点击查看

在开启 IPv6 后 BT 下载有一定的资源搜寻能力提升，能连接到更多的用户，间接可提升 BT 下载速率，这对于没有公网 IPv4 的小伙伴是一个福音。由于需要网络完整支持 IPv6，否则会导致部分功能异常，所以目前默认是关闭的。

*   使用 host 网络模式只需在宿主机 IPv6 网络正常的情况下在容器启动命令中添加`IPV6_MODE=true`环境变量或者后续修改配置文件中有关 IPv6 的选项并重启容器即可。
*   使用 briage 、 macvlan 等网络模式则还需要配置 Docker 使容器获得 全球单拨 IPv6 地址并且容器网络正常。由于配置过程太过复杂，所以一般来说并不推荐。

### 内网端口转发设置

  
点击查看

以下为 Open­Wrt 系统中的端口转发设置示例：

[![](https://imgcdn.p3terx.com/post/20201111034138.png#vwid=1895&vhei=808)
](https://imgcdn.p3terx.com/post/20201111034138.png#vwid=1895&vhei=808)

### AriaNg (WebUI) 使用教程

  
点击查看

《[Aria2 前端面板 (GUI、WebUI) AriaNg 使用教程](https://p3terx.com/archives/aria2-frontend-ariang-tutorial.html)》

### RPC 服务 SSL/TLS 加密

  
点击查看

*   使用 web server 反代 RPC 端口（推荐）。
*   复制证书到配置目录下，修改配置文件中的相关参数。注意路径前缀为容器内的路径，即`/config/`。

进阶玩法
----

Aria2 Pro 具有非常多的隐藏功能与玩法等待你去发觉，比如通过创建多个容器，你甚至可以在同一设备上同时进行 BT 下载、PT 下载、自动上传 OneDrive 、自动上传 Google Drive 等功能，但不仅限于这些。想象力没有上限，需要自己思考。授人以鱼不如授人以渔，所以只写大概思路与示例。

### PT 下载

  
点击查看

> **NOTICE:** 部分 PT 站对 Aria2 有特殊检测机制，可能存在封号风险，使用前需要注意 PT 站的相关规则与条款。

*   PT 下载需要加入`-e UPDATE_TRACKERS=false`参数禁用自动更新 BT tracker ，然后设定 BT 端口为任意五位数端口，比如`25236`。启动命令示例：

```
docker run -d \
    --name aria2-pro \
    --restart unless-stopped \
    --log-opt max-size=1m \
    --network host \
    -e PUID=$UID \
    -e PGID=$GID \
    -e RPC_SECRET=P3TERX \
    -e RPC_PORT=6800 \
    -e LISTEN_PORT=25236 \
    -v ~/aria2-config:/config \
    -v ~/aria2-downloads:/downloads \
    -e UPDATE_TRACKERS=disable \
    p3terx/aria2-pro
```

*   修改配置文件中的客户端伪装设置。

### 同时使用 BT 与 PT

  
点击查看

*   使用 Aria2 Pro 镜像创建名为`aria2-bt`的容器 RPC 端口设置为`6801`，BT 端口设置为`6999`，配置目录设置为`~/aria2-bt-config`，下载目录设置为`~/bt-downloads`。启动命令示例：

```
docker run -d \
    --name aria2-bt \
    --restart unless-stopped \
    --log-opt max-size=1m \
    --network host \
    -e PUID=$UID \
    -e PGID=$GID \
    -e RPC_SECRET=P3TERX \
    -e RPC_PORT=6801 \
    -e LISTEN_PORT=6999 \
    -v ~/aria2-bt-config:/config \
    -v ~/bt-downloads:/downloads \
    p3terx/aria2-pro
```

*   使用 Aria2 Pro 镜像创建名为`aria2-pt`的容器 RPC 端口设置为`6802`，BT 监听端口设置为`25236`，PT 下载不需要设置 DHT 端口所以忽略，配置目录设置为`~/aria2-pt-config`，下载目录设置为`~/pt-downloads`，加入`-e UPDATE_TRACKERS=disable`参数禁用自动获取 BT tracker 。启动命令示例：

```
docker run -d \
    --name aria2-pt \
    --restart unless-stopped \
    --log-opt max-size=1m \
    --network host \
    -e PUID=$UID \
    -e PGID=$GID \
    -e RPC_SECRET=P3TERX \
    -e RPC_PORT=6802 \
    -e LISTEN_PORT=25236 \
    -v ~/aria2-pt-config:/config \
    -v ~/pt-downloads:/downloads \
    -e UPDATE_TRACKERS=disable \
    p3terx/aria2-pro
```

*   修改`~/aria2-pt-config/aria2.conf`中关于 PT 的必要的配置，重启容器。
*   使用 AriaNg 分别通过`6801`和`6802`端口连接到这两个容器中的 Aria2 。
*   Enjoy !

### 联动 RCLONE 自动上传

文件下载到本地后自动调用 RCLONE 上传到指定网盘，本地不保留文件，实现 OneDrive 和 Google Drive 等网盘的伪离线下载。

#### RCLONE 官方版

  
点击查看

*   启动命令加入`-e SPECIAL_MODE=rclone`参数设定特殊模式环境变量后开启 RCLONE 自动上传功能，容器初次启动会安装 RCLONE ，且每次重启会自动更新 RCLONE。启动命令示例：

```
docker run -d \
    --name aria2-pro \
    --restart unless-stopped \
    --log-opt max-size=1m \
    --network host \
    -e PUID=$UID \
    -e PGID=$GID \
    -e RPC_SECRET=P3TERX \
    -e RPC_PORT=6800 \
    -e LISTEN_PORT=6888 \
    -v ~/aria2-config:/config \
    -v ~/rclone-downloads:/downloads \
    -e SPECIAL_MODE=rclone \
    p3terx/aria2-pro
```

*   之前若使用过 RCLONE 直接把配置文件（`rclone.conf`）复制到 Aria2 Pro 配置目录下即可。 RCLONE 配置文件可以在宿主机的默认位置找到：`~/.config/rclone/rclone.conf`
*   初次使用或者想要配置 RCLONE 可使用`docker exec -it aria2-pro rclone config`命令进入容器内的 RCLONE 交互菜单选项，配置方法可参考：《[Rclone 安装配置教程](https://p3terx.com/archives/rclone-installation-and-configuration-tutorial.html)》。
*   最后根据实际情况修改 Aria2 Pro 配置文件目录下`script.conf`文件中的网盘名称(`drive-name`)和网盘路径(`drive-dir`)这两个选项的值。

#### RCLONE 魔改版/增强版

  
点击查看

有些小伙伴可能有特殊需求，比如使用世纪互联的 OneDrive ，或者想突破 Google Drive 的 750G 上传限制，这就需要用到相应的 RCLONE 魔改版，比如 gclone、fclone 等。这些魔改版由于没有完善安装升级方式和技术支持，所以并不好做整合，但可以使用一些特殊的方法让 Aria2 Pro 支持。

简单来讲就是在宿主机下载或安装相应的魔改版，把二进制文件的路径映射到容器的 `/usr/local/bin/rclone`。

这里以 gclone 为例子，安装后其二进制文件路径在 `/usr/bin/gclone`，把它映射到容器 `/usr/local/bin/rclone`。假设 ser­vice ac­count 文件保存目录在 `~/service-account`，那么把它映射到容器中的 `/service-account` 目录。启动命令示例：

```
docker run -d \
    --name aria2-pro \
    --restart unless-stopped \
    --log-opt max-size=1m \
    --network host \
    -e PUID=$UID \
    -e PGID=$GID \
    -e RPC_SECRET=P3TERX \
    -e RPC_PORT=6800 \
    -e LISTEN_PORT=6888 \
    -v ~/aria2-config:/config \
    -v ~/rclone-downloads:/downloads \
    -v /usr/bin/gclone:/usr/local/bin/rclone \
    -v ~/service-account:/service-account \
    -e SPECIAL_MODE=rclone \
    p3terx/aria2-pro
```

其他操作和官方版一致，详见上一章节，这里不再赘述。需要注意的是配置文件可能需要做相应的调整，比如 gclone 的 ser­vice ac­count 文件路径。

#### RCLONE 上传到不同的网盘

使用不同的容器进行下载并通过 RCLONE 自动上传到不同的网盘。

  
点击查看

*   使用 Aria2 Pro 镜像创建名为`aria2-onedrive`的容器 RPC 端口设置为`6803`，BT 端口设置`33333`，配置目录为`~/aria2-onedrive-config`，下载目录为`~/onedrive-downloads`。启动命令示例：

```
docker run -d \
    --name aria2-onedrive \
    --restart unless-stopped \
    --log-opt max-size=1m \
    --network host \
    -e PUID=$UID \
    -e PGID=$GID \
    -e RPC_SECRET=P3TERX \
    -e RPC_PORT=6803 \
    -e LISTEN_PORT=33333 \
    -v ~/aria2-onedrive-config:/config \
    -v ~/onedrive-downloads:/downloads \
    -e SPECIAL_MODE=rclone \
    p3terx/aria2-pro
```

*   使用 Aria2 Pro 镜像创建名为`aria2-gdrive`的容器 RPC 端口设置为`6804`，BT 端口设置`44444`，配置目录为`~/aria2-gdrive-config`，下载目录为`~/gdrive-downloads`。启动命令示例：

```
docker run -d \
    --name aria2-gdrive \
    --restart unless-stopped \
    --log-opt max-size=1m \
    --network host \
    -e PUID=$UID \
    -e PGID=$GID \
    -e RPC_SECRET=P3TERX \
    -e RPC_PORT=6804 \
    -e LISTEN_PORT=44444 \
    -v ~/aria2-gdrive-config:/config \
    -v ~/gdrive-downloads:/downloads \
    -e SPECIAL_MODE=rclone \
    p3terx/aria2-pro
```

*   参考上个章节分别配置这两个容器的 RCLONE 。
*   使用 AriaNg 分别通过`6803`和`6804`端口连接到这两个容器中的 Aria2 。
*   Enjoy !

### 文件自动归档/分类

下载完成后自动移动文件到指定目录。这个功能的主要目的是区分已下载完成的文件。也可以通过自定义临时下载目录把它当做一个简单的文件分类功能。如果你在 NAS 中使用 Jel­lyfin 、Emby 之类的影音媒体服务器软件，可以实现下载完成后自动移入媒体库目录后自动加载。

  
点击查看

启动命令中增加 `-e SPECIAL_MODE=move` 参数添加环境变量即可，默认情况下文件下载完成后将移动到下载目录下的 `completed` 子目录。通过 We­bUI （或其它 RPC 方式）指定临时下载目录为 `/downloads/movie` 则下载完成后文件会移动到下载目录下的 `completed/movie` 子目录中，指定临时下载目录为 `/downloads/tv` 则下载完成后文件会移动到下载目录下的 `completed/tv` 子目录中，依次类推，实现自动分类。

> **TIPS:** 这里的`movie`、`tv`子目录只是举例，并不是固定的，可以根据自己的喜好和实际情况来填写。

若要移动到下载目录以外的某个目录，除了添加必须的环境变量以外，还需要额外对一个目录进行映射，但不能是下载目录下的子目录（目录映射套娃的后果自行体会）。考虑到此目录可能用于共享，故容器不会对这个目录进行权限更改，宿主机端的目录需提前创建并设置好相应的权限。比如将容器内的 `/completed` 映射到宿主机的 `~/aria2-completed` 目录：

```
docker run -d \
    --name aria2-pro \
    --restart unless-stopped \
    --network host \
    --log-opt max-size=1m \
    -e PUID=$UID \
    -e PGID=$GID \
    -e RPC_SECRET=P3TERX \
    -e SPECIAL_MODE=move \
    -v ~/aria2-config:/config \
    -v ~/aria2-downloads:/downloads \
    -v ~/aria2-completed:/completed \
    p3terx/aria2-pro
```

最后还需要编辑配置目录下的 `script.conf` 文件，将 `dest-dir` 的值设置为容器内的路径，即上面例子中的 `/completed`。

> **TIPS:** 由于容器技术的局限性，不同的映射目录被定义为不同的磁盘，即使它们在宿主机中处于同一个磁盘上，在移动文件时也将会以全新写入的方式进行。就像是在 PC 上同一硬盘不同分区之间的文件移动。

遇到问题如何处理
--------

很多问题的产生都是没仔细阅读文档和基础知识薄弱所导致的，认真学习基础知识，仔细阅读文档中的每一个字，你会发现答案就在其中。

以下是一些基本的排错流程，视实际情况而定，**不要无脑复制粘贴**。

### 重启

Aria2 Pro 具有自我修复机制，遇到问题首先重启。如果你修改过配置文件和附加功能脚本，先删除后重启。

```
docker restart aria2-pro
```

### 重装

```
docker rm -f aria2-pro
docker rmi p3terx/aria2-pro
rm -rf ~/aria2-config
docker pull p3terx/aria2-pro
docker run <...>
```

### 更新

也许你的问题在最新的版本中已经得到解决，多多关注[项目页面](https://p3terx.com/go/aHR0cHM6Ly9naXRodWIuY29tL1AzVEVSWC9kb2NrZXItYXJpYTItcHJv)动态。一些重要更新会在 [Aria2 Channel](https://p3terx.com/go/aHR0cHM6Ly90Lm1lL0FyaWEyX0NoYW5uZWw) 推送。

以下是使用 [Watchtower](https://p3terx.com/archives/docker-watchtower.html) 一键更新 Aria2 Pro 镜像与容器的命令示例：

```
docker run --rm \
    -v /var/run/docker.sock:/var/run/docker.sock \
    containrrr/watchtower -cR \
    aria2-pro
```

### 查看日志

查看日志才能更好的找到问题的根本，即使你看不懂，也要学会如何查看。

*   查看实时日志

```
docker logs -f --tail 30 aria2-pro
```

*   导出日志

```
docker logs aria2-pro > ~/aria2-pro.log
```

### 提问

加入 [Aria2 TG 群](https://p3terx.com/go/aHR0cHM6Ly90Lm1lL2FyaWEyYw)和小伙伴们一起讨论。但要注意提问的方式和提供有用的信息，提问前请确定已经阅读过《[提问的智慧](https://p3terx.com/go/aHR0cHM6Ly9naXRodWIuY29tL3J5YW5oYW53dS9Ib3ctVG8tQXNrLVF1ZXN0aW9ucy1UaGUtU21hcnQtV2F5L2Jsb2IvbWFzdGVyL1JFQURNRS16aF9DTi5tZA)》，这能更好的帮助你去解决问题和节约时间。诸如 “为什么不能使用？”、“那你能帮帮我吗？” 之类的问题应该没有人会知道。另外不要把热心网友当做淘宝客服，没人会陪你打太极，帮你是缘分，不帮是本分。

### 提交 BUG

提交 BUG 请前往项目 [issues](https://p3terx.com/go/aHR0cHM6Ly9naXRodWIuY29tL1AzVEVSWC9kb2NrZXItYXJpYTItcHJvL2lzc3Vlcw) 页面，但请注意您需要先知道什么是 BUG 。

* * *

[更多 Aria2 教程](https://p3terx.com/tag/aria2/)

相关 TG 群组：[Aria2 Group](https://p3terx.com/go/aHR0cHM6Ly90Lm1lL2FyaWEyYw)

重要更新推送频道：[Aria2 Channel](https://p3terx.com/go/aHR0cHM6Ly90Lm1lL0FyaWEyX0NoYW5uZWw)

* * *

本博客已开设 [Telegram 频道](https://p3terx.com/go/aHR0cHM6Ly90Lm1lL1AzVEVSWF9aT05F)，欢迎小伙伴们订阅关注。

> **本文作者：** [P3TERX](https://p3terx.com/)
> 
> **本文链接：** [https://p3terx.com/archives/docker-aria2-pro.html](https://p3terx.com/archives/docker-aria2-pro.html)
> 
> **版权声明：** 本博客所有文章除特别声明外，均采用 [CC BY-NC-SA 4.0](https://p3terx.com/go/aHR0cHM6Ly9jcmVhdGl2ZWNvbW1vbnMub3JnL2xpY2Vuc2VzL2J5LW5jLXNhLzQuMC9kZWVkLnpo) 许可协议。非商业转载及引用请注明出处（作者、原文链接），商业转载请联系作者获得授权。