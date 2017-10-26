[TOC]
# Vue笔记

## 一些原理层问题

- Vue中将data中定义的值称为模型,通过操控模型，模型会反向的去改变DOM
- **DOM Listeners**和**Data Bindings**是实现双向绑定的关键

<h4>1.Vue中的MVVM模式</h4>
> Vue 是以数据驱动模型的,它自身将数据和DOM进行绑定,一旦创建绑定，那么两者就会同步，数据的变化就会同步到dom树上
-  ViewModel
    + 是一个对象,同步模型和视图,是一个vue的实例,是作用于某个HTML元素上的,可以使body也可以是id所指向的标签
 
<h4>2.angular 与 vue 的区别,选择vue的原因</h4>
> 原因一:vue 相比较angular来说更加灵活,不像angular使用起来必须遵守它的规则,vue仅仅是个视图层,所以可以将vue嵌入一个现有的页面,也可以做成一个一整套的SPA <br>
> 原因二:angular中当监听的值越来越多的时候, 会变得越来越慢，scope中数值的变化就会触发脏检查，就会触发一次脏检查。而**Vue采用的是基于依赖追踪的观察系统**,所有的**数据变更触发都是独立的** 

## 基本指令学习

### v-text
```html
        v-text可以将一个变量的值渲染到指定的元素中,例如：
        <div v-text="msg"></div>
        new Vue({
            data:{
                msg:'hello ivan'                                            
               }
        });
        
        输出结果：
        <div>hello ivan</div>              

```
### v-html

```html
      双大括号和v-text会将数据解释为纯文本，而非 HTML 。
      为了输出真正的 HTML ，你需要使用 v-html 指令：
      例如：<div v-html="rawHtml"></div>
          new Vue({
              data:{
                  rawHtml:'<h1>hello ivan</h1>'
                }
          })
          
        被插入的内容都会被当做 HTML,但是对于没有HTML标签的数据绑定时作用同v-text和{{}}
        
    注意：使用v-html渲染数据可能会非常危险，因为它很容易导致 XSS（跨站脚本） 攻击，
    使用的时候请谨慎，能够使用{{}}或者v-text实现的不要使用v-html

```
### v-cloak

```html
    v-cloak指令保持在元素上直到关联实例结束编译后自动移除，v-cloak和 CSS 规则如 [v-cloak] { display: none } 一起用时，
    这个指令可以隐藏未编译的 Mustache 标签直到实例准备完毕。
    通常用来防止{{}}表达式闪烁问题
    例如：
    <style>
     [v-cloak] { display: none } 
    </style>
    
     <!-- 在span上加上 v-cloak和css样式控制以后，浏览器在加载的时候会先把span隐藏起来，知道 Vue实例化完毕以后；
     才会将v-cloak从span上移除，那么css就会失去作用而将span中的内容呈现给用户 -->
    <span v-cloak>{{msg}}</span>    
    
     new Vue({
              data:{
                  msg:'hello ivan'
                }
          })
    
```
    
### v-model以及双向数据绑定

```html
    1、在表单控件或者组件上创建双向绑定
    2、v-model仅能在如下元素中使用：
       input
       select
       textarea
       components（Vue中的组件）
       
    3、举例：
       <input type="text" v-model="uname" />
       
     new Vue({
              data:{
                  uname:'' //这个属性值和input元素的值相互一一对应，二者任何一个的改变都会联动的改变对方
                }
          })
          
    4、修饰符（了解）：
        .lazy - 取代 input 监听 change 事件
        .number - 自动将输入的字符串转为数字
        .trim - 自动将输入的内容首尾空格去掉
```
  
### v-bind

```html
    1、作用：可以给html元素或者组件动态地绑定一个或多个特性，例如动态绑定style和class
    
    2、举例：
        <img v-bind:src="imageSrc">   
        <div v-bind:class="{ red: isRed }"></div>
        <div v-bind:class="[classA, classB]"></div>
        <div v-bind:class="[classA, { classB: isB, classC: isC }]">
        <div v-bind:style="{ fontSize: size + 'px' }"></div>
        <div v-bind:style="[styleObjectA, styleObjectB]"></div>
           
    3、缩写形式
        <img :src="imageSrc">
        <div :class="{ red: isRed }"></div>
        <div :class="[classA, classB]"></div>
        <div :class="[classA, { classB: isB, classC: isC }]">
        <div :style="{ fontSize: size + 'px' }"></div>
        <div :style="[styleObjectA, styleObjectB]"></div>

```
  
### v-for  

```html
      1、作用：通常是根据数组中的元素遍历指定模板内容生成内容
      2、用法举例：
          <div v-for="item in items">
              {{ item.text }}
            </div>
            new Vuew({
                data:{
                    items:[{text:'1'},{text:'2'}]                
                    }
            });
      3、可以为数组索引指定别名（或者用于对象的键）：
          Vue1.0写法:
            <div v-for="(index,item) in items"></div>
            <div v-for="(key,val) in user"></div>
              new Vue({
                data:{
                    items:[{text:'1'},{text:'2'}],
                    user:{uname:'ivan',age:32}
                    }
            });
            
          Vue2.0写法:
            <div v-for="(item, index) in items"></div>
            <div v-for="(val, key) in user"></div>
            <div v-for="(val, key, index) in user"></div>            
             new Vue({
                data:{
                    items:[{text:'1'},{text:'2'}],
                    user:{uname:'ivan',age:32}
                    }
            });
            
       4、v-for 默认行为试着不改变整体(为了性能考虑的设计)，而是替换元素。
       迫使其重新排序的元素,在Vue2.0版本中需要提供一个 key 的特殊属性;
       在Vue1.0版本中需要提供一个 track-by="$index":
       
       Vue2.0写法：
       <div v-for="item in items" :key="item.id">
          {{ item.text }}
        </div>
        
      5、vue1.0与vue2.0对于v-for使用区别总结：
          1、vue1.0中有$index ，而vue2.0中将$index移除
          2、vue1.0中 (index,item) in list  而 vue2.0 变成了 (item,index) in list的写法
          3、vue1.0中使用 track-by来标记dom对象的唯一性，vue2.0中改成了 :key
          
       
```
  
### v-if

```html
    1、作用：根据表达式的值的真假条件来决定是否渲染元素，如果条件为false不渲染（达到隐藏元素的目的），为true则渲染。
    在切换时元素及它的数据绑定被销毁并重建
    
    2、示例：
        <!-- Handlebars 模板 -->
        {{#if isShow}}
          <h1>Yes</h1>
        {{/if}}

        通常我们使用写法居多：
        <h1 v-if="isShow">Yes</h1>
        
        也可以用 v-else 添加一个 “else” 块：
        <h1 v-if="isShow">Yes</h1>
        <h1 v-else>No</h1>
        
        注意：v-else 元素必须紧跟在 v-if 元素否则它不能被识别。
        
         new Vue({
                data:{
                   isShow:true
                    }
            });
        
```
  
### v-show

```html
    1、根据表达式的真假值，切换元素的 display CSS 属性，如果为false，则在元素上添加 display:none来隐藏元素，
    否则移除display:none实现显示元素
    

    2、示例：
         <h1 v-show="isShow">Yes</h1>
        
         new Vue({
                data:{
                   isShow:true
                    }
            });
            
    3、v-if和v-show的总结：
         v-if和v-show 都能够实现对一个元素的隐藏和显示操作,但是v-if是将这个元素添加或者移除到dom中，而v-show
         是在这个元素上添加 style="display:none"和移除它来控制元素的显示和隐藏的
```
  
### v-on

```html
      1、作用：绑定事件监听，表达式可以是一个方法的名字或一个内联语句，如果没有修饰符也可以省略。
      用在普通的html元素上时，只能监听 原生DOM事件。用在自定义元素组件上时，也可以监听子组件触发的自定义事件。
      
      2、常用事件：
          v-on:click
          v-on:keydown
          v-on:keyup
          v-on:mousedown
          v-on:mouseover
          v-on:submit
          ....
          
      3、v-on提供了很多事件修饰符来辅助实现一些功能，例如阻止冒泡等
        事件修饰符有如下：
        .stop - 调用 event.stopPropagation()。
        .prevent - 调用 event.preventDefault()。
        .capture - 添加事件侦听器时使用 capture 模式。
        .self - 只当事件是从侦听器绑定的元素本身触发时才触发回调。
        .{keyCode | keyAlias} - 只当事件是从侦听器绑定的元素本身触发时才触发回调。
        .native - 监听组件根元素的原生事件。
        
      4、示例：
          <!-- 方法处理器 -->
        <button v-on:click="doThis"></button>
        <!-- 内联语句 -->
        <button v-on:click="doThat('hello', $event)"></button>
        <!-- 缩写 -->
        <button @click="doThis"></button>
        <!-- 停止冒泡 -->
        <button @click.stop="doThis"></button>
        <!-- 阻止默认行为 -->
        <button @click.prevent="doThis"></button>
        <!-- 阻止默认行为，没有表达式 -->
        <form @submit.prevent></form>
        <!--  串联修饰符 -->
        <button @click.stop.prevent="doThis"></button>
   
        
       5、v-on的缩写形式：可以使用@替代 v-on:
        <button @click="doThis"></button>
```

## 功能模块学习

### 过滤器的实现
- 第一种 实现私有过滤器

```javascript
在vue对象中写入
filters:{
        format:function(data){
            var res= '';
            var year = data.getFullYear();
            var month = data.getMonth() + 1;
            var day = data.getDate();
            res = year +'-' + month +'-'+ day;
            return res;
        }
    }
```
>然后在所控制的区域中
```html
<div id="app1">
        {{ time | format }}
</div>
```

- 第二种 实现全局过滤器

```javascript
在vue对象外写入
Vue.filter('datefmt',function(data){//此处data这个参数值为"|"前面的值
   以下为过滤的条件
   var res = '';
   var year = data.getFullYear();
   var month  = data.getMonth()+1;
   var day = data.getDate();
   res = year+"-"+month+"-"+day;
   return res;
})
```
> 然后全局都可以使用 | datafmt(filter中自定义的名字)来过滤

### keycode指令以及自定义指令的实现

-  Vue1.0中有内置的enter up down 等可以直接实现的
` <input type="text" v-model="pname" @keydown.enter="addData"> `

-  Vue1.0中也可以像这样自定义按键
    - 首先要写上`Vue.directive('on').keyCodes.f2=113`(需要写上键的名称以及键码)
    - 然后即可在`<input type="text" v-model="pname" @keydown.f2="addData">`中使用f2键 
-  Vue2.0中自定义按键
```javascript
只需要加上这样的配置就可以
 Vue.config.keyCodes = {
    f2:113
}
然后如上一样可相同操作
```

### 使用vue进行ajax操作

- get操作
    - 使用`this.$http.get(url参数).then(function(res){内容处理程序})`
```javascript
this.$http.get('http://127.0.0.1:8899/api/getprodlist')
            .then(function(response){
                // 1.0 处理服务器异常信息提示
                if(response.body.status != 0){
                    alert(response.body.message);
                    return;
                }

    // 2.0 处理正常的数据逻辑
this.list =  response.body.message;
```

- post操作
    - 使用`this.$http.post(需要传到的url地址,{传入的参数},{emulateJSON:true}).then(function(res){内容处理程序})`

- jsonp操作
    - `this.$http.jsonp('http://vueapi.ittun.com/api/jsonp')`

- ajax操作中定义的函数在什么时候执行?
    - 在对象被实例化之后通过生命周期函数调用此方法 `created:function(){this.getlist()}`



### 使用vue进行动画操作

#### 使用vue实现过渡动画

```css
<style type="text/css">
         .show-enter-active,   //此处.show为在transition中定义的name的值
         .show-leave-active{transition:all ease 1s;} //这两个在动画的全程执行 

         .show-enter,.show-leave-to{padding-left:100px;}
         .show-enter-to,.show-leave{padding-left:10px;}

</style>

```

```html
<body>
<div id="app"> 
     <transition name="show"> //定义控制动画的类名
     <div  v-show="isshow">{{msg}}</div>
     </transition>  
</body>
```

#### 使用animate实现过渡动画(常用)

```html
<div id="app"> 
     <transition
     enter-active-class="animated fadeInRight"  
     leave-active-class="animated fadeOutRight"
         >
         //在transition标签汇总定义类名1.enter-active-class 2.leave-active-class
     <div  style="width:100px" v-show="isshow">{{msg}}</div>
     </transition>  
   <button @click="toggle">动画出现隐藏</button>
</div>  

```

#### 使用动画钩子函数实现过渡动画

>这种方式个人看来最为繁琐, 首先在transition标签中要定义 before-enter,enter,after-enter三个事件,**需要为其中动画的标签绑定transition属性**

> 进入：beforeEnter（进入之前），enter（进入动画刚开始），afterEnter（进入动画结束），enterCancelled（进入被中断）；<br>
退出：beforeLeave（退出之前），leave（退出动画刚开始），afterLeave（退出动画结束），leaveCancelled（退出被中断）；

```html
<div id="app">
        <transition @before-enter="beforeEnter" @enter="enter" @after-enter="afterEnter">
            <div v-show="isshow" class="show">{{msg}}</div>
        </transition>
        <button @click="toggle">动画出现隐藏</button>
    </div>
```

```javascript
beforeEnter: function(el) {//进入动画之前定义 如果在css中已经定义则可以不绑定这个事件
                el.style.transform = 'translateX(100px)'; //定义对象的初始位置
            },
enter: function(el, done) {
                // 1.设定动画的每一帧             
                el.offsetWidth;
                // 2.设定动画的结束位置
                el.style.transform = 'translateX(10px)'
                // 3.手动结束动画 
                done(); //vue中内置方法
                // console.log(this.isshow)
},

afterEnter: function() {
                //进入动画后原先设定的isshow就会变成true状态
                // console.log(this.isshow)
                this.isshow = !this.isshow
}

```


## vue之组件

### 定义组件三种方法
```html
<div id="app">
        <login></login>
        <register></register>
        <ai></ai>
</div>   
<template id="ba">
    <h1>你好</h1>
</template>

>   注: 第一种写法
    var login = Vue.extend({ //定义
        template: '<h1>hello</h1>'
    })
    Vue.component('login', login);//注册

>   注:第二种写法
    Vue.component('register', {
        template: '<h1>hi</h1>'  //必须有标签包裹
    })

>   注: 第三种写法
    Vue.component('ai', {
>       注:名称不能写成js中关键字
        template: '#ba'
    })

```

### 组件中实现指令以及事件绑定以及子组件的注册

```html
<template id="account">
        {{msg}}            //直接在标签中绑定事件就可以了
        <a href="#" @click="loginRe">如果我变成回忆</a> |<a href="">终于没那么幸运</a>
</template>

 <div id="app">
        <account>
           <login></login> //在父组件中注册必须放置在父组件中
        </account>
           <login></login> //如果定义在vue中 就是把vue当成是父组件 在这里可以看做像是全局使用
</div>

```

```javascript
   Vue.component('account', {
        template: "<div><h1>账户</h1><login></login></div>",  //如果Vue2.0的话需要一个根元素包裹

         methods: {//定义方法
             loginRe: function() {
                 alert('你懂的')
             }
         },
         data: function() {  //组件中的data是一个函数   要在函数中return 出数据对象 保证组件间的数据定义不会混淆
             return {
                 msg: "账号组件页面"
             }
         },
        components:{ //注意是components!!!!
            'login':{
                template:"<h3>子账户</h3>"
            }
        }
    })

```


### 组件中实现指令以及事件绑定以及子组件的注册

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Document</title>
</head>
<script src="../vue221.js"></script>
<body>
    <div id="app">

        <!-- 在click事件中改变cname的值,在component标签检测到cname值改变后会改变组件 -->
      <a href="#" @click="cname='login'">登录</a>
      <a href="#" @click="cname='register'">注册</a>
    <component :is="cname"></component>   
            <!-- 通过：is="xxx"的方式绑定变量 -->
    </div>

    <script type="text/javascript">
//2.0写法中需要包裹根元素 所以无论如何应该严谨的使用根元素包裹而不是直接使用text
        Vue.component('login',{
          template:'<div>登陆页面</div>'
        })

         Vue.component('register',{
          template:'<div>注册页面</div>'
        }) 
      new Vue({
        el:"#app",
        data:{
          cname:''  //页面中的使用到的变量都需要在data 中进行定义
        }
      })
    </script>
</body>
</html>
```
 
## 组件间传值

### 实现父组件传值给子组件

```html
    <template id="demo">
        {{id}}   //----->  此处"id"为在子组件中定义的用于接收父组件值的参数
    </template>
    <div id="app">
      <login :id="msg"></login> // ":id"为props中定义的key  后面的值为父组件中数据的变量名
    </div>
    <script type="text/javascript">
      new Vue({
        el:"#app",
        data:{
          msg:"流星雨文字" 
        },
        components:{
          'login':{
            template:'#demo',
            props:['id'] //------>定义接收父组件值的参数
          }
        }
      })
```

### 实现子组件向父组件传值

```html
<body>
   <template id="hhh">
       <button @click="sendData">发送数据</button> 
       <!-- 在模板中写入要发送数据的事件 -->
   </template>
    <div id="app">
         <sss @send="getData"></sss> 
         <!-- send后写的是回调函数  "send"是发送数据时候的键名 在子组件中为this.$emit(x,y)中x的值-->
    </div>
    <script type="text/javascript">
      new Vue({
            el:"#app",
            methods:{
              getData(input){
                alert(input)
              }
            },
            components:{
               'sss':{
                 template:"#hhh",
                 methods:{
                  sendData:function(){
                    this.$emit('send','Hey Girl!') 
                    //组件发送数据的方式 this.$emit('send','')
                  }
                 }
               } 
            }

        })
    </script>
</body>
```

### 实现组件之间传值第一种

> 这种方式不会将数据作为停留 不方便进行存取操作, 只能作为值的搬运工
- 首先定义一个全局的vue对象 例如取名为transfer.js,**作为组件间值的中转站** 如下：
```javascript
import vue from vue

export default  new vue

```

- 然后再发送组件中
```javascript
//1.0 导入中转对象
 import  transfer from '../transfer.js'
//2.0 在此假设有个事件 作为发送数据的事件 
 sendMsg(){
   transfer.$emit("要传的值的key","要穿的value")
 }
```

- 最后在接受组件中
```javascript
// 1.0 导入中转站对象
 import  transfer from '../transfer.js'

// 接受数据
 mounted(){   //
  transfer.$on("传来的值的key",function(res){
       //res就是传过来的值
    })
 }
 
```

### 实现组件之间传值第二种

> 使用浏览器的localstorage来存取数据，作为组件间的公共区域  好处是可以作为中间区作为数据的缓存地方
需要定义一个共有的js文件作为方法定义的输出口
![](imgs/localstorage方案.jpg)

> 而后只需要在用到其中方法的组件中按需导入即可`import {xxx} from ../`
## 使用vue获取html中的对象

- Vue1.0方式

```html
  <div id='app'>
    <button @click="getDom">获取dom对象</button>
    <div v-el:dom> hello dom</div> 
    //v-el:xxx 获取dom
    <button @click="getComponent">获取dom对象</button>
    <login v-ref:component></login>
    //v-ref:xxx获取组件
  </div>

  new Vue({
      el:"#app",
      methods:{
        getDom:function(){
          console.log(this.$els.dom) //-->this.$els.dom "dom为在v-el:xxx xxx的值"
        },
        getComponent:function(){
          alert(this.$refs.component.msg) //-->this.$refs.component "dom为在v-ref:xxx xxx的值"
          -->this.$els.dom "dom为在v-el:xxx xxx的值"
        }
      },
      components:{
        'login':{
          data:function(){
            return{
              msg:'你知道的'
            }
          },template:'<h3>这是个组件对象</h3>'
        }
      }
     })

```

- Vue2.0方式
    + v2.0中不一样,通过统一ref就就可以获取到在标签内与上面不一样统一要写成ref="xxx"在获取对象的时候this.$refs.xxx

```html
<div id='app'>
    <button @click="getDom">获取dom对象</button>
    <div ref="dom"> hello dom</div>  //--> ref="xxx"
    <button @click="getComponent">获取dom对象</button>
    <login ref="component"></login><!-- ref="xxx"可获取组件和对象 -->
</div>

<!-- 在methods中  this.$refs.dom 或者 this.$refs.component 就能获取到 -->

getDom:function(){
          console.log(this.$refs.dom)
},
```

## vue之路由

### 路由嵌套

```html
<body>
    <div id="app">
      <div id="demo"></div>
       <!--  <route-link to="此处填路径"> -->
        <router-link to="/account/login">登录</router-link>
        <router-link to="/account/register">注册</router-link>
        <router-view></router-view>  
<!--         <route-view>用于呈现通过路径找到的内容 -->
    </div>
    <script type="text/javascript">
    var app = Vue.extend({ 
        template: "<div>账户管理<router-view></router-view></div>"/* --->用于替换根组件中的router-view*/
    }) //!impotant  
    
    var login = Vue.extend({
        template: '<div>登录组件</div>'
    });

    new login().$mount("#demo")//构造器实例化挂在到html中的标签中

    var register = Vue.extend({
        template: '<div>注册组件</div>'
    });

    /*创建路由器对象 其中routes是一个对象*/
    var router = new VueRouter({
        routes: [ //途径数组
            {
                path: '/account',
                component: app,
                //此处不需要加引号''  嵌套路由的话要把子组件放入 children数组中,并且path中不需要加'/'
                children: [{
                        path: 'login',
                        component: login //此处不需要加引号'' 

                    },
                    {
                        path: 'register',
                        component: register

                    }]
            }
        ]
    })


    new Vue({
        el: "#app",
        router: router

    })
    </script>
</body>

```

### 路由的传参

```html
<body>
    <div id="app">
        <router-link to="/login/Rose">登录</router-link>
        <router-link to="/register/love">注册</router-link>
        <router-view></router-view>
    </div>
    <script type="text/javascript">
    /*1.0创建一个根组件*/
    var app = Vue.extend({}) //!impotant 

     /*2.0创建其他组件*/ 
    var login = Vue.extend({
        /*第一种参数值的获取方式*/
        template: '<div>登录组件{{val}}</div>',
        data:function(){
            return{
                val:''
            }
        },
        created:function(){
            this.val =  this.$route.params.username
        }
    });

    var register = Vue.extend({
        /*第二种参数值的获取方式*/
        template: '<div>注册组件{{this.$route.params.nickname}}</div>'
    });

    /*3.0创建路由器对象 配置路由规则 其中routes是一个对象*/
    var router = new VueRouter({
        routes: [ //途径数组
            {
                path: '/login/:username',  // :username 为路由获取时的参数名
                component: login //此处不需要加引号'' 
            },
            {
                path: '/register/:nickname',
                component: register
            }
        ]
    })
    new Vue({
        el: "#app",
        router: router
    })
    </script>
</body>

```

## 监听值的变化

### 使用watch

```html
<body>
    <div id="app">
        <input type="text" v-model="firstName">
        <input type="text" v-model="lastName">
        {{fullName}}
    </div>
    <script type="text/javascript">
    new Vue({
        el: '#app',
        data: {
            firstName: '123',
            lastName: '456',
            fullName: '789'
        },
        watch: {
             'firstName': function(newval,oldval){
                this.fullName = this.firstName+this.lastName
            },
            'lastName': function(newval,oldval){
                this.fullName = this.firstName+this.lastName
            }

        }
    })
    </script>
</body>
```
- 除了能够监控自己的data数据之外  还能监控路由对象
在写完了 router:router后
       watch:{'$route':function(newroute,oldroute){ } }

### computed使用

- 通常用computed 因为当发现值没有改变的时候 直接从缓存里面拿数据
```html
<body>
  <div id="app">
  <input type="text" v-model="firstName">
  <input type="text" v-model="lastName">
  {{fullName}} // 此处为computed中定义
  </div>
  
</body>
<script>
new Vue({
  el:'#app',
  data:{
    firstName:'heima',
    lastName:'itcast',

  },
  computed:{
    fullName:function(){
    return this.firstName+this.lastName
  }
}
  
});
</script>
```

## vue的钩子函数
![](imgs/lifecycle.png)

> 如上图所见 一个vue对象创建后到销毁会有一系列的事件,从开始创建、初始化数据、编译模板、挂载Dom、渲染->更新->渲染、卸载等一系列过程

<h4>1.beforeCreate</h4>
> 在实例初始化后，数据观测(data observe)和event/watched事件之前被调用。

<h4>2.created</h4>
> 实例已经创建完了之后被调用,在这一步,实例已经完成了配置:数据观测(data observere),属性和方法的运算,watch/event 事件的回调。然而 **挂载阶段还没有开始**,$el属性目前不可见 `补充:$el是vm的el属性所指向的DOM节点`

<h4>3.beforeMount</h4>
> 在挂载之前被调用，相关的render首次被调用

<h4>4.mounted</h4>
> el 被创建的vm.$el替换,并挂载到实例上去后调用该钩子

<h4>5.beforeUpdate</h4>
> 数据更新时候调用，发生在虚拟DOM重新渲染和打补丁之前,**你可以在这个钩子中进一步的更改状态不会触发附加的重渲染过程**

<h4>6.updated</h4>
> 由于数据更新导致的虚拟DOM重新渲染和打补丁,在这之后会调用该钩子

<h4>7.beforeDestroy</h4>
> 实例销毁之前调用。在这一步，实例仍然完全可用。

<h4>8.destroy</h4>
> Vue 实例销毁后调用。调用后，Vue 实例指示的所有东西都会解绑定，所有的事件监听器会被移除，所有的子实例也会被销毁。 该钩子在服务器端渲染期间不被调用。