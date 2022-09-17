# server certificate verification failed.CAfile: /etc/ssl/certs/ca-certificates.crt_AUKO16的博客-CSDN博客
![](https://csdnimg.cn/release/blogv2/dist/pc/img/original.png)

版权声明：本文为博主原创文章，遵循 [CC 4.0 BY-SA](http://creativecommons.org/licenses/by-sa/4.0/) 版权协议，转载请附上原文出处链接和本声明。

**问题描述：**   
从GitHub地址下载内容，提示服务器证书验证失败，没有CRLfile。  
[curl](https://so.csdn.net/so/search?q=curl&spm=1001.2101.3001.7020): (60) server certificate verification failed. CAfile: /etc/ssl/certs/ca-certificates.crt CRLfile: none  
![](https://img-blog.csdnimg.cn/20200327142633396.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80NjQzOTE5Mw==,size_16,color_FFFFFF,t_70)
  
**问题原因：**   
根本原因是计算机不信任对Gitlab服务器上使用的证书进行签名的证书颁发机构。  
这并不意味着证书可疑，它可以是自签名的，也可以由不在系统的CA列表中的机构/公司签名。

```python
curl performs SSL certificate verification by default, using a "bundle" of Certificate Authority (CA) public keys (CA certs). 

If the default bundle file isn't adequate, you can specify an alternate file using the --cacert option.

If this HTTPS server uses a certificate signed by a CA represented in the bundle, the certificate verification probably failed due to a problem with the certificate (it might be expired, or the name might not match the domain name in the URL).

If you'd like to turn off curl's verification of the certificate, use the -k (or --insecure) option.


```

**问题解决：** 

*   方案一：  
    关闭系统的安全认证。  
    能解决所有证书问题，操作简单，系统面临一定的风险。

```powershell

export GIT_SSL_NO_VERIFY=1

```

*   方案二：  
    告诉计算机(系统)信任这个网站的证书(颁发机构)。  
    能解决GitHub网站证书的问题，且继续保证系统安全，相对复杂。  
    [操作方法以及更多讨论详情](https://stackoverflow.com/questions/7814423/ssl-works-with-browser-wget-and-curl-but-fails-with-git)，这里有一个看起来很好地回答(并没有亲测)。  
    **Pete Clark：** “如果在ubuntu机器上运行，则可能是Web服务器的CA证书不在/etc/ssl/certs/ca-certificates.crt文件中。我遇到了一个托管在Web服务器上的git服务器，该服务器具有由www.incommon.org签名的SSL证书。您可以将中间证书添加到ca-certificates文件中，如下所示：”

```powershell
wget http://cert.incommon.org/InCommonServerCA.crt
openssl x509 -inform DER -in InCommonServerCA.crt -out incommon.pem
cat /etc/ssl/certs/ca-certificates.crt incommon.pem > ca-certs2.crt
sudo cp /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/ca-certificates.crt.bak
sudo cp ca-certs2.crt /etc/ssl/certs/ca-certificates.crt

```