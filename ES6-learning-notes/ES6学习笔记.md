[TOC]
# 一、const 语法声明

> const(声明的是一个只读的常量) , const与let类似,**只是let只能在声明的时候赋值**,`const a = 123`,不可以随意修改
- 一旦声明,常量的值就可不改变
- **作用域与let相似**只在声明所在的块级作用域内有效
- const命令声明的常量和let一样也是**不提升**,同样存在**Temporal Dead Zone**( 暂时性死区),只在声明之后有效

# 二、Promise

## 2.1 基本使用
> promise就是个构造函数
```js
    var p =  new Promise(function(resolve,reject){
        reject("可以把数据传递出去")
    })
    p.then(function(data){
        console.log(data) //在then中就可以接收到这个数据
    })
```

## 2.2 ajax使用 

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

## 2.3 解决回调地狱的问题
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

## 2.4 如果要用promise同步执行/顺序调用
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

## 2.5 请求某个资源,如果没有返回,就渲染默认值
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

##  3.1 命名导出
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
## 3.2 默认导出
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
```js
function* entries(obj) {
  for (let key of Object.keys(obj)) {
    yield [key, obj[key]];
  }
}

for (let [key, value] of entries(obj)) {
  console.log(key, '->', value);
}
```


# 五、字符串方法扩展
## 5.1 includess()方法
- 判断字符串中是否包含指定字符(有的话true, 否则false)
- **参数一**：匹配的字符串 **参数二**: 从第几个开始匹配

## 5.2 startWith()
- 判断字符串中是否以特定的字符串开始

## 5.3 endWith()
- `endsWith()`判断字符串是否以特定的子串结束


# 六、函数扩展学习

1.可以传递默认值
2.参数可以进行解构解析
3.`...rest` (rest参数,名称可以自定义) --- 把单个数据项解析为数组
```js
    function foo(a,b,...param){
        console.log(a);  // 1
        console.log(b);  // 2
        console.log(param); //[3, 4, 5]
    }
    foo(1,2,3,4,5);
```
4.`...` 扩展运算符

- **用途一:传递参数**
```js
function foo(a,b,c,d){
    console.log(a+b+c+d)
}
var arr = [1,2,3,4]
foo(...arr)//解构成单个项传入数组
```

- **用途二：合并数组**

```js
    let arr1 = [1,2,3];
    let arr2 = [4,5,6];
    let arr3 = [...arr1,...arr2];
    console.log(arr3);//[1,2,3,4,5,6]
```

## 6.1 箭头函数
- 这样声明一个函数
    + 如果箭头函数的代码块多于一条语句的话，使用大括号 ` {} ` 
```js
    let foo = () => console.log(123)

    //多条语句不能省略{}
    let foo2 = () => {alert(1);console.log(123)}
```

### 6.1.1 箭头函数注意事项

 1. 箭头函数的this绑定看的**是this所在的函数定义在哪个对象下，绑定到哪个对象则this就指向哪个对象**
 2. 箭头函数不可以new， 也就是说它不是构造函数
 3. 函数内部不可以使用arguments，可以用rest参数替代

 ```js
    let foo2 = (...rest) => {
    console.log(rest)
}
    foo2(1,3,4)

    //结果 [1,3,4]
 ```


# 七、模板字符串
ES6中拼接字符串采用`${}我是变量外的文字`的方式,将变量包裹在{}中 
```js
<!-- 旧版字符串拼接 -->
var name = 'luling'
console.log('名字是' + name)

<!-- 新版字符串拼接 -->
var name = 'lulinglong'
console.log(`名字是{name}`)
```

# 八、Generator 函数
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

## 8.1 两个神奇的地方
### 8.1.1 g()并不是执行函数
不是执行函数,也不会返回函数的运行结果,而是一个**指向内部状态**的指针对象,也就是(Iterator Object)遍历器对象
### 8.1.2 分段执行
```js
function* g() {
    yield 'a';
    yield 'b';
    yield 'c';
    return 'ending';
}

var gen = g();
gen.next()
// //{value: "a", done: false}  
// a表示g函数执行到一个yield之后得到的执,false表示还没有执行完
 
gen.next()
// {value: "b", done: false} 
gen.next()
// {value: "c", done: false} 
gen.next()
// {value: "ending", done: true}  执行完成 true
```

## 8.2 关于yield 语句
（1）遇到yield语句，就暂停执行后面的操作，并将紧跟在**yield后面的那个表达式的值**，**作为**返回的对象的value属性**值**。

