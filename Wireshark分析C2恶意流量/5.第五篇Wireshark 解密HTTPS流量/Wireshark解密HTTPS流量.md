# Wireshark解密HTTPS流量

2020-09-04阅读 8900

在审查可疑网络活动时，经常遇到加密流量。大多数网站使用HTTPS协议，各种类型的恶意软件也使用HTTPS，查看恶意软件产生的数据对于了流量内容非常有帮助。

本文介绍了如何利用Wireshark从pcap中解密HTTPS流量。可以使用基于文本的日志进行解密方法，日志中包含最初记录pcap时捕获的加密密钥数据。

## **HTTPS Web流量**

HTTPS流量通常显示一个域名。例如，在Web浏览器中查看https://www.wireshark.org，在自定义的Wireshark列显示中查看时，pcap将显示www.wireshark.org作为此流量的服务器名称。但无法知道其他详细信息，例如实际的URL或从服务器返回的数据。

![image-20211009165429113](assets/image-20211009165429113.png)

![image-20211009165445980](assets/image-20211009165445980.png)

## **加密密钥日志文件**

加密密钥日志是一个文本文件。

![image-20211009165501917](assets/image-20211009165501917.png)

最初记录pcap时，使用中间人（MitM）技术创建这些日志。如果在记录pcap时未创建任何此类文件，则无法解密该pcap中的HTTPS通信。

## **示例分析**

### **有密钥日志文件的HTTPS流量**

Github存储库中有一个受密码保护的ZIP文件，其中包含pcap及其密钥日志文件。ZIP中包含的pcap通过密钥日志解密后，可以访问恶意软件样本。

链接：

https://github.com/pan-unit42/wireshark-tutorial-decrypting-HTTPS-traffic

![image-20211009165522761](assets/image-20211009165522761.png)

![image-20211009165544891](assets/image-20211009165544891.png)

从ZIP（密码：infected）中提取pcap和密钥日志文件：

```javascript
Wireshark-tutorial-KeysLogFile.txt
Wireshark解密HTTPS-SSL-TLS-traffic.pcap教程
```

![image-20211009165621735](assets/image-20211009165621735.png)

### **没有密钥日志文件的HTTPS流量**

在Wireshark中打开解密的HTTPS-SSL-TLS-traffic.pcap Wireshark教程，使用Web筛选器：

> （http.request或tls.handshake.type eq 1）和！（ssdp）

此pcap来自Windows 10主机上的Dridex恶意软件，所有Web流量（包括感染活动）都是HTTPS。没有密钥日志文件，看不到流量的任何详细信息，只有IP地址，TCP端口和域名：

![image-20211009165638681](assets/image-20211009165638681.png)

### **加载密钥日志文件**

在Wireshark中打开解密的HTTPS-SSL-TLS-traffic.pcap Wireshark，使用菜单路径Edit –> Preferences来打开Preferences菜单：

![image-20211009165653820](assets/image-20211009165653820.png)

在Preferences菜单的左侧，单击Protocols：

![image-20211009165705228](assets/image-20211009165705228.png)

如果使用的是Wireshark版本2.x，需要选择SSL。如果使用的是Wireshark 3.x版，需要选择TLS。选择SSL或TLS后，可以看到（Pre）-Master-Secret日志文件名。单击“浏览”，然后选择名为Wireshark-tutorial-KeysLogFile.txt的密钥日志文件：

![image-20211009165715511](assets/image-20211009165715511.png)

![image-20211009165742697](assets/image-20211009165742697.png)

![image-20211009165805795](assets/image-20211009165805795.png)

### **密钥日志文件的HTTPS流量**

单击“确定”后，Wireshark会在每条HTTPS行下列出解密的HTTP请求：

![image-20211009165819735](assets/image-20211009165819735.png)

在此pcap中可以看到隐藏在HTTPS通信中对microsoft.com和skype.com域的HTTP请求，还发现由Dridex发起的以下流量：

```javascript
foodsgoodforliver[.]com – GET /invest_20.dll
105711[.]com – POST /docs.php
```

对foodsgoodforliver[.]com的GET请求返回了Dridex的DLL文件。对105711[.]com的POST请求是来自受Dridex感染的Windows主机的命令和控制（C2）通信。

