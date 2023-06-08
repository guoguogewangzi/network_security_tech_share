[Security-Onion-Solutions安全洋葱安装方法](http://www.hackdig.com/08/hack-124518.htm)
=============================================================================

2020-08-31 18:40

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gepj%253Dtmf_xw%253F046%252Fwbi353unaxCJt5O8QhkUNcDRyskWg3N0XNairq2AnbBW5nql5z2pGzBstqWbiBWbPGKjAaiw0QZp3ImvBMhGBM8biWY%252Fgnp_zibmm%252Fnc.cipq.jpg)

**文章作者：命运**

securityonion安全洋葱介绍：

安全洋葱是一款开源的入侵检测系统、集成了日志分析、流量分析安全告警如：Grafana、TheHive、Playbook、Fleet / Osquery、Winlogbeat，集众多安全软件工具为一身的开源流量分析平台

Securityonion的部署方式

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gnp%253Dtmf_xw%253F046%252FQZK1NzjGad7HGoyhWRNXwFknbyNd0aeeEciBzUQp5Y9ekaZxSpWgciU0cNBld84eeciYdTnOtvAXzOuBMhGBM8biWY%252Fgnp_zibmm%252Fnc.cipq.png)

配置文档

https://docs.securityonion.net/en/2.0/getting-started.html

securityonion-2.1.0-rc2基于DOCKER环境搭建、如果DOCKER命令不熟悉可以跳转到我的第一篇文章中查看DOCKER的扩展命令、后续我们会经常用到。

下载地址

https://download.securityonion.net/file/securityonion/securityonion-2.1.0-rc2.iso

虚拟机配置要求：

系统Centos7 64位 、磁盘200G、内存最少12G、CPU4核

开始安装

新建虚拟机

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gnp%253Dtmf_xw%253F046%252FwROciXAayQjgRl4wGJvncCmYSlkFbi9OAb1PJHzYe11blquPfZtrONIPcNBld84eeciYdTnOtvAXzOuBMhGBM8biWY%252Fgnp_zibmm%252Fnc.cipq.png)

稍后安装操作系统

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gnp%253Dtmf_xw%253F046%252Fgm85RAETc5abVvID4JIUhK9BB4REl3203MRgefp7OSuNYIP6zTai1citcNBld84eeciYdTnOtvAXzOuBMhGBM8biWY%252Fgnp_zibmm%252Fnc.cipq.png)

选择centos7 64位

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gnp%253Dtmf_xw%253F046%252FwuUIrVTYTKb02ctAB61TPvme85WWNUv3t5q31kA2hTr35V7cp4uneEcNBld84eeciYdTnOtvAXzOuBMhGBM8biWY%252Fgnp_zibmm%252Fnc.cipq.png)

虚拟机名称和存放的位置

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gnp%253Dtmf_xw%253F046%252FAXvs2W07VJR66cienZr1jjXyo9tMT6BD3vpcizYTcizKrxOtF9GoBmtE3cNBld84eeciYdTnOtvAXzOuBMhGBM8biWY%252Fgnp_zibmm%252Fnc.cipq.png)

硬盘建议200G

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gnp%253Dtmf_xw%253F046%252FA8t2XHc6dSkLaiwW6ev55vB26qDjaiCo2TP6uQaisySoXkhzD7Vq6ZGCWcNBld84eeciYdTnOtvAXzOuBMhGBM8biWY%252Fgnp_zibmm%252Fnc.cipq.png)

点击完成 即可创建虚拟机

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gnp%253Dtmf_xw%253F046%252Fw0E7pmeVZLllurYq7wNML2nBfFKlci4zzvS10QSzObiwpgcbi3naifOlmKcNBld84eeciYdTnOtvAXzOuBMhGBM8biWY%252Fgnp_zibmm%252Fnc.cipq.png)

点击编辑虚拟机

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gnp%253Dtmf_xw%253F046%252FAw7Zcpz5NxYIgS2jbigJ8hXBqsxCkZOsS4w7S3biu2gvV6WD2OdNpy2LcNBld84eeciYdTnOtvAXzOuBMhGBM8biWY%252Fgnp_zibmm%252Fnc.cipq.jpg)

内存设置8G

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gnp%253Dtmf_xw%253F046%252FASkFcN8mMGL9aiye4P6GLci4QgzzG7xrnBgchsdNlzqvYaimhJRIRZchGcNBld84eeciYdTnOtvAXzOuBMhGBM8biWY%252Fgnp_zibmm%252Fnc.cipq.jpg)