（2）**下一次调用next方法**时，再**继续往下执行**，直到遇到下一个yield语句。

（3）如果**没有再遇到新的yield语句**，就一直**运行到函数结束**，直到return语句为止，并将return语句后面的表达式的值，作为返回的对象的value属性值。

（4）如果该函数没有return语句，则返回的对象的value属性值为`undefined`。

- 注意 yield
    + yield语句**只能用于function*的作用域**，如果function*的内部还定义了其他的普通函数，则函数内部不允许使用yield语句。
    + yield语句如果参与运算，必须**用括号括起来**。
```js
    console.log(3 + (yield 4)) //打印7
```

## 8.3 利用for of 语句遍历Generator函数
注意:使用`for of`遍历函数无法遍历到`return`后的值
```js
function* foo() {
    yield 1;
    yield 2;
    yield 3;
    yield 4;
    yield 5;
    return 6;
}

let a = foo();

for (let v of a) {
  console.log(v);
}
// 1 2 3 4 5
//到6的时候return 终止跳出了 不会打印6
```

# 九、const 与 let 声明
## 9.1 ES6的块级作用域
- ES6中`{}`中就可以形成一个块级作用域,在块级作用域内的变量不能被外部访问到
- 在块级作用域内部,变量只能先声明再使用,不存在**预解析**,
```js
    if(true){
        console.log(a)
        let a = 3
        //Uncaught ReferenceError: a is not defined
    }
```

## 9.2 let 
`let`声明在**同一个作用域**下不能重复赋值 ,会出现如下的错误:
```js
    Uncaught SyntaxError: Identifier 'a' has already been declared    
```  

## 9.3 const
1. const 声明的常量**不能重新赋值**
2. const 声明的常量**必须初始化**

## 9.4 TDZ 的概念
> 如果区块中存在let和const命令，这个区块对这些命令声明的变量，**从一开始就形成了封闭作用域**。凡是**在声明之前就使用这些变量，就会报错**。这在语法上，称为“暂时性死区”（temporal dead zone，简称 TDZ）。

# 十、变量的解构赋值

## 10.1 数组的解构赋值
```js
    let [a, b, c] = [1, 2, 3]

    同时也可以赋默认值

    let [a=111, b, c] = [, 123, 4];
```

## 10.2 对象的解构赋值
- 对象的赋值不用考虑顺序问题,因为**原本就是一一对应的**
```js
    let { foo, bar } = { foo: 'hi', bar: 'haha' }
```
### 10.2.1 对象属性别名
- **如果有了别名，那么原来的名字就无效了**
```js
    //如果有了别名，那么原来的名字就无效了
    let { foo: foo2, bar } = {bar: 'hi', foo: 'girl'}
    console.log(foo2) // girl
    console.log(foo) //undefined
```

## 10.3 字符串的解构赋值
```js
    let [a, b, c, d] = "hell"
    console.log(a,b,c,d)

    //结果 --->  h e l l

    获取字符串的长度
    let { length } = "hi"
    console.log( length ) // ---> 2
```


# 十一、类与继承的概念
## 11.1 关于类class
- 语法 `class`+ `类的名称`
- **构造函数**中放入在`constructor`中
- **静态方法**定义在`static`后面
    + 静态方法只能通过原函数调用
    
```js
        class Animal{
            //构造函数
            constructor(name){
                this.name = name
            }

            //静态方法
            static showinfo(){
                console.log(123)
            }
            
            showName(){
                console.log(this.name)
            }
        }

        let dog = new Animal('xiaoming')
        dog.showName() //'小明'
        Animal.showInfo()  // 123
```
## 11.2 关于继承
```js
    //承接上面代码
    class Dog extends Animal{
        constructor(name, color){
            super( name )  //调用父类
            this.color = color 
        }

        showColor(){
            console.log(this.color)
        }
    }

    let Instance = new Dog('doudou','yellow')
    Instance.showColor()
```

# ES6对象方法学习

## Object.keys()
- 语法: `Object.keys(obj)`
- 会返回一个由给定对象**自身可枚举属性**组成的数组(由对象键名组成的数组)。数组中属性名的排列顺序与`for...in`循环遍历的一致,`for...in` 还**会枚举其原型链上的属性**
- `Object.keys`返回的所有元素都是**字符串**
```js
    var obj = { name: 'lulinglong', age: '18' }
    Object.keys(obj)
    //返回结果----->["name", "age"]
```