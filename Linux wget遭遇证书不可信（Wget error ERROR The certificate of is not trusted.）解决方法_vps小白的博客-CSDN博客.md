# Linux wget遭遇证书不可信（Wget error: ERROR: The certificate of is not trusted.）解决方法_vps小白的博客-CSDN博客
新安装的debian9系统使用中发现[wget](https://so.csdn.net/so/search?q=wget&spm=1001.2101.3001.7020)时提示证书问题，搜索资料得知是缺少ca-certificates包引起。  
Linux安装ca-certificates包命令

```
yum install -y ca-certificates

```

```
apt-get install -y ca-certificates

```