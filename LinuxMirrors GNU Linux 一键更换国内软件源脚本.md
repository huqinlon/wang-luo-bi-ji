# LinuxMirrors: GNU/Linux 一键更换国内软件源脚本
[![](https://gitee.com/SuperManito/LinuxMirrors/raw/main/docs/img/icon/github-1.svg)
](https://gitee.com/link?target=https%3A%2F%2Fgithub.com%2FSuperManito%2FLinuxMirrors)  [![](https://gitee.com/SuperManito/LinuxMirrors/raw/main/docs/img/icon/github-2.svg)
](https://gitee.com/link?target=https%3A%2F%2Fgithub.com%2FSuperManito%2FLinuxMirrors)    [![](https://gitee.com/SuperManito/LinuxMirrors/raw/main/docs/img/icon/gitee.svg)
](https://gitee.com/SuperManito/LinuxMirrors)

*   **`GNU/Linux` 一键更换国内软件源脚本**
*   **本项目旨在为从事计算机相关行业的朋友们提供便利**
*   **理论支持所有架构的环境，ARM 环境已经过测试**

* * *

* * *

*   ### [](#%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%95)使用方法
    
    bash <(curl \-sSL https://gitee.com/SuperManito/LinuxMirrors/raw/main/ChangeMirrors.sh)
    
    *   完整复制上面的命令到终端按回车键即可执行，若无法安装 `curl` 软件包可复制源码到本地后手动执行
    *   为了适配所有环境，建议使用 `Root` 用户执行脚本，切换命令为 `sudo -i` ，如遇报错请查看常见问题与帮助
    *   如果您使用的环境没有安装或不支持简体中文环境，请通过 `SSH客户端工具` 使用，否则将无法正确选择交互内容
    *   执行脚本过程中会自动备份原有源无需手动备份，期间会在终端输出多个主观选择交互内容，可按回车键快速确定
    *   脚本支持在原有源配置错误或者不存在的情况下使用，并且可以重复使用；脚本变更的软件源默认使用 `HTTP` 协议
    
    > **未启用的源：**   
    > _Debian 系 Linux 默认禁用了**源码仓库**和**预发布软件源**，若需启用请将 `/etc/apt/sources.list` 文件中相关内容的所在行**取消注释**_  
    > _RedHat 系 Linux 部分仓库**默认没有启用**，若需启用请将 `/etc/yum.repos.d` 目录下相关 **repo** 文件中的 `enabled` 值修改为 `1`_
    
    #### [](#%E8%84%9A%E6%9C%AC%E6%89%A7%E8%A1%8C%E6%B5%81%E7%A8%8B)脚本执行流程
    
    *   └ 选择国内源 `交互`
        *   └ 检测如果是 RHEL或CentOS 系统选择是否安装/覆盖 EPEL 扩展国内源 `交互`
    *   └ 选择软件源使用的 WEB 协议 `交互`
    *   └ 检测 防火墙 和 SELINUX 如果开启并且系统是 RHEL或CentOS 选择是否关闭 `交互`
    *   └ 备份原有源
        *   └ 检测如果存在重复的备份文件选择是否覆盖 `交互`
    *   └ 更换国内源
    *   └ 选择是否更新软件包 `交互`
        *   └ 选择是否清理已下载的软件包缓存 `交互`

* * *

*   ### [](#%E5%85%B6%E5%AE%83%E8%84%9A%E6%9C%AC)其它脚本
    
    *   `Docker` 一键安装脚本
        
        bash <(curl \-sSL https://gitee.com/SuperManito/LinuxMirrors/raw/main/DockerInstallation.sh)
        
        > `Docker CE`：Docker Community Edition 镜像仓库，用于下载并安装 Docker 相关软件包  
        > `Docker Hub`：Docker Hub 镜像仓库，默认为官方提供的公共库，用于切换下载镜像时的来源仓库，又称镜像加速器
        
        > **注意：**   
        > 脚本集成安装 `Docker Engine` 与 `Docker Compose`，可手动选择安装版本和下载源，还可手动选择镜像加速器，支持国内外服务器环境和 `ARM`架构处理器环境使用。
        

* * *

*   ### [](#%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98%E4%B8%8E%E5%B8%AE%E5%8A%A9)常见问题与帮助
    
    *   如果提示 `Command 'curl' not found` 则说明当前未安装 `curl` 软件包
        
        sudo yum install \-y curl || sudo apt-get install \-y curl
        
    *   如果提示 `Command 'wget' not found` 则说明当前未安装 `wget` 软件包
        
        sudo yum install \-y wget || sudo apt-get install \-y wget
        
    *   如果提示 `bash: /proc/self/fd/11: No such file or directory`，请切换至 `Root` 用户执行
        

* * *

### [](#license)LICENSE

Copyright © 2022, [SuperManito](https://gitee.com/link?target=https%3A%2F%2Fgithub.com%2FSuperManito). Released under the [GPL-2.0](https://gitee.com/link?target=https%3A%2F%2Fgithub.com%2FSuperManito%2FLinuxMirrors%2Fblob%2Fmain%2FLICENSE).

> 项目已设立开源许可协议，传播时需在显著位置标注来源和作者，请尊重作者的知识成果  
> 建议通过命令直接调用脚本，如有意见与建议您可以提交至 [Issues](https://gitee.com/link?target=https%3A%2F%2Fgithub.com%2FSuperManito%2FLinuxMirrors%2Fissues)

**如果您觉得这个项目不错对您有所帮助的话，方便在右上角给颗 ⭐ 并分享给更多的朋友吗？**