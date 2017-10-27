[TOC]
# HTML整理笔记

## 1 HTML中常见的面试题

### 1.1 当你在浏览器地址栏输入URL,按下回车后究竟发生了什么?
<a href="http://www.cnblogs.com/xianyulaodi/p/6547807.html">链接跳转</a>
> 1.HTTP请求发送,通过DNS查询IP,这一步包括 DNS 具体的查找过程，包括：浏览器缓存->系统缓存->路由器缓存...,通过Socket发送数据(建立网络通信至少需要一对**端口号**)。<br/>
> 2.浏览器给web服务器发送一个HTTP请求<br/>
> 3.服务器的永久重定向相应<br/>
> 4.浏览器跟踪重定向地址<br/>
> 5.服务器处理请求<br/>
> 6.服务器返回一个HTTP响应<br/>
> 7.浏览器显示HTML    <br/>
> 8.浏览器发送请求获取嵌入在HTML中的资源(如图片,等等)<br/>
> 9.浏览器发送异步0请求<br/>



### 1.2 浏览器的渲染过程

- 1、Create/Update DOM And request css/image/js：浏览器请求到HTML代码后，在生成DOM的最开始阶段（应该是 Bytes → characters 后），并行发起css、图片、js的请求，无论他们是否在HEAD里。

    **注意：发起js文件的下载request并不需要DOM处理到那个script节点，比如：简单的正则匹配就能做到这一点，虽然实际上并不一定是通过正则：）。这是很多人在理解渲染机制的时候存在的误区**

- 2、Create/Update Render CSSOM：CSS文件下载完成，开始构建CSSOM

- 3、Create/Update Render Tree：所有CSS文件下载完成，CSSOM构建结束后，和 DOM 一起生成 Render Tree。

- 4、Layout：有了Render Tree，浏览器已经能知道网页中有哪些节点、各个节点的CSS定义以及他们的从属关系。下一步操作称之为Layout，顾名思义就是计算出每个节点在屏幕中的位置。

- 5、Painting：Layout后，浏览器已经知道了哪些节点要显示（which nodes are visible）、每个节点的CSS属性是什么（their computed styles）、每个节点在屏幕中的位置是哪里（geometry）。就进入了最后一步：Painting，按照算出来的规则，通过显卡，把内容画到屏幕上。

> 以上五个步骤前3个步骤之所有使用 “Create/Update” 是因为DOM、CSSOM、Render Tree都可能在第一次Painting后又被更新多次，比如JS修改了DOM或者CSS属性。

> Layout 和 Painting 也会被重复执行，除了DOM、CSSOM更新的原因外，图片下载完成后也需要调用Layout 和 Painting来更新网页。


### 1.3 常见的浏览器内核有哪些？
+ **Trident内核**：IE,MaxThon,TT,The World,360,搜狗浏览器等。[又称MSHTML]
+ **Gecko内核**：Netscape6及以上版本，FF,MozillaSuite/SeaMonkey等。
+ **Presto内核**：Opera7及以上。      [Opera内核原为：Presto，现为：Blink;]
+ **Webkit内核**：Safari,Chrome等。   [ Chrome的：Blink（WebKit的分支）]
+ **EdgeHTML内核**：Microsoft Edge。  [此内核其实是从MSHTML fork而来，删掉了几乎所有的IE私有特性]

### 1.4 简述一下你对HTML语义化的理解。
+ 1.0用正确的标签做正确的事情。
+ 2.0HTML语义化让页面的内容结构化，结构更清晰，便于对浏览器、搜索引擎解析；
+ 3.0即使在没有样式CSS的情况下也能以一种文档格式显示，并且是容易阅读的；
+ 4.0搜索引擎的爬虫也依赖于HTML标记来确定上下文和各个关键字的权重，有利于SEO；
+ 5.0使阅读源代码的人更容易将网站分块，便于阅读、维护和理解。

### 1.5 页面导入样式时，使用Link和@import有什么区别？
- （1）link属于XHTML标签，除了加载CSS外，还能用于定义RSS, 定义rel连接属性等作用；*而@import是CSS提供的，只能用于加载CSS*;
- （2）页面被加载的时，link会同时被加载，而@import引用的CSS会*等到页面被加载完再加载*;
- （3）**import**是CSS2.1 提出的，只在**IE5**以上才能被识别，而link是XHTML标签，无兼容问题;    

### 1.6 介绍一下你对浏览器内核的理解
- 主要分成两部分：渲染引擎(layout engineer或Rendering Engine)和JS引擎。
    +（1）渲染引擎：负责取得网页的内容（HTML、XML、图像等等）、整理讯息（例如加入CSS等），以及计算网页的显示方式，然后会输出至显示器或打印机。浏览器的内核的不同对于网页的语法解释会有不同，所以渲染的效果也不相同。所有网页浏览器、电子邮件客户端以及其它需要编辑、显示网络内容的应用程序都需要内核。
    +（2）JS引擎则：解析和执行javascript来实现网页的动态效果。
