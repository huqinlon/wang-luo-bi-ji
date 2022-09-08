# N1盒子armbian蓝牙连接详细步骤_weixin_39658178的博客-CSDN博客_n1 蓝牙
论坛里没有搜到特别详细的N1盒子连接蓝牙的教程，一些帖子提到但是要么不详细，要么对照操作无法成功，昨晚在刷了xiangsm提供的最新Armbian\_5.77系统后，听说蓝牙驱动问题已经解决，于是决定尝试连接蓝牙，中间遇到一些坑，特地记录一下，供各位参考。

1.  首先下载Armbian-5.77镜像包，因为做服务器用，所以我选的是[debian](https://so.csdn.net/so/search?q=debian&spm=1001.2101.3001.7020)无桌面版。https://www.right.com.cn/forum/thread-510423-1-1.html
    
2.  下载完成后，将镜像写入U盘，写盘工具很多，个人喜欢用balenaEtcher，简单方便而且镜像不用解压可以直接写盘。
    
3.  U盘写结束后，将xiangsm编译好的meson-gxl-s905d-phicomm-n1-xiangsm.dtb拷贝进U盘的dtb目录，然后修改uEnv.ini文件，将 dtb\_name=/dtb/meson-gxl-s905x-\*\*\*\*\*.dtb这行修改为 dtb\_name=/dtb/meson-gxl-s905d-phicomm-n1-xiangsm.dtb
    

另外，为了方便ssh连接，在U盘根目录创建新文件ssh（一定注意不要带任何扩展名），如果你安装的是desktop版，不打算使用ssh，或者U盘根目录已经有这个文件的话，这一步可以省略。

4.  把U盘插入N1盒子，插上网线和电源，开机，等待几分钟系统启动。
    
5.  系统启动后，找到N1的IP地址，打开putty连接ssh
    
6.  ssh进系统后，根据系统提示修改root密码，创建一个新用户。
    
7.  运行sudo armbian-config，设置时区，配置连接wifi网络。完成后退出armbian-config，然后按照MaxGo的教程修改apt源为国内源，https://www.right.com.cn/forum/thread-342558-1-1.html 修改国内源完成后，分别执行sudo update和sudo upgrade更新系统。更新完毕后sudo reboot重启。
    
8.  重启后，继续ssh连接，然后执行sudo armbian-config，进去后选择Network，接着选择BT Install，耐心等待蓝牙组件安装完毕，然后退出。
    
9.  接着执行sudo apt install pulseaudio-module-bluetooth 安装pulseaudio组件。安装完成后，分别执行sudo killall pulseaudio和pulseaudio --start启动pulseaudio服务。
    
10.  开始进入蓝牙连接阶段，首先执行sudo hciconfig -a查看蓝牙控制器信息，确认无误后，执行sudo hciconfig hci0 up打开蓝牙控制器，然后执行sudo bluetoothctl打开蓝牙管理器。
    
11.  先后执行power on，discoverable on，agent on，然后执行scan on搜集周围的蓝牙设备，记录下要连接的设备地址后，执行trust <设备地址>信任设备，然后再执行pair <设备地址>配对，此时，要配对的设备上可能会弹出提示，点确认。
    
12.  如以上步骤都没有问题，则执行connect <设备地址>，稍候即可顺利连接蓝牙，可以运行info <设备地址>确认状态。
    

至此，N1盒子蓝牙连接完毕。