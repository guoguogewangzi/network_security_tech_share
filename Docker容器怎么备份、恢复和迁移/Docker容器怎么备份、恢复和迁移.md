[Docker容器怎么备份、恢复和迁移](https://www.yisu.com/zixun/580623.html)
============================================================

2022-05-26 14:47:45

本文小编为大家详细介绍“Docker容器怎么备份、恢复和迁移”，内容详细，步骤清晰，细节处理妥当，希望这篇“Docker容器怎么备份、恢复和迁移”文章能帮助大家解决疑惑，下面跟着小编的思路慢慢深入，一起来学习新知识吧。

**1\. 备份容器**

首先，为了备份docker中的容器，我们会想看看我们想要备份的容器列表。要达成该目的，我们需要在我们运行着docker引擎，并已创建了容器的linux机器中运行 docker ps 命令。  

```
# docker ps
```

![Docker容器怎么备份、恢复和迁移](Docker%E5%AE%B9%E5%99%A8%E6%80%8E%E4%B9%88%E5%A4%87%E4%BB%BD%E3%80%81%E6%81%A2%E5%A4%8D%E5%92%8C%E8%BF%81%E7%A7%BB.assets/28024.png)

在此之后，我们要选择我们想要备份的容器，然后去创建该容器的快照。我们可以使用 docker commit 命令来创建快照。  

```
# docker commit -p 30b8f18f20b4 container-backup
```

![Docker容器怎么备份、恢复和迁移](Docker%E5%AE%B9%E5%99%A8%E6%80%8E%E4%B9%88%E5%A4%87%E4%BB%BD%E3%80%81%E6%81%A2%E5%A4%8D%E5%92%8C%E8%BF%81%E7%A7%BB.assets/28025.jpg)

该命令会生成一个作为docker镜像的容器快照，我们可以通过运行 docker images 命令来查看docker镜像，如下。  

```
# docker images
```

![Docker容器怎么备份、恢复和迁移](Docker%E5%AE%B9%E5%99%A8%E6%80%8E%E4%B9%88%E5%A4%87%E4%BB%BD%E3%80%81%E6%81%A2%E5%A4%8D%E5%92%8C%E8%BF%81%E7%A7%BB.assets/28026.png)

正如我们所看见的，上面做的快照已经作为docker镜像保存了。现在，为了备份该快照，我们有两个选择，一个是我们可以登录进docker注册中心，并推送该镜像;另一个是我们可以将docker镜像打包成tar包备份，以供今后使用。  

如果我们想要在docker注册中心上传或备份镜像，我们只需要运行 docker login 命令来登录进docker注册中心，然后推送所需的镜像即可。

```
# docker login
```

![Docker容器怎么备份、恢复和迁移](Docker%E5%AE%B9%E5%99%A8%E6%80%8E%E4%B9%88%E5%A4%87%E4%BB%BD%E3%80%81%E6%81%A2%E5%A4%8D%E5%92%8C%E8%BF%81%E7%A7%BB.assets/28027.png)

复制代码 代码如下:

\# docker tag a25ddfec4d2a arunpyasi/container-backup:test# docker push arunpyasi/container-backup

![Docker容器怎么备份、恢复和迁移](Docker%E5%AE%B9%E5%99%A8%E6%80%8E%E4%B9%88%E5%A4%87%E4%BB%BD%E3%80%81%E6%81%A2%E5%A4%8D%E5%92%8C%E8%BF%81%E7%A7%BB.assets/28028.png)

如果我们不想备份到，而是想要将此镜像保存在本地机器中，以供日后使用，那么我们可以将其作为tar包备份。要完成该操作，我们需要运行以下 docker save 命令。  

```
# docker save -o ~/container-backup.tar container-backup
```

![Docker容器怎么备份、恢复和迁移](Docker%E5%AE%B9%E5%99%A8%E6%80%8E%E4%B9%88%E5%A4%87%E4%BB%BD%E3%80%81%E6%81%A2%E5%A4%8D%E5%92%8C%E8%BF%81%E7%A7%BB.assets/28029.jpg)

要验证tar包是否已经生成，我们只需要在保存tar包的目录中运行 ls 命令即可。  

**2\. 恢复容器**

接下来，在我们成功备份了我们的docker容器后，我们现在来恢复这些制作了docker镜像快照的容器。如果我们已经在注册中心推送了这些docker镜像，那么我们仅仅需要把那个docker镜像拖回并直接运行即可。  

```
# docker pull arunpyasi/container-backup:test
```

![Docker容器怎么备份、恢复和迁移](Docker%E5%AE%B9%E5%99%A8%E6%80%8E%E4%B9%88%E5%A4%87%E4%BB%BD%E3%80%81%E6%81%A2%E5%A4%8D%E5%92%8C%E8%BF%81%E7%A7%BB.assets/28030.jpg)  

但是，如果我们将这些docker镜像作为tar包文件备份到了本地，那么我们只要使用 docker load 命令，后面加上tar包的备份路径，就可以加载该docker镜像了。

```
# docker load -i ~/container-backup.tar
```

现在，为了确保这些docker镜像已经加载成功，我们来运行 docker images 命令。  

```
# docker images
```

在镜像被加载后，我们将用加载的镜像去运行docker容器。  

```
# docker run -d -p 80:80 container-backup
```

![Docker容器怎么备份、恢复和迁移](Docker%E5%AE%B9%E5%99%A8%E6%80%8E%E4%B9%88%E5%A4%87%E4%BB%BD%E3%80%81%E6%81%A2%E5%A4%8D%E5%92%8C%E8%BF%81%E7%A7%BB.assets/28031.jpg)

**3\. 迁移docker容器**

迁移容器同时涉及到了上面两个操作，备份和恢复。我们可以将任何一个docker容器从一台机器迁移到另一台机器。在迁移过程中，首先我们将把容器备份为docker镜像快照。然后，该docker镜像或者是被推送到了docker注册中心，或者被作为tar包文件保存到了本地。如果我们将镜像推送到了docker注册中心，我们简单地从任何我们想要的机器上使用 docker run 命令来恢复并运行该容器。但是，如果我们将镜像打包成tar包备份到了本地，我们只需要拷贝或移动该镜像到我们想要的机器上，加载该镜像并运行需要的容器即可。  

读到这里，这篇“Docker容器怎么备份、恢复和迁移”文章已经介绍完毕，想要掌握这篇文章的知识点还需要大家自己动手实践使用过才能领会，如果想了解更多相关内容的文章，欢迎关注亿速云行业资讯频道。