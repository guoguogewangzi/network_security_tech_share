[从IP和样本进行拓线Hunting](https://mp.weixin.qq.com/s?__biz=Mzg3NzczOTA3OQ==&mid=2247485926&idx=1&sn=f9588865b7a129eb02b247b99d647840&chksm=cf1f24cef868add809dbcd57c59c067c11542e7902f56bc10999b6f2ec953f65264472ad604f&scene=178&cur_album_id=2941079300322885634#rd)
================================================================================================================================================================================================================================================================

RainSec2023-05-24 10:12本文共 2885 字阅读完需 12 分钟

> 从IP和样本进行拓线Hunting

前言
--

在情报生产中当你拿到一批样本，应该思考如何通过样本里的C2或者特征去拓线找到更多的恶意样本或者C2链接，本文将给出从一个样本出发进行拓线分析的例子，大佬轻喷。

QuasarRAT
---------

### 样本获取

样本来源推荐一个网站：MalwareBazaar | Browse malware samples (abuse.ch)

其中收录了大量的样本，而且存在一些标识Tag帮助你快速找到自己想要的样本，当然也是有对应的搜索语句的，学习一下即可。这里用`signature:QuasarRAT`进行搜索

![图片](%E4%BB%8EIP%E5%92%8C%E6%A0%B7%E6%9C%AC%E8%BF%9B%E8%A1%8C%E6%8B%93%E7%BA%BFHunting.assets/640.png)

这些都是被标记为`QuasarRAT`的样本，挑一些出来看看：

![图片](%E4%BB%8EIP%E5%92%8C%E6%A0%B7%E6%9C%AC%E8%BF%9B%E8%A1%8C%E6%8B%93%E7%BA%BFHunting.assets/640-16849864655291.png)

```
SHA256 hash:  0183491852a035f91d926bc25b7f09e7f145c59429cfef10e3f9963caa95c068
SHA1 hash:  89cb5af4f1953fb7b26c592d95461e0a2a9a546d
MD5 hash:  5e48a824853c3c6b9fe64223fe12d7cb
```

样本准备好了，接下来就是进行一些行为分析，由于笔者不是专业的二进制样本分析师，所以我们借助沙箱和VirusTotal来进行分析。

### 样本分析

把样本扔到各家的沙箱里，并且去VirusTotal查询一下当前样本的信息和关联情况

#### 微步云沙箱

可以看到命中了一些沙箱的规则，但是没有检测到有释放的文件或者连接的远控server

![图片](%E4%BB%8EIP%E5%92%8C%E6%A0%B7%E6%9C%AC%E8%BF%9B%E8%A1%8C%E6%8B%93%E7%BA%BFHunting.assets/640-16849864655292.png)

![图片](%E4%BB%8EIP%E5%92%8C%E6%A0%B7%E6%9C%AC%E8%BF%9B%E8%A1%8C%E6%8B%93%E7%BA%BFHunting.assets/640-16849864655293.png)

#### VirusTotal

主力还是我们的vt，我们重点关注`RELATION`这个页面，其中包含了一些关联的样本和IP信息

![图片](%E4%BB%8EIP%E5%92%8C%E6%A0%B7%E6%9C%AC%E8%BF%9B%E8%A1%8C%E6%8B%93%E7%BA%BFHunting.assets/640-16849864655294.png)

![图片](%E4%BB%8EIP%E5%92%8C%E6%A0%B7%E6%9C%AC%E8%BF%9B%E8%A1%8C%E6%8B%93%E7%BA%BFHunting.assets/640-16849864655295.png)

对于多引擎无检出的优先级并不高，因为有些样本会通过连接白名单来判断当前的网络情况，所以我们优先判断`59.26.93.6`和`13.107.4.50`

#### 59.26.93.6

![图片](%E4%BB%8EIP%E5%92%8C%E6%A0%B7%E6%9C%AC%E8%BF%9B%E8%A1%8C%E6%8B%93%E7%BA%BFHunting.assets/640-16849864655296.png)

关联样本有很多，但是名称很相近，跟进查看其他样本信息

![图片](%E4%BB%8EIP%E5%92%8C%E6%A0%B7%E6%9C%AC%E8%BF%9B%E8%A1%8C%E6%8B%93%E7%BA%BFHunting.assets/640-16849864655297.png)

![图片](%E4%BB%8EIP%E5%92%8C%E6%A0%B7%E6%9C%AC%E8%BF%9B%E8%A1%8C%E6%8B%93%E7%BA%BFHunting.assets/640-16849864655308.png)

几乎都是已经明确是`QuasarRAT`的样本，这样一来，我们从一个QuasarRAT样本找到了这个关联IP，而这个IP关联了多个已经被判定为QuasarRAT的样本,那么笔者就认为这个IP就是QuasarRAT的远控IP，后续进行分析。

#### 13.107.4.50

![图片](%E4%BB%8EIP%E5%92%8C%E6%A0%B7%E6%9C%AC%E8%BF%9B%E8%A1%8C%E6%8B%93%E7%BA%BFHunting.assets/640-16849864655309.png)

可以看到这个IP关联的样本很多，笔者挑了几个看了一下涉及到的病毒种类也比较多，无法判定是单纯的QuasarRAT Server，所以这里就不做后续分析，有兴趣的师傅可以自己分析一下VirusTotal - IP address - 13.107.4.50

![图片](%E4%BB%8EIP%E5%92%8C%E6%A0%B7%E6%9C%AC%E8%BF%9B%E8%A1%8C%E6%8B%93%E7%BA%BFHunting.assets/640-168498646553010.png)

![图片](%E4%BB%8EIP%E5%92%8C%E6%A0%B7%E6%9C%AC%E8%BF%9B%E8%A1%8C%E6%8B%93%E7%BA%BFHunting.assets/640-168498646553011.png)

![图片](%E4%BB%8EIP%E5%92%8C%E6%A0%B7%E6%9C%AC%E8%BF%9B%E8%A1%8C%E6%8B%93%E7%BA%BFHunting.assets/640-168498646553012.png)

### IP分析

我们拿到了`59.26.93.6`这个IP，接下来去空间测绘引擎查询一下IP的信息，本文以Shadon和Fofa为例

语句：

```
Shadon:  ip:"59.26.93.6"
Fofa  :  ip\="59.26.93.6"
```

结果中首先观察到这里：

![图片](%E4%BB%8EIP%E5%92%8C%E6%A0%B7%E6%9C%AC%E8%BF%9B%E8%A1%8C%E6%8B%93%E7%BA%BFHunting.assets/640-168498646553013.png)

很多端口的banner都是这串字符，这是`msrpc`的banner特征，所以这个就被我们pass

然后我们查看https的证书，发现存在证书特征关键字`Quasar Server CA`

![图片](%E4%BB%8EIP%E5%92%8C%E6%A0%B7%E6%9C%AC%E8%BF%9B%E8%A1%8C%E6%8B%93%E7%BA%BFHunting.assets/640-168498646553014.png)

Shadon也显示了该IP对应的证书信息

![图片](%E4%BB%8EIP%E5%92%8C%E6%A0%B7%E6%9C%AC%E8%BF%9B%E8%A1%8C%E6%8B%93%E7%BA%BFHunting.assets/640-168498646553015.png)

到这里，我们找到了一个相关的特征，即证书的Subject或者Issuer是`Quasar Server CA`，接下来使用Shadon进行拓线

### 特征拓线

我们通过特征在Shadon搜索更多证书与`Quasar Server CA`相关的服务器

`ssl.cert.subject.cn:"Quasar Server CA"`

![图片](%E4%BB%8EIP%E5%92%8C%E6%A0%B7%E6%9C%AC%E8%BF%9B%E8%A1%8C%E6%8B%93%E7%BA%BFHunting.assets/640-168498646553016.png)

直接相关的结果有19个，主要分布在毛里求斯、香港和德国。对应的端口也多为443端口，少量的1337和8009端口

然后通过Shadon的聚合模式查看其他的特征信息，首先是JARM

![图片](%E4%BB%8EIP%E5%92%8C%E6%A0%B7%E6%9C%AC%E8%BF%9B%E8%A1%8C%E6%8B%93%E7%BA%BFHunting.assets/640-168498646553017.png)

获得`QuasarRAT`Server出现的JARM值，然后我们通过单独搜索JARM去尝试Hunting

`ssl.jarm:"2ad2ad0002ad2ad0002ad2ad2ad2adf9fdf4eeac344e8b5003264da73585be"`

![图片](%E4%BB%8EIP%E5%92%8C%E6%A0%B7%E6%9C%AC%E8%BF%9B%E8%A1%8C%E6%8B%93%E7%BA%BFHunting.assets/640-168498646553018.png)

看到12w个结果，基本可以确定不怎么靠谱，误报风险极极极极极极极极极极极极极极大

其他的JARM看了结果也很多，所以这里就不考虑JARM，其他的指纹查看结果价值也不大

该样本拓线出证书特征为`Quasar Server CA`

QakBot
------

QBot是一个模块化的信息窃取器，也被称为Qakbot或Pinkslipbot。它从受感染的系统中窃取金融数据，以及使用C2服务器进行有效载荷定位和下载的加载器。

使用文章使用 Sophos NDR 检测到新的 QakBot C2 服务器 – Sophos News中提到的C2进行分析

### 173.18.122.24

`ip:"173.18.122.24"`

搜索结果只开放了443端口，查看一下端口的特征值，这里注意返回的banner信息和网页hash

![图片](%E4%BB%8EIP%E5%92%8C%E6%A0%B7%E6%9C%AC%E8%BF%9B%E8%A1%8C%E6%8B%93%E7%BA%BFHunting.assets/640-168498646553119.png)

通过网页hash进行拓展

`http.html_hash:501510358`

![图片](%E4%BB%8EIP%E5%92%8C%E6%A0%B7%E6%9C%AC%E8%BF%9B%E8%A1%8C%E6%8B%93%E7%BA%BFHunting.assets/640-168498646553120.png)

结果显示近9W条，这显然证明只通过网页hash颗粒度不够，由于shadon看不了这个IP的JARM指纹，我们通过fofa查看

![图片](%E4%BB%8EIP%E5%92%8C%E6%A0%B7%E6%9C%AC%E8%BF%9B%E8%A1%8C%E6%8B%93%E7%BA%BFHunting.assets/640-168498646553121.png)

尝试通过JARM指纹和网页hash一起查询

`http.html_hash:501510358 ssl.jarm:"21d14d00021d21d21c42d43d0000007abc6200da92c2a1b69c0a56366cbe21"`

![图片](%E4%BB%8EIP%E5%92%8C%E6%A0%B7%E6%9C%AC%E8%BF%9B%E8%A1%8C%E6%8B%93%E7%BA%BFHunting.assets/640-168498646553122.png)

共找到了88个结果，这个结果就在可接受的用来优化的范围内了，但还是要注意下其他的特征

例如我们的原IP banner信息：

```
HTTP/1.1 200 OK
Content\-Length: 4833
Server: nginx/1.9.12
```

可以看到首页的数据基本都相符，那么我们就暂时以这88个数据为基础再提取其他的特征，通过聚合查看其他特征

其中对于`http.headers_hash`只存在一个值`-1219739159`

![图片](%E4%BB%8EIP%E5%92%8C%E6%A0%B7%E6%9C%AC%E8%BF%9B%E8%A1%8C%E6%8B%93%E7%BA%BFHunting.assets/640-168498646553123.png)

此时只有一个特征的值就可以拿来进行下一步操作，我们通过`html_hash`和`headers_hash`反查JARM

`http.html_hash:501510358 http.headers_hash:-1219739159`

![图片](%E4%BB%8EIP%E5%92%8C%E6%A0%B7%E6%9C%AC%E8%BF%9B%E8%A1%8C%E6%8B%93%E7%BA%BFHunting.assets/640-168498646553124.png)

除了我们开头提取的`21d`这个JARM，还获取到了一个有一定数量的`04d`

将JARM筛选也加上

`http.html_hash:501510358 http.headers_hash:-1219739159 ssl.jarm:"04d02d00004d04d04c04d02d04d04d9674c6b4e623ae36cc2d998e99e2262e"`

![图片](%E4%BB%8EIP%E5%92%8C%E6%A0%B7%E6%9C%AC%E8%BF%9B%E8%A1%8C%E6%8B%93%E7%BA%BFHunting.assets/640-168498646553125.png)

![图片](%E4%BB%8EIP%E5%92%8C%E6%A0%B7%E6%9C%AC%E8%BF%9B%E8%A1%8C%E6%8B%93%E7%BA%BFHunting.assets/640-168498646553126.png)

惊喜来了，这些IP的banner全部符合之前IP的情况，端口也都是统一的443，所以我们可以认为这些IP也是疑似QakBot

那么这时候我们就可以把JARM的条件去掉，只用http.html\_hash:501510358 http.headers\_hash:-1219739159

![图片](%E4%BB%8EIP%E5%92%8C%E6%A0%B7%E6%9C%AC%E8%BF%9B%E8%A1%8C%E6%8B%93%E7%BA%BFHunting.assets/640-168498646553127.png)

可以看到从1个IP拓线出了110个疑似QakBot的IP

剩下的那个IP就留给师傅们去尝试

结语
--

笔者最近从事Hunting和情报相关工作，做简单的抛砖引玉，希望各位师傅不吝赐教！期待与各位师傅一起进步！