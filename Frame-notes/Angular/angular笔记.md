# Angular

## 1 基础学习

### 性能优化参考
<a>https://github.com/atian25/blog/issues/5</a>

### 简单指令
- 在angular中以ng-开头的html标签属性，称之为指令
- ng-app: 选择angular去管理哪一部分的html代码, 管理的是ng-app所在
   元素的子元素及其本身
- ng-click: 也是用来注册点击事件
- ng-model: 可以指定一个属性值，这个属性就表示当前文本框的value值
- ng-init: 可以对数据进行初始化操作，给一个默认值。

### 模块
- 创建模块:`var app = angular.module('模块名',[])`
- *注意* 如果不依赖其他模块，也需要提供一个空的数组
- 需要在ng-app指令的属性值上写我们的模块名(房子)

### 控制器
- 创建控制器:`app.controller('控制器的名字',function($scope){})`
- 如果要改变页面{{testName}}的值，就直接改变$scope.testName值就可以
- *注意* 要显示数据模型的值,就要在它所在标签或者父标签上加上ng-controller指令
  ng-controller的值就是我们想要显示的数据模型所在控制器的名字
  :ng-controller="window"

### 双向数据绑定
- 数据模型($scope.属性)的改变，会影响内容的显示(文本框的值)
- 我们改变了文本框的值，对应的数据模型发生了改变.
- 这种相互影响的关系就称之为双向数据绑定.
```html
<body ng-app="myApp">
    <div  ng-controller="demoController">
            {{myname}}
            <button ng-click="setName()" >起名字</button>
    </div>

    <div ng-controller="addcount">
        <input type="text" ng-model="A">+
        <input type="text" ng-model="B">+
        <input type="button" ng-click="add()" value="相加">
            <input type="text" ng-model="result">
    </div>
    <script type="text/javascript" src="js/angular.js"></script>
    <script type="text/javascript">
            var myApp = angular.module("myApp",[]);
            myApp.controller("demoController",function($scope){
            
                $scope.setName=function(){
                $scope.myname ='小人';
                }
            })
            myApp.controller("addcount",function($scope){

                $scope.A=1
                $scope.B=2

                $scope.add=function(){
                $scope.result = +$scope.A + +$scope.B 
                }
            })       
    </script>
</body>
```

### 在angular中操作DOM
> 在angular中操作DOM是不推崇的，但是也提供了一种方法
- 运用**angular.element()**,是jqLite对象;
    + 使用方式相当于$，但其中传入的参数不是选择器，而是原生DOM对象例如*angular.element(document.getElementById())*

### MVC 思想
- M : Model: 存储，获取数据
- V : View 视图，把数据呈现给用户
- C : Controller 做一些控制和调度的操作

### 总结Angular开发的具体流程
- 1.在html中引入angular.js文件;
- 2.在需要的html中加入ng-app指令并指定个名字，告诉angular要管理的模块；
- 3.在js中创建模块 var xxx = angular.module("ng-app的名字",[]);
- 4.在页面上需要控制的模块加入控制器指令例如*ng-controller="控制器名字"*,然后再js中创建控制器:
   + myapp.controller("控制器名字",function($scope){})
- 5.建模：根据具体的结构抽象出js对象;
- 6.通过$scope做一些初始化操作 *$scope.name='小红'*；
- 7.通过ng-model,ng-click等等将属性的值在页面上展示出来
- 8.写一些具体的JS操作；

### 创建控制器的三种方式:
- 传统方式:把全局函数当控制器(不创建模块和控制器):
```javascript
     function demoController($scope){
      $scope.name = '小明'
    }
```

- 面向对象方式：无需$scope:
```html
     <div ng-controller="demoController as obj">
     	{{obj.name}}
     </div>
	 
	 <script type="" src="js/angular.js"></script>
	 <script type="text/javascript">

	 	 var app = angular.module("main",[]);
	 	 app.controller("demoController",function(){
	 	 	 this.name = '小明';
	 	 })
	 </script>				
```
- 安全的方式创建控制器
    - 就是为了避免压缩后代码无法运行
```javascript
    // 把第二个参数改为一个数组,在数组把我们需要的参数的名字写上
    // 回调函数就写在数组的最后一个元素上
    // *注意*：数据中传入的元素的顺序,要和function的中顺序一一对应
    app.controller('demoController',['$scope','$log',function($scope,$log){//通过这种方式在形参中可以写任意名字，注意单引号
      $scope.msg = 'hello World!'
      $log.log('哈哈哈哈！')
    }])
```