针对foodsgoodforliver[.]com的HTTP GET请求的HTTP流：

![image-20211009165843468](assets/image-20211009165843468.png)

![image-20211009165859315](assets/image-20211009165859315.png)

可以从pcap导出此恶意软件，使用菜单路径文件–>导出对象–> HTTP从pcap导出该文件：

![image-20211009165923489](assets/image-20211009165923489.png)

使用file命令确认这是一个DLL文件，然后使用shasum -a 256获取文件的SHA256哈希：

![image-20211009165944105](assets/image-20211009165944105.png)

该恶意软件的SHA256哈希为：

```javascript
31cf42b2a7c5c558f44cfc67684cc344c17d4946d3a1e0b2cecb8e*173cb2f
```

还可以检查来自此Dridex感染的C2流量，下图显示了其中一个HTTP流的示例：

![image-20211009165956380](assets/image-20211009165956380.png)

## **参考**

https://unit42.paloaltonetworks.com/wireshark-tutorial-decrypting-https-traffic/

https://cloud.tencent.com/developer/article/1692440



# 实战

# 方法一：Key log File方式解密（相当于客户端）

1、通过设置环境变量截取浏览器的pre_master_secret,进而实现解密HTTPS的目的。

2、环境变量中新建用户变量SSLKEYLOGFILE=路径\sslkey.log文件，之后再wireshark中ssl配置中制定该文件位置即可。

3、如果是chrome浏览器的数据流，直接配置“SSLKEYLOGFILE”就可以解密

# 1. 配置系统变量

新建sslkey.log空文件，路径：C:\Program Files\sslkey.log

![image-20211009210630654](assets/image-20211009210630654.png)

这里变量名为SSLKEYLOGFILE（一定）,变量值可以自己任意定义，这里我将相应的log文件保存到C:\Program Files\sslkey.log下（设置后可以通过Chrome浏览器打开任意一个HTTPS网址，此时查看变量值对应路径，已经生成sslkey.log）

![image-20211009210252068](assets/image-20211009210252068.png)

# 2. 配置Wireshark

配置wireshark并加载相应的证书文件

![image-20211009210414465](assets/image-20211009210414465.png)

如果不是TLS协议，那么就是SSL协议

![image-20211009210506870](assets/image-20211009210506870.png)

# 3.  启动抓包

访问https://www.freebuf.com/,可以看到相应的TLS流量已经被成功解密

![image-20211009212358905](assets/image-20211009212358905.png)



可以看到已解密出http流量，追踪http流

![image-20211009213019544](assets/image-20211009213019544.png)



明文流量

![image-20211009213051562](assets/image-20211009213051562.png)



sslkey.log文件内容如下：通过设置环境变量截取浏览器的pre_master_secret

![image-20211009214937776](assets/image-20211009214937776.png)



# 方法二：服务器私钥方式解密（相当于服务端）

从服务器上导出带私钥的P12格式的证书，或者直接导出服务器的私钥。



# 1. 生成自签名证书

### 1.1 生成私钥和CSR文件

CSR文件 证书签名请求文件，包含了服务器的密钥对，CA按照会校验这个文件，然后验证CSR请求文件。



执行命令：

```shell
[root@VM-0-10-centos ~]# openssl req -newkey rsa:2048 -nodes -keyout test_key.pem -out csr.pem
```

解释：

```css
-nodes :表示私钥不加密，若不带参数将提示输入密码
-newkey rsa:2048 -keyout test_key.pem 表示生成私钥(PKCS8格式)
```

需要填些信息，自签名的除了密码，其他的无所谓。

```shell
Generating a 2048 bit RSA private key
.....................+++
.......+++
writing new private key to 'test_key.pem'
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [XX]:CN //国家名称（2个字母代码）[XX]：CN     
State or Province Name (full name) []:sichuan //州或省名
Locality Name (eg, city) [Default City]:chengdou //地点名称（如城市）
Organization Name (eg, company) [Default Company Ltd]:test //  组织名称（如公司）
Organizational Unit Name (eg, section) []:testunit  //  组织单位名称（例如，部分）
Common Name (eg, your name or your server's hostname) []:testssl.com  //  通用名称（例如，您的姓名或服务器的主机名）
Email Address []:testssl@163.com //  电子邮件地址

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:12345m
An optional company name []:
```

