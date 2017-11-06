[TOC]
# Ajax学习笔记
## 1 http协议的请求方式
- get ： 获取数据
- post  ：用来添加数据
- delete  ： 用来删除数据
- put  :用来修改数据


- HTTP协议
   即超文本传输协议，网站是基于HTTP协议的。
   HTTP协议是由从客户机到服务器的请求(Request)和从服务器到客户机的响应(Response)进行了约束和规范。
   即HTTP协议主要由请求和响应构成。



## 2 http请求
- HTTP协议

        即超文本传输协议，网站是基于HTTP协议的。
        HTTP协议是由从客户机到服务器的请求(Request)和从服务器到客户机的响应(Response)进行了约束和规范。
        即HTTP协议主要由请求和响应构成。
- HTTPS协议

        以安全为目标的HTTP通道，简单讲是HTTP的安全版。
        即HTTP下加入SSL层，HTTPS的安全基础是SSL，因此加密的详细内容就需要SSL。
        SSL即安全套接层，是为网络通信提供安全及数据完整性的一种安全协议。

- 一般流程
> 1.0 建立TCP连接 <br/>
> 2.0 Web浏览器向Web服务器发送请求命令<br/>
> 3.0 Web浏览器发送请求头信息<br/>
> 4.0 Web服务器应答<br/>
> 5.0 Web服务器发送应答头信息<br/>
> 6.0 Web服务器向浏览器发送数据<br/>
> 7.0 Web服务器关闭TCP连接     
          
- 一般由四部分组成
> 1.0 HTTP请求的方法或者动作，是GET还是POST;<br/>
> 2.0 正在请求的URL；<br/>
> 3.0 请求头 包含客户端的信息，身份验证信息等等；<br/>
> 4.0 请求体，请求正文，包含客户提交的查询字符串信息，表单信息等等  

- HTTP响应一般由三部分组成
> 1.0 一个数字或文字组成的状态码，用来显示请求失败或者成功；<br/>

            状态码由三位数字构成，其中首尾数字定义了状态码的类型
            1XX：信息类，表示收到了Web浏览器的请求正在进一步处理；
            2XX：成功，表示用户请求被正确接受，理解和处理例如：200 OK；
            3XX: 重定向，表示请求没有成功，客户必须采取进一步操作；
            4XX：客户端错误，表示客户端提交的请求有错误，例如404 NOT；
            5XX: 服务器错误，表示服务器不能完成对请求的操作 例如：500
            
> 2.0 响应头，包含服务器类型，日期时间，内容长度类型<br/>
> 3.0 响应体， 相应正文

- 常用HTTP状态码
    - 200 ok :表示请求一切正常<br/>
    - 301 Moved Permanently ：重定向，用户请求的文档在其他地方,新的URL在locatioin头中给出；<br/>
    - 302 Found ：临时重定向，新的URL被视作临时性替代，不是永久性<br/>
    - 304 Not Modified 客户端有缓冲的文档并发出了一个条件性的请求。服务器告诉客户，原来缓冲的文档还可以继续使用.<br/>
    - 400 Bad Request 请求语法出现错误;<br/>
    - 403 forbidden  资源不可用<br/>
    - 404 Not Found :无法找到指定位置的资源<br/>
    - 405 Method Not Allowed :请求方法对指定的资源不适用<br/>
    - 500 Internal Server Error ：服务器发生了错误<br/>
    - 501 Not Implemented :服务器不能满足需求<br/>

## 3 get的用法

            $f = $_GET["abc"];
            echo '<span>'.$f.'</span>'

- **form表单的默认请求方式是get方式**
- **get请求会把表单的数据作为url的参数，对任何人都是可见的**
- **一般用于获取信息，一般在2000字符内**

## 4 post的用法
- **一般用于修改服务器上的资源，从表单中发送一些数据**
   **在表单提交后**
     

            <div>
                <form action="./page-post.php" method="post" >//此处method改为post方式传输数据
                用户名：<input type="text" name="username"><br>
                密码    <input type="text" name="password"><br>
                        <input type="submit" value="登录">
                </form>
            </div>
       

  **提交后会跳转到验证页**

                <?php
                    $username = $_POST['username'];
                    $pw = $_POST['password']; 
                
                    header("Content-Type:text/plain; charset=utf-8");
                    if($username == "admin" && $pw =="12345"){
                        echo "用户名密码正确";  
                    }else{
                        echo  "用户名错误";
                    }
                ?>

### get和post的区别

1. get是从服务器上获取数据，post是向服务器传送数据。
2.  get是把参数数据队列加到提交表单的ACTION属性所指的URL中，值和表单内各个字段一一对应，在URL中可以看到。post是通过HTTP post机制，将表单内各个字段与其内容放置在HTML HEADER内一起传送到ACTION属性所指的URL地址。用户看不到这个过程。
3. 对于get方式，服务器端用Request.QueryString获取变量的值，对于post方式，服务器端用Request.Form获取提交的数据。
4. get传送的数据量较小，不能大于2KB。post传送的数据量较大，一般被默认为不受限制。但理论上，IIS4中最大量为80KB，IIS5中为100KB。
5. get安全性非常低，post安全性较高。但是执行效率却比Post方法好。
 
