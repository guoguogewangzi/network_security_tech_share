- [ JavaScript、AJAX、CGI基础](#head1)
	- [ 一、网页使用js的三种方式](#head2)
		- [ 1、直接添加脚本](#head3)
			- [0x01 1.html文件](#head4)
			- [0x02 结果：](#head5)
		- [ 2、使用script标记插入脚本](#head6)
			- [0x01 1.html文件](#head7)
			- [0x02 结果：](#head8)
		- [ 3、链接脚本文件（最常用）](#head9)
			- [0x01 1.html文件](#head10)
			- [0x02 1.js](#head11)
			- [0x03 实操：](#head12)
			- [0x04 1.html文件](#head13)
			- [0x05 1.js文件](#head14)
			- [0x06 结果：访问：http://127.0.0.1:8000/1.html，成功调用1.js文件](#head15)
	- [二、JavaScript基础 ](#head16)
		- [ 1、JS支持两种类型注释](#head17)
		- [ 2、JS变量](#head18)
			- [ (1)变量的声明](#head19)
			- [ (2)变量的命名](#head20)
			- [ (3)变量区别大小写](#head21)
		- [ 3、JS数据类型](#head22)
		- [ 4、JS运算符](#head23)
		- [ 5、JS控制语句](#head24)
		- [ 6、JS函数](#head25)
			- [0x01 例子：](#head26)
			- [0x02 结果：](#head27)
		- [ 7、JS对象](#head28)
			- [ （1）window对象](#head29)
				- [0x00 举例：](#head30)
				- [0x01 1.html文件](#head31)
				- [0x02 1.js文件](#head32)
				- [0x03 结果：](#head33)
			- [ （2）data对象](#head34)
				- [0x00 举例：](#head35)
				- [0x01 1.html文件](#head36)
				- [0x02 1.js文件](#head37)
				- [0x03 结果：](#head38)
			- [ （3）setTimeout函数和setInterval函数](#head39)
				- [0x00 举例：](#head40)
				- [0x01 1.html文件](#head41)
				- [0x02 1.js文件](#head42)
				- [0x03 结果：](#head43)
			- [ （4）Math对象](#head44)
				- [0x00 举例：](#head45)
				- [0x01 html文件](#head46)
				- [0x02 js文件](#head47)
				- [0x03 结果：](#head48)
			- [ （5）String对象](#head49)
				- [0x00 例子：字符串本身就是一个对象](#head50)
			- [ （6）全局函数](#head51)
	- [ 三、ajax局部更新网页流程图：浏览器操作服务器](#head52)
		- [ 1、流程](#head53)
		- [ 2、根据不同的浏览器插件异步请求对象：](#head54)
		- [ 3、标准的XMLHttpRequest属性](#head55)
		- [ 4、标准的XMLHttpRequest方法](#head56)
		- [ 5、异步流程：](#head57)
		- [ 6、同步流程：（很少用到）](#head58)
		- [ 7、举例：通过ajax，浏览器获取服务器端指定的文件内容](#head59)
			- [0x01 1.html文件](#head60)
			- [0x02 1.js文件](#head61)
			- [0x03 file.txt文件](#head62)
			- [0x04 结果：](#head63)
	- [ 四、CGI编程](#head64)
		- [ 1、什么是CGI?](#head65)
		- [ 2、举例：](#head66)
			- [0x01 利用python2.7开启http服务](#head67)
			- [0x02 cgi.html文件](#head68)
			- [0x03 cgi.c文件](#head69)
			- [0x04 编译cig.c文件](#head70)
			- [0x05 目录结构如下：](#head71)
			- [0x06 结果：](#head72)
		- [ 3、案例2：以get形式发送内容](#head73)
			- [0x01 cgi.html文件](#head74)
			- [0x02 cgi.js文件](#head75)
			- [0x03 cgi2.c文件](#head76)
			- [0x04 编译cgi2.c文件](#head77)
			- [0x05 目录结构如下：](#head78)
			- [0x06 结果：](#head79)
		- [ 4、案例3：以post形式发送请求](#head80)
			- [0x01 ajax.html文件](#head81)
			- [0x02 cgi.js文件](#head82)
			- [0x03 cgi3.c文件](#head83)
			- [0x04 编译cgi3.c文件](#head84)
			- [0x05 目录结构如下：](#head85)
			- [0x06 结果：](#head86)

	- [参考](#head87)


# <span id="head1"> JavaScript、AJAX、CGI基础</span>



## <span id="head2"> 一、网页使用js的三种方式</span>

首先开启http服务：

```shell
C:\Users\86188\Desktop>python3 -m http.server
```

<img src="assets/image-20211017213619885.png" alt="image-20211017213619885" style="zoom:70%;" />


### <span id="head3"> 1、直接添加脚本</span>

#### <span id="head4">0x01 1.html文件</span>

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<!-- 直接添加脚本 -->
<input type="button" value="Click me" onclick="alert('hello world');"> 
</body>
</html>
```

#### <span id="head5">0x02 结果：</span>

![image-20210929151718115](assets//image-20210929151718115.png)

### <span id="head6"> 2、使用script标记插入脚本</span>

#### <span id="head7">0x01 1.html文件</span>

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>

<body>
<!-- 使用script标记插入脚本 -->
<script type="text/javascript">
    alert("hello world")
</script>
</body>

</html>
```

#### <span id="head8">0x02 结果：</span>

![image-20210929151803788](assets//image-20210929151803788.png)

### <span id="head9"> 3、链接脚本文件（最常用）</span>

#### <span id="head10">0x01 1.html文件</span>

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
    <!-- 链接脚本文件 -->
    <!-- 声明：哪个js文件，在访问本文件时，会去请求1.js资源 -->
    <script type="text/javascript" src="1.js"></script>
<body>
    <!-- 调用：通过onclick属性值 -->
    <input type="button" name="mybutton" value="点击一下" onclick="myfun()">
</body>

</html>
```

#### <span id="head11">0x02 1.js</span>

 ```js
 function myfun(){
 	alert("oh, see you the third time!");
 	alert("Bye Bye!");     
 }
 ```

#### <span id="head12">0x03 实操：</span>

#### <span id="head13">0x04 1.html文件</span>

![image-20210929152439323](assets//image-20210929152439323.png) 

#### <span id="head14">0x05 1.js文件</span>

 ![image-20210929152430135](assets//image-20210929152430135.png)

 

#### <span id="head15">0x06 结果：访问：http://127.0.0.1:8000/1.html，成功调用1.js文件</span>

![image-20210929152351664](assets//image-20210929152351664.png) 

 

## <span id="head16">二、JavaScript基础 </span>

### <span id="head17"> 1、JS支持两种类型注释</span>

(1)行注释：（//注释）

(2)快注释：（/\*注释\*/）



### <span id="head18"> 2、JS变量</span>

#### <span id="head19"> (1)变量的声明</span>

使用var关键字进行变量的声明：var x;

声明的时候可以同时对变量初始化：var y=4;

使用逗号将多个变量隔开：var x,y=5,z='hello';

#### <span id="head20"> (2)变量的命名</span>

变量必须由字母，数字和下划线组成

字母或者是下划线开头，不可以是数字

变量不可以是保留字

#### <span id="head21"> (3)变量区别大小写</span>



### <span id="head22"> 3、JS数据类型</span>

![image-20210929154250796](assets//image-20210929154250796.png)

### <span id="head23"> 4、JS运算符</span>

![image-20210929154508939](assets//image-20210929154508939.png)

### <span id="head24"> 5、JS控制语句</span>

![image-20210929154601770](assets//image-20210929154601770.png)



![image-20210929154804499](assets//image-20210929154804499.png)



![image-20210929154932715](assets//image-20210929154932715.png)

### <span id="head25"> 6、JS函数</span>

![image-20210929155026614](assets//image-20210929155026614.png)

#### <span id="head26">0x01 例子：</span>

1.html

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
    <!-- 链接脚本文件 -->
    <script type="text/javascript" src="1.js"></script>
<body>
    <input type="button" name="mybutton" value="点击一下" onclick="fun1()">
</body>

</html>
```

1.js

```js
function myfun(arg){
    var a =100;
    alert(typeof(a));

    var str = arg;
    alert(str);
    return a;
}

function fun1(){
    var b ="b = ";  //js中：变量的数据类型是根据值来决定
    b+= myfun(10);  //返回的值是数字可以当作字符串使用
    alert(b)
}
```



#### <span id="head27">0x02 结果：</span>

![QQ录屏20210929160551](assets//QQ录屏20210929160551.gif)



### <span id="head28"> 7、JS对象</span>

![image-20210929162820296](assets//image-20210929162820296.png)

#### <span id="head29"> （1）window对象</span>

![image-20210929162857588](assets//image-20210929162857588.png)

##### <span id="head30">0x00 举例：</span>

##### <span id="head31">0x01 1.html文件</span>

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>使用windows对象</title>
</head>
    <!-- 链接脚本文件 -->
    <script type="text/javascript" src="1.js"></script>
<body>
    <input type="button" name="" value="获取当前网页的url" onclick="do_get_url()">
    <br>
    <br>
    <input type="button" name="" value="跳转网页" onclick="do_set_url()">
    <br>
    <br>
    <input type="button" name="" value="关闭当前网页" onclick="do_close_html()">
</body>

</html>
```



##### <span id="head32">0x02 1.js文件</span>

```js
function do_get_url(){
    var myurl="当前网页的url:";
    myurl+=window.location.href;
    alert(myurl);

}

function do_set_url(){
    //在原网页进行网页的跳转
    window.open('http://www.baidu.com')

    //在新页面打开
    //window.open('http://www.baidu.com')

}

function do_close_html(){
    //浏览器不同，结果可能不同
    window.close();

}
```



##### <span id="head33">0x03 结果：</span>

![QQ录屏20210929164142](assets//QQ录屏20210929164142.gif)



#### <span id="head34"> （2）data对象</span>

![image-20210929164746620](assets//image-20210929164746620.png)

##### <span id="head35">0x00 举例：</span>

##### <span id="head36">0x01 1.html文件</span>

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>获取当前时间</title>
</head>

    <script type="text/javascript" src="1.js"></script>
<body>

</html>
```

##### <span id="head37">0x02 1.js文件</span>

```js
var d = new Date();
document.write( "<br>");
document.write("现在时间是：");
document.write(d.getFullYear());
document.write("年");
document.write(d.getMonth()+1);
document.write("月");
document.write(d.getDate());
document.write("日");
document.write(" 星期");
document.write(d.getDay()+" ");
document.write(d.getHours());
document.write("点");
document.write(d.getMinutes());
document.write("分");
document.write(d.getSeconds());
document.write("秒");
document.write("<br>");
document.write("<br>");
```



##### <span id="head38">0x03 结果：</span>

![image-20210929165629181](assets//image-20210929165629181.png)

#### <span id="head39"> （3）setTimeout函数和setInterval函数</span>

![image-20210929170617997](assets//image-20210929170617997.png)



##### <span id="head40">0x00 举例：</span>

##### <span id="head41">0x01 1.html文件</span>

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>从当前时间开始计时</title>
</head>

    <script type="text/javascript" src="1.js"></script>
<body onload="start_onload()"> <!--onload属性用于设置后面js语句:start_onload()在页面打开时就立即执行,相当于点了一下开始按钮-->
    <div align="center">
        <h1>Qfedu Timer</h1>
        <input type="text" id="time">
        <br>
        <br>
        <input type="button" value="开始" onclick="start()">
        <input type="button" value="暂停" onclick="stop()">

    </div>
</html>
```



##### <span id="head42">0x02 1.js文件</span>

```js
var stop_flag = 0;

function zidingyi() {

    var time = new Date();
    var h = time.getHours();
    var m = time.getMinutes();
    var s = time.getSeconds();
    document.getElementById('time').value = h + ":" + m + ":" + s;
    stop_flag = setTimeout("zidingyi()", 1000);   //setTimeout():设置用浏览器访问url时,经过1秒钟后，自动调用传参传入的函数名，称为回调函数,过1秒后只执行一次，而setInterval()函数每过1秒钟，就执行一次回调函数，本次重复调用自己，每过1秒也执行一遍setTimeout

}

function start_onload() {
    zidingyi();
}

function start() {
    zidingyi();

}

function stop() {

    clearTimeout(stop_flag);   //理解为释放setTimeout对象

}
```



##### <span id="head43">0x03 结果：</span>

![QQ录屏20210929191753](assets//QQ录屏20210929191753.gif)



#### <span id="head44"> （4）Math对象</span>

![image-20210929192618363](assets//image-20210929192618363.png)

##### <span id="head45">0x00 举例：</span>

##### <span id="head46">0x01 html文件</span>

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>简易计算器</title>
</head>

    <script type="text/javascript" src="1.js"></script>
<body> 
    <div align="center">
        <h2>简易计算器</h2>
        <br>
        <br>
        num1:<input type="text" id="num1">
        <br>
        <br>
        num2:<input type="text" id="num2">
        <br>
        <br>
        结果:<input type="text" id="ret">
        <br>
        <br>
        <input type="button" value="最大值" onclick="mymath(1)">
        &nbsp;
        <input type="button" value="最小值" onclick="mymath(2)">
        &nbsp;
        <input type="button" value="加法" onclick="mymath(3)">
        &nbsp;
        <input type="button" value="减法" onclick="mymath(4)">
    </div>
</html>
```



##### <span id="head47">0x02 js文件</span>

```js
function mymath(arg){

    //获取文本内容
    var num1 = document.getElementById("num1").value;
    var num2 =document.getElementById("num2").value;
    //判断是否是数字
    if (isNaN(num1) || isNaN(num2))
    {

        alert("请输入数字!")

    }
    else
    {

        switch(arg)
        {

            case 1:
                document.getElementById('ret').value=Math.max(num1,num2);
            break;
            case 2:
                document.getElementById('ret').value=Math.min(num1,num2);
            break;
            case 3:
                document.getElementById('ret').value=Number(num1) + Number(num2);
            break;
            case 4:
                document.getElementById('ret').value=Number(num1) - Number(num2);
            break;
        }
    }
}
```



##### <span id="head48">0x03 结果：</span>

![QQ录屏20210929201010](assets//QQ录屏20210929201010.gif)





#### <span id="head49"> （5）String对象</span>

![image-20210929201922203](assets//image-20210929201922203.png)

##### <span id="head50">0x00 例子：字符串本身就是一个对象</span>

![image-20210929202118543](assets//image-20210929202118543.png)





#### <span id="head51"> （6）全局函数</span>

![image-20210929202830559](assets//image-20210929202830559.png)





## <span id="head52"> 三、ajax局部更新网页流程图：浏览器操作服务器</span>

![img](assets//wps3DB9.tmp.jpg) 

 

### <span id="head53"> 1、流程</span>

1、创建XMLHttpRequest对象

2、设置回调函数

3、Open创建服务器请求

4、Send向服务器发送请求

5、服务器有结果会自动调用fun回调函数

在回调函数里面根据服务器返回的响应信息，更新用户界面

 

### <span id="head54"> 2、根据不同的浏览器插件异步请求对象：</span>

```js
function get_xmlreq()
{
    var xmlhttp=null;
    if(window.XMLHttpRequest)//自动检测当前浏览器的版本，如果是IE5.0以上的高版本的浏览器
    {//code for IE7+,Firefox,Chrome,Opera,Safari
        xmlhttp=new XMLHttpRequest();
    }
    else//如果浏览器是低版本
    {
        xmlhttp =new ActiveXObject('Microsoft.XMLHTTP');//创建请求对象
    }
    return xmlhttp;
}
```

在js文件中开始定义这个函数，其他js函数直接调用就能创建一个异步请求对象

 

### <span id="head55"> 3、标准的XMLHttpRequest属性</span>

![img](assets//wps3DCA.tmp.jpg) 

 

### <span id="head56"> 4、标准的XMLHttpRequest方法</span>

![img](assets//wps3DCB.tmp.jpg) 

 

### <span id="head57"> 5、异步流程：</span>

1、创建对象

2、设置回调函数，fun函数

3、open创建服务器请求

4、send向服务器发送请求

5、服务器有结果会自动调用fun回调函数

 

### <span id="head58"> 6、同步流程：（很少用到）</span>

1、创建对象

2、open建立对服务器的请求

3、Send向服务器发送请求

4、fun函数处理，服务器返回结果

 

### <span id="head59"> 7、举例：通过ajax，浏览器获取服务器端指定的文件内容</span>

#### <span id="head60">0x01 1.html文件</span>

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>获取服务器指定文件内容</title>
</head>

    <script type="text/javascript" src="1.js"></script>
<body>
<div align="center">
    <h2>获取服务器指定文件内容</h2>
    <br>
    <br>
    <input type="button"  value="发送请求" onclick="myfun()">
    <br>
    <br>
    <label id="mylabel">^_^</label>
</div>

</body>

</html>
```

#### <span id="head61">0x02 1.js文件</span>

```js
function get_xmlreq()
{
    var xmlhttp=null;
    if(window.XMLHttpRequest)//自动检测当前浏览器的版本，如果是IE5.0以上的高版本的浏览器
    {//code for IE7+,Firefox,Chrome,Opera,Safari
        xmlhttp=new XMLHttpRequest();
    }
    else//如果浏览器是低版本
    {
        xmlhttp =new ActiveXObject('Microsoft.XMLHTTP');//创建请求对象
    }
    return xmlhttp;
}

function myfun()
{ //创建XMLHttpRequest对象
	// alert("oh, see you the third time!");
	// alert("Bye Bye!");  
    
    var xmlhttp=null;
    xmlhttp=get_xmlreq();
    //设置回调函数并结构服务器的状态
    xmlhttp.onreadystatechange=function()
    {
        //如果服务器的状态为4表示完成，状态码为200表示OK,则可以获取服务器返回的数据
        if(xmlhttp.readyState==4 && xmlhttp.status==200)
        {

            //获取数据并对网页执行局部的操作
            var ret=xmlhttp.responseText;
            //ret接收数据
            document.getElementById('mylabel').innerHTML=ret;  //将ret数据写入到mylabel标签

        }    
    
    }
    //创建http请求，并发送请求服务器
    //创建http请求
    xmlhttp.open("GET",'file.txt',true)
    //清理缓存
    xmlhttp.setRequestHeader("If-Modified-Since","0");
    //发送请求
    xmlhttp.send();

}
```

#### <span id="head62">0x03 file.txt文件</span>

```txt
hello world
```



#### <span id="head63">0x04 结果：</span>

![QQ录屏20210929211040](assets//QQ录屏20210929211040.gif)





## <span id="head64"> 四、CGI编程</span>



### <span id="head65"> 1、什么是CGI?</span>

在我们网页端没有办法直接运行C程序，网页端：html，js,ajax都没有问题，但是C语言不行，通过CGI可以服务器与硬件沟通

![image-20210930111145357](assets//image-20210930111145357.png)



CGI可以用任何一种语言编写，只要这种语言具有标准输入，标准输出，和获取环境变量

![image-20210930111510979](assets//image-20210930111510979.png)

![image-20210930111634882](assets//image-20210930111634882.png)



### <span id="head66"> 2、举例：</span>

#### <span id="head67">0x01 利用python2.7开启http服务</span>

```shell
root@VM-0-10-centos www]# python2.7  -m CGIHTTPServer
```



#### <span id="head68">0x02 cgi.html文件</span>

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>案例1：cgi测试</title>
</head>
<body>
	<FORM ACTION="./cgi-bin/cgi1.cgi">
		<P>这是第一个CGI测试程序
			<br>
		<INPUT TYPE='SUBMIT' VALUE="确定">
	</FORM>
</body>
</html>

```



#### <span id="head69">0x03 cgi.c文件</span>

```c
#include <stdio.h>

int main(void)
{
	//cgi程序中的第一行必须是以下这个
	printf("Content-Type: text/html;charset=utf-8\r\n\r\n");

	printf("<html>\n<TITLE>CGI1:CGI hello!</TITLE>");
	printf("<center><H1>hello,祥祥你好 </H1></center>\n");
	printf("<center><H1>hello,this is first CGI demo! </H1></center>\n</html>");
	return 0;
}
```



#### <span id="head70">0x04 编译cig.c文件</span>

```shell
gcc cgi.c -o cgi1.cgi
```

cgi程序编译完毕后，必须以.cgi作为可执行文件的后缀名



#### <span id="head71">0x05 目录结构如下：</span>

```shell
www/
├── cgi-bin
│   ├── cgi1.cgi
│   └── cgi.c
└── cgi.html
```



#### <span id="head72">0x06 结果：</span>

![123](assets//123.gif)



### <span id="head73"> 3、案例2：以get形式发送内容</span>

#### <span id="head74">0x01 cgi.html文件</span>

```html
<!DOCTYPE html>
<html lang="en">

<head>
	<meta charset="UTF-8">
	<title>案例2：以get形式发生请求</title>
</head>

<script type="text/javascript" src="cgi.js"></script>

<body>
	data1:<input type="text" id="data1" />
	<br>
	data2:<input type="text" id="data2" />
	<br>
	<br>
	result:<label id="result"></label>
	<br>
	<br>
	<input type="button" id="add" value="相加" onclick="calc(0)" />
	<input type="button" id="minus" value="相减" onclick="calc(1)" />
</body>

</html>
```



#### <span id="head75">0x02 cgi.js文件</span>

```js
function calc(action) {

    if (isNaN(document.getElementById("data1").value) || isNaN(document.getElementById("data2").value)) {

        alert("请输入有效的数字！");
        document.getElementById("data1").value = '';
        document.getElementById("data2").value = '';

    }
    else {

        var sendData = "";
        sendData += (document.getElementById("data1").value);
        if (0 == action) {
            sendData += "+";   //5+
        }
        else if (1 == action) {

            sendData += "-";  //5-

        }
        sendData += (document.getElementById("data2").value); //5+6 或 5-6
        loadData(sendData);        //sendData:5+6

    }

}
function getXMLHttpRequest() {

    var xmlhttp = null;
    if (window.XMLHttpRequest)//自动检测当前浏览器的版本，如果是IE5.0以上的高版本的浏览器
    {//code for IE7+,Firefox,Chrome,Opera,Safari
        xmlhttp = new XMLHttpRequest();
    }
    else//如果浏览器是低版本
    {
        xmlhttp = new ActiveXObject('Microsoft.XMLHTTP');//创建请求对象
    }
    return xmlhttp;

}

function loadData(sendData) {      //sendData:5+6

    var xmlhttp = null;
    var url = "/cgi-bin/cgi2.cgi?";
    url += sendData;            //url:/cgi-bin/cgi2.cgi?5+6

    //创建XMLHttpRequest对象
    xmlhttp = getXMLHttpRequest();

    //设置回调函数
    xmlhttp.onreadystatechange = function () {

        if (xmlhttp.readyState == 4 && xmlhttp.status == 200) {
            document.getElementById("result").innerHTML = xmlhttp.responseText;

        }

    }
    xmlhttp.open('GET', url, true);
    xmlhttp.setRequestHeader('If-Modified-Since', '0'); //清楚缓存
    xmlhttp.send();


}
```

#### <span id="head76">0x03 cgi2.c文件</span>

```c
#include <stdio.h>
#include <stdlib.h>

int main(void)
{

    char *data = NULL;
    float a = 0.0, b = 0.0;
    char c = 0;
    //必不可少的第一步语句
    printf("content-type:text/html\n\n");

    //获取从服务器接收的数据
    data = getenv("QUERY_STRING");   //data: 5=6
    if (NULL == data)
    {

        printf("error");
        return 0;
    }

    sscanf(data, "%f%c%f", &a, &c, &b);
    if ('+' == c)
    {

        printf("%f", a + b);
    }
    else if ('-' == c)
    {

        printf("%f", a - b);
    }
    else
    {

        printf("error");
    }
    return 0;
}
```



#### <span id="head77">0x04 编译cgi2.c文件</span>

```shell
[root@VM-0-10-centos cgi-bin]# gcc cgi2.c -o cgi2.cgi
```



#### <span id="head78">0x05 目录结构如下：</span>

```shel
www
├── [drwxr-xr-x]  cgi-bin
│   ├── [-rw-r--r--]  cgi2.c
│   └── [-rwxr-xr-x]  cgi2.cgi
├── [-rw-r--r--]  cgi.html
└── [-rw-r--r--]  cgi.js

```



#### <span id="head79">0x06 结果：</span>

![QQ录屏20210930164449](assets//QQ录屏20210930164449.gif)



### <span id="head80"> 4、案例3：以post形式发送请求</span>

#### <span id="head81">0x01 ajax.html文件</span>

```html
<!DOCTYPE html>
<html lang="en">

<head>
	<meta charset="UTF-8">
	<title>案例3：以post形式发生请求</title>
</head>

<script type="text/javascript" src="cgi.js"></script>

<body>
	data1:<input type="text" id="data1" />
	<br>
	data2:<input type="text" id="data2" />
	<br>
	<br>
	result:<label id="result"></label>
	<br>
	<br>
	<input type="button" id="add" value="相加" onclick="calc(0)" />
	<input type="button" id="minus" value="相减" onclick="calc(1)" />
</body>

</html>
```

#### <span id="head82">0x02 cgi.js文件</span>

```js
function calc(action) {

    if (isNaN(document.getElementById("data1").value) || isNaN(document.getElementById("data2").value)) {

        alert("请输入有效的数字！");
        document.getElementById("data1").value = '';
        document.getElementById("data2").value = '';

    }
    else {

        var sendData = "";
        sendData += (document.getElementById("data1").value);
        if (0 == action) {
            sendData += "+";   //5+
        }
        else if (1 == action) {

            sendData += "-";  //5-

        }
        sendData += (document.getElementById("data2").value); //5+6 或 5-6
        loadData(sendData);        //sendData:5+6

    }

}
function getXMLHttpRequest() {

    var xmlhttp = null;
    if (window.XMLHttpRequest)//自动检测当前浏览器的版本，如果是IE5.0以上的高版本的浏览器
    {//code for IE7+,Firefox,Chrome,Opera,Safari
        xmlhttp = new XMLHttpRequest();
    }
    else//如果浏览器是低版本
    {
        xmlhttp = new ActiveXObject('Microsoft.XMLHTTP');//创建请求对象
    }
    return xmlhttp;

}

function loadData(sendData) {      //sendData:5+6

    var xmlhttp = null;
    var url = "/cgi-bin/cgi3.cgi";

    //创建XMLHttpRequest对象
    xmlhttp = getXMLHttpRequest();

    //设置回调函数
    xmlhttp.onreadystatechange = function () {

        if (xmlhttp.readyState == 4 && xmlhttp.status == 200) {
            document.getElementById("result").innerHTML = xmlhttp.responseText;

        }

    }
    xmlhttp.open('POST', url, true);
    xmlhttp.setRequestHeader('If-Modified-Since', '0'); //清楚缓存
    xmlhttp.send(sendData);


}
```



#### <span id="head83">0x03 cgi3.c文件</span>

```c
#include <stdio.h>
#include <stdlib.h>

int main(void)
{

    char *dataLen = NULL,buff[100] = {0};
    float a = 0.0, b = 0.0;
    char c = 0;
    int len= 0;
    //必不可少的第一步语句
    printf("content-type:text/html\n\n");


    dataLen = getenv("CONTENT_LENGTH");   
    if (NULL == dataLen)
    {

        printf("error1");
        return 0;
    }
    else
    {
        len = atoi(dataLen);
        if(len>0)
        {
            if(NULL ==fgets(buff,len+1,stdin))  //fget获取数据：buff==5+6
            {

                    printf("error2");
                    return 0;

            } 
            else
            {

                sscanf(buff, "%f%c%f", &a, &c, &b);
                if ('+' == c)
                {

                    printf("%f", a + b);
                }
                else if ('-' == c)
                {

                    printf("%f", a - b);
                }
                else
                {

                    printf("error3");
                }   
            }   

        }

    }
    return 0;
}
```



#### <span id="head84">0x04 编译cgi3.c文件</span>

```shell
[root@VM-0-10-centos cgi-bin]# gcc cgi3.c -o  cgi3.cgi
```



#### <span id="head85">0x05 目录结构如下：</span>

```shell
/home/www/
├── [drwxr-xr-x]  cgi-bin
│   ├── [-rw-r--r--]  cgi3.c
│   └── [-rwxr-xr-x]  cgi3.cgi
├── [-rw-r--r--]  cgi.html
└── [-rw-r--r--]  cgi.js
```

#### <span id="head86">0x06 结果：</span>

![QQ录屏20210930173838](assets//QQ录屏20210930173838.gif)



# <span id="head87">参考</span>

https://www.bilibili.com/video/BV1Bf4y1A7qq?p=111





# 补充



**1.一个完整的立即执行函数表达式 (IIFE) 的语法结构如下：** 

```javascript
e = (function (e) {
    // 函数主体：逻辑处理部分
    return e; // 这个 return 返回处理后的结果
})(argument); // 括号内的 argument 是传递的实参

```



**2.void**

```javascript
void 0 !== i && i && i.navigator && i.navigator.userAgent && i.alert && (n = "test");
```

等价

```javascript
if (void 0 !== i && i && i.navigator && i.navigator.userAgent && i.alert) {
    n = "test";
}
```

解读：
首先：执行这个表达式： i && i && i.navigator && i.navigator.userAgent && i.alert
其次：两者判断：0 和 （i && i && i.navigator && i.navigator.userAgent && i.alert），判断符号为：!==(不完全等于)，类型不同 或 值不相等 时返回 true
最后：如果条件成立，则执行：n = "test"



**3.return后面可以有以下形式**

```javascript
return {"X-s":b,"X-t":o,};
```



**4.for循环：1）好似没有增量，2）没有{}**

```javascript
let c = "";
let input = "abcd";


for (i = 0; i < input.length;) 
    (a = input.charCodeAt(i++)), 
    (b = input.charCodeAt(i++)), 
    (result = (a + b) >> 1), 
    (c = c + String.fromCharCode(result));
```

等价

```javascript
let c = "";
let input = "abcd";

for (i = 0; i < input.length;) {
    let a = input.charCodeAt(i++); // 获取第一个字符的 Unicode 编码
    let b = input.charCodeAt(i++); // 获取第二个字符的 Unicode 编码
    let result = (a + b) >> 1; // 将两个字符的编码相加后右移一位
    c = c + String.fromCharCode(result); // 将结果转为字符并添加到 c
}
```

解释：

1.增量在循环体里

2.即循环体通过逗号连接为一句



**5.三元运算符：condition ? valueIfTrue : valueIfFalse**

```javascript
//假设a为真
let a = true; // 假设a为真
let t = {name: "Kimi", age: 30}; // 假设t是一个对象
let result = a ? _JSON$stringify(t) : ""; // result将会是JSON字符串，即'{"name":"Kimi","age":30}'

// 假设a为假
let a = false; // 假设a为假
let t = {name: "Kimi", age: 30}; // 假设t是一个对象
let result = a ? _JSON$stringify(t) : ""; // result将会是空字符串，即''

```



解读：
如果condition为真（truthy），则表达式的结果是valueIfTrue；如果condition为假（falsy），则结果是valueIfFalse。



# 结束！