> 最开始渲染引擎和JS引擎并没有区分的很明确，后来JS引擎越来越独立，内核就倾向于只指渲染引擎。


### 1.7 Doctype作用？标准模式与兼容模式各有什么区别？
- （1）<!DOCTYPE>声明位于HTML文档中的第一行，处于html标签之前，用于告知浏览器的解析器用什么文档标准解析这个文档。DOCTYPE不存在或格式不正确会导致文档以兼容模式呈现。
- （2）标准模式的排版和JS运作模式都是以该浏览器支持的最高标准运行。在兼容模式中，页面以宽松的向后兼容的方式显示，模拟老式浏览器的行为以防止站点无法工作。

### 1.8 HTML5有哪些新特性、移除了哪些元素？如何处理HTML5新标签的浏览器兼容问题？如何区分 HTML 和 HTML5？
- HTML5 现在已经不是 SGML 的子集，主要是关于图像，位置，存储，多任务等功能的增加。
- 绘画 canvas;
- 用于媒介回放的 video 和 audio 元素;
- 本地离线存储 localStorage 长期存储数据，浏览器关闭后数据不丢失;
- sessionStorage 的数据在浏览器关闭后自动删除;
- 语意化更好的内容元素，比如 article、footer、header、nav、section;
- 表单控件，calendar、date、time、email、url、search;
- 新的技术webworker, websockt, Geolocation;

### 1.9请描述一下 cookies，sessionStorage 和 localStorage 的区别？
- `cookie`是网站为了标识用户身份而储存在用户本地终端（Client Side）上的数据（通常已经过加密）。cookie数据始终在同源的http请求中携带（即使不需要），也会在浏览器和服务器间来回传递。
- `sessionStorage`和`localStorage`不会自动把数据发给服务器，仅在本地保存。

- 存储大小：
  + cookie数据大小不能超过4K。
  + sessionStorage和localStorage虽然也有存储大小的限制，但比cookie大得多，可以达到5M或更大。
- 有效时间：
  + cookie          设置的cookie过期时间之前一直有效，即使窗口或浏览器关闭
  + sessionStorage  数据在当前浏览器窗口关闭后自动删除
  + localStorage    存储持久数据，浏览器关闭后数据不丢失除非主动删除数据


### 1.10 iframe 有哪些缺点？
- iframe会阻塞主页面的Onload事件；
- 搜索引擎的检索程序无法解读这种页面，不利于SEO；
- iframe和主页面共享连接池，而浏览器对相同域的连接有限制，所以会影响页面的并行加载。
- 使用iframe之前需要考虑这两个缺点。如果需要使用iframe，最好通过JavaScript动态给iframe添加src属性值，这样可以绕开以上两个问

### 1.11 HTML5 应用程序缓存和浏览器缓存有什么区别？

>应用程序缓存是 HTML5 的重要特性之一，提供了离线使用的功能，让应用程序可以获取本地的网站内容，例如 HTML、CSS、图片以及 JavaScript。这个特性可以提高网站性能，它的实现借助于 manifest 文件，如下：
```html
<!doctype html>
<html manifest=”example.appcache”>
…..
</html>
```
> 与传统浏览器缓存相比，它不强制用户访问的网站内容被缓存。

### 1.12 浏览器加载和渲染html的顺序

- 1 IE下载的**顺序是从上到下**，渲染的顺序也是从上到下，下载和渲染是同时进行的。 

- 2 在渲染到页面的某一部分时，其上面的所有部分都已经下载完成（并不是说所有相关联的元素都已经下载完）。 

- 3 如果遇到语义解释性的标签嵌入文件（JS脚本，CSS样式），那么此时IE的下载过程会启用单独连接进行下载。 

- 4 样式表在下载完成后，将和以前下载的所有样式表一起进行解析，解析完成后，将对此前所有元素（含以前已经渲染的）重新进行渲染。 

- 5  JS、CSS中如有重定义，后定义函数将覆盖前定义函数。 

- JS的加载 

    + 1 **不能并行下载和解析**（阻塞下载）。 

    + 2 当引用了JS的时候，浏览器发送1个js request就会一直等待该request的返回。因为浏览器需要1个稳定的DOM树结构，而JS中很有可能有代码直接改变了DOM树结构，比如使用 document.write 或appendChild,甚至是直接使用的location.href进行跳转，浏览器为了防止出现JS修改DOM树，需要重新构建DOM树的情况，所以 就会阻塞其他的下载和呈现. 


## 2 HTML基础知识内容

### 2.1 SGML的了解

### 2.2无序标签
- dl  dt  dd ;
- ul  li ;
- ol li  

