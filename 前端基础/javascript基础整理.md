[TOC]
# 1.基础知识补充
## 1.1 类型转换
### 1.1. 1转换成字符串
- String(值) --->什么类型的数据都能转
- x.toString() --->不能转null和undefined
- 值+""   --->隐式转换 

### 1.1.2 转换成数值
- Number(值)  //最不推荐的方式
- parseInt("字符串")  || parseFloat("字符串")
- 值-0    --->隐式转换
- ~~字符串
    + 例如:~~"a12" --->如果字符串中有英文字符 会返回0
> @如果本身不是个数值类型的字符串,转换之后**会变成NaN**<br/>
 

### 1.1.3 转换成布尔值
- Boolean
- if(xxx)
- !!值
> 

## 1.2数据类型
### 1.2.1 基本数据类型
- number 数值型 
    + NaN   (特殊的number)
- string 字符串型 
- boolean 布尔型 
- undefined 未定义 
- null  (特殊的Obj) 
> 基本数据类型是**直接存储在栈中的** ----->**大量拼接字符串**的时候**会非常消耗内存**

###1.2.2 复杂数据类型
- object
> 复杂数据类型 **只将地址存储到栈中**,**数据存储在堆中** 地址指向存储在堆汇总的数据

## 1.3 部分运算符操作
- Math.pow(a,b) //求a的b次方
- Math.round(c) //求c的四舍五入
- Math.ceil(a) // 求a的向上取整
- Math.floor(a) //求a的向下取整

## 1.4 逻辑语句

### 1.4.1 if语句
```javascript
if (condition) {
        statement1
    } else if (condition) {
        statement2
    } else {
        statement3
    }
```

### 1.4.2 三元表达式
```javascript
   //举个栗子:
    var age = 18;
    age>18?alert(1):alert(2)
```

### 1.4.3 switch
```javascript
    switch (expression) {
        case value:
            statement
            break;
        case value:
            statement
            break;
        default:
            statement
    }
```

### 1.4.5 循环语句for
```javascript
for (initialization; expression; post-loop-expression)｛
	statement 
｝
```
### 1.4.6 break和continue
- break是**跳出循环** 执行循环语句下面的代码
- continue是**跳出当前循环** ,执行下一次循环

### 1.4.7 while语句

- while语句 先判断后执行
> `while(expression) statement`

- do while 语句 先执行后判断
> `do {statement} while (expression);`


# 2.字符串
## 2.1 字符串的一些方法
其中`concat()` 、`slice()`、`indexOf()`为数组字符串**共有的方法**
### 3.1.1 charAt()
- `charAt(x)` ：返回指定位置的字符串
```js
var str  = "1234567"
console.log(str.charAt(2))  ---->返回 "3"
```
### 2.1.2 substring()、substr()、slice()
> 三者功能相似,*都可以省略第二个参数*,其中参数缺略有不同
- substring(start,end) 
    + start:开始位置
    + end:结束位置
- substr(start,length)
    + start(开始位置)
    + length(要截取的长度)
- slice(start,end)
    + start:开始位置
    + end:结束位置
    
### 2.1.3 concat()
- `concat()`可用于连接两个字符串     

### 2.1.4 trim()
- `trim()`方法可以去掉两端的空格,不影响原来的字符串

### 2.1.5 indexOf()
- `indexOf()`方法能够返回指定元素在数组/字符串中的位置 **如果没有,则返回-1** (在数组去重中引用较多)

### 2.1.6 split(separator,howmany)
- 参数separator:字符串或者是正则表达式
- 参数howmany(可选):返回的子串不会多于这个参数指定的数组


# 3.数组
## 3.1 数组的声明
```js
        //通过构造函数声明（更加通用）
        var arr = new Array("a", "b", "c");
        //通过字面量声明（更加简便）
        var arr = ["a","b","c"];
```
## 3.2 数组的赋值和取值
- 赋值
    + 数组名[索引号] = 值 //arr[0]=100
- 取值（通过数组名和索引即可取到对应的值）
    + 数组名[索引号]  //console.log(arr[0])

