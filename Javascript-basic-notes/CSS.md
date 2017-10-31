[TOC]



# CSS基本概念

##  CSS盒模型
> 总共有两种<br/>   
(1)总共有两种：第一种是IE盒模型，另一种是W3C盒模型<br/>
(2)盒模型构成：margin+border+padding+content<br/>
(3)区别：IE的盒模型content把pdaading和border也算了进去

### 统一盒模型解决方式
> 将页面设置为标准模式
```html
    <!DOCTYPE html>  
```


## 处理文本段落的属性
- 1.0 white-space:属性设置如何处理元素内的空白
    + normal 默认。空白会被浏览器忽略。 
    + pre 空白会被浏览器保留。其行为方式类似 HTML 中的 pre 标签。 
    + **nowrap 文本不会换行**(强制不换行效果)，文本会在在同一行上继续，直到遇到 br 标签为止。 
    + pre-wrap 保留空白符序列，但是正常地进行换行。 
    + pre-line 合并空白符序列，但是保留换行符。 
    + inherit 规定应该从父元素继承 white-space 属性的值。

- 2.0 word-wrap:属性用来标明是否允许浏览器在单词内进行断句
    + normal: 只在允许的断字点换行(浏览器保持默认处理) 
    + break-word:在长单词或URL地址内部进行换行 

- 3.0 word-break:属性用来标明怎么样进行单词内的断句。   
    + normal：使用浏览器默认的换行规则。 
    + break-all:允许再单词内换行 
    + keep-all:只能在半角空格或连字符处换行 

- **超出显示省略号效果**
```css
 p{text-overflow:ellipsis;//文本溢出显示省略号;
   overflow:hidden;
   white-space:nowrap;//强制不换行;
   }
```


##  清除浮动的几种方式
- 第一种：在浮动元素的末尾加上空的标签例如`<div style=”clear:both”></div> `  
   + 缺点分析:会造成很多无意义的空标签，违反结构与样式分离的原则
- 第二种：br标签和其自身的html属性,如`<br clear="all">`
   + 缺点分析:违反结构与样式分离的原则
- 第三种: 父元素设置`overflow:hidden`(在IE6中还需要触发haslayout，例如`zoom:1`);
   + 优点：不存在语义与结构化问题
   + 缺点分析:内容增多的不会换行而是被影藏掉
- 第四种: 父元素设置`overflow:auto`(无需加上`zoom:1`);
   + 缺点分析:多个嵌套后，firefox某些情况造成内容全选
- 第五种: 父元素也设置浮动;
   + 缺点分析:会造成与父元素相邻的元素布局混乱
- 第六种: 父元素设置`display:table`;
   + 缺点分析:虽然语义化明确,但是会改变盒模型属性
- 第七种: 使用after伪元素的方式 (由于IE6-7不支持,需要设置zoom:1来触发hasLayout);
```css
    .clearfix:after { 
        content: "."; 
        display: block; 
        height: 0; 
        clear: both; 
        visibility: hidden; 
    } 
```

## 重要概念之BFC(Block Formatting contexts)
>概念：对css有了解的朋友肯定都知道盒式模型这个概念，对一个元素设置css，首先需要知道这个元素是block还是inline类型。而BFC就是用来格式化块级盒子，同样管理inline类型的盒子还有IFC，以及其他的FC。那首先我们就来看看FC的概念。
- Formatting Context：指页面中一个渲染区域，并且拥有一套渲染规则，他决定了其子元素如何定位，以及与其他元素的相互关系和作用。

- BFC：块级格式化上下文，它是指一个独立的块级渲染区域，只有Block-level Box参与，该区域拥有一套渲染规则来约束块级盒子的布局，且与区域外部无关。

- 触发条件：(满足下列条件之一就可以触发)
    + 1 根元素，即HTML元素

    + 2 float的值不为none

    + 3 overflow的值不为visible

    + 4 display的值为inline-block、table-cell、table-caption

    + 5 position的值为absolute或fixed

- 作用一：可以阻止元素被浮动元素浮动
    + <说明>与浮动元素响铃的已生成BFC的元素不能与浮动元素相互重覆盖
    + 以此可以达到两列布局以及多列布局的一种方式:
 ```css
    .left{
        float:left;
        width:180px;
    }
    .center{
        overflow:hidden;
        height:116px;        
    }
    .right{
        width:180px;
        float:right;
    }
 ``` 
>通过以上的设置,由于中间的盒子在BFC中，不会与浮动的盒子重叠,就可以达到宽度自适应的效果。在**两列布局的时候也很常用**

- 作用二：可以包含浮动元素，即解决**高度塌陷**
    + 使得父元素包含子元素，常见的方式是为父元素设置overflow：hidden或者浮动父元素。根本原因在于创建BFC的元素，子浮动元素也会参与其高度计算，即不会产生高度塌陷问题。

