# 【授人鱼不如授人以渔】史上最简单的黑群晖DSM7.X引导编译方法，小学生都能操作！（黑群晖DSM7.X引导用arpl编译教程） - GXNAS博客
       去年写过一篇黑群晖DSM7.X引导编译教程（[教程链接](https://wp.gxnas.com/11358.html)），只不过要求需要有一定的动手能力才能编译成功，因此难倒不少人。上个月，一位巴西人在github上分享的源代码（github地址是https://github.com/fbelavenuto/arpl），让黑群晖DSM7.X引导的编译变得非常简单，一点都不夸张的说：简单到连小学生都能操作！感谢这位巴西的大佬！

* * *

【编译要求】

        **掌握计算机基本操作，有耐心。** 

有同学举手说：“我看不懂英语。”

那么你会百度吗？

会！

好的，百度搜索栏输入“翻译”回车，会吗？

会！

Very Good！我们继续。。。。。。

* * *

【编译前的准备工作】

        由于需要在NAS的机器上进行引导的编译，请事先准一下：

※ 如果你决定使用物理机安装群晖系统的，那么需要把机器装好，包括键盘、鼠标、显示器、硬盘、网线等等，如果还有其他外设（比如：额外添加的网卡、扩展卡、阵列卡等）要装起来，让所有的硬件处于可以正常工作的状态，编译系统会自动检测你使用的硬件并且自动加载驱动进行编译；

※ 如果你决定使用虚拟机安装群晖系统的，那么需要配置好虚拟机，包括设置CPU、内存、存储大小等等，以及有直通硬盘、直通核显、直通网卡、直通扩展卡、直通阵列卡等外设的，全部设置好，编译系统会自动检测虚拟机的硬件信息并且自动加载驱动进行编译；

※ 如果你有科学出国的环境能正常访问github网站和google网站的，那是最好的，可以减少编译等待的时间。要是没有也可以编译，只需要耐心等就是了。

* * *

【编译步骤】

1、到【[github](https://github.com/fbelavenuto/arpl/releases)】把编译引导需要用的文件下载到电脑上（不是在NAS这台机器）。截止2022年8月13日，github上最新的版本是v0.4-alpha2（如果将来作者更新，可以下载最新的版本），我下载的img文件，这个格式是通用的，物理机可以用，虚拟机也可以用。

[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1660322158-1.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1660322158-1.jpg)

2、下载后的文件名是arpl-0.4-alpha2.img.zip，这个是一个压缩包。[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1660322162-2.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1660322162-2.jpg)

3、利用电脑的解压软件，把arpl-0.4-alpha2.img.zip解压出来，得到另外一个文件arpl.img。[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1660322169-3.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1660322169-3.jpg)

4、删除arpl-0.4-alpha2.img.zip，只留下arpl.img即可。[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1660322169-4.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1660322169-4.jpg)

5、如果你是用物理机安装的，可以使用rufus写盘工具把arpl.img刷到U盘。如果是PVE虚拟机安装群晖的，可以上传arpl.img到PVE，用qm importdisk命令转换成群晖虚拟机的虚拟引导文件。如果是用ESXI或者VMware安装的群晖虚拟机，可以使用StarWind V2V Image Converter工具来转换格式。我是用ESXI虚拟机安装的，所以把arpl.img转成了arpl.vmdk和arpl-flat.vmdk。[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1660322169-5.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1660322169-5.jpg)

6、把arpl.vmdk和arpl-flat.vmdk两个文件上传到ESXI，设置为群晖虚拟机的引导。[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1660322171-6.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1660322171-6.jpg)

7、我的群晖虚拟机配置很简单，你们不用照抄我的，请根据自己实际使用环境配置即可。[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1660322172-7.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1660322172-7.jpg)

8、虚拟机配置好了就打开虚拟机的电源。物理机安装的话，把刷好的U盘放到NAS主机上，开机进BIOS设置从U盘启动。编译系统启动后会显示以下的界面，直接按回车进入。[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1660322173-8.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1660322173-8.jpg)

9、编译系统启动中，如果你的路由器已经开启DHCP的话，此时系统会自动去获取IP地址，请耐心等待。[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1660322174-9.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1660322174-9.jpg)

10、当编译系统最下面一行显示有“root@”开头的时候，就表示已经启动好了，需要找出编译系统的IP地址。[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1660322175-10.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1660322175-10.jpg)

11、在局域网另外一台电脑的浏览器（建议使用谷歌浏览器），打开编译系统显示的IP地址和端口，会显示以下界面。[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1660322177-11.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1660322177-11.jpg)

12、在第一行“Choose a model”回车。[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1660322177-12.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1660322177-12.jpg)

13、这时会显示出本机可编译黑群晖的型号，如果你的CPU比较老的话，有可能不会显示“DS918+”这个型号。[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1660322178-13.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1660322178-13.jpg)

14、选择你想要编译的黑群晖型号，我这选择的是DS918+，用方向键选好以后按回车键。[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1660322179-14.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1660322179-14.jpg)

15、在“Choose a Build Number”处回车。[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1660322180-15.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1660322180-15.jpg)

16、选择你想要编译黑群晖的版本，我选择的是最新的7.1.1-42951版本，用方向键选择以后按回车键。[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1660322181-16.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1660322181-16.jpg)

17、在“Choose a serial number”处回车。[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1660322183-17.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1660322183-17.jpg)

18、选择“Generate a random serial number”回车的话，编译系统会随机生成一个序列号。如果你想使用自定义的序列号，可以选择“Enter a serial number”回车后输入你想要使用的序列号。我这里使用随机生成。[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1660322183-18.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1660322183-18.jpg)

19、需要加载十代CPU核显驱动的，在“Addons”处回车。如果使用的CPU不是10代，此步骤跳过不做。[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1660322184-19.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1660322184-19.jpg)

20、需要加载十代CPU核显驱动的，在“Add an addon”处回车。如果使用的CPU不是10代，此步骤跳过不做。[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1660322185-20.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1660322185-20.jpg)

21、需要加载十代CPU核显驱动的，在“i915 Intel iGPU Drivers(10th Gen)”处回车。如果使用的CPU不是10代，此步骤跳过不做。[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1660322186-21.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1660322186-21.jpg)

22、需要加载十代CPU核显驱动的，在这个界面直接回车就行，不要输入任何内容（温馨提醒：有些主板可能存在兼容性问题，建议编译引导时先不开启该补丁，等安装好系统以后再去打补丁。）。如果使用的CPU不是10代，此步骤跳过不做。

[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1660472889-QQ20220814182531.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1660472889-QQ20220814182531.jpg)

23、在“Exit”处回车返回上级菜单。[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1660322187-22.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1660322187-22.jpg)

24、在“Build the loader”处回车，开始编译。[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1660322189-23.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1660322189-23.jpg)

25、编译过程中，界面上会有进度条在跑进度，请耐心等待，等待的时间视你的网络环境而定（如果有科学出国的环境，请把此IP地址放到强制代理名单，可以加快编译速度）。[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1660322190-24.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1660322190-24.jpg)

26、引导编译完成后，系统会自动跳回这个界面，并且会多出一行菜单“Boot the loder”，在这行菜单上回车。

（2022年11月25日更新）

arpl v1.0-beta3版本或者以上，支持路过编译系统直接从群晖引导启动，具体设置请阅读《[arpl编译群晖引导成功后加快引导盘启动教程](https://wp.gxnas.com/12800.html)》。

[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1660322191-25.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1660322191-25.jpg)

27、把NAS主机手动重启一次，编译好的引导就会自动启动，该项目编译出来的引导启动后显示的界面如下，会显示有：系统型号，系统版本，pid，vid，sn，mac等等。[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1660322193-26.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1660322193-26.jpg)

28、在电脑上打开群晖助手，等待一段时间后，会搜索到DSM未安装的信息，IP地址、型号和版本与刚才编译的一致，这就对了。如果你的电脑搜索不出来的话，把电脑防火墙关掉后再试一下。[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1660322194-27.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1660322194-27.jpg)

29、接下来就可以安装系统了，怎么样，是不是超级简单？后面的安装过程我就不演示了。

* * *

温馨提醒：

※ 使用该项目编译引导有任何建议或者意见的，请直接向【[项目作者的Issues](https://github.com/fbelavenuto/arpl/issues)】提交，提交之前先看一下Issuse的内容，看看是否有人跟你一样的问题已经得到解决，避免重复提交同样的问题。

※ 作者是巴西人，不懂中文，提交Issuse请使用英文进行书写。

* * *

【arpl编译好的引导，修改SN/MAC和添加多网卡的方法】（2022年9月17日更新）

30、把NAS重启一次，在启动菜单选第三行回车；

[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1663386233-b1.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1663386233-b1.jpg)

31、在“Choose a serial number”处回车；[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1663384243-a1.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1663384243-a1.jpg)

32、在“Enter a serial number”处回车；[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1663384248-a2.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1663384248-a2.jpg)

33、输入你想要使用的SN，输完了按一次回车；[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1663384248-a3.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1663384248-a3.jpg)

34、在“yes”处回车；[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1663384248-a4.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1663384248-a4.jpg)

35、在“Cmdline menu”处回车；[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1663384249-a5.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1663384249-a5.jpg)

36、在“Define a custom MAC”处回车；[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1663384249-a6.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1663384249-a6.jpg)

37、输入你想要使用的mac地址（这里修改的是mac1），输完了按一次回车；[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1663384249-a7.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1663384249-a7.jpg)

38、当屏幕显示如下图的时候，把NAS重启一次；[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1663384250-a8.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1663384250-a8.jpg)

39、启动菜单选第三行回车；

[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1663386233-b1.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1663386233-b1.jpg)

40、看到刚才修改的mac地址已经生效了，并且IP也会自动改变，不是之前的IP地址了，在浏览器打开新的地址和端口；[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1663384250-a9.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1663384250-a9.jpg)

41、在“Cmdline menu”处回车；[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1663384251-a10.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1663384251-a10.jpg)

42、在“Add/edit an cmdline item”处回车；[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1663384251-a11.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1663384251-a11.jpg)

43、输入 netif_num 回车，修改网卡数量；[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1663384252-a12.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1663384252-a12.jpg)

44、默认是单网口，所以只有1；[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1663384252-a13.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1663384252-a13.jpg)

45、NAS是双网卡的话，把 1 删了，改成 2 回车；[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1663384254-a14.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1663384254-a14.jpg)

46、在“Add/edit an cmdline item”处回车；[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1663384255-a15.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1663384255-a15.jpg)

47、输入 mac2  回车；[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1663384265-a16.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1663384265-a16.jpg)

48、输入mac2的值，回车；[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1663384265-a17.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1663384265-a17.jpg)

49、在“Show user cmdline”处回车；[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1663384266-a18.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1663384266-a18.jpg)

50、此时屏幕会显示刚才设置的网口数量以及mac1和mac2的地址，查看一下设置的是否正确，按回车返回；[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1663384266-a19.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1663384266-a19.jpg)

51、如果设置不对，就重复上述的动作继续修改，设置正确了就在“Exit”处回车；[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1663384267-a20.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1663384267-a20.jpg)

52、在“Build the loader”处回车；[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1663384268-a21.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1663384268-a21.jpg)

53、当屏幕上显示“Ready!”的时候，就表示已经修改好了；[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1663384268-a22.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1663384268-a22.jpg)

54、屏幕自动跳转到菜单，在“Boot the loader”处回车；[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1663384268-a23.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1663384268-a23.jpg)

55、引导启动好了以后，屏幕上会显示刚才设置的SN、网口数量和两个MAC值。[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1663384269-a24.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1663384269-a24.jpg)

56、登录群晖，控制面板，信息中心，看到有两个网卡信息，设置正确了。[![](https://wp.gxnas.com/wp-content/uploads/2022/08/1663384270-a25.jpg)
](https://wp.gxnas.com/wp-content/uploads/2022/08/1663384270-a25.jpg)