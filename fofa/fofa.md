![image-20211001012404125](img/fofa/image-20211001012404125.png)

**1、FOFA通过title网站标题**

![image-20211001012656259](img/fofa/image-20211001012656259.png)



**fofa语句：**

```fofa
title="深圳职业技术学院"
```

![image-20211001012902238](img/fofa/image-20211001012902238.png)



**2、FOFA通过body页面内容**

![image-20211001013424490](img/fofa/image-20211001013424490.png)



**fofa语句：**

```fofa
body="深圳职业技术学院创建于1993年"
```

![image-20211001013452679](img/fofa/image-20211001013452679.png)



**3、FOFA通过domain可以搜索子域名**

**fofa语句：**

```fofa
domain="szpt.edu.cn"
```

![image-20211001014032605](img/fofa/image-20211001014032605.png)



**4、FOFA根据地区搜索**

country="CN" 搜索指定国家(编码)的资产

region="Xinjiang"  搜索指定行政区的资产

city="beijing" 搜索指定城市的资产



**0x01 fofa语句：国家**

```fofa
title="深圳职业技术学院" && country="CN"
```

![image-20211001014705208](img/fofa/image-20211001014705208.png)



**0x02 fofa语句：地区**

```fofa
domain="xuegod.cn" && region="HK"
```

![image-20211001015005197](img/fofa/image-20211001015005197.png)



**5、FOFA通过icon图标搜索资产**

**0x01 获取ico图片**

![image-20211001015916053](img/fofa/image-20211001015916053.png)



**0x02 上传icon图标**

![image-20211001020104596](img/fofa/image-20211001020104596.png)



**6、FOFA通过JavaScript文件查询**

![image-20211001020845315](img/fofa/image-20211001020845315.png)



**fofa语句：**

```fofa
js_name="/system/resource/js/counter.js" && domain="szpt.edu.cn"
```

![image-20211001021158097](img/fofa/image-20211001021158097.png)



**7、通过使用FOFA规则列表搜索CMS资产**

**0x01 点击规则列表**

![image-20211001022052202](img/fofa/image-20211001022052202.png)

**0x02 可以通过< >左右翻页，点击其中"PHPSHE"进行搜索**

![image-20211001022340000](img/fofa/image-20211001022340000.png)



**fofa语句：**

```fofa
app="PHPSHE"
```

![image-20211001022625560](img/fofa/image-20211001022625560.png)





**结束！**