CPU设置4核

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gnp%253Dtmf_xw%253F046%252FATzs5PW9oszSR1M9q77biVjOSa1t6VlIBtW8oOchBsJBuXR2O5AwkJbicNBld84eeciYdTnOtvAXzOuBMhGBM8biWY%252Fgnp_zibmm%252Fnc.cipq.jpg)

添加一快网卡

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gnp%253Dtmf_xw%253F046%252FQE4AcAci7DjhMvo41kbiVwDmA656EmvvWQlRfKEpyciTnn4F0pLD1B79WcNBld84eeciYdTnOtvAXzOuBMhGBM8biWY%252Fgnp_zibmm%252Fnc.cipq.jpg)

两快网卡都使用桥接模式

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gnp%253Dtmf_xw%253F046%252FQ4qMFy3JtBdItXKMs9ZRwjD9OoZs1jkBLmMYvN2LgBsciVfVgtn4fwaicNBld84eeciYdTnOtvAXzOuBMhGBM8biWY%252Fgnp_zibmm%252Fnc.cipq.jpg)

最后选择下载好的镜像点击确定即可

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gnp%253Dtmf_xw%253F046%252FA8b6mMkEY37Iveds3hZjKE89FATwn9ybisBdkHu5TMxfvJGafSfuVuYcNBld84eeciYdTnOtvAXzOuBMhGBM8biWY%252Fgnp_zibmm%252Fnc.cipq.jpg)

开启虚拟机、直接选择安装安全洋葱

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gnp%253Dtmf_xw%253F046%252FQlbmbq8JQcykai2AuJ15NWQq3PZqoEVOCG1PZqcy2h0Wnl8xvsSSJyTcNBld84eeciYdTnOtvAXzOuBMhGBM8biWY%252Fgnp_zibmm%252Fnc.cipq.png)

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gnp%253Dtmf_xw%253F046%252FwD7DFx0hGdfZ3fKg9MXwPJaik8sn0xs69FbREBN1GrYzbfE5OyE3q3HcNBld84eeciYdTnOtvAXzOuBMhGBM8biWY%252Fgnp_zibmm%252Fnc.cipq.jpg)

输入yes、之后创建安全洋葱系统账号，我这里创建的账号密码为:onion/admin123

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gnp%253Dtmf_xw%253F046%252FgZvfbLbiQ2qQw7EoHBQl90cilKDYpK6Eqvujai3TxxCQT2SAZCtz4aChncNBld84eeciYdTnOtvAXzOuBMhGBM8biWY%252Fgnp_zibmm%252Fnc.cipq.jpg)

回车后静静等待安装、安装的快慢取决于你电脑的配置，反正我安装了半小时....

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gnp%253Dtmf_xw%253F046%252FwNtU6Sdffj4GhoMWZeyvUeb9HGPaiciJNyuH1FNfLkCTsZcpd5ldRnE7cNBld84eeciYdTnOtvAXzOuBMhGBM8biWY%252Fgnp_zibmm%252Fnc.cipq.jpg)

安装系统镜像结束、按回车重启系统

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gnp%253Dtmf_xw%253F046%252FQNNMuSxCafra7wUoKxRcik0OPwG3p8yP6X64lciCW4zMKBkONUO6wz3bcNBld84eeciYdTnOtvAXzOuBMhGBM8biWY%252Fgnp_zibmm%252Fnc.cipq.jpg)

重启后会让你输入账号密码、此账号密码就是刚刚安装时创建的onion/admin123

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gnp%253Dtmf_xw%253F046%252FAduPVuoz9cZhbbiLhkFeouX2x3W0ULuS2Jsog6zAmaimC2aiSPECFlS2LcNBld84eeciYdTnOtvAXzOuBMhGBM8biWY%252Fgnp_zibmm%252Fnc.cipq.jpg)

选择安装类型、按上下来切换、空格选中、tab切换到OK或者Cancel下

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gnp%253Dtmf_xw%253F046%252FA2tzBqxE0sIJ3z2lsZnsair6YQrZuTVaidKXSsl7ujcRnTbmqWmMG0FccNBld84eeciYdTnOtvAXzOuBMhGBM8biWY%252Fgnp_zibmm%252Fnc.cipq.jpg)

设置主机名

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gnp%253Dtmf_xw%253F046%252FAQZY3Oci31vk9pAgyncEQkenQWSklUUQZxrfOabiOJQ0cieaixrYzeGusVcNBld84eeciYdTnOtvAXzOuBMhGBM8biWY%252Fgnp_zibmm%252Fnc.cipq.jpg)

