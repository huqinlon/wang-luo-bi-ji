# OpenMediaVault (OMV) 切换国内源 - 知乎
### 分类

在安装OMV-Extras后，OpenMediaVault的源共分为六部分： - Debian 软件源：`/etc/apt/sources.list` - openmediavault 软件源：`/etc/apt/sources.list.d/openmediavault.list` - kernel-backports 软件源：`/etc/apt/sources.list.d/openmediavault-kernel-backports.list` - 本地缓存：`/etc/apt/sources.list.d/openmediavault-local.list` - 系统安全更新：`/etc/apt/sources.list.d/openmediavault-os-security.list` - OMV-Extras 扩展源：`/etc/apt/sources.list.d/omvextras.list` 以下对上述源一一说明如何修改为国内源以加速。

### 修改方法

1.  使用WinSCP工具进入上述目录打开相应文件进行编辑。
2.  使用Puttty、Xshell、MobaXterm等ssh工具登陆后，使用nano、vi等Debian自带软件在命令行中修改，如修改Debian软件源：

```text
nano /etc/apt/sources.list
```

1.  使用Puttty、Xshell、MobaXterm等ssh工具登陆后，如将所有Debian官方地址（`httpredir.debian.org`）替换为中科大镜像站地址（`mirrors.ustc.edu.cn`），则可在命令行中输入：

```text
sudo sed -i 's/httpredir.debian.org/mirrors.ustc.edu.cn/g' /etc/apt/sources.list
```

### 修改内容

以下均以`openmediavault usul 5.x`为例，`openmediavault arrakis 4.x`请自行根据情况修改。

### Debian 软件源

Debian的国内镜像站非常多，以替换为清华开源镜像站为例，内容如下：

```text
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ buster main contrib non-free
# deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ buster main contrib non-free

deb https://mirrors.tuna.tsinghua.edu.cn/debian/ buster-updates main contrib non-free
# deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ buster-updates main contrib non-free
```

_注：如需下载源码，自行解除_`_deb-src_`_行的注释_

### openmediavault 软件源

我在4月30日向清华开源镜像站（TUNA）提交了[openmediavault的同步请求](https://link.zhihu.com/?target=https%3A//github.com/tuna/issues/issues/810)，然后发现TUNA维护人员已经于4月30日当天上午开始同步openmediavault了（见[#629](https://link.zhihu.com/?target=https%3A//github.com/tuna/issues/issues/629)），你说巧不巧，我真是大写的尴尬。不过发现还没有同步OMV-Extras扩展源，所以也一并请求了，维护人员已经同意，并由TUNA维护在了[北外开源镜像站](https://link.zhihu.com/?target=https%3A//mirrors.bfsu.edu.cn/OpenMediaVault/)里面。

请自行备份`/etc/apt/sources.list.d/openmediavault.list`，或将原有内容每行前添加“#”注释后，添加以下内容：

```text
deb https://mirrors.bfsu.edu.cn/OpenMediaVault/public/ usul main
deb https://mirrors.bfsu.edu.cn/OpenMediaVault/public/ usul-proposed main
deb https://mirrors.bfsu.edu.cn/OpenMediaVault/public/ usul partner
```

### kernel-backports 软件源

其实这个和Debian软件源一样，以替换为清华开源镜像站为例，内容如下：

```text
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ buster-backports main contrib non-free
```

### 本地缓存

无需修改。

### 系统安全更新

其实这个也和Debian软件源一样，以替换为清华开源镜像站为例，内容如下：

```text
deb https://mirrors.tuna.tsinghua.edu.cn/debian-security buster/updates main contrib non-free
# deb-src https://mirrors.tuna.tsinghua.edu.cn/debian-security buster/updates main contrib non-free
```

_注：如需下载源码，自行解除_`_deb-src_`_行的注释_

### OMV-Extras 扩展源

经过请求，已由TUNA维护在[北外开源镜像站](https://link.zhihu.com/?target=https%3A//mirrors.bfsu.edu.cn/OpenMediaVault/)中。 请自行备份`/etc/apt/sources.list.d/omvextras.list`，或将原有内容每行前添加“#”注释后，添加以下内容：

```text
deb https://mirrors.bfsu.edu.cn/OpenMediaVault/openmediavault-plugin-developers/usul buster main
deb [arch=amd64] https://download.docker.com/linux/debian buster stable
deb http://linux.teamviewer.com/deb stable main
deb https://mirrors.bfsu.edu.cn/OpenMediaVault/openmediavault-plugin-developers/usul-testing buster main
deb https://mirrors.bfsu.edu.cn/OpenMediaVault/openmediavault-plugin-developers/usul-extras buster main
```

_注1：我仔细观察了北外开源镜像站的同步文件结构，似乎对_`_openmediavault 4.x_`_的_`_OMV-Extras_`_同步不完整，因此不建议_`_openmediavault arrakis 4.x_`_使用。_

_注2：其中第二行的_`_docker-ce_`_也可以切换为国内源，详见各开源镜像站帮助文档：_[清华](https://link.zhihu.com/?target=https%3A//mirrors.tuna.tsinghua.edu.cn/help/docker-ce/)_、_[中科大](https://link.zhihu.com/?target=http%3A//mirrors.ustc.edu.cn/help/docker-ce.html)_、_[华为云](https://link.zhihu.com/?target=https%3A//mirrors.huaweicloud.com/)_、_[阿里云](https://link.zhihu.com/?target=https%3A//developer.aliyun.com/mirror/docker-ce)_、_[腾讯云](https://link.zhihu.com/?target=https%3A//mirrors.cloud.tencent.com/help/docker-ce.html)_。_