### 2.3 行内元素有哪些？块级元素有哪些？空（void）元素有哪些？
>声明：CSS规范规定，每个元素都有**display**属性，确定该元素的类型，每个元素都有默认的display值，如div的display默认值为“block”，则为“块级”元素；span默认display属性值为“inline”，是“行内”元素。
- （1）行内元素有：
    + a b span img input select strong（强调的语气）
- （2）块级元素有：
    +div ul ol li dl dt dd h1 h2 h3 h4…p
- （3）常见的空元素：
    + br hr img input link meta
- （4）鲜为人知的空元素：
    + area base col command embed keygen param source track wbr

### 2.4 一些常用不常用的标签整理
- strong:加粗
- ins:下划线    
- em:倾斜    
- del:删除线
- sub标签可以定义下标文本
>  包含在 <sub> 标签和其结束标签 </sub> 中的内容将会以当前文本流中字符高度的一半来显示，但是与当前文本流中文字的字体和字号都是一样的。
- i:通常用于放一些logo或者其他的小背景图    

### 2.5 Label的作用是什么？如何使用？
>label标签来定义表单控制间的关系，当用户选择该标签时，浏览器会自动将焦点转到和标签相关的表单控件上。
```html
<label for="Name">Number:</label>
<input type="text" name="Name" id="Name" />
<label>Date:<input type="text" name="B" /></label>

```
- `placeholder`不兼容IE6~9 可以用**label替代**

### 2.6 运用a链接创建锚点
>锚点是文档中某行的一个记号，类似于书签，用于链接到文档中的某个位置。当定义锚点后，我们可以创建直接跳至该锚点（比如页面中某个小节）的链接，这样使用者就无需不停地滚动页面来寻找他们需要的信息了。
- 在使用a元素创建锚点时，需要使用name属性为其命名，代码如下所示：
```html
<a name=”anchorname1”>锚点一</a>
<!-- 然后就可以创建链接，直接跳转到锚点，代码如下所示： -->
<a href=”#anchorname1”>回到锚点一</a>
```
- 简单用a链接实现跳转到顶部的效果
```html
<a href="#top">
```

### 2.7 title和alt区分一下
<h4>title:</h4>

> title既是标签也是属性，当用作属性的时候，**用来为元素提供额外的信息**

<h4>alt:</h4>
- **为所有的图片添加alt属性**,这样符合W3C标准
> alt通常用于 `img`  `input` 元素中，**当无法给用户显示图片的时候的替代文字**


## 3 关于meta标签

### 3.1 meta标签解析
```html
        <meta charset="UTF-8">
        <!-- 指定字符编码 "UTF-8" -->
        <meta name="viewport" content="width=device-width,initial-scale=1,user-scalable=no,minimal-ui" />
        <!-- 视图窗口,移动端特属标签, 其中minimal-ui 意为在网站加载时默认加载地址栏和导航栏 -->
        <meta http-equiv="X-UA-Compatible" content="IE=edge" />
        <!-- X-UA-Compatible : 设置浏览器优先使用什么模式渲染页面
             content="IE=edge,chrome=1" 优先使用最新的IE和chrome来渲染
        -->
        <meta name="renderer" content="webkit" />
        <!--默认使用 webkit 内核: safari chrome  安卓默认浏览器
        取值:
                content="webkit" 采用webkit内核(360极速模式)
                content="ie-comp" 采用IE兼容内核
                content="ie-stand" 采用IE标准内核
        -->
        <meta name="apple-mobile-web-app-capable" content="yes" />
        <!-- 网站开启对web app 程序的支持(隐藏苹果工具栏和菜单栏)
        取值:
                content="yes"   伪装成app 
                content="no"    显示工具栏和菜单栏
         -->
        <meta name="apple-mobie-web-app-status-bar-style" content="black">
        <!-- 当启动webapp功能时,显示手机信号 时间 电池的顶部导航栏颜色
        取值：
                content="defult" 白色
                content="black"  黑色
                content="black-translucent" 灰色半透明
         -->
        <meta name="apple-touch-full-screen" content="yes">
        <meta name="screen-orientation" content="portrait" />
        <!-- uc强制竖屏-->
        <meta name="format-detection" content="telephone=no" />
        <!-- 格式检测
        取值:
                content="telephone=no"  忽略将页面中的数字识别为电话号码
                content="telephone=yes" 启动电话功能
         -->
        <meta name="format-detection" content="email=no" />
        <!-- 格式检测: 忽略识别邮箱-->
        <meta http-equiv="Cache-Control" content="no-siteapp" />
        <!-- http-equiv="Cache-Control" content="no-siteapp"
        作用：
                防止百度转码
         -->
        <meta name="x5-orientation" content="portrait" />
        <!-- QQ强制竖屏 -->
        <meta name="full-screen" content="yes" />
        <!-- UC强制全屏 -->
        <meta name="keywords" content="机器猫商城" />
        <!-- 关键字 -->
        <meta name="description" content="机0" />
        <!-- 描述信息 -->
```
	