## 2 更多指令学习

### 解决在angular加载前的表达式闪烁
#### 1.0 ng-bind
- 使用方式:在标签中加入**ng-bind**=""属性
```html
    例如:<p ng-bind="name"></p>
    //不会在angular加载前显示表达式,在加载了 angular  后再p标签内部生成msg的值
```


#### 2.0 ng-clock
- 使用方式:在标签中加入**ng-clock**类名，将表达式放入有ng-clock的类名的元素中
```css
            <style>
            .ng-cloak{
                display:none;
            }
            </style>
```
> 首先在style中设置**display：none**,在angular加载完成后会移除这个类

```html
            <div class="ng-cloak">
                {{msg}}
                {{msg}}
                {{msg}}
            </div>
```

### ng-repeat

- ng-repeat 一般用法:
```html
<body ng-app="myApp">
<div ng-controller="demoController">
    <ul>
        <!-- ng-repeat 遍历生成数据，类似for in 循环的语法 -->
        <p ng-repeat="list in home" >
            {{list.name}} , {{list.age}}
        </p>
    </ul>
</div>
<script src="libs/angular.js"></script>
<script>
    // 1.创建模块
    var app = angular.module('myApp', [])

    // 2.创建控制器
    app.controller('demoController',['$scope',function($scope){
        // 初始化数据
        $scope.home = [
            {name:'小明',age:18},
            {name:'小红',age:18},
            {name:'小朋',age:28},
            {name:'小月',age:19},
            {name:'小黑',age:18},
            {name:'小白',age:20}
        ]
    }])
</script>
</body>
``` 
> 值得注意的是如果遍历数组而数组中**存在相同的数值**例如**[1,2,3,4,5,1]**,那么要写成ng-repeat="item in arr **track by $index**"

-----------------------------------

- ng-repeat遍历时提供方法:
    + $odd : 为true时，表明当前数据是否是第[奇]数条
    + $even: 为true时，表明当前数据是否是第[偶]数条
    + $first: 为true时，表明当前数据是否是第1条
    + $last: 为true时，表明当前数据是否是最后一条
    + $middle: 为true时，表明当前数据是否是中间的数据

   + 运用于隔行变色案例中:
```html
    <!--  $odd, ng-repeat在每次遍历时都提供这样的值，为true表明当前数据是否第奇数条,从索引为0开始判断的 -->
    <!--  $even, ng-repeat在每次遍历时都提供这样的值，为true表明当前数据是否第偶数条,从索引为0开始判断的 -->
      <li class="{{ $odd ?'red':'green'}}" ng-repeat="item in data">
        {{item.name}},{{item.age}}
      </li>
```
```html
    <ul>
    	<li class="{{$index%2==0?'red':'green'}}" ng-repeat="item in data">
    		{{item.name}}:{{item.age}}
    	</li>
    </ul>
```

### ng-class
- ng-class 可以帮助我们控制元素的class样式，
- ng-class 可以独立在其他元素上使用，并非一定要和ng-repeat结合

```html
    <!--  ng-class,动态的添加class样式,
      以对象的形式书写，angular会把属性值为true的属性名当作样式添加到class
      class="green" -->
    <li ng-class="{red:item.age>=20, green:item.age>=10&&item.age<20,blue:item.age<10}" ng-repeat="item in data">
      {{item.name}},{{item.age}}
    </li>
```

### ng-show/ng-hide
- 控制元素的显示与否,ng-show与ng-hide作用是相反的(只是设置display:none来隐藏元素)

```html
    <!-- ng-show,控制元素的显示或隐藏,值为true时显示，为false隐藏-->
    <p ng-show="isshow">我是中国人，我爱自己的祖国!</p>
    <!-- ng-hide 值为true时，隐藏当前元素 -->
    <p ng-hide="true">我是小明!</p>

    <script type="text/javascript">
        var app  = angular.module("main",[]);
        app.controller("demo",['$scope',function($scope){
             var isshow = true;
             $scope.showOrhide=function(){
                $scope.isshow = !$scope.isshow;
             }
        }])
    </script>
```


