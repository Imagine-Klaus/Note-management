# axios 拓展

#### 1、简介

- `Axios`是基于`promise`的`HTTP`库，可以用于浏览器和 node.js 的服务器端通信。

- 官网： [axios](https://github.com/axios/axios)

#### 2、使用方式：

- `script` 导入： `<script src="https://unpkg.com/axios/dist/axios.min.js"></script>` 
- `npm` 安装：`npm install axios` `npm install vue-axios`
- `import Axios from 'axios'`; `import VueAxios from 'vue-axios'`;
- `Vue.use(VueAxios,Axios)`;
    

#### 3、注意点：

- **`axios` 不支持跨域请求数据**

#### 4、Vue 中使用方法

- ** 与vue-resource 类似*

```javascript
    this.$http.get('http://localhost:16688/slides111')
        .then((res) => {
            //这里使用箭头函数的原因是axios的回调函数中都有各自的作用域,使用箭头函数的话作用域就继承自外部作用域上下文
            this.list = res.data;
        })
```

#### 5、使用方法：

- 第一种使用方法：

```javascript
    import axios from 'axios'


    axios( {
        methods:'get',
        url:'xxxx'
    } ).then( res => {
        console.log( res )
    } )
```


- 第二种使用方法：

```javascript
    import axios from 'axios'


    axios.get('url').then((res) => {
        console.log( res )
    })


    //如果需要传参的话 ,需要在请求地址后面加个对象
    axios.get( 'xxx' , {
        params:{
            id:12345
        }
    }).then( res => console.log( res ))
```

- 第三使用方法.创建Axios实例(提取公用js)：
- 官方说明：

提取公用js文件

```javascript
    import axios from 'axios';
    export var HTTP = axios.create({
      baseURL: 'http://localhost:16688/'  // 提取域名
    });
```

调用公用js文件
```javascript
    import {HTTP} from '../kits/common.js'

    HTTP.get('slides').then((res) => {
        this.list = res.data;
    })
```