选择管理网卡、这里我们选择ens33为管理网卡、ens34为镜像口

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gnp%253Dtmf_xw%253F046%252FQ0qDwOuMHrmRbRthlNScRxMKai03l2ByMUOASw2WSgq8Juv8kYJE28cicNBld84eeciYdTnOtvAXzOuBMhGBM8biWY%252Fgnp_zibmm%252Fnc.cipq.jpg)

设置静态IP

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gnp%253Dtmf_xw%253F046%252FQwHfdpgjvxX6lbiUY5hf5zgrdD23oc2ZlwJlmj6mGfg6KZ61zciai37KgcNBld84eeciYdTnOtvAXzOuBMhGBM8biWY%252Fgnp_zibmm%252Fnc.cipq.jpg)

因为是桥接模式需要查看本地IP地址、本地IP为192.168.0.4,我们在安全洋葱下配置地址为192.168.0.10

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gnp%253Dtmf_xw%253F046%252FgU8PkaixVNZF0biWcv5DGDbiKZHo5prSSpLXZ0WYJGUBQLbiLBKYphp8wjcNBld84eeciYdTnOtvAXzOuBMhGBM8biWY%252Fgnp_zibmm%252Fnc.cipq.png)

配置静态地址为192.168.0.10

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gnp%253Dtmf_xw%253F046%252FwCsen4FtcyJ54psVrzYzAcitnVgvIwhai8OS3ZIbiIRHLavRY9hrA9cCrcNBld84eeciYdTnOtvAXzOuBMhGBM8biWY%252Fgnp_zibmm%252Fnc.cipq.jpg)

掩码默认24位

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gnp%253Dtmf_xw%253F046%252Fg8cmciBETgCUVJUlR0x1aiJ1WzJ78U3KsBohvj4WZbijmBuciVObbiYvcWscNBld84eeciYdTnOtvAXzOuBMhGBM8biWY%252Fgnp_zibmm%252Fnc.cipq.jpg)

设置网关

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gnp%253Dtmf_xw%253F046%252FQhWQZYcaizCOJvMvkTzpmoZCLkJQ0ubf5yaiU8XE76ghqNQXxORaibbi5ycNBld84eeciYdTnOtvAXzOuBMhGBM8biWY%252Fgnp_zibmm%252Fnc.cipq.jpg)

设置DNS

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gnp%253Dtmf_xw%253F046%252FwoFbBd5jksabirkdaisnZrqTah5cixysUtAbiGHk2IlmCwcbaAMN4RJ2S0cNBld84eeciYdTnOtvAXzOuBMhGBM8biWY%252Fgnp_zibmm%252Fnc.cipq.jpg)

备用DNS

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gnp%253Dtmf_xw%253F046%252FA9FQszJciUIzFEpQaW5Lhaaciokkx264nYmVpbymsAtqYMociQMIhz7trcNBld84eeciYdTnOtvAXzOuBMhGBM8biWY%252Fgnp_zibmm%252Fnc.cipq.jpg)

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gnp%253Dtmf_xw%253F046%252Fw1XcxpPwWFNGHHxoRrCh3XIaiPFcciL8ojBVnAoqxWHyUBo9XNXmFuWbcNBld84eeciYdTnOtvAXzOuBMhGBM8biWY%252Fgnp_zibmm%252Fnc.cipq.jpg)

创建安全洋葱web登录账号密码、账号必须以邮件格式

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gnp%253Dtmf_xw%253F046%252FAaigc8LNr8lmSrbcBhzfrO7DzkFJxc0dozd9Wdcid5mUUTW5cb5phLp1cNBld84eeciYdTnOtvAXzOuBMhGBM8biWY%252Fgnp_zibmm%252Fnc.cipq.jpg)

创建密码

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gnp%253Dtmf_xw%253F046%252FwJQZ2G4Fn7pbVTDnaiV4QjQRkosqU4big5fRWz5GlqDfdkYaT3RPRyaiscNBld84eeciYdTnOtvAXzOuBMhGBM8biWY%252Fgnp_zibmm%252Fnc.cipq.jpg)

重复密码

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gnp%253Dtmf_xw%253F046%252FAxxONcxm71fHWrHOCDDpR6MJRrlwfM24JSt1xPAciFDGaiIoQtVqX90QcNBld84eeciYdTnOtvAXzOuBMhGBM8biWY%252Fgnp_zibmm%252Fnc.cipq.jpg)

