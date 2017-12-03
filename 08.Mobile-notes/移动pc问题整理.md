[TOC]
# 移动端 

##  1.使用MUI的时候

- 点击的时候出现`Unable to preventDefault inside passive event listener due to target being treated as passive`
- 原由：不明 是chrome为了提高页面滑动流畅度而折腾出来的东西
```css 
  解决方案1:
css可以加上* { touch-action: none; } 样式去掉
```
```javascript 
解决方案2:  
注册处理函数时,用如下方,明确声明为不是被动的
window.addEventListener('touchmove', func, { passive: false })
```
<a href="https://segmentfault.com/a/1190000008512184" target="blank">文章参考链接</a>

## 2.META相关

### 2.1 添加到主屏后的标题（IOS）

> <meta name="apple-mobile-web-app-title" content="标题"> 

### 2.2 启用 WebApp 全屏模式（IOS）

> 当网站添加到主屏幕后再点击进行启动时，可隐藏地址栏（从浏览器跳转或输入链接进入并没有此效果）
```html

<meta name="apple-mobile-web-app-capable" content="yes" /> 
<meta name="apple-touch-fullscreen" content="yes" /> 
<!--more-->
```

### 3.3 百度禁止转码

> 通过百度手机打开网页时，百度可能会对你的网页进行转码，往你页面贴上它的广告，非常之恶心。不过我们可以通过这个meta标签来禁止它：<br>
`<meta http-equiv="Cache-Control" content="no-siteapp" />`

### 3.4 设置状态栏的背景颜色（IOS）

> 设置状态栏的背景颜色，只有在 "apple-mobile-web-app-capable" content="yes" 时生效

`<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent" /> `

-----------

> content 参数：<br>
`default` ：状态栏背景是白色。<br>
`black` ：状态栏背景是黑色。<br>
`black-translucent `：状态栏背景是半透明。 如果设置为 `default` 或 `black`,网页内容从状态栏底部开始。 如果设置为 `black-translucent `,网页内容充满整个屏幕，顶部会被状态栏遮挡。

### 3.5 移动端手机号码识别（IOS）
> 在 iOS Safari （其他浏览器和Android均不会）上会对那些看起来像是电话号码的数字处理为电话链接，比如：
7位数字，形如：1234567 <br>
带括号及加号的数字，形如：(+86)123456789<br>
双连接线的数字，形如：00-00-00111<br>
11位数字，形如：13800138000<br>

```html
<meta name="format-detection" content="telephone=no" />
开启电话功能

<a href="tel:123456">123456</a>
开启短信功能：

<a href="sms:123456">123456</a> 
```

### 3.6 移动端邮箱识别（Android）

>与电话号码的识别一样，在安卓上会对符合邮箱格式的字符串进行识别，我们可以通过如下的meta来管别邮箱的自动识别：<br>
`<meta content="email=no" name="format-detection" />`<br> 
同样地，我们也可以通过标签属性来开启长按邮箱地址弹出邮件发送的功能：<br>
`<a mailto:dooyoe@gmail.com">dooyoe@gmail.com</a>`

## 3.7 添加智能 App 广告条 Smart App Banner（IOS 6+ Safari）

`<meta name="apple-itunes-app" content="app-id=myAppStoreID, affiliate-data=myAffiliateData, app-argument=myURL">`


### 3.8 优先使用最新版本 IE 和 Chrome

> `<meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1" /> `

### 3.9 viewport模板

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<meta content="width=device-width,initial-scale=1.0,maximum-scale=1.0,user-scalable=no" name="viewport">
<meta content="yes" name="apple-mobile-web-app-capable">
<meta content="black" name="apple-mobile-web-app-status-bar-style">
<meta content="telephone=no" name="format-detection">
<meta content="email=no" name="format-detection">
<title>标题</title>
<link rel="stylesheet" href="index.css">
</head>
<body>
这里开始内容
</body>
</html>
```
### 3.10 winphone系统a、input标签被点击时产生的半透明灰色背景怎么去掉

```css
<meta name="msapplication-tap-highlight" content="no">
```


## 4. 移动端如何定义字体font-family

- 三大手机系统的字体：

```html
//ios 系统

默认中文字体是Heiti SC
默认英文字体是Helvetica
默认数字字体是HelveticaNeue
无微软雅黑字体
//android 系统

默认中文字体是Droidsansfallback
默认英文和数字字体是Droid Sans
无微软雅黑字体
//winphone 系统

默认中文字体是Dengxian(方正等线体)
默认英文和数字字体是Segoe
无微软雅黑字体
各个手机系统有自己的默认字体，且都不支持微软雅黑 如无特殊需求，手机端无需定义中文字体，使用系统默认
英文字体和数字字体可使用 Helvetica ，三种系统都支持

* 移动端定义字体的代码 */
body{font-family:Helvetica;}
```


## 5. 移动端touch事件(区分webkit 和 winphone)

> 当用户手指放在移动设备在屏幕上滑动会触发的touch事件
移动设备上的web网页是有300ms延迟的，玩玩会造成按钮点击延迟甚至是点击失效。

<h4>解决方案:</h4>

>1.fastclick可以解决在手机上点击事件的300ms延迟 <br>
zepto的touch模块，tap事件也是为了解决在click的延迟问题 <br>

### 6.1 网友遇到过问题的问题借用

> 我一想就用原生的select来解决，效果也是挺不错的，然而，在iphone上，轻触select的时候select会出现闪退的现,<a href=" http://youtu.be/0J_RNTMzHyQ ">youtube链接</a> <br/>
解决办法就是在**修改Fastclick的源码**，**判断点击目标的类型如果是select的话，就return，不用再去经过fastclick的逻辑**

## 7. ios系统中元素被触摸时产生的半透明灰色遮罩怎么去掉

> 问题描述：ios用户点击一个链接，会出现一个半透明灰色遮罩, 如果想要禁用，可设置-webkit-tap-highlight-color的alpha值为0，也就是属性值的最后一位设置为0就可以去除半透明灰色遮罩
```css
a,button,input,textarea{-webkit-tap-highlight-color: rgba(0,0,0,0);}