最终生成了密钥对文件和CSR文件。

```shell
[root@VM-0-10-centos ~]# ll /root/
total 24
-rw-r--r-- 1 root root 1090 Dec  9 10:11 csr.pem
-rw-r--r-- 1 root root 1704 Dec  9 10:11 test_key.pem
```

![image-20211010040046631](assets/image-20211010040046631.png)





### 1.2 生成自签名证书

```shell
[root@VM-0-10-centos ~]# openssl x509 -signkey test_key.pem -in csr.pem -req -days 3650 -out test_cert.pem
Signature ok
subject=/C=CN/ST=sichuan/L=chengdou/O=asiainfo/OU=testunit/CN=testssl.com/emailAddress=testssl@163.com
Getting Private key
```

解释：

```shell
-signkey 是指明使用的密钥对，-in 表示csr文件，-days 表示有效期，out 最终生成的x509格式的证书文件:test_cert.pem
```

![image-20211010040008995](assets/image-20211010040008995.png)



有时候需要PKCS#12 格式即.p12结尾的证书，命令如下：

```shell
[root@VM-0-10-centos ~]# openssl pkcs12 -export -in test_cert.pem  -out test_cert.p12 -inkey test_key.pem 
```

![image-20211010040216600](assets/image-20211010040216600.png)

生成test_cert.p12 文件,需要输出密码`12345m`



### 1.3 提取公钥

单独提取公钥的命令：

```shell
[root@VM-0-10-centos ~]# openssl rsa -in test_key.pem  -pubout -out test_public_key.pem
```

![image-20211010040432628](assets/image-20211010040432628.png)



### 1.4 提取私钥

```shell
[root@VM-0-10-centos ~]# openssl rsa -in test_key.pem -passin pass:12345m
```

![image-20211010040904444](assets/image-20211010040904444.png)

新建test_private.pem文件，用来保存生成的私钥信息

```txt
-----BEGIN RSA PRIVATE KEY-----
MIIEpQIBAAKCAQEAx0sPH8ZJmPw5vXgdKMXJvLtAuHl778vmF1KQcakU0UG4j9Ua
DOiRkkLlFNzCwNzNuO+RyUlBvX7xTQiPjc4BJXlax4R8x0zk9rrmwM+zm2aWCdjE
4F3ZFjTtcRtr7nyUgPjUCzBaxIAk0IPc9H5X23d/A4i4m/heQdvCMUQ4Nt1Z50lu
/SJ8RUSIkUhqP0P+H5E/CMERQZ3OCfcpGxyLFXd2hEopj9NBOZRQaaknU6mh0r9I
dOAqQ70WP3pS+PrYuWQ1tIyJ8tZODVlaGznd52zPnxV6i1cX8VAKRflfE/Z5d35/
tdo8DmLQvpya8JkHDNcdPyNVlDktVDkZ3aA5IwIDAQABAoIBAQCc+P38pQ4rNd+0
4Pr2tnNj+InNw8Je6lddX2buX6NJ/14DzHIsYLLoZVBS0l+D0Wcol2pZBR6Gx4oA
hxGrO+ujPKbIKA6wXFysCiLfE7oAWRkswFSx9LmcyCuNv2+P9QhzXYqHFZso01Yr
9vD+ktVYxBKDEhl2oHZ9oT6f13//lY16gWIO8r8vE23ms1isSmM7nVQCn3/Z4cSA
W3NgamrY6JuX4DX6wjljJrrr7Cs0ptJv6B0JcbdFTjyF2fGFEsKrNWK9LJ2DqBrt
H+NwLyxt6OpsoHjrUkVuoS4fzB1hdbzglLd6pybeROmSuhpQMJQ9uYdpdY9j+YOk
/CrfCU4hAoGBAO489R/uJ7DqDfh8RQw/zT0rqaU8SOS5E3FH82M85grVSJdaDHpy
m77wfqraNshBbE2ww/8jLcp3as1w9gVoySXCgyOczW/h6rkJYbjdUVqqPoTM9Kh7
cAhZM2E7J2QWCvF6A7xMrKwL8aJw14tQgILBCqv1rq1B2hAwUF7lHODdAoGBANYm
y0Uod3Dy5VMIQGjZxxjXfrQBBVLfOKke7slSA5UVTDBRH3uFwhx5Sd4z0qpy5YN4
hNpryfod++diq05hp424VP1zBV83rJsUYOxdfv7Co6rx0FVEURFLtKZW7Lwt5BSl
DpIPEGTqetKT+blNUgDIi+Lre1GQ1anLQdwkguH/AoGBAIHgmYGew+SetJ8SoFHd
NnZypzl9c/+9JexL3VPsjzy4qapZXRQkBuRBF8zsyZDCmdiA1VjyRLRsBb2aJ4Ur
av063WM8yIKGFwBpLO0rOn7C8PJVCcwP/zON7YTESuU1XyHl8sNkyX3BxSlZeyhn
6qddtgOpHGE31TmeHTKelO/BAoGBAKTH3QkuBhoHDKZDZyxGxog66OehEXOfB346
qwqF7PO+G43iHhNx7Nb0DP7JaYR2PFxZVACtiBDJGZgFuELrqX3HJL6QzsVhlZMO
rzWxC5MEOePDhcbWdnqUgkCsCiUSOroX94ozxS2hrzhxXGpVMJAhwGGvSIiOpZmt
KNQ30+4JAoGAY7jxuUqYPjCEym/6eCpeJsUVVY5c9aXGgRSdVTN1/R2EJJob65RZ
g7+FxdIqbYllfIEGeov8x1Ja401WiDf41OZ4mimmb6AwALkIAoldHaFgTaxuFiyD
B9mddIIUhupQxW072DW9hv3jaBcyrqBqOYhy123zOK6OHwqkmIENeks=
-----END RSA PRIVATE KEY-----
```