> **建议：**
1. get方式的安全性较Post方式要差些，包含机密信息的话，建议用Post数据提交方式；<br>
2. 在做数据查询时，建议用Get方式；而在做数据添加修改或删除时，建议用Post方式；<br>
> **补充** 
>  GET和POST本质上就是TCP链接
>  GET产生一个TCP数据包;POST产生两个TCP数据包。
>  对于GET方式的请求，浏览器会把http header和data一并发送出去，服务器响应200(返回数据);
而对于POST，浏览器先发送header，服务器响应100 continue，浏览器再发送data，服务器响应200 ok(返回数据)。
>  并不是所有浏览器都会在POST中发送两次包，Firefox就只发送一次


## 5 PHP遍历数组的方法
- 使用for循环
         
        $arr = array(1,2,3,4,5,6);
        $length = count($arr);
        for ($i = 0; $i < $length; $i++) {  
            echo $arr[$i]."<br>";  
        }  
     
- 使用foreach循环

        foreach($arr as $value){
            echo $value;
        }     

## 5 通过iframe方式可以在页面上实现局部刷新
**首先在主页面中写入**

           <iframe width="0" height="0" frameborder="0" name="myframe">
               
           </iframe>

**然后再iframe.php的子页面中写入**

        <?php 
        
        $username = $_POST['username'];
        $password = $_POST['password'];
        if ($username =='admin' && $password =='12345') {
        ?> 
            <script type="text/javascript">
            parent.document.getElementById("info").innerHTML="YES";
            </script>
        <?php } 
        else{?>
        
        <script type="text/javascript">
            parent.document.getElementById("info").innerHTML="NO";
        </script>

        <?php } ?>

> **缺点：会为页面增加不必要的标签**

## 那么咱来聊聊Ajax

- ajax的请求过程是怎样的

        1.0 创建一个XMLHttpRequest对象
        2.0 打开和服务器的链接
        3.0 给服务器发送请求
        4.0 通过onreadystatechange事件监听服务器的响应并执行回调函数

- ajax返回回来的数据类型有哪些？

> 常用的数据类型有 **JSON**,**XML**,**Text/HTML**格式

- ajax的优点和缺点怎么说？

        优点：
            1.0 能够实现页面的局部刷新；
            2.0 更快的响应速度；
            3.0 减轻服务器负担；
            4.0 基于标准化并被广泛支持，无需使用多余插件；

        缺点：
            1.0 ajax不支持浏览器back按钮；
            2.0 ajax暴露了与服务器的交互细节，有安全问题；
            3.0 对搜索引擎的支持比较弱；
            4.0 破坏了程序的异常机制；
            5.0 不容易调试；    


## 关于XML格式

**什么是XML**

            1、XML指可扩展的标记语言
            2、主要用来输出和存储数据 --- 注意：设置宗旨是【传输数据】，而非显示数据
            3、XML标签没有预定义，需要自行定义标签 --- 就是说XML具有自我描述性

**XML数据格式的缺点**

            1、原数据占用的数据量比较大，不利于大量数据的网络传输
            2、解析不太方便

**XML与html的区别**

            html是用于显示数据的，而XML是用来传输数据的

## 关于json你要知道
- JSON相比XML

            更轻量级，更易解析，更快、

- JSON数据解析
> JSON数据在传输的过程中是以文本即字符串的方式进行解析的，所以在js中使用时通常要转化为对象格式以便使用

- 转换的方式

             JSON.parse()	     --- 把json形式的字符串转成对象
             JSON.stringify()   --- 把对象转成字符串
             var obj = eval(‘(’ + str1 + ‘)’)			 --- 把字符串解析成JS代码并执行
            

## 关于跨域
- 什么情况下会跨域

            1.0 网络协议不同，如http协议访问https协议。
            2.0 端口不同，如80端口访问8080端口。
            3.0 域名不同，如qianduanblog.com访问baidu.com。

> 浏览器有个策略叫----"**同源策略**"。所谓同源策略,就是同协议，同域名，同端口，是浏览器保护用户信息安全的一种手段，它对来至不同源的文档或脚本对当当前文档的读写做出了限制

- 实现跨域的原理 ： 

> **实现跨域都是把动态资源伪装成脚本来实现跨域**

- 实现跨域的方法 :

        1、jsonp，
        2、让后台配合设置ACAO(access-control-allow-origin)
        3、设置反向代理
        2、document.domain + iframe
        3、location.hash + iframe
        4、window.name + iframe
        5、window.postMessage(兼容性不好)



## Jsonp的基本原理

- 利用script的特性(可以把引用过来的内容当做js代码执行)**动态创建**scirpt，通过src使用**get**的请求方式向服务端发送跨域请求，通过响应的内容调用事先定义好的方法。

        优点：可以让我们越过浏览器的同源策略发送请求；
        缺点：必须后台配合你开发出jsonp结口

- **Jsonp与ajax的区别**：

        首先ajax的核心是通过XMLHttpRequest获取非本网页内容；
        json呢，是一种数据交换的格式，js原生支持，后台几乎全部支持；
        jsonp呢，是基于浏览器同源策略下进行跨域的一种调用方式  

## artTemplate使用  

### 编写模板
- 使用一个**type = "text/html"**的script的标签来存放模板

        例如:<script type="text/html" id="template">
                <h1>{{title}}</h1>
                {{each books as value}}
                <div>
                    {{value}}
                </div>
                {{/each}}         
                
            </script>  

### 渲染模板
- template("id",数据)

                var  data= {
                title:"书名",
                books:["三国演义","水浒传","西游记"]
                }
    
            //添加数据后渲染
            var html = template("template",data);
            var container = document.getElementById("container")
            container.innerHTML=html;
