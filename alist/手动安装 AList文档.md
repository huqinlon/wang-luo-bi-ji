# 手动安装 | AList文档
1.  [Home](https://alist.nn.ci/zh/)
2.  [Guide](https://alist.nn.ci/zh/guide/)
3.  [安装](https://alist.nn.ci/zh/guide/install/)
4.  [手动安装](https://alist.nn.ci/zh/guide/install/manual.html)

### 获取 AList

打开 [AList Releaseopen in new window](https://github.com/Xhofe/alist/releases) 下载待部署系统对应的文件。最新版的前端已经和后端打包好了，不用再下载前端文件了。

### 运行

```
 tar -zxvf alist-xxxx.tar.gz

chmod +x alist

./alist server

./alist admin 
```

```
 tar -zxvf alist-xxxx.tar.gz

chmod +x alist

./alist server

./alist admin 
```

```
 unzip alist-xxxx.zip

.\alist.exe server

.\alist.exe admin 
```

```
 scoop install alist

alist server 
```

xxxx 指的是不同系统/架构对应的名称，一般 Linux-x86/64 为 alist-linux-amd64。如果你的 glibc 版本太低，建议下载 musl 版本

当你看到 `start server@0.0.0.0:5244` 的输出，之后没有报错，说明操作成功。 第一次运行时会输出初始密码。程序默认监听 5244 端口。 现在打开 `http://ip:5244` 可以看到登录页面，WebDAV 请参阅 [WebDav](https://alist.nn.ci/zh/guide/webdav.html)。

相关信息

对于所有平台，您可以使用以下命令来静默启动、停止和重新启动。 （v3.4.0 及更高版本）

```
 alist start

alist stop

alist restart 
```

### 守护进程

使用任意方式编辑 `/usr/lib/systemd/system/alist.service` 并添加如下内容，其中 path\_alist 为 AList 所在的路径

```
[Unit]
Description=alist
After=network.target
 
[Service]
Type=simple
WorkingDirectory=path_alist
ExecStart=path_alist/alist server
Restart=on-failure
 
[Install]
WantedBy=multi-user.target 
```

然后，执行 `systemctl daemon-reload` 重载配置，现在你可以使用这些命令来管理程序：

*   启动: `systemctl start alist`
*   关闭: `systemctl stop alist`
*   配置开机自启: `systemctl enable alist`
*   取消开机自启: `systemctl disable alist`
*   状态: `systemctl status alist`
*   重启: `systemctl restart alist`

使用任意方式编辑 `~/Library/LaunchAgents/ci.nn.alist.plist` 并添加如下内容，修改 `path_alist` 为 AList 所在的路径，`path/to/working/dir` 为 AList的工作路径

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
    <dict>
        <key>Label</key>
        <string>ci.nn.alist</string>
        <key>KeepAlive</key>
        <true/>
        <key>ProcessType</key>
        <string>Background</string>
        <key>RunAtLoad</key>
        <true/>
        <key>WorkingDirectory</key>
        <string>path/to/working/dir</string>
        <key>ProgramArguments</key>
        <array>
            <string>path_alist/alist</string>
            <string>server</string>
        </array>
    </dict>
</plist> 
```

然后，执行 `launchctl load ~/Library/LaunchAgents/ci.nn.alist` 加载配置，现在你可以使用这些命令来管理程序：

*   开启: `launchctl start ~/Library/LaunchAgents/ci.nn.alist`
*   关闭: `launchctl stop ~/Library/LaunchAgents/ci.nn.alist`

任何你了解的方式，此处不再提供。

### 如何更新

下载新版Alist，把之前的替换了即可。

[

上一页

一键脚本

](https://alist.nn.ci/zh/guide/install/script.html)[

下一页

使用 Docker

](https://alist.nn.ci/zh/guide/install/docker.html)