# 2.部署Tomcat_https环境

docker官网搜索tomcat镜像，链接：https://hub.docker.com/_/tomcat

![image-20211010022459935](assets/image-20211010022459935.png)



拉取并启动：`docker run -it --rm -p 8888:8080 tomcat:9.0`，需要改一下：--rm参数去掉：退出不删除容器，添加 -d参数：后台启动，最终启动命令：

[root@localhost ~]# docker run -it  -p 8888:8080  -d tomcat:9.0

![image-20211010022735967](assets/image-20211010022735967.png)



确保docker服务已开启：

[root@localhost ~]# systemctl status docker

![image-20211010023058105](assets/image-20211010023058105.png)



启动容器

[root@localhost ~]# docker run -it  -p 8888:8080  -d tomcat:9.0

![image-20211010023829288](assets/image-20211010023829288.png)



进入容器

[root@localhost ~]# docker exec -it 83b6 /bin/bash

![image-20211010023816135](assets/image-20211010023816135.png)

查看目录，webapp为web应用程序目录，删除原来的webapp重命名webapp.dist为webapps

root@83b6526b30ab:/usr/local/tomcat# rm -rf webapps && mv webapps.dist/ webapps

![image-20211010024104286](assets/image-20211010024104286.png)

重启tomcat容器

[root@localhost ~]# docker restart 83b6

![image-20211010024351070](assets/image-20211010024351070.png)



浏览器访问：http://192.168.111.137:8888/，tomcat容器启动成功

![image-20211010024622204](assets/image-20211010024622204.png)



查看版本：系统为Debian/Ubuntu ,更新源为：bullseye版本

root@83b6526b30ab:/usr/local/tomcat# cat /etc/*-release

![image-20211010024920885](assets/image-20211010024920885.png)



搜索对应的更新源：

![image-20211010025112912](assets/image-20211010025112912.png)



获得更新源：

![image-20211010025149428](assets/image-20211010025149428.png)



备份并添加更新源:

```shell
root@83b6526b30ab:/usr/local/tomcat# mv /etc/apt/sources.list /etc/apt/sources.list.bak
root@83b6526b30ab:/usr/local/tomcat# cat <<EOF > /etc/apt/sources.list
deb http://mirrors.ustc.edu.cn/debian bullseye main
deb-src http://mirrors.ustc.edu.cn/debian bullseye main

deb http://mirrors.ustc.edu.cn/debian-security/ bullseye-security main
deb-src http://mirrors.ustc.edu.cn/debian-security/ bullseye-security main

deb http://mirrors.ustc.edu.cn/debian bullseye-updates main
deb-src http://mirrors.ustc.edu.cn/debian bullseye-updates main
EOF
root@83b6526b30ab:
```