### ng-if
- 控制元素的显示与否:*值为false时，元素会从dom移除*

```html
    <!-- ng-if，也能控制元素的显示或隐藏,为true时显示,为false时【会将当前dom元素移除】 -->
    <p ng-if="true">我是中国人，我爱自己的祖国!</p>
    <h1>ng-if="false"</h1>
    <p ng-if="false">我是中国人，我爱自己的祖国!</p>
```

### ng-switch
- 当ng-switch-when 对应的值等于ng-switch对应值时，当前dom元素就显示

```html
    <div ng-switch="name">
      <div ng-switch-when="小明">
        我是小明，我在这里！
      </div>
      <div ng-switch-when="小红">
        我是小红!
      </div>
    </div>

    app.controller('demoController',['$scope', function($scope){
        $scope.name = '小明'

        switch($scope.name){
            case '小明':
                console.log(123);
                break;
        }
    }])
```

### 其他常用事件指令

> 不同于以上的功能性指令，Angular还定义了一些用于和事件绑定的指令：

- ng-blur：失去焦点
- ng-focus： 获得焦点
- ng-change：内容改变
- ng-copy：复制
- ng-click: <div ng-click="aa()"></div>
- ng-dblclick：双击
- ng-submit：  form表单提单


## 3 自定义指令

### 创建自定义指令
- *注意: 名字要符合驼峰合法*

```javascript
    // 1.创建模块对象
    var app = angular.module('directiveApp', [])

    // 2.创建自定义指令
    // 第一个参数：是指令的名字,必须是驼峰命名法
    //             使用时把大写改成小写，在原来大写前加上-
    // 第二个参数：和控制器的第二个参数类似!
    app.directive('myBtn', [function(){
      // 在这里直接return 一个对象就可以了
      return {
        // template属性，是封装的ui
        template:'<div><button>我是按钮</button></div>'
      }
    }])
```

### 自定义指令中回调里返回的对象的属性
- template: 需要提供一个html字符串,最终会被添加到当前页面使用了自定义指令的位置
- templateUrl:需要提供一个html文件路径，angular会发请求，请求对应的文件，
 把文件内容作为模板插入到自定义指令中间
            : 也可以提供一个script标签的id, angular会把script标签中的内容作为
              模板插入到自定义指令中间,*注意* 要改变script标签的type="text/ng-template"

- restrict: 也是需要提供一个字符串，限制自定义指令的使用形式
    + A : Attribute 表示只能以属性的形式使用
    + C : Class     表示只能以类样式名的形式使用
    + E : Element   表示只能以自定义标签的形式使用
    + ACE : 如果希望多种方
    + 式合适，就把对应值组合在一起。

- replace：需要提供一个布尔值，为true时，模板会被用来替换自定义指令所在标签，
        * 否则是插入到自定义指令中间
        
- transclude: 需要一个布尔值, 为true时会将自定义标签中的内容插入到模板中，
        * 拥有ng-transclude 指令的标签中间

- scope：需要一个对象: 可以用来获取自定义指令的属性值,
    -  给当前对象添加一个属性(如:tmp),属性值以@开头，后面跟上自定义指令的属性名
       然后就可以在模板中使用{{tmp}} 来得到对应的属性值
       + `scope: { tmp:'@mystyle'}`  {{tmp}}
       + `scope: { mystyle:'@'}`     {{mystyle}}

- link: 需要一个function 这是function在angular解析到相应指令时就会执行一次,
    + scope      ：类似于$scope,只不过scope的属性只能在模板中使用
    + element    : 自定义指令所在标签对应的对象(jqLite)
    + attributes : 包含了自定义指令所在标签的必有属性

-  综合案例参考
```html
<body ng-app="app" >
 <div my-btn mystyle="login">
 注册
</div>
 <div class="my-btn" mystyle="register">
 登录
 </div>

<script type="text/ng-template" id="dpl">
        <div>  //必须有东西包裹住
           <p>{{msg}}</p> 
           <button class="{{mystyle}} ng-transclude">{{msg}}</button>
        </div>
</script>
    <script type="text/javascript" src="js/angular.js"></script>
    <script type="text/javascript">
        var app = angular.module("app",[]);
        app.directive('myBtn',[function(){
            return{
                templateUrl:'dpl',
                restrict:'ECA',
                replace:true,
                transclude:true,
                scope:{                    
                     mystyle:'@'
                },
                link:function(scope,ele,att){
                    scope.msg = "像我这样优秀的人"
                }
            }
        }])
    </script>
</body>
```