- 作用三：防止margin重叠:
    + 方式：只要将两个元素不放入同一个BFC中即可

## text-align
- 设置文本或者元素水平位置
    + 只对**行内块元素/行内元素水平位置**有用，对块状元素不起作用。(input是行内元素)

## `vertical-align`
> `vertical-align`只对`inline-block`和`inline`元素起作用
## margin的应用
- `margin：0 auto` 可以让拥有宽度的元素根据父元素的宽度水平居中
- `margin-left:auto` 让左边距填满,即元素会被挤到父元素的最右边   
- 使用**margin:auto**实现居中,(并不常用),代码如下:
```css
    .container{
         width: 300px;
         height: 300px;
         position:relative;
         background: green;
      }
      .son{
          width: 100px; 
          height: 100px;
          background: red;
          position:absolute;
          left: 0; top: 0; right: 0; bottom: 0;//必须都设置为0;
          margin-right:auto;
      }
``` 
## 实现元素在父盒子中居中的几种方式整理:
- 在盒子内居中
    + 方式一：`top:50%;left:50%;margin-top:-自身高一半;margin-left:- 自身宽度一半`;
    + 方式二: 使用`transfrom:translate(-50%,-50%)`;
    + 方式三: `display: flex;justify-content: center;align-items: center;`
    + 方式四: 父盒子`position:relative`,子盒子`position:absolute`,然后同时设置left,right,top,bottm都为0,最后设置`margin:auto`;
    + 方式五: 最不好的方法：使用maigin 或 定位 去拼凑数据

## CSS三角的写法
```css
    div {
        width: 0;     
        height: 0;
        margin: 100px auto;
        border-width: 10px;    /* 数值控制三角大小 */
        border-style: solid dashed dashed dashed;  /* dashed 是为了照顾ie6 灰色底的问题 */
        border-color: #f00 transparent transparent transparent;   /* 上 右  下 左 模式 */
         /* transparent透明  背景颜色默认也是透明的 */
        overflow: hidden;  /* ie6 高度问题 */
    }
```
## 实现局部滚动条滚动的CSS方式
```html
<style>
    ::-webkit-scrollbar {
        width: 4px;
    }
    ::-webkit-scrollbar-track {
        -webkit-box-shadow: inset 0 0 6px rgba(255,255,211,0);
        border-radius: 10px;
    }
    ::-webkit-scrollbar-thumb {
        border-radius: 10px;
        background: rgba(0,0,0,0);
        -webkit-box-shadow: inset 0 0 6px rgba(255,125,232,0);
    }
    li{
        width: 100%;
        height: 40px;
        line-height: 40px;
        list-style: none;
        text-align: center;
        border: 1px solid #4DD7C0;
    }
    *{
        margin: 0;
        padding: 0;
    }
    .demo{
        width:200px; 
        height: 400px; 
        overflow: hidden;
        w-x:hidden; 
        overflow-y: scroll;
         background:blue;
        -webkit-overflow-scrolling: touch;
    }
</style>
<body>
<div class="demo" >
    <div style="width:100%;">
        <li><a href="#">列表</a></li>
        <li>列表</li>
        <li>列表</li>
        <li>列表</li>
        <li>列表</li>
        <li>列表</li>
        <li>列表</li>
        <li>列表</li>
        <li>列表</li>
        <li>列表</li>
        <li>列表</li>
        <li>列表</li>
    </div>
</div>
</body>
```

## 关于继承
- 可以继承的样式:font-size font-family color,ul li dl dd dt list-xxx font-xxx line-xxx text-xxx
- 不可继承的样式:height、min-height、max- height、width、min-width、max-width、display、margin、border、padding、background、overflow、position、left、right、top、 bottom、z-index、float、clear、table-layout、vertical-align、page-break-after、 page-bread-before和unicode-bidi。

## padding撑盒子的问题
- div不加上宽度,加padding不会撑开盒子
- 如果加上宽度，加padding会撑开盒子

## 解决li与li之间默认的空白
方案一：

> 设置为float:left;

方案二：

> ul设置为`font-size:0`(chrome不支持，因为chrome小于12px按12px处理)，可以用`letter-spacing:-3px` 处理

## visibility
visibility中`collapse`的作用:

> 当在表格元素中使用的时候，这个值可以删除一行或一列。*但是不会影响布局*,被行或者列占据的空间会留给其他元素时候。<br/>     
当不是表格元素使用的时候,只会呈现出`hidden`的效果

