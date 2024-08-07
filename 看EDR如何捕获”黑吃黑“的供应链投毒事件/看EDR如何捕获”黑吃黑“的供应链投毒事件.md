[看EDR如何捕获”黑吃黑“的供应链投毒事件](https://mp.weixin.qq.com/s?__biz=Mzg5MTc3ODY4Mw==&mid=2247501434&idx=1&sn=6817a6e664184c11e2d1e405924fa220)
===================================================================================================================================

微步在线研究响应中心2023-04-26 17:54本文共 4500 字阅读完需 18 分钟

> 免杀爱好者如何步步沦为“肉鸡”，戳我查看全过程

**前言**

为什么有了杀软还要EDR？

顺丰安全用他们亲历的一次供应链投毒事件证明了EDR的价值：有攻击团伙在互联网上发布和宣传“免杀Gh0st远控工具”来吸引“免杀爱好者”，并在其中隐藏设置了一个CobaltStrike后门，让“免杀爱好者”们最终成为攻击团伙的“猎物”。

尽管没有任何前置情报和知识库，但利用微步OneSEC中的行为检测和内存检测等技术，顺丰安全团队及时发现了未知威胁，并成功捕获了这一起由黑产团伙发起的供应链投毒攻击事件，这也是该团伙首次被发现和公开。在与顺丰的同事友好交流和联合分析后，根据该团伙“黑吃黑”的攻击特点，我们引用“螳螂捕蝉，黄雀在后”的典故，将之命名为“黄雀”。

如何有效检测并响应供应链攻击等高级威胁？以本次顺丰安全捕获“黄雀”事件为例，足够好用的EDR，加上训练有素的安全运营团队，不仅能够及时发现威胁，还能在实战中帮助安全运营团队降低应对威胁的复杂度，简化处置流程。

这篇原文转载自“顺丰安全应急响应中心”微信公众号。

**  
背景**

大家好，我是顺丰的安全研究员K，我每天的工作就是搞搞前沿攻方技术研究，内部赋能提升安全防御能力，应急重大安全事件。

在这个美好的早晨，我像往常一样沐浴着温暖的阳光、啃着我美味的馒头、刷着GitHub机器人推送的日报。突然，一个可疑的项目立马吸引了我的注意——“重写免杀版Gh0st……”，好家伙，我倒要看看是什么免杀可绕完国内主流杀软！”

![图片](%E7%9C%8BEDR%E5%A6%82%E4%BD%95%E6%8D%95%E8%8E%B7%E2%80%9D%E9%BB%91%E5%90%83%E9%BB%91%E2%80%9C%E7%9A%84%E4%BE%9B%E5%BA%94%E9%93%BE%E6%8A%95%E6%AF%92%E4%BA%8B%E4%BB%B6.assets/640.jpeg)

手里的馒头瞬间不香了，我立马在公司的安全研究环境sflab里安装起了这个“强大”的免杀版Gh0st。

哔——哔——哔——，一阵急促的告警声打破了这个清晨的平静，直觉告诉我有些不太对劲。

![图片](%E7%9C%8BEDR%E5%A6%82%E4%BD%95%E6%8D%95%E8%8E%B7%E2%80%9D%E9%BB%91%E5%90%83%E9%BB%91%E2%80%9C%E7%9A%84%E4%BE%9B%E5%BA%94%E9%93%BE%E6%8A%95%E6%AF%92%E4%BA%8B%E4%BB%B6.assets/640-16891348516111.png)

“CobaltStrike远控”？得益于sflab完备的监测和狩猎能力，很快我搞清楚了这是真实的攻击行为，——我，被上线了！！

我的内心开始变得紧张又愤怒。我敲，这是什么牛鬼蛇神，把 “安全研究员”都上线了，这传出去我的面子往哪搁啊！（好吧，我承认我内心有点小激动！）再说，作为一个安全研究员，我的基本修养是绝对不允许这种不道德的行为持续危害其他表哥们的好吧！这必须得把它的恶行昭告天下！于是，我马上联线上微步在线的老表们，一起展开了这起开源木马投毒事件的分析。

**正文**
------

经过调查发现，这款开源的高度免杀版Gh0st木马目前正在大范围传播，据称可免杀多种主流杀软：开发者不仅制作了新颖的下载页面，还设法增加了搜索引擎的收录权重，吸引了许多免杀马爱好者的下载使用。

![图片](%E7%9C%8BEDR%E5%A6%82%E4%BD%95%E6%8D%95%E8%8E%B7%E2%80%9D%E9%BB%91%E5%90%83%E9%BB%91%E2%80%9C%E7%9A%84%E4%BE%9B%E5%BA%94%E9%93%BE%E6%8A%95%E6%AF%92%E4%BA%8B%E4%BB%B6.assets/640-16891348516112.png)

然而通过分析我们发现，这款软件实际上隐藏了后门木马，会在相关Git项目中释放和执行，使相关设备沦为“肉鸡”。由于是针对攻击工具的投毒，我们引用“螳螂捕蝉，黄雀在后”的典故，将该团伙命名为“黄雀”。

*   该组织通过使用包含免杀特征的长标题来提高搜索权重，或者将工具地址的广告发布到知名的安全论坛或微信公众号中，以扩大影响范围。此外，他们还会设计类似于安全人员名称的GitHub用户名和投放者，以提高知名度，并进行大规模的传播活动;

*   该木马工具以开源的Gh0st为基础，在优化代码和完善功能的同时，增强了反检测和免杀的能力，因此备受木马使用者青睐。木马使用者想要远程控制他人主机，却不知道自身也被病毒工具中的木马所控制；

*   微步通过对相关样本、IP 和域名的溯源分析，提取多条相关 IOC ，可用于威胁情报检测。微步威胁感知平台 TDP 、本地威胁情报管理平台 TIP 、威胁情报云 API 、互联网安全接入服务 OneDNS 、主机威胁检测与响应平台 OneEDR 等均已支持对此次攻击事件和团伙的检测。

### **0x01 团伙分析**

#### 1、团伙画像

<table><tbody><tr><td colspan="2"><p class="both">团伙画像</p></td></tr><tr><td width="121"><p class="both">特点</p></td><td width="320"><p class="both">描述</p></td></tr><tr><td width="50"><p class="both">平台</p></td><td width="300"><p class="both">Windows</p></td></tr><tr><td width="50"><p class="both">攻击目标</p></td><td width="300"><p class="both">木马免杀技术爱好者，或使用相关木马的攻击者</p></td></tr><tr><td width="50"><p class="both">攻击地区</p></td><td width="300"><p class="both">中国</p></td></tr><tr><td width="50"><p class="both">攻击目的</p></td><td width="300"><p class="both">以“黑吃黑“的形式获取木马使用者的主机权限</p></td></tr><tr><td width="50"><p class="both">武器库</p></td><td width="300"><p class="both">跨段跳转技术，CobaltStrike</p></td></tr></tbody></table>

#### 2、技术特点

    该组织使用安全人员名称SecurityNo1，且在标题中使用免杀关键字来提高搜索权重，然后通过微信公众号等公开blog进行大范围宣传。

由于该GitHub项目中的确包含了Gh0st源码功能，加上公开宣传，使得许多免杀马爱好者会不加辨别地打开并执行该项目，最终成为该项目拥有者的“肉鸡”。

![图片](%E7%9C%8BEDR%E5%A6%82%E4%BD%95%E6%8D%95%E8%8E%B7%E2%80%9D%E9%BB%91%E5%90%83%E9%BB%91%E2%80%9C%E7%9A%84%E4%BE%9B%E5%BA%94%E9%93%BE%E6%8A%95%E6%AF%92%E4%BA%8B%E4%BB%B6.assets/640-16891348516113.png)

![图片](%E7%9C%8BEDR%E5%A6%82%E4%BD%95%E6%8D%95%E8%8E%B7%E2%80%9D%E9%BB%91%E5%90%83%E9%BB%91%E2%80%9C%E7%9A%84%E4%BE%9B%E5%BA%94%E9%93%BE%E6%8A%95%E6%AF%92%E4%BA%8B%E4%BB%B6.assets/640-16891348516114.png)

![图片](%E7%9C%8BEDR%E5%A6%82%E4%BD%95%E6%8D%95%E8%8E%B7%E2%80%9D%E9%BB%91%E5%90%83%E9%BB%91%E2%80%9C%E7%9A%84%E4%BE%9B%E5%BA%94%E9%93%BE%E6%8A%95%E6%AF%92%E4%BA%8B%E4%BB%B6.assets/640-16891348516125.png)

![图片](%E7%9C%8BEDR%E5%A6%82%E4%BD%95%E6%8D%95%E8%8E%B7%E2%80%9D%E9%BB%91%E5%90%83%E9%BB%91%E2%80%9C%E7%9A%84%E4%BE%9B%E5%BA%94%E9%93%BE%E6%8A%95%E6%AF%92%E4%BA%8B%E4%BB%B6.assets/640-16891348516126.png)

![图片](%E7%9C%8BEDR%E5%A6%82%E4%BD%95%E6%8D%95%E8%8E%B7%E2%80%9D%E9%BB%91%E5%90%83%E9%BB%91%E2%80%9C%E7%9A%84%E4%BE%9B%E5%BA%94%E9%93%BE%E6%8A%95%E6%AF%92%E4%BA%8B%E4%BB%B6.assets/640-16891348516127.png)

截至目前分析完毕，该github项目已经存在大量的Star和Fork数，可以发现运行使用该项目的潜在受害者数量较多，如下图所示:

![图片](%E7%9C%8BEDR%E5%A6%82%E4%BD%95%E6%8D%95%E8%8E%B7%E2%80%9D%E9%BB%91%E5%90%83%E9%BB%91%E2%80%9C%E7%9A%84%E4%BE%9B%E5%BA%94%E9%93%BE%E6%8A%95%E6%AF%92%E4%BA%8B%E4%BB%B6.assets/640-16891348516128.png)

### **0x02 样本分析**

#### 1、基本信息

<table><tbody><tr><td width="134"><p class="both">Sha256</p></td><td width="343"><p class="both">9a9b5258c771d025cbbeff42e21fdec09e1560495764afa22bd752a71cb15744</p></td></tr><tr><td width="129"><p class="both">SHA1</p></td><td width="323"><p class="both">818c409f7898a814458d7ec8910eff63834beded</p></td></tr><tr><td width="129"><p class="both">MD5</p></td><td width="323"><p class="both">a1864a201f5f71cb2960a31c610e8b18</p></td></tr><tr><td width="129"><p class="both">文件类型</p></td><td width="323"><p class="both">exe类型</p></td></tr><tr><td width="129"><p class="both">文件大小</p></td><td width="323"><p class="both">12.96M</p></td></tr><tr><td width="129"><p class="both">文件名称</p></td><td width="323"><p class="both">CcRemote.exe</p></td></tr><tr><td width="129"><p class="both">功能描述</p></td><td width="323"><p class="both">MFC编译，关闭函数内嵌恶意代码，使用跨段跳转从32位到64位执行远程线程注入explore程序，最终建立CobaltSrtike通信</p></td></tr></tbody></table>

#### 2、详细分析

样本执行流程图如下所示：

![图片](%E7%9C%8BEDR%E5%A6%82%E4%BD%95%E6%8D%95%E8%8E%B7%E2%80%9D%E9%BB%91%E5%90%83%E9%BB%91%E2%80%9C%E7%9A%84%E4%BE%9B%E5%BA%94%E9%93%BE%E6%8A%95%E6%AF%92%E4%BA%8B%E4%BB%B6.assets/640-16891348516129.png)

由于该病毒程序使用MFC框架打包编译生成，找到相应的消息处理函数映射表，经过处理后，如下图所示；

![图片](%E7%9C%8BEDR%E5%A6%82%E4%BD%95%E6%8D%95%E8%8E%B7%E2%80%9D%E9%BB%91%E5%90%83%E9%BB%91%E2%80%9C%E7%9A%84%E4%BE%9B%E5%BA%94%E9%93%BE%E6%8A%95%E6%AF%92%E4%BA%8B%E4%BB%B6.assets/640-168913485161210.png)

通过对其中的消息函数进行逐一分析，发现在消息处理函数映射表中的关闭消息函数中存在异常可疑行为，增加了相关的注入函数，如下图所示；

![图片](%E7%9C%8BEDR%E5%A6%82%E4%BD%95%E6%8D%95%E8%8E%B7%E2%80%9D%E9%BB%91%E5%90%83%E9%BB%91%E2%80%9C%E7%9A%84%E4%BE%9B%E5%BA%94%E9%93%BE%E6%8A%95%E6%AF%92%E4%BA%8B%E4%BB%B6.assets/640-168913485161211.png)

但是查看该github公开源码，发现并没有相关函数；

![图片](%E7%9C%8BEDR%E5%A6%82%E4%BD%95%E6%8D%95%E8%8E%B7%E2%80%9D%E9%BB%91%E5%90%83%E9%BB%91%E2%80%9C%E7%9A%84%E4%BE%9B%E5%BA%94%E9%93%BE%E6%8A%95%E6%AF%92%E4%BA%8B%E4%BB%B6.assets/640-168913485161312.png)

当点击关闭按钮时，便会触发相关的消息函数，等同于启动该病毒程序。当启动关闭按钮时，隐藏在病毒程序中的shellcode便会开始执行，最终使运行计算机成为一台“肉鸡”，如下图所示；

![图片](%E7%9C%8BEDR%E5%A6%82%E4%BD%95%E6%8D%95%E8%8E%B7%E2%80%9D%E9%BB%91%E5%90%83%E9%BB%91%E2%80%9C%E7%9A%84%E4%BE%9B%E5%BA%94%E9%93%BE%E6%8A%95%E6%AF%92%E4%BA%8B%E4%BB%B6.assets/640-168913485161313.png)

![图片](%E7%9C%8BEDR%E5%A6%82%E4%BD%95%E6%8D%95%E8%8E%B7%E2%80%9D%E9%BB%91%E5%90%83%E9%BB%91%E2%80%9C%E7%9A%84%E4%BE%9B%E5%BA%94%E9%93%BE%E6%8A%95%E6%AF%92%E4%BA%8B%E4%BB%B6.assets/640-168913485161314.png)

进入该恶意函数后，首先会通过字符串数组索引相关下标的方式解密字符串；

![图片](%E7%9C%8BEDR%E5%A6%82%E4%BD%95%E6%8D%95%E8%8E%B7%E2%80%9D%E9%BB%91%E5%90%83%E9%BB%91%E2%80%9C%E7%9A%84%E4%BE%9B%E5%BA%94%E9%93%BE%E6%8A%95%E6%AF%92%E4%BA%8B%E4%BB%B6.assets/640-168913485161315.png)

![图片](%E7%9C%8BEDR%E5%A6%82%E4%BD%95%E6%8D%95%E8%8E%B7%E2%80%9D%E9%BB%91%E5%90%83%E9%BB%91%E2%80%9C%E7%9A%84%E4%BE%9B%E5%BA%94%E9%93%BE%E6%8A%95%E6%AF%92%E4%BA%8B%E4%BB%B6.assets/640-168913485161316.png)

该函数期间不断通过根据字符串获取相应的模块及函数地址，然后准备调用相关函数；

![图片](%E7%9C%8BEDR%E5%A6%82%E4%BD%95%E6%8D%95%E8%8E%B7%E2%80%9D%E9%BB%91%E5%90%83%E9%BB%91%E2%80%9C%E7%9A%84%E4%BE%9B%E5%BA%94%E9%93%BE%E6%8A%95%E6%AF%92%E4%BA%8B%E4%BB%B6.assets/640-168913485161317.png)

![图片](%E7%9C%8BEDR%E5%A6%82%E4%BD%95%E6%8D%95%E8%8E%B7%E2%80%9D%E9%BB%91%E5%90%83%E9%BB%91%E2%80%9C%E7%9A%84%E4%BE%9B%E5%BA%94%E9%93%BE%E6%8A%95%E6%AF%92%E4%BA%8B%E4%BB%B6.assets/640-168913485161318.png)

通过使用相关函数进行令牌访问，从而允许本程序启用特权允许进程执行以前无法执行的系统级操作；

![图片](%E7%9C%8BEDR%E5%A6%82%E4%BD%95%E6%8D%95%E8%8E%B7%E2%80%9D%E9%BB%91%E5%90%83%E9%BB%91%E2%80%9C%E7%9A%84%E4%BE%9B%E5%BA%94%E9%93%BE%E6%8A%95%E6%AF%92%E4%BA%8B%E4%BB%B6.assets/640-168913485161319.png)

通过遍历进程列表获取explore程序，该程序为待注入程序；

![图片](%E7%9C%8BEDR%E5%A6%82%E4%BD%95%E6%8D%95%E8%8E%B7%E2%80%9D%E9%BB%91%E5%90%83%E9%BB%91%E2%80%9C%E7%9A%84%E4%BE%9B%E5%BA%94%E9%93%BE%E6%8A%95%E6%AF%92%E4%BA%8B%E4%BB%B6.assets/640-168913485161320.png)

解密进程注入的shellcode，该shellcode使用zip压缩方式在病毒程序中保存；

![图片](%E7%9C%8BEDR%E5%A6%82%E4%BD%95%E6%8D%95%E8%8E%B7%E2%80%9D%E9%BB%91%E5%90%83%E9%BB%91%E2%80%9C%E7%9A%84%E4%BE%9B%E5%BA%94%E9%93%BE%E6%8A%95%E6%AF%92%E4%BA%8B%E4%BB%B6.assets/640-168913485161321.png)

使用VirtualAllocEx进行远程进程注入，注入成功并写入上文解压好的shellcode；

![图片](%E7%9C%8BEDR%E5%A6%82%E4%BD%95%E6%8D%95%E8%8E%B7%E2%80%9D%E9%BB%91%E5%90%83%E9%BB%91%E2%80%9C%E7%9A%84%E4%BE%9B%E5%BA%94%E9%93%BE%E6%8A%95%E6%AF%92%E4%BA%8B%E4%BB%B6.assets/640-168913485161322.png)

上文中的注入的explore程序位64位，而病毒程序代码为32位，所以需要取出在病毒程序中以uuid保存的shellcode代码，目的是使用**跨段跳转**，从32位切换到64位代码执行，从而保证注入的64位程序explore中的shellcode可以正常运行，最后跳转到恶意执行shellcode上;

![图片](%E7%9C%8BEDR%E5%A6%82%E4%BD%95%E6%8D%95%E8%8E%B7%E2%80%9D%E9%BB%91%E5%90%83%E9%BB%91%E2%80%9C%E7%9A%84%E4%BE%9B%E5%BA%94%E9%93%BE%E6%8A%95%E6%AF%92%E4%BA%8B%E4%BB%B6.assets/640-168913485161323.png)

由于这里涉及跨段跳转，使用windbg进行调试，当前运行到跨段shellcode入口；

![图片](%E7%9C%8BEDR%E5%A6%82%E4%BD%95%E6%8D%95%E8%8E%B7%E2%80%9D%E9%BB%91%E5%90%83%E9%BB%91%E2%80%9C%E7%9A%84%E4%BE%9B%E5%BA%94%E9%93%BE%E6%8A%95%E6%AF%92%E4%BA%8B%E4%BB%B6.assets/640-168913485161324.png)

该函数主要目的是跳转到下一行代码，在x64位下会使用段跳转；

![图片](%E7%9C%8BEDR%E5%A6%82%E4%BD%95%E6%8D%95%E8%8E%B7%E2%80%9D%E9%BB%91%E5%90%83%E9%BB%91%E2%80%9C%E7%9A%84%E4%BE%9B%E5%BA%94%E9%93%BE%E6%8A%95%E6%AF%92%E4%BA%8B%E4%BB%B6.assets/640-168913485161425.png)

当前调试为x64，所以会进行跨段跳转。构造段跳转堆栈参数，格式为cs:eip，cs在x32下为0x23，在64位下为0x33，于是该病毒程序通过此种方法成功绕过多个杀毒引擎检测；

![图片](%E7%9C%8BEDR%E5%A6%82%E4%BD%95%E6%8D%95%E8%8E%B7%E2%80%9D%E9%BB%91%E5%90%83%E9%BB%91%E2%80%9C%E7%9A%84%E4%BE%9B%E5%BA%94%E9%93%BE%E6%8A%95%E6%AF%92%E4%BA%8B%E4%BB%B6.assets/640-168913485161426.png)

![图片](%E7%9C%8BEDR%E5%A6%82%E4%BD%95%E6%8D%95%E8%8E%B7%E2%80%9D%E9%BB%91%E5%90%83%E9%BB%91%E2%80%9C%E7%9A%84%E4%BE%9B%E5%BA%94%E9%93%BE%E6%8A%95%E6%AF%92%E4%BA%8B%E4%BB%B6.assets/640-168913485161427.png)

根据特殊值获取线程函数地址，并将传入的注入进程explore的句柄以及恶意执行shellcode内存地址进行使用，最终进行远程线程注入，启动注入在explore的恶意shellcode;

![图片](%E7%9C%8BEDR%E5%A6%82%E4%BD%95%E6%8D%95%E8%8E%B7%E2%80%9D%E9%BB%91%E5%90%83%E9%BB%91%E2%80%9C%E7%9A%84%E4%BE%9B%E5%BA%94%E9%93%BE%E6%8A%95%E6%AF%92%E4%BA%8B%E4%BB%B6.assets/640-168913485161428.png)

![图片](%E7%9C%8BEDR%E5%A6%82%E4%BD%95%E6%8D%95%E8%8E%B7%E2%80%9D%E9%BB%91%E5%90%83%E9%BB%91%E2%80%9C%E7%9A%84%E4%BE%9B%E5%BA%94%E9%93%BE%E6%8A%95%E6%AF%92%E4%BA%8B%E4%BB%B6.assets/640-168913485161429.png)

通过监控注入进程explore的恶意shellcode的内存地址，成功断下入口点。shellcode通过PEB结构获取模块kernel32.dll内存地址，然后通过比对函数名来获取HeapCreate和RtlAllocateHeap的函数地址；

![图片](%E7%9C%8BEDR%E5%A6%82%E4%BD%95%E6%8D%95%E8%8E%B7%E2%80%9D%E9%BB%91%E5%90%83%E9%BB%91%E2%80%9C%E7%9A%84%E4%BE%9B%E5%BA%94%E9%93%BE%E6%8A%95%E6%AF%92%E4%BA%8B%E4%BB%B6.assets/640-168913485161430.png)

![图片](%E7%9C%8BEDR%E5%A6%82%E4%BD%95%E6%8D%95%E8%8E%B7%E2%80%9D%E9%BB%91%E5%90%83%E9%BB%91%E2%80%9C%E7%9A%84%E4%BE%9B%E5%BA%94%E9%93%BE%E6%8A%95%E6%AF%92%E4%BA%8B%E4%BB%B6.assets/640-168913485161431.png)

![图片](%E7%9C%8BEDR%E5%A6%82%E4%BD%95%E6%8D%95%E8%8E%B7%E2%80%9D%E9%BB%91%E5%90%83%E9%BB%91%E2%80%9C%E7%9A%84%E4%BE%9B%E5%BA%94%E9%93%BE%E6%8A%95%E6%AF%92%E4%BA%8B%E4%BB%B6.assets/640-168913485161432.png)

获取相关函数地址后，引用函数申请内存，准备解密实际恶意代码；

![图片](%E7%9C%8BEDR%E5%A6%82%E4%BD%95%E6%8D%95%E8%8E%B7%E2%80%9D%E9%BB%91%E5%90%83%E9%BB%91%E2%80%9C%E7%9A%84%E4%BE%9B%E5%BA%94%E9%93%BE%E6%8A%95%E6%AF%92%E4%BA%8B%E4%BB%B6.assets/640-168913485161433.png)

解密后的shellcode为CobaltStrike的小马，dump相关内存解析其配置，其C2伪装为正常域名；

![图片](%E7%9C%8BEDR%E5%A6%82%E4%BD%95%E6%8D%95%E8%8E%B7%E2%80%9D%E9%BB%91%E5%90%83%E9%BB%91%E2%80%9C%E7%9A%84%E4%BE%9B%E5%BA%94%E9%93%BE%E6%8A%95%E6%AF%92%E4%BA%8B%E4%BB%B6.assets/640-168913485161434.png)

![图片](%E7%9C%8BEDR%E5%A6%82%E4%BD%95%E6%8D%95%E8%8E%B7%E2%80%9D%E9%BB%91%E5%90%83%E9%BB%91%E2%80%9C%E7%9A%84%E4%BE%9B%E5%BA%94%E9%93%BE%E6%8A%95%E6%AF%92%E4%BA%8B%E4%BB%B6.assets/640-168913485161435.png)

继续执行相关的小马，会发现下载了一张疑似正常的图片，访问该服务器链接也是正常的图片；

![图片](%E7%9C%8BEDR%E5%A6%82%E4%BD%95%E6%8D%95%E8%8E%B7%E2%80%9D%E9%BB%91%E5%90%83%E9%BB%91%E2%80%9C%E7%9A%84%E4%BE%9B%E5%BA%94%E9%93%BE%E6%8A%95%E6%AF%92%E4%BA%8B%E4%BB%B6.assets/640-168913485161436.png)

![图片](%E7%9C%8BEDR%E5%A6%82%E4%BD%95%E6%8D%95%E8%8E%B7%E2%80%9D%E9%BB%91%E5%90%83%E9%BB%91%E2%80%9C%E7%9A%84%E4%BE%9B%E5%BA%94%E9%93%BE%E6%8A%95%E6%AF%92%E4%BA%8B%E4%BB%B6.assets/640-168913485161437.png)

但是在偏移0x8FD的位置为CobaltStrike的大马，dump其内存并解析相关配置信息如下所示；

![图片](%E7%9C%8BEDR%E5%A6%82%E4%BD%95%E6%8D%95%E8%8E%B7%E2%80%9D%E9%BB%91%E5%90%83%E9%BB%91%E2%80%9C%E7%9A%84%E4%BE%9B%E5%BA%94%E9%93%BE%E6%8A%95%E6%AF%92%E4%BA%8B%E4%BB%B6.assets/640-168913485161438.png)

![图片](%E7%9C%8BEDR%E5%A6%82%E4%BD%95%E6%8D%95%E8%8E%B7%E2%80%9D%E9%BB%91%E5%90%83%E9%BB%91%E2%80%9C%E7%9A%84%E4%BE%9B%E5%BA%94%E9%93%BE%E6%8A%95%E6%AF%92%E4%BA%8B%E4%BB%B6.assets/640-168913485161439.png)

可以确定本次事件是以投放CobaltStrike木马发展相关免杀爱好者设备的肉鸡为最终目的，相关沙箱检测如下所示；

![图片](%E7%9C%8BEDR%E5%A6%82%E4%BD%95%E6%8D%95%E8%8E%B7%E2%80%9D%E9%BB%91%E5%90%83%E9%BB%91%E2%80%9C%E7%9A%84%E4%BE%9B%E5%BA%94%E9%93%BE%E6%8A%95%E6%AF%92%E4%BA%8B%E4%BB%B6.assets/640-168913485161540.png)

### **0x03 关联分析**

通过对该样本其中的特征进行提取后，然后在微步样本库中进行hunting，发现该攻击者历史上至少存在以下三个版本。

#### 1、版本信息

<table><tbody><tr><td width="67"><p class="both">出现时间</p></td><td width="55"><p class="both">文件名</p></td><td width="245"><p class="both">Sha256</p></td><td width="143"><p class="both">C&amp;C</p></td></tr><tr><td width="19"><p class="both">2022-10-22</p></td><td width="75"><p class="both">Fonts.dll</p></td><td width="245"><p class="both">05c13c2468f146808732b737e34e04b</p><p class="both">00356e3f722abd1b48f7184543c3d0b42</p></td><td width="123"><p class="both">Shellcode有问题，无C&amp;C</p></td></tr><tr><td width="19"><p class="both">2023-02-28</p></td><td width="75"><p class="both">zlibwapi.dll</p></td><td width="245"><p class="both">8e2c54bf0c6bc70232063182bfb105dbb</p><p class="both">5790784e59042acd5f691cc4f9e9de5</p></td><td width="123"><p class="both">update.exchange.ac.cn</p></td></tr><tr><td width="19"><p class="both">2023-03-01</p></td><td width="75"><p class="both">CcRemote.exe</p></td><td width="245"><p class="both">9a9b5258c771d025cbbeff42e21fdec09</p><p class="both">e1560495764afa22bd752a71cb15744</p></td><td width="123"><p class="both">update.exchange.ac.cn</p></td></tr></tbody></table>

可以发现该攻击者除了本次github投毒事件外，至少还针对输入法以及Zlib压缩工具等进行投毒。

#### 2、溯源信息

通过使用X社区的Graph功能，确定C&C：update.exchange.ac.cn拥有者历史上使用过C&C:update.1drv.info，两个C&C均历史绑定过腾讯云ip: 1.12.229.12。相关C&C域名的注册者经过查询为相关的域名贩卖者。

![图片](%E7%9C%8BEDR%E5%A6%82%E4%BD%95%E6%8D%95%E8%8E%B7%E2%80%9D%E9%BB%91%E5%90%83%E9%BB%91%E2%80%9C%E7%9A%84%E4%BE%9B%E5%BA%94%E9%93%BE%E6%8A%95%E6%AF%92%E4%BA%8B%E4%BB%B6.assets/640-168913485161541.png)

![图片](%E7%9C%8BEDR%E5%A6%82%E4%BD%95%E6%8D%95%E8%8E%B7%E2%80%9D%E9%BB%91%E5%90%83%E9%BB%91%E2%80%9C%E7%9A%84%E4%BE%9B%E5%BA%94%E9%93%BE%E6%8A%95%E6%AF%92%E4%BA%8B%E4%BB%B6.assets/640-168913485161542.png)

**总结**
------

    通过对这个投毒事件的分析，我们对该木马工具的行为和特征有了更深入的了解，猜测其目的可能包含以下几个：

1.  窃取敏感信息：该组织通过对免杀版工具进行投毒，有针对性地攻击专门从事网络安全行业的研究者们的终端；利用后门工具窃取终端中的敏感信息，如用户名、密码、证书，甚至是所服务的客户数据等。

2.  窃取知识产权：近年来攻防演练为行业内大火，如果安全人员从事的是红队研究类相关工作，该组织可能会利用后门工具窃取其电脑中的Red Team工具、0day技术文档等知识产权。

3.  进行进一步攻击：该组织可以利用后门工具在安全人员电脑中植入恶意代码，进而进行更为深入和广泛的攻击，如攻击受害者所服务的企业、整个网络甚至国家。

因此，我们需要时刻保持警惕，防范网络上传播的各种工具后门软件的攻击也是非常重要的。

**处置建议**

如果屏幕前的你发现自己好巧不巧刚好用过这个工具，就要尽快采取以下处置措施了：

1. 排查安装有该后门软件的主机，及时清理。

2. 断开与攻击源的连接：立即断开与该工具的连接，以防止攻击者继续操纵和控制终端；然后需要立即删除相关文件并清理系统。

3. 修改密码和加强身份验证：攻击者可能会利用该工具获取了终端上保存的用户名和密码等敏感信息，因此，需要及时修改其密码，并加强身份验证措施，例如使用双重认证等措施。

4. 加强软件下载的安全管理：我们需要加强对开源工具安全的管理和控制，例如在下载前注意审查该开源工具的源码、检查其依赖项、建立安全审查和评估机制等，以减少投毒事件的风险。

   * * *

