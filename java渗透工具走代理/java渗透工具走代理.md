# 0x01开代理：全局路由

![image-20240104153038762](java%E6%B8%97%E9%80%8F%E5%B7%A5%E5%85%B7%E8%B5%B0%E4%BB%A3%E7%90%86.assets/image-20240104153038762.png)



## 监听端口：10808

![image-20240104153300536](java%E6%B8%97%E9%80%8F%E5%B7%A5%E5%85%B7%E8%B5%B0%E4%BB%A3%E7%90%86.assets/image-20240104153300536.png)



# 0x02开proxifier



设置代理服务器

![image-20240104153155334](java%E6%B8%97%E9%80%8F%E5%B7%A5%E5%85%B7%E8%B5%B0%E4%BB%A3%E7%90%86.assets/image-20240104153155334.png)





## 设置代理规则

![image-20240104153354996](java%E6%B8%97%E9%80%8F%E5%B7%A5%E5%85%B7%E8%B5%B0%E4%BB%A3%E7%90%86.assets/image-20240104153354996.png)



# 0x03 HttpExample2.java：http请求jsonip.com获取公网ip

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;

public class HttpExample2 {
    public static void main(String[] args) {
        try {
            // 创建URL对象
            URL url = new URL("https://jsonip.com/"); // 替换为实际的URL
            // 打开HTTP连接
            HttpURLConnection connection = (HttpURLConnection) url.openConnection();
            // 设置请求方法为GET
            connection.setRequestMethod("GET");
			
            int responseCode = connection.getResponseCode();
            System.out.println("Response Code: " + responseCode);
            // 读取响应内容
            BufferedReader reader = new BufferedReader(new InputStreamReader(connection.getInputStream()));
            String line;
            StringBuilder response = new StringBuilder();
            while ((line = reader.readLine()) != null) {
                response.append(line);
            }
            reader.close();
            // 打印响应内容
            System.out.println("Response Body: " + response.toString());
            // 关闭连接
            connection.disconnect();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

```



# 0x04 HttpExample.java：http请求python3开启http服务，查看请求的ip

```java
import java.io.BufferedReader;
import java.io.InputStreamReader;
import java.net.HttpURLConnection;
import java.net.URL;

public class HttpExample {
    public static void main(String[] args) {
        try {
            // 创建URL对象
            URL url = new URL("http://42.194.233.78:8000/"); // 替换为实际的URL
            // 打开HTTP连接
            HttpURLConnection connection = (HttpURLConnection) url.openConnection();
            // 设置请求方法为GET
            connection.setRequestMethod("GET");
			
            int responseCode = connection.getResponseCode();
            System.out.println("Response Code: " + responseCode);
            // 读取响应内容
            BufferedReader reader = new BufferedReader(new InputStreamReader(connection.getInputStream()));
            String line;
            StringBuilder response = new StringBuilder();
            while ((line = reader.readLine()) != null) {
                response.append(line);
            }
            reader.close();
            // 打印响应内容
            System.out.println("Response Body: " + response.toString());
            // 关闭连接
            connection.disconnect();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

```



## 编译

`javac HttpExample.java`

## 生成

`HttpExample.class`



## 编译

`javac HttpExample2.java`

## 生成

`HttpExample2.class`





# 0x05开启python3 http服务

![image-20240104153727765](java%E6%B8%97%E9%80%8F%E5%B7%A5%E5%85%B7%E8%B5%B0%E4%BB%A3%E7%90%86.assets/image-20240104153727765.png)









# 0x06执行HttpExample2.class：`java HttpExample2`，公网IP为代理ip

![image-20240104154159848](java%E6%B8%97%E9%80%8F%E5%B7%A5%E5%85%B7%E8%B5%B0%E4%BB%A3%E7%90%86.assets/image-20240104154159848.png)

## 查看v2rayN日志，【socks->proxy】，已走代理

![image-20240104154545043](java%E6%B8%97%E9%80%8F%E5%B7%A5%E5%85%B7%E8%B5%B0%E4%BB%A3%E7%90%86.assets/image-20240104154545043.png)



# 0x07执行HttpExample.class:`java HttpExample`

![image-20240104154403127](java%E6%B8%97%E9%80%8F%E5%B7%A5%E5%85%B7%E8%B5%B0%E4%BB%A3%E7%90%86.assets/image-20240104154403127.png)

## 请求ip为代理ip

![image-20240104154417013](java%E6%B8%97%E9%80%8F%E5%B7%A5%E5%85%B7%E8%B5%B0%E4%BB%A3%E7%90%86.assets/image-20240104154417013.png)





## 查看v2rayN日志，【socks->proxy】，已走代理

![image-20240104154800273](java%E6%B8%97%E9%80%8F%E5%B7%A5%E5%85%B7%E8%B5%B0%E4%BB%A3%E7%90%86.assets/image-20240104154800273.png)



# 0x08部署cve-2018-2894漏洞环境

![image-20240104160115917](java%E6%B8%97%E9%80%8F%E5%B7%A5%E5%85%B7%E8%B5%B0%E4%BB%A3%E7%90%86.assets/image-20240104160115917.png)







# 0x09 webshell连接

![image-20240104160148923](java%E6%B8%97%E9%80%8F%E5%B7%A5%E5%85%B7%E8%B5%B0%E4%BB%A3%E7%90%86.assets/image-20240104160148923.png)







# 0x10`tcpdump -i eth0 -w 1.pcap -vv` 抓包

![image-20240104160222144](java%E6%B8%97%E9%80%8F%E5%B7%A5%E5%85%B7%E8%B5%B0%E4%BB%A3%E7%90%86.assets/image-20240104160222144.png)





# 0x11执行命令

![image-20240104160322755](java%E6%B8%97%E9%80%8F%E5%B7%A5%E5%85%B7%E8%B5%B0%E4%BB%A3%E7%90%86.assets/image-20240104160322755.png)



# 0x12数据包查看攻击IP为代理IP

![image-20240104160426737](java%E6%B8%97%E9%80%8F%E5%B7%A5%E5%85%B7%E8%B5%B0%E4%BB%A3%E7%90%86.assets/image-20240104160426737.png)

