[TOC]
# 一、const 语法声明

> const(声明的是一个只读的常量) , const与let类似,**只是let只能在声明的时候赋值**,`const a = 123`,不可以随意修改
- 一旦声明,常量的值就可不改变
- **作用域与let相似**只在声明所在的块级作用域内有效
- const命令声明的常量和let一样也是**不提升**,同样存在**Temporal Dead Zone**( 暂时性死区),只在声明之后有效

# 二、Promise

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

# 三、es6的导入导出

##  3.1-命名导出
- 在任何变量名或者函数前加上`export`,就可以将它导出
```js
    export const str =  '123'
    export function test () {
        console.log('导出')
    }
``` 
- 然后再另一个文件中导入
```js
    import { str, test } from 'xxx地址'

    <!-- 然后就可以使用了 -->
```
## 3.2-默认导出
- 使用`export default`导出,默认导出模块本身
- 用这种方式导出一个页面只能使用一次
```js
    <!-- 无需指定变量名函数名等等 -->
    exports default function () {
        console.log(123)
    }
```
- 对应的`import` 不同加 `{}`
```js
    import abc from 'xxx地址'
    abc()
```

# 四、使用for..of 遍历

1.语法
```js
    for( var value of arr ) {
        console.log( i )       
    }
```

2.与`forEach`的区别
> `forEach`无法`break`,`return`,`continue`配合使用


## 4.1 for..of 遍历对象的方式
普通情况下, 如果用`for..of`遍历对象的话会报错 `obj is not iterable`
解决方法有两种:
--------
1.使用`Object.keys`方法将键名生成一个数组
```js
    var obj = { name: 'lulinglong', age: 18 }
    for( var key of Object.keys( obj ) ) {
        console.log(`${key}----${obj[key]}`)
    }
```

2.使用`Generator`函数将对象重新包装一下


# 五、模板字符串
ES6中拼接字符串采用`${}我是变量外的文字`的方式,将变脸包裹在{}中 
```js
<!-- 旧版字符串拼接 -->
var name = 'luling'
console.log('名字是' + name)

<!-- 新版字符串拼接 -->
var name = 'lulinglong'
console.log(`名字是{name}`)
```

# 六、Generator 函数
`Generator`函数时ES6中引入的新型函数,用于异步编程的,与普通函数不用的地方
1.函数声明`function`后要跟`*`号
2.函数内部使用`yield`语句,定义不同的内部状态
```js
    <!-- 一个典型的Generator函数 -->
    function* g () {
        yield 1;
        yield 2;
        yield 3;
        return 4;
    }
    <!-- 总共四个阶段 -->
```

## 6.1 两个神奇的地方
1. **g()并不是执行函数**,也不会返回函数的运行结果,而是一个**指向内部状态**的指针对象,也就是(Iterator Object)遍历器对象
2.  