## 抽离样式模块收藏用
```css
body, h1, h2, h3, h4, h5, h6, hr, p, blockquote, dl, dt, dd, ul, ol, li, pre, form, fieldset, legend,
 button, input, textarea, th, td { margin:0; padding:0; }
        body, button, input, select, textarea { font:12px/1.5tahoma, arial, \5b8b\4f53; }
        h1, h2, h3, h4, h5, h6{ font-size:100%; }
        address, cite, dfn, em, var { font-style:normal; }
        code, kbd, pre, samp { font-family:couriernew, courier, monospace; }
        small{ font-size:12px; }
        ul, ol { list-style:none; }
        a { text-decoration:none; }
        a:hover { text-decoration:underline; }
        sup { vertical-align:text-top; }
        sub{ vertical-align:text-bottom; }
        legend { color:#000; }
        fieldset, img { border:0; }
        button, input, select, textarea { font-size:100%; }
        table { border-collapse:collapse; border-spacing:0; }
```

## 元素竖向百分比设定是相对于容器的高度吗？
> 当子元素的高度设为50%的时候,的确是相对于父元素的百分之50,而`margin,padding`特殊,他们设定的百分比相对于**父亲的宽度**

## 解决在chrome浏览器下表单自动填充后背景色为黄色的问题:
```css
input:-webkit-autofill{
    -webkit-box-shadow:0 0 0px 1000px white inset; //个人理解是运用白色内阴影遮挡黄色背景
    -webkit-text-fill-color:#333;
}

```

## 处理字间距
- text-indent:用于设置缩进
- letter-spacing:用于设置字间距 
- word-spaing:用于设置单词间的间距(对中文无效)



## background-repeat新增的两个值
- *space*:向两端对齐平铺,多余留白

- *round*:向两端对齐平铺,**多余直接拉伸**


## CSS中超出隐藏滚动条效果(多用于移动端)
> *以影藏X轴为例:*通过父元素**overflow:hidden** 子元素设置**overflow-x:auto;white-space:nowrap**;
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <style type="text/css">
          *{
            margin:0;
            padding:0;
          }
        .grandpa{
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%,-50%);
            border:1px solid #ccc;
            height: 100px;
            width: 800px;
            overflow: hidden;
        }
        .father{
                    height:100%;
                    overflow-x: auto;
                    overflow-y:hidden;          
                    white-space:nowrap;
        }
        li{
            width: 30px;
            white-space:nowrap;
            margin-left: 10px;
            text-align: center;
            list-style: none;
            font-size: 20px;
            border-bottom:1px solid #ccc;
            float: left;
        }
    </style>
</head>
<body>
    <div class="grandpa">
        <ul  class="father">
            <li>我市1222222222222222222222222222222222我i阿速度阿苏大送达苏打阿    </li>
        </ul>
    </div>
</body>
</html>
```

- 另外附上其他demo
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport"
          content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
    <title>Title</title>
</head>
<style>
    ::-webkit-scrollbar {
        width: 4px;
    }
    ::-webkit-scrollbar-track {
        -webkit-box-shadow: inset 0 0 6px rgba(255,255,211,0);
        border-radius: 10px;
    }
    ::-webkit-scrollbar-thumb {
        border-radius: 10px;
        background: rgba(0,0,0,0);
        -webkit-box-shadow: inset 0 0 6px rgba(255,125,232,0);
    }
    li{
        width: 100%;
        height: 40px;
        line-height: 40px;
        list-style: none;
        text-align: center;
        border: 1px solid #4DD7C0;
    }
    *{
        margin: 0;
        padding: 0;
    }
    .father{
        width:200px; 
        height: 400px; 
        overflow: hidden;
        w-x:hidden; 
        overflow-y: scroll; 
        background:blue;
        -webkit-overflow-scrolling: touch;
    }
</style>
<body>
<div class="father">
    <div style="width:100%;">
        <li><a href="#">列表</a></li>
        <li>列表</li>
        <li>列表</li>        <li>列表</li>

        <li>列表</li>
        <li>列表</li>
        <li>列表</li>
        <li>列表</li>
        <li>列表</li>
        <li>列表</li>
        <li>列表</li>
        <li>列表</li>
        <li>列表</li>
        <li>列表</li>
        <li>列表</li>
        <li>列表</li>

    </div>
</div>
</body>
</html>
```

## chrome浏览器css字体大小小于12px无效的解决办法
> 设置body为`webkit-text-size-adjust`

## webkit表单元素的默认外观怎么重置

> `.css{-webkit-appearance:none;}`

## webkit表单输入框placeholder的颜色值改变 (安卓不行)

```css
input::-webkit-input-placeholder{color:#AAAAAA;}
input:focus::-webkit-input-placeholder{color:#EEEEEE;}

```

## 如果需要手动写动画,你认为最小的事件间隔是多久?
> 多数显示器的频率是60HZ,就是说一秒刷新60,那么最短的间隔就是`1/60*1000 = 16.7ms`

## overflow:scroll在IOS上滑动不流畅的解决办法
> 加上`-webkit-overflow-scrolling:touch`



