[TOC]
# const 语法声明

> const(声明的是一个只读的常量) , const与let类似,**只是let只能在声明的时候赋值**,`const a = 123`,不可以随意修改
- 一旦声明,常量的值就可不改变
- **作用域与let相似**只在声明所在的块级作用域内有效
- const命令声明的常量和let一样也是**不提升**,同样存在**Temporal Dead Zone**( 暂时性死区),只在声明之后有效

# Promise

## 基本使用
> promise就是个构造函数
```js
    var p =  new Promise(function(resolve,reject){
        reject("可以把数据传递出去")
    })
    p.then(function(data){
        console.log(data) //在then中就可以接收到这个数据
    })
```

## ajax使用 

- 首先封装一个Promise方法 *此处举例发送ajax请求*
```js
   var p1 = function(url){
       return new Promise(resolve,reject){  //必须把promise方法return 出去
         $.get(url,function(data){
             resolve(data)
         })
       }   
   }
   //使用数据
   p1(url).then(function(data){
      console.log(data)
   })
   
```

## 解决回调地狱的问题
> 如果一个页面加载需要多个ajax请求发出去,如何知道最后一次ajax接收到后再一起渲染呢?<br/>
这时候就可以解放多重回调(回调地狱)的方式,使用promise了,使用在前后请求数据不依赖的情况下

-  Promise.all可以接受一个元素为promise对象的数组作为参数,当这个数组里面的所有promise对象都变成resolve时,该方法才会返回
```js
 //同样先封装一个promise对象发送ajax请求
 var p1 = function(url){
       return new Promise(resolve,reject){
         $.get(url,function(data){
             resolve(data)
         })
       }   
   }

    Promise.all([
        p1("url1"),
        p2("url1"),
        p3("url1"),
        p4("url1"),
        p5("url1")
        //这些请求是并发执行的
    ]).then(function(result){
        console.log(result) // 这个结果是所有数据的结果组成的数组
    })

```

## 如果要用promise同步执行/顺序调用
```js
   var p = function(url){
       return new Promise(resolve,reject){
         $.get(url,function(data){
             resolve(data)
         })
       }   
   }
  var arr = []
            p("https://cnodejs.org/api/v1/topics?tab=ask")
            .then(function(data){
                arr.push(data);
                return p("https://cnodejs.org/api/v1/topics?tab=share")
            }).then(function(data){
                arr.push(data);
                return p("https://cnodejs.org/api/v1/topics?tab=ask")
            }).then(function(data){
                arr.push(data);
                return p("https://cnodejs.org/api/v1/topics?tab=good")
            }).then(function(data){
                arr.push(data);
                console.log(arr);
            })
                
            })

```

## 请求某个资源,如果没有返回,就渲染默认值
-  需要利用到`race`接口 ,赛跑接口
```js
    var p = url=>{
        return new Promise((resolve,reject)=>{
            $.get(url,data=>resolve(data))
        })
    }
    Promise.race([  
        p("url1"),
        p("url2"),
        p("url3"),
        p("url4")
    ]).then(result=>console.log(result))
    //result的数据是四个中最先获取到的那一个
```

- 另一种玩法,**数据如果1秒内没有加载完成则抛出请求数据超时提醒**
```js
var p = url=>{
        return new Promise((resolve,reject)=>{
            $.get(url,data=>resolve(data))
        })
    }

var p2 = ()=>{
    return new Promise((resolve,reject)=>{
        setTimeout(()=>{reject("请求数据超时")},1000) 
        //如果超过1秒的话抛出提醒 进入catch
    })
}
Promise.race([
    p("https://cnodejs.org/api/v1/topics?tab=ask"),
    p2()
]).then(result=>{
    console.log(result)
}).catch(data=>console.log(data))
```
