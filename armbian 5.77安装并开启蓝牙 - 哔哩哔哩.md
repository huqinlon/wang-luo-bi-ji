# armbian 5.77安装并开启蓝牙 - 哔哩哔哩
最近买了个n1，想给他配一个蓝牙键盘，就折腾了一下

笔者使用的系统是armbian 5.77 （debian）。

**首先前提是你刷好了系统**  
**使用ssh连接入终端，这里推荐使用xshell，很好用**

![](https://i0.hdslb.com/bfs/article/4adb9255ada5b97061e610b682b8636764fe50ed.png)

首先开启config

`sudo armbian-config`

进去之后选择Network，再点击BT Install，一路y下去，等待蓝牙组件完成安装，然后退出。（按tab键选择back/Exit）

![](https://i0.hdslb.com/bfs/article/05d0cb041527fd820aa4b17cc073d33b5c6f0041.jpg@942w_1076h_progressive.webp)

**安装完成后应该是这种样子**

![](https://i0.hdslb.com/bfs/article/c15114a0d24b85a5589c0bf1d23bf7023806a44a.jpg@942w_1080h_progressive.webp)

之后执行该命令来进入蓝牙管理页面

sudo hciconfig -a

之后root应该被替换成了蓝牙控制器，再执行

`sudo bluetoothctl`

之后分别执行

`power on discoverable on agent on`

然后扫描蓝牙设备

`scan on`

记住你想连接设备的mac，可以提前用手机之类的连接后记录下来

`trust`

信任该蓝牙设备，然后再执行

`pair`

之后就大功告成了

若要退出蓝牙控制面板，可以直接输入

`quit`

![](https://i0.hdslb.com/bfs/article/4adb9255ada5b97061e610b682b8636764fe50ed.png)

这里是solaking的博客转b站投稿

如果想看更多内容欢迎访问我的博客

索拉金SolaKing.com