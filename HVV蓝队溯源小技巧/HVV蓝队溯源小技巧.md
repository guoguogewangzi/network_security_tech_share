[HVV蓝队溯源小技巧](https://mp.weixin.qq.com/s?__biz=MzkwMzUyNDIwMA==&mid=2247483864&idx=1&sn=08b721fa57f8c59c4d405d7f5eadfd9f)
========================================================================================================================

安全社2023-08-11 09:18本文共 561 字阅读完需 2.5 分钟

> 🔰Microsoft Office的溯源小技巧🔰

## 🔰0x01 前言

在HVV中，钓鱼手段还是比较吃香的，特别是邮件钓鱼和私信钓鱼的场景中。在钓鱼的时候最常见的就是word文档捆马。本文会介绍在**Microsoft Office**系列产品的溯源小技巧。

## 🔰0x02 沙盘推演

✅钓鱼者使用**Microsoft Office**系列产品

✅钓鱼者登录**Office账号**（笔记本用户常见）

❌有几率显示作者昵称不显示邮箱

❓WPS未测

## 🔰0x03 Word溯源

1.把docx改成压缩包格式 如：zip

2.打开压缩包下的**/docProps/core.xml**即可显示Word作者的邮箱

![图片](HVV%E8%93%9D%E9%98%9F%E6%BA%AF%E6%BA%90%E5%B0%8F%E6%8A%80%E5%B7%A7.assets/640.png)

## 🔰0x04 Excel溯源

1.把Excel改成压缩包格式 如：zip

2.打开压缩包下的**/xl/workbook.xml**即可显示路径地址

![图片](HVV%E8%93%9D%E9%98%9F%E6%BA%AF%E6%BA%90%E5%B0%8F%E6%8A%80%E5%B7%A7.assets/640-16919127283491.png)

## 🔰0x05 PPT溯源

1.把pptx改成压缩包格式 如：zip

2.打开压缩包下的**/docProps/core.xml**即可显示PPT作者的邮箱

![图片](HVV%E8%93%9D%E9%98%9F%E6%BA%AF%E6%BA%90%E5%B0%8F%E6%8A%80%E5%B7%A7.assets/640-16919127283492.png)

🔰0x06 结束语

**蓝队寄语：别给我逮住了，逮住了遭老罪咯！**

**红队寄语：做好信息保护，防止溯源！**

**作者寄语：参加这次国H的兄弟们，希望都打出好成绩，开出完美的第一炮！收到钓鱼邮件先别急着丢沙箱，试一试这个小技巧也许有意想不到的收获。因为在实战中很多RT都是使用虚拟机进行的，制作钓鱼邮件因为虚拟机里大概率没有按照环境，所以上物理机生成Word！**







# 实战测试



## 0x01 Word溯源

### 1）新建`.docx文档`

<img src="HVV%E8%93%9D%E9%98%9F%E6%BA%AF%E6%BA%90%E5%B0%8F%E6%8A%80%E5%B7%A7.assets/image-20230813155843477.png" alt="image-20230813155843477" style="zoom:33%;" />





### 2）新建成功，文件名：123.docx

![image-20230813155953546](HVV%E8%93%9D%E9%98%9F%E6%BA%AF%E6%BA%90%E5%B0%8F%E6%8A%80%E5%B7%A7.assets/image-20230813155953546.png)



### 3）确定账号已登录

<img src="HVV%E8%93%9D%E9%98%9F%E6%BA%AF%E6%BA%90%E5%B0%8F%E6%8A%80%E5%B7%A7.assets/image-20230813160039389.png" alt="image-20230813160039389" style="zoom:33%;" />



### 4）改后缀为zip

![image-20230813160135693](HVV%E8%93%9D%E9%98%9F%E6%BA%AF%E6%BA%90%E5%B0%8F%E6%8A%80%E5%B7%A7.assets/image-20230813160135693.png)

### 5）双击打开，找到文件：`123.zip/docProps/core.xml`

<img src="HVV%E8%93%9D%E9%98%9F%E6%BA%AF%E6%BA%90%E5%B0%8F%E6%8A%80%E5%B7%A7.assets/image-20230813160203714.png" alt="image-20230813160203714" style="zoom:33%;" />





### 6）打开`core.xml`,此处可以看到`作者昵称`

<img src="HVV%E8%93%9D%E9%98%9F%E6%BA%AF%E6%BA%90%E5%B0%8F%E6%8A%80%E5%B7%A7.assets/image-20230813160436726.png" alt="image-20230813160436726" style="zoom:33%;" />





## 0x02 Excel溯源

### 1）新建`.xlsx文档`

<img src="HVV%E8%93%9D%E9%98%9F%E6%BA%AF%E6%BA%90%E5%B0%8F%E6%8A%80%E5%B7%A7.assets/image-20230813161445290.png" alt="image-20230813161445290" style="zoom:33%;" />



### 2）同上，注：确保账户已登录



### 3）打开`core.xml`,此处可以看到`作者昵称`

<img src="HVV%E8%93%9D%E9%98%9F%E6%BA%AF%E6%BA%90%E5%B0%8F%E6%8A%80%E5%B7%A7.assets/image-20230813161910094.png" alt="image-20230813161910094" style="zoom:33%;" />







## 0x03 PPT溯源

### 1）新建`.pptx文档`

<img src="HVV%E8%93%9D%E9%98%9F%E6%BA%AF%E6%BA%90%E5%B0%8F%E6%8A%80%E5%B7%A7.assets/image-20230813162057773.png" alt="image-20230813162057773" style="zoom:33%;" />

### 2）同上，注：确保账户已登录







### 3）打开`core.xml`,此处可以看到`作者昵称`

<img src="HVV%E8%93%9D%E9%98%9F%E6%BA%AF%E6%BA%90%E5%B0%8F%E6%8A%80%E5%B7%A7.assets/image-20230813162209538.png" alt="image-20230813162209538" style="zoom:33%;" />