## 移动端禁止选中内容

> 如果你不想用户可以选中页面中的内容，那么你可以在css中禁掉：
```css
.user-select-none {
  -webkit-user-select: none;  
  -moz-user-select: none;     
  -ms-user-select: none;            
}
```

## flex 详解

>  设为`flex`布局的盒子 子元素的`float`,`clear`,`vertical-align`将失效

### 1 容器的属性

```
    //flex 有六个属性
    1. flex-direction  
    2. flex-wrap 
    3. flex-flow 
    4. justify-content 
    5. align-items 
    6. align-content 

```
#### 1.1 flex-direction 
**决定主轴的排列方向**
```css
.box{
    flex-direction:row | row-reserve | colum | colum-reserve
}
```  
--------------
![](./imgs/flex-direction.png)

> row:
> 主轴水平方向**从左向右**排列

> row-reserve:
> 主轴水平方向**从右向左**排列(从屏幕最右边向左排列)
  
> column:
> 主轴为竖直方向**从上到下**排列
  
> column-reserve:
> 主轴为竖直方向**从下到上**排列

#### 1.2 flex-wrap
这个属性用来定义元素在轴线排列**是否换行**
```css
.box{
  flex-wrap: nowrap | wrap | wrap-reverse;
}
```
> 其中 `wrap-reserve` 设定后元素反向换行,换行,第一行在下面

#### 1.3 flex-flow
是 `flex-direction` 和`flex-wrap`的简写 默认值:`row nowrap`

```css
.box {
  flex-flow: <flex-direction> || <flex-wrap>;
}
```

#### 1.4 justify-content
定义在**主轴上的对齐方式**
```css
.box {
  justify-content: flex-start | flex-end | center | space-between | space-around;
}
```

#### 1.5 align-items
> 定义在**交叉轴**的排列方式
```css
.box {
  align-items: flex-start | flex-end | center | baseline | stretch;
}
```
- baseline:项目第一行的文字的基线对齐

- strench:**如果项目没有设置高度或者设为auto,那么将占满屏幕**

####  1.6 align-content
> 定义多跟轴线的对齐方式，如果项目只有一条基线,该属性不起作用


### 2. 设置在子元素的属性
- order
- flex-grow
- flex-shrink
- flex-basis
- flex
- align-self

#### 2.1 order属性
> order 属性定义元素的排列顺序。数值越小越靠前
```css
 .item{order:1}

```
![](./imgs/order.png)

#### 2.2 flex-grow属性
> 定义元素的放大比例,默认为**0** <br/>
> 如果有一个元素的值为2而其他元素都为1的话,那么会占据剩余空间**比别人多一倍**
```css
   .item{flex-grow:1}
```

#### 2.3 flex-shrink属性
> 定义元素的缩小比例,默认为1,即如果空间不足,项目将缩小<br/>
> 如果有一个设置为零而其他设置为1时，**当空间不足，设置为0的不会缩小**    

#### 2.4 flex-basis属性
> 该属性定义了再分配多余空间之前,项目占据的主轴空间
```css
item{ flex-basis:<length> | auto}
```

#### 2.5 flex属性 (优先使用)
> 是 flex-grow flex-shrink flex-basis 的缩写 默认 `0 1 auto`

#### 2.6 align-self
> 允许单个元素与其他不一样的对齐方式,可以覆盖`align-items`属性
```css
   .item{auto | flex-end | flex-start | center | stretch | baseline }
```



## 设置透明三种方式
### rgba
- `background:rgba(0,0,0,0.3)` 
    + **不影响内部内容**
### opacity
- `opacity:0.3`
    + 内部内容也会透明 且IE6/7/8 不支持

### 兼容IE 透明
> `filter:alpha(opacity=20)`









<!DOCTYPE html>
<html>
<head>
    <title></title>
    <style type="text/css">
       .top{
            position:fixed;
            right:30px;
            display:none;
            bottom:15%;
            width:40px;
            height:40px;
            line-height: 40px;
            text-align:center;
            border:1px solid #ccc;
            border-radius:20px;
            color:#ccc;
            cursor:pointer;
            z-index:500;
       }
       .top a{
            text-decoration:none;
       }
       .top:hover{
            transition: all .5s ease;
             transform:rotateY(360deg);
             box-shadow:0 0 5px black;
       }
    </style>
</head>
<body>
   <div class="top">
      <a  href="#top">top</a>
   </div>
   <script type="text/javascript">
   window.onload=function(){
      window.onscroll=function(){
         var back = document.querySelector('.top')
         var t = document.documentElement.scrollTop|| document.body.scrollTop
         if(parseInt(t) > 2000){
            back.style.display="block"
         }
         else{
            back.style.display="none"
         }
      }
  } 
   </script>
</body>
</html>