界面登录的方法我们选择web登录

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gnp%253Dtmf_xw%253F046%252FghTTT6UhD2bZQGBV1yUnCCZgai3uPyYy6rGly8kZmqGTXTcirWSIaibibiacNBld84eeciYdTnOtvAXzOuBMhGBM8biWY%252Fgnp_zibmm%252Fnc.cipq.jpg)

选择yes允许so-allow访问web工具、因为安装完成后我们要使用sudo so-allow 来启动web界面

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gnp%253Dtmf_xw%253F046%252FwQJOPZEbrLWeye0G2jWTJzdxzoN3KKYtNBmfa3o31jp3GzZNtxVGsDcNBld84eeciYdTnOtvAXzOuBMhGBM8biWY%252Fgnp_zibmm%252Fnc.cipq.jpg)

这里IP直接为空、选择OK

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gnp%253Dtmf_xw%253F046%252FwXeOB62PkQrxOyY9mpQIws5l2cNFfNU6T9xLgH7v3X2cQJF5w4VoaiLcNBld84eeciYdTnOtvAXzOuBMhGBM8biWY%252Fgnp_zibmm%252Fnc.cipq.jpg)

点击yes 开始安装

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gnp%253Dtmf_xw%253F046%252FgVMlHKK10kHv8fwbCB7By7MrQg8kqtP1cipFTsblFw1yamF8baibi42PGcNBld84eeciYdTnOtvAXzOuBMhGBM8biWY%252Fgnp_zibmm%252Fnc.cipq.jpg)

开始安装进度

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gnp%253Dtmf_xw%253F046%252FAkhKo80OkTnaWPCEPwvv7wg0QYRdTwIYHdWsWgpCp632M8XJTEytdAcNBld84eeciYdTnOtvAXzOuBMhGBM8biWY%252Fgnp_zibmm%252Fnc.cipq.jpg)

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gnp%253Dtmf_xw%253F046%252FQ8HEFb6Qxd3SkHttEtVWa230qPKjTkNLaillDGwX8NjX9Yhpwn5HZZxcNBld84eeciYdTnOtvAXzOuBMhGBM8biWY%252Fgnp_zibmm%252Fnc.cipq.jpg)

使用onion/admin123 登录到系统中、之后使用sudo so-allow 启动服务、密码为admin123，之后选择a回车后输入允许登录安全洋葱的主机地址

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gnp%253Dtmf_xw%253F046%252FAEyKIaFHCxWkGrwOTQ2kvnseCn4TewWMdFyESUMojXdG1BHaluoHW1cNBld84eeciYdTnOtvAXzOuBMhGBM8biWY%252Fgnp_zibmm%252Fnc.cipq.jpg)

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gnp%253Dtmf_xw%253F046%252FgHQ74kyRF3E0qXrbinncywbNuJkbi32FMLraivA1IHPYJn8r7Kxlf6kFbicNBld84eeciYdTnOtvAXzOuBMhGBM8biWY%252Fgnp_zibmm%252Fnc.cipq.jpg)

到此安全洋葱才算真正安装完成。

我们修改下root密码

运行sudo passwd，首次运行需要输入onion密码、输入成功后直接可以设置root密码

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gnp%253Dtmf_xw%253F046%252FgHnx82C0HFLsmrtzNGKE90THbiSVC33Gnd9vcNqqcglbgP9YvSzrXWecNBld84eeciYdTnOtvAXzOuBMhGBM8biWY%252Fgnp_zibmm%252Fnc.cipq.jpg)

设置了root密码后我们可以使用CRT/XSHELL对安全洋葱ssh远程登录、检查下80、443端口是否启动、如果80 443端口并未启动、我们可以在root权限下使用docker start $(docker ps -qa) 来启动所有已挂起的镜像

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gnp%253Dtmf_xw%253F046%252FgPJKvOnLeBq1qBd560wcQnsYS4IuSa7UM6KObiSoydHZnAO47Mo6kXVcNBld84eeciYdTnOtvAXzOuBMhGBM8biWY%252Fgnp_zibmm%252Fnc.cipq.jpg)

我们打开浏览器输入https://192.168.0.10,输入我们安装时候创建的web登录账号密码