```

## 8. 部分android系统中元素被点击时产生的边框怎么去掉

> **android**用户点击一个链接，会出现**一个边框或者半透明灰色遮罩**, 不同生产商定义出来额效果不一样，可设置`-webkit-tap-highlight-color`的`alpha`值为`0`去除部分机器自带的效果

```css
a,button,input,textarea{
    -webkit-tap-highlight-color: rgba(0,0,0,0;)
    -webkit-user-modify:read-write-plaintext-only; 
}
-webkit-user-modify有个副作用，就是输入法不再能够输入多个字符
```


## 9. webkit表单元素的默认外观怎么重置

> `.css{-webkit-appearance:none;}`

## 10. webkit表单输入框placeholder的颜色值能改变么
```css

input::-webkit-input-placeholder{color:#AAAAAA;}
input:focus::-webkit-input-placeholder{color:#EEEEEE;}

```


## 11. 关闭iOS键盘首字母自动大写

> 在iOS中，默认情况下键盘是开启首字母大写的功能的，如果启用这个功能，可以这样：<br>
`<input type="text" autocapitalize="off" />`

## 12. 关闭iOS输入自动修正

> 和英文输入默认自动首字母大写那样，IOS还做了一个功能，默认输入法**会开启自动修正输入内容**，这样的话，用户经常要操作两次。如果不希望开启此功能，我们可以**通过input标签属性来关闭掉：** <br>
`<input type="text" autocorrect="off" /> `

## 13. 禁止文本缩放

>当移动设备横竖屏切换时，文本的大小会重新计算，进行相应的缩放，当我们不需要这种情况时，可以选择禁止：
```html

html {
　　      -webkit-text-size-adjust: 100%;
}

```
- 注意：必须设置 `meta viewport`。

## 14. 移动端如何清除输入框内阴影
> 在iOS上，输入框默认有内部阴影，但无法使用 box-shadow 来清除，如果不需要阴影，可以这样关闭：
```css
input,
textarea {
　　border: 0; 
　　-webkit-appearance: none; 
}
```

## 15. 如何禁止保存或拷贝图像（IOS）

> `img { -webkit-touch-callout: none; }`

## 16. 屏幕旋转的事件和样式

<h4>事件:</h4>
>`window.orientation`，取值：正负90表示横屏模式、0和180表现为竖屏模式；
```javascript
window.orientation
window.onorientationchange = function(){
            switch(window.orientation){
                case -90:
                case 90:
                alert("横屏:" + window.orientation);
                case 0:
                case 180:
                alert("竖屏:" + window.orientation);
                break;
            }
} 
```
<h4>样式</h4>
```css
//竖屏时使用的样式
@media all and (orientation:portrait) {
    .css{}
}

//横屏时使用的样式
@media all and (orientation:landscape) {
    .css{}
}
```

## 17. audio元素和video元素在ios和andriod中无法自动播放

- 应对方案：**触屏即播**
```javascript
$('html').one('touchstart',function(){
    audio.play()
})
```

## 18. 手机拍照和上传图片

```html
<input type="file">的accept 属性

<!-- 选择照片 -->
<input type=file accept="image/*">
<!-- 选择视频 -->
<input type=file accept="video/*">
使用总结：
ios 有拍照、录像、选取本地图片功能
部分android只有选取本地图片功能
winphone不支持
input控件默认外观丑陋

```

##  19. 消除transition闪屏

```css
.css{
    
    -webkit-transform-style: preserve-3d;
    
    -webkit-backface-visibility: hidden;
}
```

## 20. 保证动画流畅性

> 设计高性能CSS3动画的几个要素
尽**可能地使用合成属性transform和opacity来设计CSS3动画**，
**不使用position的left和top**来定位
利用translate3D开启GPU加速
```css
.css {
   -webkit-transform: translate3d(0, 0, 0);
   -moz-transform: translate3d(0, 0, 0);
   -ms-transform: translate3d(0, 0, 0);
   transform: translate3d(0, 0, 0);
}

```

## 21. android 上去掉语音输入按钮

> `input::-webkit-input-speech-button {display: none}`


## 22. Retina屏幕1px 边框的问题
```css
      Element {
          border-width: thin
      }
```

# PC端

##  使用a链接取消默认行为

> 在有一次使用a链接取消默认跳转 在执行过onclick事件后进行跳转操作的时候`<a href="javascript:;"></a> ` 的,由于需要a执行click事件后再进行跳转,所以使用了`location.href="xxx"` 由于没有**取消a链接的默认行为**,始终不能成功


> 至于**a 链接先onclick 还是 先跳转** 每个浏览器的效果都不一样

- 所以一般取消a链接默认行为有如下方法

```javascript
if(e && e.preventDefault){
  e.preventDefault
}else{
    window.event.returnValue = false;
    return false  
}
```

##  点击按钮等待数据的思考
> 1.当点击按钮等待是数据响应的时候,当信息还没返回回来需要禁用按钮或者添加遮罩禁止用户再次点击按钮防止造成数据错误。


> 2. 当点击下载下一页或者说点击加载更多的时候加个loading状态 1.是让用户知道数据正在加载，不给用户造成页面卡死的假象,2.防止用户再次点击造成数据混乱