## 3.3数组的遍历 

```js
for (var i = 0; i < arr.length; i++) {
        console.log(arr[i]);
    }
```

## 3.4 操作数组的一些方法
### 3.4.1 join()
    ```js
     var arr = [1,3,4,5,6];
      var Str = arr.join("|");   //返回一个用“|” 切割而成的字符串
      console.log(Str) // ---->"1|3|4|5|6"
    ```
>  join() 方法**不会改变原来的数组**，调用该方法会返回按既定字符分割而成的**字符串**

### 3.4.2 push()和pop()
- `push()`方法 ：从数组的最后面推入一个数据 
- `pop()` 方法 ：删除数组中最后一个元素
```js
//push方法
var arr = [1,2,3];
arr.push(4,5,6)
console.log(arr)

//pop方法
var arr =[1,3,6];
console.log(arr.pop())  //删除元素的末尾项并返回被删除元素
console.log(arr)
```
### 3.4.3 shift() 和 unshift()
- `shift`方法：从数组的首端删除一个元素  ---->返回被删除元素的值
- `unshift`方法: 从数组的首端添加一个元素
```js
//shift方法
var arr = [4,5,67]
console.log(arr)   --- >  [5,67]    
console.log(arr.shift())   ----> 5

// unshift方法
var arr = [4,5,67] 
arr.unshift(6)   //添加数字6到数组中去
console.log(arr.unshift())  -- >4   //返回数组的长度 
```
### 3.4.4 sort()
- `sort()`对数组进行正序排列
```js
/* 方式一 */
var arr = [1,4,2,5,67];
arr.sort();
console.log(arr); -->[1,2,4,5,67]

/* 方式二 */
var arr = [1,4,2,5,67];
arr.sort(function(a,b){
    return a-b; 
}
/* 输出正序排列 */


var arr = [1,4,2,5,67];
arr.sort(function(a,b){
    return b-a;      
}
console.log(arr)----> [67,5,2,4,1]
/*输出从大到小的排列*/

```    
### 3.4.5 reverse()
- `reserve()` 用于数组的反转
```js
var arr = [1,4,5,6,2,3]
arr.reverse();
console.log(arr);  ---> [3,2,6,5,4,1  ]
```
### 3.4.6 concat()
- `concat()`方法用于将两个数组拼接起来 并返回一个拼接完成后的新数组,**不会影响原数组!!**
```js
var arr1 = [13,'爱你','小丽']
var arr2 = [123,'想你','Lily']
var arr3 = arr1.concat(arr2);
console.log(arr3) ---->[13,'爱你','小丽',123,'想你','Lily']
```
### 3.4.7 slice()
- `slice(start,end)` 从已有的数组中返回选定的元素 **不会修改原来的数组,只是返回一段从中截取的数组**
    + start:必选 指定从何处开始选取
    + end :可选 规定从何处结束选取
```js
var arr = [3,4,5,7,1]
console.log(arr.slice(0,1)) --->返回[3]
console.log(arr.slice(0)) ---> 返回[3, 4, 5, 7, 1]
```
### 3.4.8 splice()
- ` splice(index,howmany,item1,.....,itemX)`
    + 必须:index 是规定添加/删除项目的位置
    + 必须:howmany 是要**删除**项目的数量,如果设置为0则不删除
    + 可选:向数组中添加的新项目
