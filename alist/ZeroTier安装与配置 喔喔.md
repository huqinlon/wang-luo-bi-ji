# ZeroTier安装与配置 | 喔喔
2022-05-27 Updated:2022-05-28

### [](#ZeroTier安装与配置 "ZeroTier安装与配置")ZeroTier安装与配置

#### [](#前言 "前言")前言

家里和公司总会有很多设备需要管理，虽然可以通过类似向日葵、`TeamViewer`等进行远程控制，但是总是不方便；  
如果是安装VPN的方式，所有的流量都走VPN，内网的设备互联比较慢；  
需要一种能将所有设备通过虚拟内网的方式连接起来，  
同时，如果设备自身在同一个内网，则直连，  
如果不在同一个内网，则通过转发的形式连接；  
另外，最好不需要中心服务器；通过P2P方式进行连接；  
[ZeroTier](https://www.zerotier.com/)刚好能满足这类需求；

#### [](#开始 "开始")开始

##### [](#1-注册账号 "1. 注册账号")1\. 注册账号

访问：[https://www.zerotier.com/](https://www.zerotier.com/) 注册账号；

##### [](#2-创建虚拟网络 "2. 创建虚拟网络")2\. 创建虚拟网络

1.  点击“Create A Network”创建一个虚拟网络：  
    ![](https://makefile.so/images/install-zerotier/zerotier-1.png)
    
2.  *   记住Network ID，后边会用到；
    *   Name随便写；
    *   Access Control选择：PRIVATE；  
        ![](https://makefile.so/images/install-zerotier/zerotier-2.png)
        
3.  配置网段：
    
    *   IPV4 Auto-Assign 选择“Easy”，然后从下发的网段选择一个即可；
    *   这里选择的是10.144.*.*，上方的Managed Routes会自动修改为：10.144.0.0/16 (LAN);
    *   表示：10.144.*.*的虚拟网段，由ZeroTier进行路由转发；  
        ![](https://makefile.so/images/install-zerotier/zerotier-3.png)
        
4.  剩下部分保留默认配置即可；接下来配置客户端。
    

##### [](#3-客户端配置 "3. 客户端配置")3\. 客户端配置

1.  下载并安装ZeroTier客户端：[https://www.zerotier.com/download/](https://www.zerotier.com/download/)
2.  按照官网提示，安装ZeroTier客户端即可；
3.  配置客户端，加入虚拟网络：```null
     
     zerotier-cli join <Network ID>
    
    ```
    
    MacOS和Windows系统，打开客户端，粘贴Network ID，然后点击“Join Network”：  
    ![](https://makefile.so/images/install-zerotier/zerotier-4.png)
    
4.  回到管理UI，找到刚刚申请加入的设备，点击左侧的“Auth?”复选框，给新加入的设备授权：
    *   “Name”随便写；
    *   “Managed IPs”，输入“10.144.0.0”至“10.144.255.255”之间的任意IP，网段对应上一步创建的网段即可；
    *   点击“Managed IPs”左侧的“+”，生成虚拟IP；  
        ![](https://makefile.so/images/install-zerotier/zerotier-5.png)
        
5.  客户端配置完成；可以回到客户端，查看是否正常：  
    ![](https://makefile.so/images/install-zerotier/zerotier-6.png)
    
6.  将多个设备加入虚拟网络；如果是云服务器，记得开放相应的端口；
7.  多个设备之间，可以使用虚拟IP互联；

#### [](#moon节点配置 "moon节点配置")moon节点配置

官方的节点，速度比较慢；可以通过自建中继节点（moon）的方式，提高连接速度；  
**_自建的moon节点服务器必须有外网IP，才有意义；_**

1.  在Linux的服务器上，配置moon服务器：```null
     cd /var/lib/zerotier-one
     
     zerotier-idtool initmoon identity.public >>moon.json
    
    ```
    
    生成的`moon.json`内容如下，记住第一行id的值，后边会用到（这里假设为12345678）：```null
     {
       "id": "12345678",
       "objtype": "world",
       "roots": [
         {
           "identity": "xxxxxxxx",
           "stableEndpoints": []
         }
       ],
       "signingKey": "xxxxxxxx",
       "signingKey_SECRET": "xxxxxxxx",
       "updatesMustBeSignedBy": "xxxxxxxx",
       "worldType": "moon"
     }
    
    ```
    
2.  将`stableEndpoints`的值，修改为moon服务器外网IP和端口号（这里假设服务器外网IP为：1.2.3.4），如：```null
     {
       "id": "12345678",
       "objtype": "world",
       "roots": [
         {
           "identity": "xxxxxxxx",
           "stableEndpoints": ["1.2.3.4/9993"]
         }
       ],
       "signingKey": "xxxxxxxx",
       "signingKey_SECRET": "xxxxxxxx",
       "updatesMustBeSignedBy": "xxxxxxxx",
       "worldType": "moon"
     }
    
    ```
    
3.  启动moon服务器：```null
     zerotier-idtool genmoon moon.json
     
     mkdir moons.d
     cp 000000xxxxxxxx.moon /moons.d/
     systemctl restart zerotier-one.service
    
    ```
    
4.  其他客户端连接moon服务器：```null
     
     
     
     
     zerotier-cli orbit 12345678 12345678
    
    ```
    
5.  如果是Windows系统：```null
     C:\Program Files (x86)\ZeroTier\One> .\zerotier-cli.bat orbit 12345678 12345678
    
    ```
    
6.  查看节点连接信息：```null
     zerotier-cli listpeers
     
     
    
    ```
    

#### [](#其他 "其他")其他

ZeroTier还有路由功能、DNS功能、转发规则等，不过一般用不到，可以参考网上的教程。

类似的虚拟内网工具，还有：`NetMaker`、`WireGuard`、`Tailscale`、`Nebula`等；不过最简单方便的还是`ZeroTier`了。

#### [](#参考链接 "参考链接")参考链接

1.  ZeroTier官网：[https://www.zerotier.com/](https://www.zerotier.com/)
2.  加入网络：[https://zerotier.atlassian.net/wiki/spaces/SD/pages/6848513/Join+a+Network](https://zerotier.atlassian.net/wiki/spaces/SD/pages/6848513/Join+a+Network)