![image-20211010025355589](assets/image-20211010025355589.png)



apt update:更新

root@83b6526b30ab:/usr/local/tomcat# apt update

![image-20211010025526712](assets/image-20211010025526712.png)





安装vim工具

root@83b6526b30ab:/usr/local/tomcat# apt install vim -y



更改/usr/local/tomcat/conf/server.xml 配置文件，找到如下位置：

![image-20211010025920074](assets/image-20211010025920074.png)



改为：

```shell
<Connector
protocol="org.apache.coyote.http11.Http11NioProtocol"
port="8080" maxThreads="200" 
ciphers="TLS_RSA_WITH_AES_128_CBC_SHA256,TLS_RSA_WITH_AES_128_CBC_SHA,TLS_RSA_WITH_AES_128_GCM_SHA256"  
scheme="https" secure="true" SSLEnabled="true"
keystoreFile="conf/testopenssl/test_cert.p12" keystorePass="12345m" keystoreType="PKCS12"
clientAuth="false" sslProtocol="TLS" sslEnabledProtocols="TLSv1.2"
connectionTimeout="20000"
redirectPort="8443"  />
```

解释：

```txt
port="8080"：https的端口8080，
ciphers="TLS_RSA_WITH_AES_128_CBC_SHA256,TLS_RSA_WITH_AES_128_CBC_SHA,TLS_RSA_WITH_AES_128_GCM_SHA256":支持密码套件,简单方便测试
keystoreFile="conf/testopenssl/test_cert.p12"：证书文件位置
keystorePass="12345m"：证书密码
keystoreType="PKCS12"：证书类型
sslProtocol="TLS"：协议类型
sslEnabledProtocols="TLSv1.2：协议版本
```

![image-20211010032321115](assets/image-20211010032321115.png)



在宿主机上复制test_cert.p12证书到tomcat容器内：/root/

[root@localhost ~]# docker cp /root/test_cert.p12 83b6526b30ab:/root/

![image-20211010033304691](assets/image-20211010033304691.png)

tomcat容器内复制test_cert.p12证书到testopenssl文件夹下

```shell
[root@localhost ~]# docker exec -it 83b6 /bin/bash
root@83b6526b30ab:/usr/local/tomcat/conf# mkdir testopenssl
root@83b6526b30ab:/usr/local/tomcat/conf# cd testopenssl/
root@83b6526b30ab:/usr/local/tomcat/conf/testopenssl# cp /root/test_cert.p12 .
root@83b6526b30ab:/usr/local/tomcat/conf/testopenssl# ls
test_cert.p12
```



![image-20211010032214141](assets/image-20211010032214141.png)

重启tomcat容器

[root@localhost ~]# docker restart 83b6

![image-20211010033749860](assets/image-20211010033749860.png)





访问：https://192.168.111.137:8888/，成功搭建https站点

![image-20211010033852963](assets/image-20211010033852963.png)



# 3、wireshark配置服务器证书



选择：菜单-->编辑-->首选项-->Protocols-->TLS/SSl，添加配置，准备好`test_private.pem私钥或test_cert.p12证书`

![image-20211010041817337](assets/image-20211010041817337.png)



解释：

```txt
IP address:服务器ip
Port：服务器https端口
Protocol:被加密的上层协议
Key File:证书或私钥(选择文件导入)
Password:证书加密密码
```

清除浏览器历史记录与缓存

![image-20211010042405307](assets/image-20211010042405307.png)



wireshark抓包:`ip.addr==192.168.111.137 and tcp.port==8888`

![image-20211010042610700](assets/image-20211010042610700.png)



访问：https://192.168.111.137:8888/，选择：高级-->同意

![image-20211010042536418](assets/image-20211010042536418.png)

解密成功，追踪TLS流或HTTP流，可以直观的看到HTTP报文的请求和响应。

![image-20211010043037803](assets/image-20211010043037803.png)



# 参考

https://www.cnblogs.com/yurang/p/11505741.html

https://www.jianshu.com/p/fca9663f1e49



# 结束！