```js
   var arr = [1,3,4,56]
   arr.splice(0,2)  --->/* 从0位置处删除2个 */返回被删除的元素
   console.log(arr)  --->被删除后返回[4,56]
   
   var arr = [243,32,11]
   arr.splice(0,0,1,3,4)  --->从0位置处开始添加元素1,3,4
   console.log(arr) ---返回[1, 3, 4, 243, 32, 11]
    
```
### 3.4.9 forEach() （ES5新增）
- forEach可以遍历数组中的每个元素
```js
  var arr = [1,3,"面试去"]
  arr.forEach(function(item,index){
      console.log(index+'-----'+item)
  })
  //结果
  0-----1
  1-----3
  2-----面试去
```
### 3.4.10 map() （ES5新增）
> `map()`能够迭代数组中的每一个元素。**并调用回调函数来处理每一个元素，并返回一个新的数组**
```js
var arr = [1,3,4,5,6]
var arr2=arr.map(function(item){
    return   item+1
})
console.log(arr2)   ----->[2,4,5,6,7]


/* 利用map遍历数组把字母规范化 */
var arr = ['adam', 'LISA', 'barT'] 
arr.map(function(item){
    //首先将所有的字母都转化为小写
    item = item.toLowerCase();
    //将首个字母截取出来转化为大写
    var a = item.substring(0,1).toUpperCase()
    var b = item.substring(1)
    return a + b;
})

/* 把一个字符串13579先变成Array——[1, 3, 5, 7, 9]， */
var str = "13579"
var arr = str.split("")
arr.map(function(item){
    return item-0;
})

---->[1, 3, 5, 7, 9]


```
### 3.4.11 filter() （ES5新增）
- `filter()`把其中的回调函数作用于数组中的每一个元素,**返回true则保留**并添如新数组中
    + **可以用来去掉重复的元素**
```js
    var arr = [2,3,4,5,6]
    arr.filter(function(ele,index,self){
        console.log(ele) //每个元素
        console.log(index)//索引
        console.log(self)  //就是arr 本身
        return true 
    })
    
    // 去掉重复的元素   
    var arr=[3,4,5,3,7,4]
    arr.filter(function(ele,index,self){
        return self.indexOf(ele)===index;  
         /* 判断第一个出现该元素的索引是否与当前索引匹配  去掉重复的 */
         /* 后续的重复元素位置与indexOf返回的位置不相等，因此被filter滤掉了。 */
           
    })


    /* 筛选出所有的素数（质数） */
    var arr  = [1,2,3,4,5,6,8,10]
            arr.filter(function(ele,index,self){
                if(ele ==1){
                    return false       
                }else{
                  for(var i=2;i<ele;i++){
                      if (ele%i==0){
                          return false;
                      }
                  }
                  return true;
                }
            })

            结果--->[2,3,5]
```

### 3.4.12 every() （ES5新增）
- `every()`判断每个元素是否符合回调函数中的条件  返回Boolean
```js
var arr = [1,4,3,2]
arr.every(function(ele,index){
    return ele>2;
})

-----> false;
```

### 3.4.13 some() （ES5新增）
- `some()`判断是否有元素符合回调函数的条件  返回Boolean
```js
var arr = [1,4,3,2]
arr.some(function(ele,index){
    return ele>2;
})

-----> true;
```
### 3.4.14 reduce()和 reduceRight() （ES5新增）
-`reduce(x,y)`
> reduce能够把回调函数作用于调用他的每一个元素上，`reduce`把运算的结果继续和序列的下一个元素作累计运算
```js
/* 算出数组中的总和 */
var arr = [1,3,4];
arr.reduce(function(x,y){
    return x + y 
})

```
- reduceRight()就是**从数组的最后一项开始**向前遍历到第一项

