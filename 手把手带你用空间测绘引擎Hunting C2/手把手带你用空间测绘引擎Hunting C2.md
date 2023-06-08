[手把手带你用空间测绘引擎Hunting C2](https://mp.weixin.qq.com/s/833z1ZIpBbdckoaq3GwqYg)
===========================================================================

RainSec2023-04-06 18:04本文共 5292 字阅读完需 21.5 分钟

> 手把手带你使用测绘引擎Hunting C2

![图片](%E6%89%8B%E6%8A%8A%E6%89%8B%E5%B8%A6%E4%BD%A0%E7%94%A8%E7%A9%BA%E9%97%B4%E6%B5%8B%E7%BB%98%E5%BC%95%E6%93%8EHunting%20C2.assets/640.png)

前言
--

笔者最近工作接触到了情报方面，觉得蛮有意思的，就尝试通过空间测绘引擎来Hunting一些C2生产情报，觉得这个流程蛮有意思，开个新坑手把手带你情报入门。

![图片](%E6%89%8B%E6%8A%8A%E6%89%8B%E5%B8%A6%E4%BD%A0%E7%94%A8%E7%A9%BA%E9%97%B4%E6%B5%8B%E7%BB%98%E5%BC%95%E6%93%8EHunting%20C2.assets/640-16849950862621.png)

什么是情报
-----

![图片](%E6%89%8B%E6%8A%8A%E6%89%8B%E5%B8%A6%E4%BD%A0%E7%94%A8%E7%A9%BA%E9%97%B4%E6%B5%8B%E7%BB%98%E5%BC%95%E6%93%8EHunting%20C2.assets/640-16849950862622.png)

> 威胁情报（Threat Intelligence）是指收集、分析和利用关于各种安全威胁的数据和信息的过程，以帮助组织识别和应对安全威胁。威胁情报可以来自多个来源，包括公共情报、私人情报、开源情报以及企业内部情报

本章中我们可以通过Hunting C2来对捕获到的IP进行一个`C2`标签的加，这样该IP就可以作为一个恶意IP进入到我们的情报库中。

此外对于某些集中攻击、流量特征明显或者针对具体行业的攻击者还可以具象化为一个家族或者团伙，以Cobalt Strike为例，`Ryuk`，`Conti`，`Egregor`和`DoppelPaymer`等几种勒索软件已经开始使用Cobalt Strike来加速其**勒索软件**部署。根据思科的报告显示，66%的勒索软件攻击涉及Cobalt Strike。

准备工作
----

Hunting C2最重要的工作就是调研一下C2服务器或者行为的一些特征，就像出去钓鱼，不同的鱼生活的环境、季节、水质情况等条件都要明确。

还是以Cobalt Strike为例，这里给出几种常见的Hunting CS服务器时关注的点：

```
Cobalt Strike Beacon
SSL证书和序列号
默认404未找到响应
默认端口50050和Banner Hash
默认SSH客户端Hash
JA3指纹、JA3S指纹和JARM TLS指纹
网站的HTML Hash
读懂C2
```

由于笔者妹有Fofa账号（Big 穷逼一个），所以本文以shadon和Quake为例

![图片](%E6%89%8B%E6%8A%8A%E6%89%8B%E5%B8%A6%E4%BD%A0%E7%94%A8%E7%A9%BA%E9%97%B4%E6%B5%8B%E7%BB%98%E5%BC%95%E6%93%8EHunting%20C2.assets/640-16849950862633.png)

开始Hunting
---------

### Cobalt Strike Beacon

Shodan已经将“Cobal Strike Beacon”添加到了 `Shodan Product`字段的值列表中，所以可以直接通过语法`product:"Cobalt Strike Beacon"`

![图片](%E6%89%8B%E6%8A%8A%E6%89%8B%E5%B8%A6%E4%BD%A0%E7%94%A8%E7%A9%BA%E9%97%B4%E6%B5%8B%E7%BB%98%E5%BC%95%E6%93%8EHunting%20C2.assets/640-16849950862634.png)

笔者在写文章时能够Hunting到的CS服务器有1200+，其中中国和美国以及中国香港行政区是出现C2服务器最多的三个地方

![图片](%E6%89%8B%E6%8A%8A%E6%89%8B%E5%B8%A6%E4%BD%A0%E7%94%A8%E7%A9%BA%E9%97%B4%E6%B5%8B%E7%BB%98%E5%BC%95%E6%93%8EHunting%20C2.assets/640-16849950862635.png)

（小声BB：它们的默认页面返回都是404 Not Found哦）

`80`、`443` 和`8443`是出现次数TOP3的端口

除此之外还可以看到在世界各地托管Cobalt Strike服务器的主要ISP是谁，腾讯和阿里巴巴真是一路领先

这种非常精确的特定搜索将用于在一些技术中确定CS服务器的独特属性

### SSL证书和序列号

Cobalt Strike本身是附带用于HTTPS通信的默认SSL证书的，所以可以通过Hunting SSL证书的值来搜索CS服务器。

```
默认证书: 
md5:950098276A495286EB2A2556FBAB6D83
sha1:6ECE5ECE4192683D2D84E25B0BA7E04F9CB7EB7C
sha256:87F2085C32B6A2CC709B365F55873E207A9CAA10BFFECF2FD16D3CF9D94D390C

默认序列号:ssl.cert.serial:146473198
```

所以可以使用 `ssl:"6ECE5ECE4192683D2D84E25B0BA7E04F9CB7EB7C"` 或者

`ssl.cert.serial:"146473198"`

进行查询

![图片](%E6%89%8B%E6%8A%8A%E6%89%8B%E5%B8%A6%E4%BD%A0%E7%94%A8%E7%A9%BA%E9%97%B4%E6%B5%8B%E7%BB%98%E5%BC%95%E6%93%8EHunting%20C2.assets/640-16849950862636.png)

![图片](%E6%89%8B%E6%8A%8A%E6%89%8B%E5%B8%A6%E4%BD%A0%E7%94%A8%E7%A9%BA%E9%97%B4%E6%B5%8B%E7%BB%98%E5%BC%95%E6%93%8EHunting%20C2.assets/640-16849950862637.png)

发现了吗，通过SHA1和通过Cert号搜索的结果是相同的，这是因为这两个字段链接在一起。

我们还可以加上之前的`product`字段查看CS出现的cert指纹形式

![图片](%E6%89%8B%E6%8A%8A%E6%89%8B%E5%B8%A6%E4%BD%A0%E7%94%A8%E7%A9%BA%E9%97%B4%E6%B5%8B%E7%BB%98%E5%BC%95%E6%93%8EHunting%20C2.assets/640-16849950862638.png)

大多数都是使用的默认证书，SSL 指纹是唯一的并且与Cobalt Strike特别相关，但是默认证书可以替换为有效的 SSL 证书，或者可以使用 Malleable C2 配置文件更改其参数（上面有一些不一样的cert就可以看出这一点）,除此之外还有不同的序列号：

![图片](%E6%89%8B%E6%8A%8A%E6%89%8B%E5%B8%A6%E4%BD%A0%E7%94%A8%E7%A9%BA%E9%97%B4%E6%B5%8B%E7%BB%98%E5%BC%95%E6%93%8EHunting%20C2.assets/640-16849950862639.png)

### 默认404未找到响应

上面的方法我们Hunting到的CS服务器可以看到默认全都是404，所以我们也可以从这个点去入手，但是对于结果需要谨慎的进行后续判断。

Cobalt Strike服务器默认的404 Not Found HTTP响应标头的`Content-Length`为 0，`Content-Type`为 `text/plain`，如下所示

```
HTTP/1.1 404 Not Found
Content\-Type: text/plain
Content\-Length: 0
```

所以可以结合这三处特征使用

`"HTTP/1.1 404 Not Found Date:" "Content-Type: text/plain" "Content-Length`

进行搜索

![图片](%E6%89%8B%E6%8A%8A%E6%89%8B%E5%B8%A6%E4%BD%A0%E7%94%A8%E7%A9%BA%E9%97%B4%E6%B5%8B%E7%BB%98%E5%BC%95%E6%93%8EHunting%20C2.assets/640-168499508626310.png)

有亿点小多。。。

攻击者可以通过不同的方式逃避这种检测方法，例如通过使用可拓展的C2配置文件更改默认响应，或者调整服务器参数和标头数据以使其与合法服务器保持一致。所以这种方式需要进行后续的威胁分析，这个本篇暂时不考虑。

### 默认端口50050和Banner Hash

CS服务器可以默认接受TCP 50050上的客户端连接

我们通过`port:50050`进行搜索：

![图片](%E6%89%8B%E6%8A%8A%E6%89%8B%E5%B8%A6%E4%BD%A0%E7%94%A8%E7%A9%BA%E9%97%B4%E6%B5%8B%E7%BB%98%E5%BC%95%E6%93%8EHunting%20C2.assets/640-168499508626411.png)

75w条的数据基数过于庞大，这时候的数据我们是不可以拿来投入生产的，需要进行更多的过滤，其中的一个方法就是对`banner`的hash做限制，CS默认的hash（2007783223）可以在这里作为过滤条件

所以我们的条件变更为`port:50050 hash:-2007783223`

![图片](%E6%89%8B%E6%8A%8A%E6%89%8B%E5%B8%A6%E4%BD%A0%E7%94%A8%E7%A9%BA%E9%97%B4%E6%B5%8B%E7%BB%98%E5%BC%95%E6%93%8EHunting%20C2.assets/640-168499508626412.png)

加上hash的限制之后直接锐减到19条，我焯

19条的数据人工去判断的成本也是完全可以接受的，quake目前好像不支持hash的搜索，这里就不展示了

### 默认SSH客户端Hash

依托于项目`hassh`,我们可以计算SSH客户端的信息，然后通过搜索这些hash相关的IP来捕获一些恶意IP，这里使用`greynoise`进行搜索，这个东西感觉和微步在线X情报社区差不多，有对IP打的tag。

```
hassh(CobaltStrike\_SSH-client) = a7a87fbe86774c2e40cc4a7ea2ab1b3c
related to: SSH-2.0\-libssh2\_1.8.0 || SSH-2.0\-libssh2\_1.7.0
```

\[搜索\](Query Results | GreyNoise Visualizer)

![图片](%E6%89%8B%E6%8A%8A%E6%89%8B%E5%B8%A6%E4%BD%A0%E7%94%A8%E7%A9%BA%E9%97%B4%E6%B5%8B%E7%BB%98%E5%BC%95%E6%93%8EHunting%20C2.assets/640-168499508626413.png)

可以看到结果中已经有被标记为恶意的CS SSH CLIENT的数据了，但是和之前不同，TOP榜里的地区居然不是中国第一名了。。。。

![图片](%E6%89%8B%E6%8A%8A%E6%89%8B%E5%B8%A6%E4%BD%A0%E7%94%A8%E7%A9%BA%E9%97%B4%E6%B5%8B%E7%BB%98%E5%BC%95%E6%93%8EHunting%20C2.assets/640-168499508626414.png)

### JA3的指纹们

#### JA3

JA3是一个开源项目，可以为客户端和服务器之间的通信创建SSL指纹。这些独特的签名可以代表客户端Hello数据包中的多个字段值。

与CS相关的已知JA3签名包括以下内容。例如，CS的Beacon使用Windows套接字来启动TLS通信,但是这种方法并不特定于Cobalt Strike。

#### JA3S

在创建了JA3之后，为TLS握手服务器端指纹创建了一种新的方法，即TLS服务器Hello消息。JA3S方法是收集以下字段在服务器Hello数据包中的字节的十进制值：版本、可接受的加密方式和扩展列表。

可以从已知的C2列表中提取所有的JA3S以对它们进行聚类，并基于其他相似之处扩大搜索范围。但是这超出了本文的范围，存在太多的误报风险。

这里结合`product`字段贴出出现频率比较高的指纹

![图片](%E6%89%8B%E6%8A%8A%E6%89%8B%E5%B8%A6%E4%BD%A0%E7%94%A8%E7%A9%BA%E9%97%B4%E6%B5%8B%E7%BB%98%E5%BC%95%E6%93%8EHunting%20C2.assets/640-168499508626415.png)

#### JARM

与JA3/JA3S类似，JARM能够对远程服务器的TLS数值进行指纹识别。它通过与目标服务器进行交互发送10个TLS客户端Hello数据包，并记录回复中的特定属性。然后，它将哈希值并创建最终的JARM指纹。

但是如果使用JARM扫描Cobalt Strike服务器，获得的结果取决于服务器所使用的Java版本。CS的文档中建议使用时首选OpenJDK 11。这使得容易识别潜在的Cobalt Strike服务器，但是也容易产生误报。（Java的东西在互联网上真是太多了）

默认配置的Cobalt Strike对应的JARM指纹是`07d14d16d21d21d00042d41d00041de5fb3038104f457d92ba02e9311512c2`

搜索如下：

![图片](%E6%89%8B%E6%8A%8A%E6%89%8B%E5%B8%A6%E4%BD%A0%E7%94%A8%E7%A9%BA%E9%97%B4%E6%B5%8B%E7%BB%98%E5%BC%95%E6%93%8EHunting%20C2.assets/640-168499508626416.png)

其他与Cobalt Strike服务器有关的JARM如下，更多JARM指纹可以参考`JARM`

![图片](%E6%89%8B%E6%8A%8A%E6%89%8B%E5%B8%A6%E4%BD%A0%E7%94%A8%E7%A9%BA%E9%97%B4%E6%B5%8B%E7%BB%98%E5%BC%95%E6%93%8EHunting%20C2.assets/640-168499508626417.png)

这种方法容易出现许多误报，并且只能帮助你了解一个IP是否可能与CS有关，工作中不要将其作为主要方法。

### 拓展网站的HTML Hash

Shodan的爬虫可以计算网站的Hash。要找到相同的网站，可以使用http.html\_hash，它是网站HTML的Hash。也可以使用这个\[工具\](ninoseki/apullo: A scanner for taking basic fingerprints (github.com))生成哈希值。

根据CS服务器的配置方式，哈希值可能基于默认设置为null。如果攻击者修改了默认配置，那么就会生成Hash。可以在html\_hash上进行判断，以找出具有相同哈希值的其服务器。

例如我们首先使用`product`字段查看已经明确的CS服务器的html hash：

![图片](%E6%89%8B%E6%8A%8A%E6%89%8B%E5%B8%A6%E4%BD%A0%E7%94%A8%E7%A9%BA%E9%97%B4%E6%B5%8B%E7%BB%98%E5%BC%95%E6%93%8EHunting%20C2.assets/640-168499508626418.png)

可以看到有一条hash为`2101032290`的数据，然后我们去除`product`标签直接进行搜索：

`http.html_hash:"2101032290"`

![图片](%E6%89%8B%E6%8A%8A%E6%89%8B%E5%B8%A6%E4%BD%A0%E7%94%A8%E7%A9%BA%E9%97%B4%E6%B5%8B%E7%BB%98%E5%BC%95%E6%93%8EHunting%20C2.assets/640-168499508626419.png)

可以看到结果有17条，后续就可以将刚才三条过滤掉对剩下的14条进行分析和确认。

同样的，这个思路还可以被用在`HTTP Header hash`

![图片](%E6%89%8B%E6%8A%8A%E6%89%8B%E5%B8%A6%E4%BD%A0%E7%94%A8%E7%A9%BA%E9%97%B4%E6%B5%8B%E7%BB%98%E5%BC%95%E6%93%8EHunting%20C2.assets/640-168499508626520.png)

例如这个`316618825`，按照上面所说的思路进行拓展之后：

`http.headers_hash:"316618825"`

![图片](%E6%89%8B%E6%8A%8A%E6%89%8B%E5%B8%A6%E4%BD%A0%E7%94%A8%E7%A9%BA%E9%97%B4%E6%B5%8B%E7%BB%98%E5%BC%95%E6%93%8EHunting%20C2.assets/640-168499508626521.png)

### 读懂C2

这条方法主要是针对一些开源的C2框架或者能够搞到源码的C2，例如`HavocFramework`

通过观察HavocFramework的代码，可以看到代码中存在定义：

```
    orgNames = \[\]string{
        "",
        "ACME",
        "Partners",
        "Tech",
        "Cloud",
        "Synergy",
        "Test",
        "Debug",
    }
    orgSuffixes = \[\]string{
        "",
        "co",
        "llc",
        "inc",
        "corp",
        "ltd",
    }
```

这其中就包含了生成的证书：ACME、Partners、Tech、Cloud、Synergy、Test、Debug

我们使用其中的一个特征来进行搜索

![图片](%E6%89%8B%E6%8A%8A%E6%89%8B%E5%B8%A6%E4%BD%A0%E7%94%A8%E7%A9%BA%E9%97%B4%E6%B5%8B%E7%BB%98%E5%BC%95%E6%93%8EHunting%20C2.assets/640-168499508626522.png)

同时，sliver也使用了这几种随机的org，\[代码\](sliver/subject.go at 97d3da75b6e24defb3a2a97443a15a632b3a8448 · BishopFox/sliver (github.com))

通过列表中的属性能够方便我们快速确定一台C2服务器

其他的“歪门邪道”
---------

Quake支持对相应包的搜索，所以我们可以寻找到一些在互联网上开放了目录的服务器，查看其中的内容

打开目录页面标题主要由正在使用的 Web 服务器决定。如果它是一个 Apache2 服务器，标题将是“Index of /”，如果它是一个 Python HTTP 服务器，内容将会是目录列表

![图片](%E6%89%8B%E6%8A%8A%E6%89%8B%E5%B8%A6%E4%BD%A0%E7%94%A8%E7%A9%BA%E9%97%B4%E6%B5%8B%E7%BB%98%E5%BC%95%E6%93%8EHunting%20C2.assets/640-168499508626523.png)

![图片](%E6%89%8B%E6%8A%8A%E6%89%8B%E5%B8%A6%E4%BD%A0%E7%94%A8%E7%A9%BA%E9%97%B4%E6%B5%8B%E7%BB%98%E5%BC%95%E6%93%8EHunting%20C2.assets/640-168499508626524.png)

攻击者通常倾向于使用Python搭建临时HTTP文件服务器，但是有时他们会忘记及时关闭Python HTTP服务器，导致我们可以通过HTTP响应进行过滤。例如我们可以输入任何安全工具或恶意软件的名称来进行过滤：

`title: "Directory listing for /" and response:"cobaltstrike"`

![图片](%E6%89%8B%E6%8A%8A%E6%89%8B%E5%B8%A6%E4%BD%A0%E7%94%A8%E7%A9%BA%E9%97%B4%E6%B5%8B%E7%BB%98%E5%BC%95%E6%93%8EHunting%20C2.assets/640-168499508626525.png)

看到目前或曾经有68个服务器托管了Cobalt Strike供攻击者下载

`title: "Directory listing for /" and response:"mimikatz"`

![图片](%E6%89%8B%E6%8A%8A%E6%89%8B%E5%B8%A6%E4%BD%A0%E7%94%A8%E7%A9%BA%E9%97%B4%E6%B5%8B%E7%BB%98%E5%BC%95%E6%93%8EHunting%20C2.assets/640-168499508626526.png)

`title: "Directory listing for /" and response:"exp"`

![图片](%E6%89%8B%E6%8A%8A%E6%89%8B%E5%B8%A6%E4%BD%A0%E7%94%A8%E7%A9%BA%E9%97%B4%E6%B5%8B%E7%BB%98%E5%BC%95%E6%93%8EHunting%20C2.assets/640-168499508626527.png)

碰巧逮到个没关的

![图片](%E6%89%8B%E6%8A%8A%E6%89%8B%E5%B8%A6%E4%BD%A0%E7%94%A8%E7%A9%BA%E9%97%B4%E6%B5%8B%E7%BB%98%E5%BC%95%E6%93%8EHunting%20C2.assets/640-168499508626528.png)

虽然妹有C2，但也是个用于托管恶意代码的服务器，可以将样本下载进行分析

结语
--

这里给出的方式还是比较少的，后续等弟弟精进一下分享些高级的Hunting手法。其中分享的手法有的只能用来进行辅助判断，在实际的生产时需要考虑误报的情况，毕竟情报误报还是比 较严重的。

其中还有一些手法例如通过地区、托管的服务商、Nmap扫描等大家感兴趣可以拓展一下，希望各位大师傅不吝赐教，轻喷。

Reference
---------

*   • Gustav Shen – Medium

*   • Michael Koczwara (@MichalKoczwara) / Twitter

*   • ninoseki/apullo: A scanner for taking basic fingerprints (github.com)

*   • salesforce/hassh: