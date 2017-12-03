# vue之路由

## 路由嵌套

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

## 路由的传参

### 1 路由传参第一种
> 通过在配置路由中绑定参数名` path: '/login/:username'` 然后在`<router-link to="/login/Rose">`**(最后一个/后的数值既为要传递过去的数值)**,然后在目标页通过` $route.params.username(自己定义的参数名)`可以获取到传递过来的值

- `v-bind="{to:'index/value'}"`----拼接
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
                /* 在传递参数的时候可以对username的值进行正则匹配
                例如
                    path: '/login/:username(\\s+)'
                */
                
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
### 2 路由传参第二种
- 路由设置`name`属性
> `{ path:'Hi2',name:'Hi2',component:Hi2 }`<br/>
研究发现当通过name属性绑定了路由之后`path`属性可以设定任意值也可以通向目标页面

- `<router-link :to="{name:'Hi2',params:{username:'lll'}}">`

- 在目标页面同样可以通过`$route.params.username`获取到传递过来的数值

## 路由的重定向
### 1 使用`redirect`重定向
> 以下实例：当Url中地址变为 “/hi3”是会重定向到"/"路由的页面 **注意：url路径中会变成/lll** `http://localhost:8080/#/`
```js
    routes:[
            {
            path:'/',
            redirect:'/lll'                
            /* 将原来component位置替换成redirect就可以了 */
            } 
    ]
```


### 2 使用`alias`重定向
> 以下实例：当Url中地址变成”/hi2“的时候会重定向到路由为`/lll`的页面<br/>
**注意：url中的地址还是path定义的地址不是重定向后的地址**<br/>
`http://localhost:8080/#/hi2`
```js
    {
        path:'/hi2',
        component:demo1,
        alias:'/lll'
    },
```   
> 在使用`alias`别名的时候 `path`不能写为`“/”`一个斜杠 **会出错**

## mode的设置和404页面的处理
- `mode`属性的两个值
    + `mode:'history'` 设置为这个时候url地址栏不会出现"#"
    + `mode:'hash'` 默认设置 有"#"出现在url地址栏
```js
export default new Router({
  mode:'history', //和 routes同级
  routes: [
    {
      path: '/',
      components:{
         default:HelloWorld,
         left:demo1,
         right:demo2
      }
    }
  ]
})
``` 
- 当用户输入一个匹配不到的页面的时候返回一个404页面
    + 需要给`path`设置为`path:'*'`
> 同样需要输入路由配置文件
```js
    path:'*',  //找不到页面时候的配置
    component:Error  /* 需要定义一个显示 404 页面的提醒组件 */
```

## 路由中的钩子函数
### 1. 直接在路由的配置中书写
> 在路由配置中的钩子函数只有`beforeEnter`
```js
path:'/hi2',
      component:demo1,
      alias:'/lll',
      beforeEnter:( to,from,next )=>{
            console.log(to)
            console.log(from)
            next();
      }
```
- `to`:路由将要跳转的信息
- `from`: 路由跳转前的路径信息
- `next()`:**如果不写不会跳转**, 可取值true/false, 在`next(false)`和不写的状况下不会跳转

### 2. 写在模板中的钩子函数
- beforeRouteEnter:在路由进入前的钩子函数,进入组件前时触发
- beforeRouteLeave: 在路由离开前的钩子函数,离开该组件页面前时触发
```js
export default {
    beforeRouteEnter:(to,from,next)=>{
        console.log(1)
        next();
    },
    beforeRouteLeave:(to,from,next)=>{
        console.log(2)
        next();
    }

    //进入时打印   1 
    //离开时打印   2
}
```


## 单页面多路由操作 
- 首先需要在app.vue中的`<router-view>`下面再两个`<router-view>`
```html
    <router-view name="left" class="left"></router-view>
    <router-view name="right" class="right"></router-view>    
    <!-- name 属性的值与路由参数中配置的组件一一对应 -->
```
- 配置主要在`index.js` 中的 `routes` 字段中
```js
        /* 首先需要导入所需要用到的组件 */
        import HelloWorld from '@/components/HelloWorld'
        import demo1 from '@/components/demo1'
        import demo2 from '@/components/demo2'
        
```
- 依然在`index.js`中
```js
        export default new Router({
            routes:[
                {
                    path:'/',
                    components:{
                        default:HelloWorld,
                        left:demo1,
                        right:demo2
                    }
                    //默认的微helloWorld组件
                    //name=“left”为demo1组件
                    //name="right"为demo2组件
                }
            ]
        })
```

## 编程式导航

### 1. this.$router.go(-1)和this.$router.go(1)
> 这两个功能跟在浏览器的前进和后退一样 <br/> 
> 示例:需要在**不是首页的组件页面显示后退按钮并实现后退功能**,首页隐藏。

- 在根组件页面添加后退按钮
    + 通过給`v-if`绑定变量`isshow`的值为`true/false`来控制是否显示
```html
<div v-if="isshow">
      <a @click="goback">返回</a>
</div>
```
- 首先是**实现监测路由控制是否显示**,通过`watch`监测`$route`的变化
```js
    watch:{
        '$route':function(newRouter,oldRouter){  
            /* 监测路由变化 */
            if(newRouter.path == "/"){
                this.isshow = false
            }else{
                this.isshow = true
            }
        }
    }
```
- 然后是**实现后退功能**
    + **注意:是$router并非是监测的$route**
    + 另外,在通过路由传递参数,另一个页面接收的方式是`this.$route.params`中的`$route`
```js
    methods:{
        goback(){
          this.$router.go(-1)
        }
    }
```

### 2. $this.$router.push('/xx')
> 这个编程式导航都作用就是跳转，比如我们判断用户名和密码正确时，需要跳转到用户中心页面或者首页，都用到这个编程的方法来操作路由。



## 导航守卫
- 主要用来跳转或者取消的方式守卫导航
- **方法**: `router.beforeEach`
    + 接受参数 to , from , **next**
    + next()方法必须调用

```js
    router.beforeEach((to,from,next) => {
        if(to.meta.requiresAuth){
            if(sessionStorage.getItem('local_key') == null){
                alert('请先登录...')
                next({path : '/login'})
            }else{
                next();
            }
        }else{
            next();
        }
    })
```