# 4逻辑题整理
## 4.1去除数组中重复的元素
```js
    //第一种   “计数方式” 检测重复的元素包括自己 如果==1 就说明只有一次，然后放入新数组中
    var arr = ["c", "a", "z", "a", "x", "a", "c"];
    var arr2 = [];
    for (var  i= 0;  i< arr.length; i++) {
        var mount =0;
        for (var j = i; j < arr.length; j++) {
                if(arr[i]==arr[j]){
                    mount++;
            }        
        }
        if(mount==1){
            // arr2.push(arr[i])
            //或者以另一种方式放入数组中
            arr2[arr2.length] = arr[i]
        }        
    }
    console.log(arr2)

    //第二种  立flag方式  如果flag为true  则放入数组中去
    
        var arr = ["c", "a", "z", "a", "x", "a", "c"];
        var arr2 = [];
        for (var i = 0; i < arr.length; i++) {
            var flag = true;//假设没有重复的
            for (var j =0 ; j < arr2.length; j++) {
                if(arr2[j]==arr[i]){ //如果数组中没有重复的 则循环完成后flag依旧为true
                    flag=false; //说明有重复得
                }
            }
            if(flag){ //如果经历过重复检测后flag没有变化 ,则放入数组中去
                arr2[arr2.length] = arr[i]
            } 
        }
        console.log(arr2)  
    
    // 利用indexOf 返回-1的方法
    var arr = [1,2,3,4,1,2];
    var arr2 = [];
    arr.forEach(function(item){
        if(arr2.indexOf(item)==-1){
           arr2.push(item)
        }
    })
    console.log(arr2)

    // 第三种  运用filter方式
    
        var arr = ["c", "a", "z", "a", "x", "a", "c"];
        arr.filter(function(ele,index,self){
            return self.indexOf(ele)==index
        }) 

        
    //第四种 用空对象记录的方式
    var arr = [1,1,3,3,4,56,7]
    var obj = {};
    temArr = [];
    for (var i = 0; i < arr.length; i++) {
            if(!obj[arr[i]]){
                temArr.push(arr[i]);
                obj[arr[i]] = 1
            }
    }
    console.log(temArr)        

    /* 第五种  使用ES6语法 */
    let arr = [1,3,4,5,3,1]
    let newArr =  [...new Set(arr)]
    console.log(newArr)
```

## 4.2 求1~100之间的质数
```js   
    var arr = [];
        for(var i =2 ;i<100;i++){
        var flag = true  //假设是质数
            for(var j=2;j<i;j++){
                if(i%j==0){
                flag=false;
                break;
                }
            }
            if(flag){
                arr.push(i)
            }
        }
console.log(arr)

```
## 4.3 利用冒泡原理，编写从大到小的冒泡排序
```js
  var arr = [4,3,2,6,7,1];
  for (var i = 0; i < arr.length-1; i++) {
      var flag = true; //假设已经是从大到小排列
     for (var j = 0; j < arr.length-1-i; j++) {//减去i是多比较的次数
      var temp;
       if(arr[j]<arr[j+1]){
           flag = false;
           temp = arr[j];
           arr[j]=arr[j+1];
           arr[j+1]=temp;
       }   
     }
     if(flag){ //如果假设正确  跳出当前循环
         break; 
     }      
  }
  console.log(arr)
```

## 4.4 getFibonacci(month),得到兔子数列对应的值。 1 1 2 3 5 8 13 。。。。。
```js
 function getFibonacci(num){
     if(num==1 || num==2){
         return 1;
     }else{
         return getFibonacci(num-1)+getFibonacci(num-2)
     }
 }
 getFibonacci(12)
```

## 4.5 求回文数 "1234321" 类似这样的  1~10000
```js
    /* 将传入的数字一个个拆分 */
    function resolve(num){
        while(num>0){
           var arr =[];
           arr.push(num%10)     
           num = parseInt(num/10);
        }
        return arr;//将每个数字拆分到数组中
    }

    function justify(arr){
        /* 通过数组比较两边的数字来判别是否相等 */
       for (var i = 0; i < arr.length/2; i++) {
                var flag = true;//假设是回文数
                if(arr[i]!==arr[arr.length-1-i]){
                    flag = false;
                    return flag;
                }         
       }
       if(flag){
           return flag;
       }                
    }

    for (var i = 0; i < 10000; i++) {
       
        if(check(fenjie(i))){
            console.log(i);
        }
    }
```

## 统计每个数组出现的次数  ！！！important
> 设定空对象,对数组进行遍历,将数组中的每一项作为obj中的键来存储次数,判断如果存在则+1,不存在则设为1
```js
var arr = [1,2,3,4,5,4,3,2,6,7,5,5]
var obj ={};
for (var i = 0; i < arr.length; i++) {
    var item = arr[i];
    if(obj[item]){
        obj[item] = obj[item]+1;
    }else{
        obj[item]=1;
    }    
}
for(k in obj){
    console.log(k+"一共出现了"+obj[k])
}
```

















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