## 4 过滤器(filter)
- 格式化数据
- 过滤数据(filter)

```html
    <ul>
        <!--  如果指定一个布尔值，或者字符串就是全文匹配 -->
      <!-- 会到对应的todos中寻找，如果当前元素有completed属性且值 为true就会被显示出来。（只会到completed属性中寻找） -->
      <li ng-repeat="item in todos | filter : {completed:true} ">
        {{item.name}},{{item.completed}}
      </li>
    </ul>
```
> **值得注意的是filter前面必须紧跟数据模型**


```html
  <h1>currency</h1>
  <!-- 在数据模型后加上|  再加上过滤器的名字 
        也可以在过滤器名字后指定参数，参数是写在冒号后面的-->
  <p>{{money | currency : '￥'}}</p>

  <h1>date</h1>
  <p>{{myDate | date : 'yyyy年MM月dd日 HH:mm:ss'}}</p>
```

- limitTo

```html
    <h1>limitTo</h1>
  <!-- 第一个参数，表明显示多少个字，第二个参数表示，从第几个字开始显示(索引从0开始) -->
  <p>{{msg | limitTo : 5 : 2}}...</p>
```

- orderBy 及 json

```html
<h1>json</h1>
 <!--  格式化显示json数据，参数指定缩近的长度 -->
 <pre>{{myJson | json : 8}}</pre>
  <h1>orderBy</h1>
  <!-- 对数据进行排序，参数，给+号就按正序排，- 就按倒序排 -->
  <span ng-repeat="item in arr | orderBy:'-'">{{item }}，</span>
```

- 在js中使用过滤器

```javascript
    <!-- $filter 需要在控制器的回调中传入 -->
    // 可以调用不同的过滤器得到相应的结果
      // 参数是一个过滤器的名字
      // 返回值是一个方法
      //        : 第一个参数是需要处理的数据
      //        : 后面的参数是当前过滤器本身需要的参数
     $scope.result =  $filter('currency')($scope.money,'￥')
```

## 5 用$watch监听数据模型

```javascript
    $scope.name = '小明'
      $scope.age = 18

      // $watch可以用来监视数据模型的变化
      // 第一个参数: 数据模型对应的名字(字符串形式)
      // 第二个参数: 相应的数据模型变化就会调用 这个函数
      // 默认会直接执行一次回调函数
      $scope.$watch('name',function(now,old){
        // 第一个参数是变化后的值
        // 第二个参数是变化前的值
        // console.log(now,old)
      })
```
> 也可以用来监听函数的返回值

```javascript
var app = angular.module("app",[]);
    app.controller("democtrl",["$scope",function($scope){
         $scope.name='小明';
         $scope.getName = function(){
          return $scope.name;
         }
         $scope.$watch("getName()",function(now,old){
          console.log(now,old);
         })
    }])
```
> $watch 只能监听$scope的属性或者方法,比如$location方法是angular自带的并不是$scope定义的,就可以`$scope.loca = $location` 然后`  $scope.$watch("loca.url()",function(now,old){}`

- 附：`$location.url()` 可以截取url中锚点的值

## 6 路由
- 根据url中不同的锚点值，在页面显示不同的内容！

### 路由使用

```javascript
    // 1.创建模块
    var app = angular.module('myApp', ['ngRoute'])

    // 2.配置路由规则(约定什么样的锚点值，对应什么样的内容)
    // 第一个参数与controller第二个参数类似
    app.config(['$routeProvider',function($routeProvider){
      
      // 第一个参数：对应的url中锚点值
      // 当angular检测到url中锚点变为/ali里，就会把template对应的内容插入到页面拥有ng-view指令的标签中
      $routeProvider.when('/ali',{
        template:'<div>阿里在二楼!</div>',
        // 指定一个控制器的名字,
        // 当前url中锚点值为/ali时就会执行控制器中的回调函数
        // 我们可以直接在template/templateUrl对应的模板中使用该控制器中对应的$scope属性值
        controller:'demoController'
        //templateUrl
      })
      .when('/baidu',{
        template:'<div>百度在1楼</div>'
      })
    }])
```