admin@qq.com/admin123

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gnp%253Dtmf_xw%253F046%252FAzwAf0EzuEjO5A19x70RFptIt7hFzBEtcioXltbideRzh1cbgMsa8CVGcNBld84eeciYdTnOtvAXzOuBMhGBM8biWY%252Fgnp_zibmm%252Fnc.cipq.jpg)

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gnp%253Dtmf_xw%253F046%252FwWsVO8cih2AxFgjWbiRUWusr9at83civJfMrtaNcIzciSHOdesWnHZb1mUcNBld84eeciYdTnOtvAXzOuBMhGBM8biWY%252Fgnp_zibmm%252Fnc.cipq.jpg)

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gnp%253Dtmf_xw%253F046%252FwfV5WzgHdEXRFbi7Cq4zUxpjauD6ClCOXE7ckTJeTpYrDaihciAtaciKl3cNBld84eeciYdTnOtvAXzOuBMhGBM8biWY%252Fgnp_zibmm%252Fnc.cipq.jpg)

Kibana 数据展示界面

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gnp%253Dtmf_xw%253F046%252FAPBnwHci5muYmT0CM9kJUhbiQeXRcI95azRLYj59RUjfRenDFz8GkjOmcNBld84eeciYdTnOtvAXzOuBMhGBM8biWY%252Fgnp_zibmm%252Fnc.cipq.jpg)

Grafana 监控服务器应用运行状态

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gnp%253Dtmf_xw%253F046%252FAcibirMdobiciCXg0jCRN3nbiHjunZ9TJbBpcie3ObHsL0gD3u9kaQBvnUoYcNBld84eeciYdTnOtvAXzOuBMhGBM8biWY%252Fgnp_zibmm%252Fnc.cipq.jpg)

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gnp%253Dtmf_xw%253F046%252FQdzciXt8zTzMKhsGkGeugRaA05l974mXCFg1Oy6MduDFr0rkrgW61ai1cNBld84eeciYdTnOtvAXzOuBMhGBM8biWY%252Fgnp_zibmm%252Fnc.cipq.jpg)

Cyberchef 界面、可以加密解密数据

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gnp%253Dtmf_xw%253F046%252FA8kNnStvq6s4ocRwdCTzWAQcWci59aikOzhW1SDnP79rTj2QdPvt1rH6cNBld84eeciYdTnOtvAXzOuBMhGBM8biWY%252Fgnp_zibmm%252Fnc.cipq.jpg)

Fleet 登录账号密码同安全洋葱账号一样 admin@qq.com/admin123

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gnp%253Dtmf_xw%253F046%252FwOIIFyKBSmDIpxT7aJxZYCpTBsyGlz4sKcihYfceOjkezGR3PVJzFcUcNBld84eeciYdTnOtvAXzOuBMhGBM8biWY%252Fgnp_zibmm%252Fnc.cipq.jpg)

Thehive 登录账号密码同安全洋葱账号一样

admin@qq.com/admin123

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gnp%253Dtmf_xw%253F046%252FA9mF6IgX3fW1dUHSUbcN9Nqtai6gbi1NDKWEAvXHYsX1ACbioC1BB8rGOcNBld84eeciYdTnOtvAXzOuBMhGBM8biWY%252Fgnp_zibmm%252Fnc.cipq.jpg)

至此安全洋葱基本已经安装介绍完成、功能也基本完善，要抓取所有流量数据必须做端口镜像、否则抓不到攻击告警的数据流。下篇文章我将介绍如何在不同的设备上做端口镜像、以及Kibana、Fleet 、Thehive的使用方法及流量分析。

温馨提示：在虚拟机中安装完成一定要打快照哦.....

**☆ END ☆**

联系/合作/投稿邮箱：239273085@qq.com

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gepj%253Dtmf_xw%253F046%252FgNHMeacEHXMmPuEciYxZlKGoLyY8yURySPLavQ7PgkuuxUaiOj12fVNstqWbiBWbPGKjAaiw0QZp3ImvBMhGBM8biWY%252Fgnp_zibmm%252Fnc.cipq.jpg)

![](Security-Onion-Solutions%E5%AE%89%E5%85%A8%E6%B4%8B%E8%91%B1%E5%AE%89%E8%A3%85%E6%96%B9%E6%B3%95.assets/imgpxy.phpurl=gnp%253Dtmf_xw%253F046%252FQcLYhVClGRvAAIbdJM7NksvaeKVwZdVtSo9029yKbimZMSxcZJr4kgnPJLUciZHnVFeXaayLhaiaiGHglyGrnjpHBa%252Fgnp_zibmm%252Fnc.cipq.png)

你点的每个赞，我都认真当成了喜欢