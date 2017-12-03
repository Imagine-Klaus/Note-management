[TOC]
# 第一节、初始Node.js
## 1.1-再说Javascript
- 什么是Javascript
    + 是一门**解释型的**单线程语言

- 常见的浏览器及内核
    + IE -- Trident 
    + FireFox -- Gecko 
    + Chrome -- webkit
    + Safari -- webkit
    + Opera -- Presto
    + Edge -- Chakra 

## 1.2-什么是Node.js
> Node.js 是一种建立在`Google Chrome V8 engine`上的`no-blocking`(非阻塞)和`event driven`(基于事件)的`I/0`平台

# 第二节、终端基本使用
## 2.1 常用操作文件命令
1. md :创建目录
2. `echo on > a.txt`: 创建空文件
3. `del`: 删除文件
4. `ls`: 查看当前目录中的文件
5. `cd`: 进入目录
    + `cd..`: 回退到上一个目录 
6. `rd /s/q 文件名称`: 删除有内容的文件
7. `ren`: 重命名
8. `type 文件名` :查看文件中的内容
9. `cd d:`: 切换盘符d盘 

## 2.2 打开应用
1. `calc`: 打开计算机
2. `write`: 写字板
3. `mspaint`：打开画图
4. `notepad`: 打开记事本

# 第三节、环境安装配置
---
## 1、安装方式

- 普通安装方式[官方网站](https://nodejs.org/zh-cn/);
- 多版本安装方式
    + 卸载已有的Node.js
    + 下载[nvm](https://github.com/coreybutler/nvm-windows)
    + 在C盘创建目录dev
    + 在dev目中中创建两个子目录nvm和nodejs
    + 并且把nvm包解压进去nvm目录中
    + 在install.cmd文件上面右键选择【以管理员身份运行】
    + 打开的cmd窗口直接回车会生成一个settings.txt文件，修改文件中配置信息
        *   `root:C:\dev\nvm` <br> 
            `path: C:\dev\nodejs`
    + 配置nvm和Node.js环境变量
        * NVM_HOME:C:\dev\nvm
        * NVM_SYMLINK:C:\dev\nodejs
    + 把配置好的两个环境变量加到Path中
        * 加入`;%NVM_HOME%;NVM_SYMLINK;`(分号不能少)

## 2、nvm常用的命令
- nvm list 查看当前安装的Node.js所有版本
- nvm install 版本号 安装指定版本的Node.js
- nvm uninstall 版本号 卸载指定版本的Node.js
- nvm use 版本号 选择指定版本的Node.js

# 第四节、代码执行方式
- REPL模式
    + REPL(read-eval-print-loop)；类似于浏览器中的控制台在命令行窗口中
        * 可以使用该REPL做一些代码或者API的测试
    + <b>如何进入REPL：</b>通过终端输入`node`敲回车就可以进入
    + <b>如何退出REPL：</b>两次ctrl+c 可以退出REPL模式，或者执行.exit命令
    + 基本操作
        * 使用下划线“ _ ”，表示上一个命令的返回结果

- 命令行模式
    + 在当前目录下打开控制台
        * 在当前文件夹下，空白区域 shift + 鼠标左键，在此处打开命令窗口
        * 编辑下 下载插件
        * cmd进去到当前目录
        * 在执行文件所在的文件夹地址栏中输入cmd，弹出命令窗口

### 第五节、全局成员概述
- `__dirname` --- 文件路径，不包含文件名称
- `__filename` --- 文件路径，包含文件名称
- `process` --- 提供node进程相关信息
    + `process.argv` 打印出来一个数组：<br>第一个参数是运行环境的路径<br>第二个是**当前执行的文件的位置**<br>从第三个参数开始表示命令行参数
    + `process.arch` 打印当前系统的架构(32位/64位)

# 第六节、模块化开发
## 6.1 传统模块化开发额的缺点
1. 命名冲突
    + 也可以使用闭包解决
2. 文件依赖

## 6.2 前端标准的模块化规范
1. AMD `requireJS`
2. CMD `seaJS`

## 6.3 服务器端标准的模块化规范
1. CommonJs -Node.js

## 6.4 模块化相关的规则
### 6.4.1 如何定义模块

> 一个js文件就是一个模块.模块内部的成员都是相互独立的

### 6.4.2 模块成员的导入和导出
> 模块成员的导出最终以`module.exports`为准<br>
> `exprots` 与 `module.exports` 的关系<br>
> module.exports = exports = {};

- 如果要导出单个成员, 一般我们使用`exports`导出 
- 如果导出的成员比较多,一般我们使用`module.exports`的方式
- **两种方式不能同时使用**

导出模块

```js
    <!-- 导出方式一 -->
    exports.sum = sum //通常用于导出单个成员
    <!-- 导出方式二 -->
    module.exports = sum 
    var sum = function () {
        console.log(123)
    }
```
> `moduel.exports` 也可以导出一个对象,对象中包含多个方法

引入模块

```js
    var sum = require('../.js')
    sum();
```

### 6.4.3 模块成员的导入和导出(global实现)
导出模块

```js
    global.sum = sum 
    var sum = function () {
        console.log(123)
    }
```
导入模块
```js
    //已经加载的模块会被缓存
    require('xx')
    console.log(global.sum)
```
### 6.4.4 模块化细节的注意
1. `require('xx')`中的`.js`后缀可以省略
2. 模块文件的三种情况: `.js`,`.json`,`.node`
    - 在不写后缀的情况下 三种模块的加载优先级 .js > .json > .node
3. **可以加载包例如** `require('vue')`
    - 原理:原先从当前文件夹的`package.json`中查找依赖,然后从当前文件向上查找`node-modules`中的包

# 七、Buffer操作
## 7.1 Buffer基本操作
> Buffer对象是Node处理二进制数据的一个接口。它是Node原生提供的全局对象，可以直接使用，不需要require(‘buffer’)。

- 实例化
    + Buffer.from(array)
    + Buffer.from(string)
    + Buffer.alloc(size)
- 功能方法
    + Buffer.isEncoding() 判断是否支持该编码
    + Buffer.isBuffer() 判断是否为Buffer
    + Buffer.byteLength() 返回指定编码的字节长度，默认utf8
    + Buffer.concat() 将一组Buffer对象合并为一个Buffer对象
- 实例方法
    + write() 向buffer对象中写入内容
    + slice() 截取新的buffer对象
    + toString() 把buf对象转成字符串
    + toJson() 把buf对象转成json形式的字符串
    + 用'+'号拼接会自动把`chunk`转为字符串

# 八、路径操作

8.1 基础路径操作
```js
    //路径操作
    const path = require( 'path' )

    path.basename('/foo/bar/xxx.html')--xxx.html
    path.basename('/foo/bar/xxx.html','html')-->xxx

```

8.2 获取路径

- `console.log(__dirname)`
- `console.log(path.dirname('xxx'))`

8.3 获取扩展名

- `path.extname('index.html')`

8.4 路径的格式化处理

- 对象转为字符串--->`path.format()` 
```js
    let objPath = {
        root: 'D:\\'文件根路径
        dir: 'D:\\Doc\\xx' 文件全路径
        base: '02.js' 文件的名称
        ext: '.js' 扩展名
        name: '02' 文件名称
    }
    let strPath = path.format(objPath)

```
- 字符串转对象--->'path.parse'
```js   
    let obj = path.parse(__filename);
    console.log( obj )
    //结果
    {
        root: 'xxx'文件根路径
        dir: 'xx' 文件全路径
        base: '02.js' 文件的名称
        ext: '.js' 扩展名
        name: '02' 文件名称
    }

```


# 九、文件操作
> 注意：凡是带有`callback`的方法都是异步的,带有`Sync`都是**同步的**

## 9.1 判断文件类型
- 声明 `const fs = require( 'fs' )`
- 是否为文件 `stat.isFile()`
- 是否为目录 `stat.isDirectory()`
```js
    const fs = require( 'fs' )
    //这里的路径是相对路径
    fs.stat('./02.js',(err,stat) => {
        if( err ) return;
        if(stat.isFile()){
            //判断是否为文件
            console.log('文件')
            console.log(stat.atime)//访问时间
            console.log(stat.ctime)//改变时间
            console.log(stat.birthtime)//出生时间
        }else if(stat.isDirectory()){
            //判断是否为目录
            console.log('目录')
        }
    })
```



### 9.1.1 同步操作
- `let ret = fs.statSync( './data.tet' )`


## 9.2 读写文件操作
### 9.2.1 读操作
- `fs.readFile(file[,options],callback)`
-  参数一`file`:文件名称
-  参数二：如果有第二个参数**并且是编码**,那么回调函数获取到的就是字符串
    + 如果没有指定,那么获取到的数据就是**Buffer实例对象**
```js
    //异步操作
    const fs = require( 'fs' )
    const path = require( 'path' )
    let strpath = path.join( __dirname, 'data.txt' )
    fs.readFile( strpath, 'utf8', ( err, data ) => {
        if( err ) return;
        //指定第二个参数
        console.log(data)

        //未指定第二个参数
        console.log(data.toString())
    } )

    //同步操作 与异步不同的是同步没有callback 是返回值
    let ret = fs.readFileSync('./data.txt', 'utf8')
    console.log( ret )
```

### 9.2.2 写文件操作
> 全部写入内存 ，不适合操作大文件
- `fs.writeFile(file,data[,option],callback)`
- 大体上与读文件操作参数相同
```js
     const fs = require( 'fs' )
     const path = require( 'path' )
     拼接绝对路径
     let strpath = path.join( __dirname, 'writeSth.txt' )
     fs.writeFile(strpath, '一生就找那颗星星', 'utf8' (err) => {
        if(!err) {
            console.log('写入成功')
        }
     })
```

## 9.3 大文件操作(流式操作)
- 读文件操作 `fs.createReadStream(path[, options])`
```js   
    const fs = require( 'fs' )
    const path = require( 'path' )

    let spath = path.join(__dirname,'pipe') //读的这个文件
    let dpath = path.join() //将要写入的地址

    let num = 1;
    //一块块写入
    readStream.on('data',(chunk)=>{
        num++;
        writeStream.write()
    })
```
> 输入流 从磁盘到内存 <br>
输出流 从内存到磁盘  

- 写文件操作 `fs.createWriteStream(path[, options])`(pipe.js)

```js
    //声明文件对象和路径对象

    const path = require('path')
    const fs = require('fs')
    
     //合成读路径和写路径,手写路径需要转义
    let spath = path.join(__dirname,'../','test2.zip')
    let dpath = path.join('C:/Users/Administrator/Desktop','test2.zip')
    
    let readStream = fs.createReadStream(spath)
    let writeStream = fs.createWriteStream(dpath)
    
    readStream.pipe(writeStream)
```

# 十、目录操作
- 创建目录 `fs.mkdir(path[, mode], callback)`
```js   
    const path = require('path')
    const fs = require('fs')
    //创建目录
    fs.mkdir(path.join(__dirname, 'abc'),(err) => {
            console.log(123)         
    })
```

- 读取目录  `fs.readdir(path[, mode],callback)`
```js   
    fs.readdir(__dirname, (err, files) => { //files包含所有的文件和文件夹
        files.forEach((item,index) => {
            fs.stat(path.join(__dirname,item),(err, stat) => {
                if(stat.isFile()){
                    console.log(item+'文件')
                }else if(stat.isDirectory()) {
                    console.log(item+'目录 ')
            })
        })
    })
```

- 删除目录 `fs.rmdir(path,callback)`
    + 如果目录中有内容就无法删除
```js
    fs.rmdir(path.join(__dirname, 'abc'),(err) => {
        console.log(err)
    })
```

## 自动化创建目录(init.js)
```js
const path = require('path')
const fs = require('fs')

let fileContent = `
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
</head>
<body>
    <div>welcome to this</div>
</body>
</html>
`;

//所要创建目录的位置
let root = 'C:\\Users\\Administrator\\Desktop'
/*初始化目录结构*/
let initData = { 
    projectName: 'New-Directory',
    data: [
        {
            type: 'dir',
            name: 'img'
        },
        {
            type: 'dir',
            name: 'js'
        },
        {
            type: 'dir',
            name: 'css'
        },
        {
            type: 'file',
            name: 'index.html'
        }
    ]
}

//创建根目录
fs.mkdir(path.join(root,initData.projectName), (err) => {
    if(err) return;
    //遍历data中的所有数据 ,对文件类型做判断
    //如果是目录, 那么mkdirSync
    //如果是文件, 那么writeFileSync 对不存在的文件写入的时候会创建文件
    initData.data.forEach((item) => {
        if(item.type == 'dir'){
            fs.mkdirSync(path.join(root,initData.projectName,item.name))
        }else if (item.type == 'file'){
            fs.writeFileSync(path.join(root, initData.projectName, item.name),fileContent)
        }
    })
})
```

# 十一、静态资源操作
## 11.1 初步实现服务器功能
- 1.引入http对象---`require('http')`
- 2.创建服务器实例对象---`http.createServer()`
- 3.绑定请求事件---`server.on('request',(req, res) =>{res.end('hello')})`
- 4.监听端口---`server.listen(3000)`
```js
    const http = require('http')
    //创建服务器实例对象
    let server = http.createServer();
    //基于事件的编程风格,绑定请求事件
    server.on('request',(req,res) => {
        res.end('hello');
    })
    //监听端口
    server.listen(3000)


    
```
- 5.承接上面，基于事件的方式可以用更简洁的方式
```js

http.createServer((req, res) => {
        res.end('ok')
}).listen(3000,'192.168.15.45',() =>{
    console.log('running')
})
//指定ip地址
```

## 11.2 处理路径的分发
- `createServer`中的`req` 和 `res`
    + `req`：`req.url`可以拿到**url地址栏中**的数据
    + `req`: `req.on` 可以用来处理事件
    + `res`: `res.write` 可以向客户端页面响应内容,可以写多次
    + `res`: `res.end` 用来**结束响应 **,只能写一次
- 如何写入中文？
> res.writeHead(200, { <br>
        'Content-Type':'text/plain;charset=utf8' <br>
  })
```js
    const http = require('http')
    http.createServer((req, res) => {
        res.writeHead(200, { 
            'Content-Type':'text/plain;charset=utf8'
         })
        //这边的路径指的是端口后面的部分
        if(req.url.startsWith('/index')){
            res.end('index')
            //write向客户端响应内容,可以写多次
            //end 方法用来完成响应,只能写一次
            res.write('hello');
            res.write('world');
        }else if(req.url.startWith('/about')){
            res.end('about')
        }
    }).listen(3000,'192.168.0.106', () => {
            console.log('it's running')
       })
```

## 11.3 处理静态资源脚本封装
```js   
    exports.staticServer = (req, res, root) =>{
        fs.readFile(path.join(root,req.url), (err, fileContent) => {
            if(err){
                res.writeHead(404, {
                    'Content-Type': 'text/plain; charset=utf8'
                })
                res.end('没有找到页面')
            }
            else{
                //声明dtype
                let dtype = 'text/plain'
                //判断文件后缀名
                let ext = path.extname(req.url)
                //查询mime 
                if(mime[ext]){
                    dtype = mime[ext]
                }
                
                //如果是text开头的 ，那么完整Content-Type 设置
                if(dtype.startsWith('text')){
                    dtype += '; charset=utf8'
                }

                //设置响应头
                res.writeHead(200, {
                    'Content-Type': dtype
                })  
                res.end(fileContent)
            }
        })
    }
```

## 11.4 get参数处理
- 声明url对象:`const url = require('url')`
- 将对象转为路径字符串: `url.format(xxx)`
- 将url参数转为对象: `url.parse(xxx,true)` --->加`true`转为对象,不加的话`query`的值是一个字符串,不利于操作 
- 通过转化后的对象`ret.query.key`能获取到指定参数值
```js
    const url = require('url')
    const str = 'http://www.baidu.com/abc/qqq?flag=123&keyword=java'
    let ret = url.parse(str,true) //将路径转换为对象
    console.log(ret.query.flag) //获取参数值
    
```

## 11.5 post 参数处理
- 声明`querystring`对象:`const querystring = require('querystring'`)
- 字符串转为对象: `querystring.parse(param)`
- 对象转为字符串: `querystring.stringify(param)`
```js   
    const querystring = require('querystring')

    //parse方法的作用就是把字符串转成对象
    let param = 'username=list&password=123'
    //如果参数中有键名一样的数值 会处理到数组中password:['123',456]
    let obj = querystring.parse(param)
    console.log(obj.username)
```
### 启动服务器模拟post

- `req.on('data',(chunk)){pdata+=chunk}`:逐块加载资源块
- `req.on('end')`:结束加载事件
```js
const querystring = require('querystring')
const http = require('http')

http.createServer((req, res) => {
    if(req.url.startsWith('/login')){
        let pdata = '';
        req.on('data', (chunk) => {
            pdata+=chunk
        })

        req.on('end', () => {
            res.end(querystring.parse(pdata).username)
        })
    }
}).listen(3000, () => {
    console.log('running...')
})
```

## 11.6 一个登陆模型
```js
const http = require('http')
const method = require('./staticServer.js')
const url = require('url')
const querystring = require('querystring')
http.createServer((req, res) => {
    //静态资源
    if(req.url.startsWith('/www')){
        method.staticServer(req, res, __dirname)
    }
    //动态资源
    //如果是get请求
    if(req.url.startsWith('/login')){
        if(req.method == 'GET'){
            let param = url.parse(req.url,true).query
            if (param.username == 'admin' && param.password == 12345){
                res.end('login success')
            }else {
                res.end('login failure')
            }
        }else if (req.method == 'POST'){
            let pdata = ''
            //基于事件处理数据
            req.on('data', (chunk) => {
                // 用'+'号拼接自动将chunk 转为字符串
                pdata += chunk
            })
            req.on('end', () => {
                let param = querystring.parse(pdata)
                if(param.username == 'admin' && param.password == 12345){
                    res.end('login success')
                }else{
                    res.end('login failure')
                }
            })
        }
    }

}).listen(3000, () => {
    console.log('runing...running')
})
```


# 十二 动态网站开发

## 12.1 初步实现 
- 功能:成绩查询功能
- 注意点:
    + 1.**在form表单中的method 和 req.url.method中的请求方式都要**
    + 2.form表单中的action中填写地址必须写协议名称"http://"之类
```js
const path = require('path')
const http = require('http')
const fs = require('fs')
const querystring = require('querystring')
// 引入配置好的数据json文件
const scoreData = require('./view/score.json')

http.createServer((req, res) => {
    if (req.url.startsWith('/query') && req.method == "GET") {
        fs.readFile(path.join(__dirname, 'view','index.tpl'), 'utf8', (err, content) => {
            if(err) {
                res.writeHead(500, {
                    'Content-Type': 'text/plain; charset=utf8'
                })
                res.end('服务器错误,请联系管理员')
            }
            res.end(content)
        })
    }else if(req.url.startsWith('/score') && req.method == "POST") {    
            //请求名称"POST", "GET"必须大写 form 表单中也是
            let pdata = ''
            req.on('data', (chunk) => {
                pdata += chunk;
            })
            req.on('end', () => {   
            console.log(123)
                // 对pdata 进行处理
                let obj = querystring.parse(pdata)
                //通过上传的参数去数据中匹配
                let result = scoreData[obj.code]
                fs.readFile(path.join(__dirname, 'view', 'result.tpl'), 'utf8', (err, content) => {
                    if(err) {
                        res.writeHead(500, {
                            'Content': 'text/plain; charset=utf8'
                        })
                        res.end('服务器错误,请联系管理员')
                    }
                    /*将原本内容中的模板字段替换成匹配的数据*/
                    content = content.replace('%chinese%', result.chinese)
                    content = content.replace('%math%', result.math)
                    content = content.replace('%english%', result.english)
                    content = content.replace('%total%', result.total)
                    res.end(content)
                })
            })
    }
}).listen(3000, () => {
    console.log('running')
})
```

## 12.2 配合art-template

### 12.2.1 模板引擎渲染方式
<h4>第一种</h4>
> 注意点:<br>
> 1.dirname拼接的时候注意加/ 如下 `__dirname+ '/mytpl.art'`<br>
> 2.格式 template(路径,数据) ---> 数据必须是对象格式
```js   
let template = require('art-template')
let html = template(__dirname + '/mytpl.art', {
    user: {
        name: 'lulinglong'
    }
})
console.log(html)
```

<h4>第二种</h4>
>步骤:<br>
1.指定模板<br>
2.将模板编译 `template.compile(tpl)`<br>
3.往render中传入数据 `render({})` ---> 必须是对象形式的<br>


```js   
let  tpl = '<ul>{{each list as value}}<li>{{value}}</li>{{/each}}</ul>'
let  render = template.compile(tpl)
let  result = render({
    list:['lulinglong', 'wangjie', 'dingding']
})

``` 
<h4>第三种</h4>
>步骤:<br>
1.直接在template中写各个数据<br>
2.在模板中直接写数据名


```js
let html = template(__dirname + '/mytpl.art', {
    chinese: '123',
    english: '124',
    math: '125',
    total: '1111'
})
```

# 十三 express使用
## 13.1 创建服务器功能
```js
    //创建express 对象
    const express = require('express')
    const app = express()

    //或者
    const app = require('express') ()
    // `/`是根路径
    app.get('/', () => (req, res) => {
        res.send('ok')
    }).listen(3000, () => {
        console.log('running')
    })

    // 另一种创建方式
    let server = app.get('', (req, res) => {
        res.send('ok')
    })

    server.listen(3000, () => {
        console.log('runnning')
    })
```

## 13.2 静态服务器功能
- `app.use(express.static)`可以创建多个目录
```js
    const express = require('express')
    const app = express();
    
    //use方法的第一个参数可以指定一个虚拟的目录
    //app.use('/abc', express.static('public'))
    let server = app.use(express.static('public'))
    server.listen(3000, () => {
        console.log('running')
    })
```
- 也可以同时指定多个虚拟目录 ,**同时创建服务**
```js
        app.use('/abc', express.static('public'))
        app.use('/eee', express.static('hello'))
        app.listen(3000, () => {
            console.log('running')
        })
```
## 13.3 express的路由功能

- 1.0`app.use((req, res) => {res.send('ok')})` 可以处理所有请求
- 2.0`app.all('/abc', (req ,res))` 发送的请求与请求方式无关
    + **use和all的区别**:`all`的话只在**指定路径**请求有用,`use`**全部的路径**都可以有效
- 3.0`app.route('/abc').get().post`  --> 可以**链式编程**
```js
    const express = require('express')
    const app = express();

    app.get('', (req, res) => {
        res.send('get data')
    })

    app.post('', (req, res) => {
        res.send('get data')
    })
    app.put('', (req, res) => {
        res.send('get data')
    })
    app.delete('', (req, res) => {
        res.send('get data')
    })
    

    //或者使用app.use可以处理所有请求
    app.use((req,res) => {
        res.send('ok')
    })

    // all 方法绑定的路由 只和请求路径有关 与请求方式无关
    app.all('/abc', (req, res) => {
        res.end('success')
    })

    //开启服务器
    app.listen(3000,  () => {
        console.log('running')
    })

```
- router方法可以指定特定的请求方式 
    + 只支持`get` 和 `post`
```js
    app.route('/hello')
        .get((req, res) => {
            res.send('get data')
         }).post((req, res) => {
            res.send('get data')
         })
```
- 4.0使用Router类文件指定特定的请求方式 
    + 创建一个router类 **并导出**
    + 创建一个主文件用于执行 , 需要导入该路由对象
```js
const express  = require('express')
const router = express.Router();

route.get('/hi', (req, res) => {
    res.send('hi router')
})

route.get('/hello', (req, res) => {
    res.send('hello router')
})

route.post('/dog', (req, res) => {
    res.send('dog router')
})

module.exports = router
```
```js
    //router类
    let birds = require('./birds')
    //指定虚拟路径
    app.use('/admin',birds)

    app.listen(3000, () => {
        console.log('running')
    })
```

## 13.4 中间件
- 本质上就是一个函数
- `next()`传递到下一个函数
- 一旦`res.send()`或者是`res.end()`的话就不会向下执行了


### 13.4.1 应用级中间件

- 传递的路径必须与上一个一致
```js   
    const express = require('express')
    const app = express();
    let total = 0
    app.use('/user', (req, res, next) => {
        console.log(Date.now())
        //没有next的话不能向下执行
        //作用:把请求传递到下一个中间件
        next()
    })

    app.use('/user', (req, res, next) => {
        console.log('访问了/user')
        next()
    })

    app.use('/user', (req, res) => {
        total++;
        console.log(total)
        res.send('result')
    })

    app.listen(3000, () => {
        console.log('running')
    })
```
### 13.4.2 中间件的挂载方式
<h4>路由中间件链式挂载</h4>

- `next('route') `: 跳转到下一个路由, 一旦跳转就**不会执行当前路由的剩余部分**
    + 下个路由的路径**必须与上一个一致**
```js
    const express = require('express')
    const app = express();

    app.get('/abc', (req, res, next) => {
        console.log(1)
        next()//必须操作next方式往下传递
        //next('route')//跳转到下一个路由的话 以下的方法就不执行了
    }, (req, res) => {
        console.log(2)
        res.send('abc')
        })

    app.get('/abc', (req, res) => {
        console.log('怎么说')
        res.send('success failure')
    })
    
    app.listen(3000, () => {
        console.log('running')
    })
```

<h4>使用回调函数数组处理路由</h4>
```js
    let cb0 = (req, res, next) => {
        console.log(1)
        next()
    }
    let cb1 = (req, res, next) => {
        console.log(2)
        next()
    }    
    let cb2 = (req, res) => {
        res.send('已经成功了')
    }

    app.get('/admin', [cb0, cb1, cb2])
    app.listen(3000, () => {
        console.log('running')
    })
```

### 13.4.2 实际使用
- 1.导入第三方件: `body-parser'`
- 2.处理参数:(两个最好都加上)
    + 处理表单出来的数据: `app.use(bodyParser.uelencoded({ extended: false }))`
    + 处理ajax传来的数据: `app.use(bodyParser.json())`
- post请求:依赖第三方件`body-parser`
    + 获取数据：`req.body`
- get请求: 不依赖第三方件 
    + 获取数据: `req.qurey`

```js
    const express = require('express')
    const app = express();
    const bodyParser = require('body-parser')

    //配置静态资源服务器 配置过后可以访问文件夹下所有内容.不需要在url中带上/public
    app.use(express.static('public'))

    //处理ajax传来的数据
    app.use(bodyParser.json())
    //挂载参数处理中间件 处理表单传来的数据  只支持post
    app.use(bodyParser.uelencoded({ extended: false }))
    
    //get参数处理
    app.get('/login', (req, res) => {
        let data = req.query;
        console.log(data)
        res.send('get data')
    })

    //post参数的处理 需要第三方的包
    app.post('/login', (req, res) => {
        let data = req.body
        console.log(data)
        res.send('成功')
    })
    app.listen(3000, () => {
        console.log('在跑了')
    })
```

## 13.5 express 配合 art-template
- 设置模板文件的路径
    + `app.set('views',路径)`
- 设置模板引擎
    + `app.set('view engine', 'art')`
    + `art`为文件的后缀名
- 设置无法匹配后缀名情况下的兼容模式
    + `app.engine('art', require('express-art-template'))`
```javascript
    const express = require('express')
    const path = require('path')
    const template = require('art-template')
    const app = express()
    //设置模板文件的路径
    app.set('views', path.join(__dirname,'view'));
    //设置模板引擎
    app.set('view engine', 'art') //art为后缀名
    
    //找到后缀名并解析,与app.set中设置的后缀名一致
    app.engine('art', require('express-art-template'))

    app.get('/list', (req, res) => {
        let data = {
            title: '水果',
            list: ['apple', 'orange']
        }
        //express提供
        //参数一:模板名称
        //参数二:渲染模板的数据
        res.render('list', data)
    })
    app.listen(3000, () => {
        console.log('runnings')
    })
```

## 13.6 图书管理系统
### 13.6.1 图书增删改查操作
<h4>编辑图书</h4>

```js
    exports.toEditBook = (req, res) => {
        let id = req.query.id  //get获取数据
        let book = null;
        data.forEach((item) => {
            //匹配数据中的唯一id的图书信息
            if(id  == item.id){
                book = item
                return; //forEach中没有break
            }
        })
        res.render(editBook,book)
    }
```

<h4>修改图书提交表单</h4>

- 需要添加一个隐藏域存储id
```js
    exports.editBook = (req, res) => {
        let info = req.body;
        data.forEach((item) => {
            if(item.id == info.id) {
                for( key in info ) {
                    item[key] = info[key]
                }
            }
        })
    } 
```

<h4>删除图书信息</h4>

```js
    exports.deleteBook = (req, res) => {
        let id = req.query.id
        data.forEach((item, index) => {
            if(item.id == id){
                data.splice(index, 1)
            }
            return;
        })
    }
```

### 13.6.2 数据库学习
#### 初始化数据库
- 1.插入数据
```js
    insert into book(name, author, category, description) value (a, b, c, d)
```

- 2.把data.json文件中的数据拼接成insert语句
```js   
    cosnt path = require('path')
    const fs = require('fs')

    fs.readFile(path.join(__dirname, '../', 'data.json'), 'utf8', (err, content) => {
        if(err) return
        let list = JSON.parse(content)
        let arr = []
        list.forEach((item) => {
            let sq; = `insert into book (name, author, category,)`
        })
    })  
```

#### 数据库基本步骤
- 1.初始化一个package.json文件,下载`npm install mysql`
-  以下为插入数据
```js
    //加载数据库驱动
    var mysql = require('mysql');

    //创建数据库链接
    var connection = mysql.createConnection({
        host: 'localhost',  //数据库所在服务器的域名或者IP地址
        user: 'root',      //登录数据库的账号
        password: '',      //登录数据库的密码
        database: 'lll'    //数据库名称 
    })
    
    执行连接操作
    connection connect();
    
    //插入操作
    let sql = 'insert into initBOOK set ?'
    let data = {
        name: '明朝那些事',
        author: '当年明月',
        category: '文学',
        description: '明朝历史'
    }
    connection.query(sql, data, function(err, results, fields) {
        if(err) throw err;
        console.log('共有',results[0].total+'条数据库')
        if(results.affectedRows ==1) {
            console.log('插入成功')
        }
    })
    connection.end()
```
- 2.插入数据
```js
    var mysql = require('mysql');

    //创建数据库链接
    var connection = mysql.createConnection({
        host: 'localhost',  //数据库所在服务器的域名或者IP地址
        user: 'root',      //登录数据库的账号
        password: '',      //登录数据库的密码
        database: 'lll'    //数据库名称 
    })
    
    执行连接操作
    connection connect();
    
    //插入操作
    let sql = 'update initBOOK set name=?,author=?,category=?,description=? where id=?';
    let data = ['浪潮之巅','无菌','计算机','it巨头兴衰']
    
    connection.query(sql, data, function(err, results, fields) {
        if(err) throw err;
        console.log('共有',results[0].total+'条数据库')
        if(results.affectedRows == 1) {
            console.log('更新成功')
        }
    })
    connection.end()
```

- 3.删除数据
```js
    //其他跟上面一样
    let sql = 'delete from initBOOK where id = ?';
    let data = [9]; //需要用数组包住
```

- 4.查询数据
```js
    let sql = 'select * from initBOOK'
    //如果需要特定的值 可以传入值
    //let sql = 'select * from initBOOK where id = ?'
    let data = null;
    connection.query(sql, data, function(err, results, fields){
        if(err) throw err;
        console.log(results[1].name)
    })
```
#### 测试通用api
```js

//封装操作数据库的通用api

const mysql = require('mysql');

exports.base = (sql,data,callback) => {
    // 创建数据库连接
    const connection = mysql.createConnection({
        host: 'localhost', // 数据库所在的服务器的域名或者IP地址
        user: 'root', // 登录数据库的账号
        password: '', // 登录数据库的密码
        database: 'book' // 数据库名称
    });
    // 执行连接操作
    connection.connect();

    // 操作数据库(数据库操作也是异步的)
    connection.query(sql,data, function(error, results, fields) {
        if (error) throw error;
        callback(results);
    });
    // 关闭数据库
    connection.end();
}
```

### 13.6.3 登录验证

```js
    const express = require('express')
    const bodyParser = require('body-parser')
    const app = express()

    app.usr(bodyParser.urlencoded({ extended: false }))
    app.use(express.static('public'))

    app.post('/check',(req, res) => {
        let param = req.body;

        let sql = 'select count(*) as total from user'

        db.base(sql, data, (result) => {
            if(result[0].total ==1) {
                res.send('login success')
            }else {
                res.send('login failure')
            }
        })
    })
    app.listen(3000, () => {
        console.log('running')
    })
```

## 14 接口开发以及Restful api
### 14.1 json接口
```js
    cosnt express = require('express')
    const db = require('./db.js')
    const app = express();
    
    //指定api路径路径
    app.get('/allBooks', (req, res) => {
        let sql = 'select * from initBOOK'
        db.base(sql, null, (result) => {
            res.json(result)
        })
    })

    app.listen(3000, () => {
        console.log('running')
    })
```
### 14.2 jsonp接口
```js
    cosnt express = require('express')
    const db = require('./db.js')
    const app = express();
    
    //修改callback函数名称
    app.set('jsonp callback name', 'cb')
    //指定api路径路径
    app.get('/allBooks', (req, res) => {
        let sql = 'select * from initBOOK'
        db.base(sql, null, (result) => {
            res.jsonp(result)
        })
    })

    app.listen(3000, () => {
        console.log('running')
    })
```

### 14.3 restful api
> restful api 是从url的格式来表达的

- 类似如下的url
- 一个`url`对应一个资源
- 参数的传递直接用`/`
```html
    get http://localhost:3000/books
    get http://localhost:3000/books/book

    //提交表单
    post http://localhost:3000/books/book

    //编辑 1 为id值  原本为?id=1
    get http://localhost:3000/books/book/1
    
    //添加
    put http://localhost:3000/books/book
    
    //删除
     
    delete http://localhost:3000/books/book/1
```

```js
    const express = require('express')
    const app = express();
    const db = require('./db.js')
    //不传参
    //app.get('/books', (req, res) => {
    //    let sql = 'select * from book'
    //    db.base(sql, null, (result) => {
    //        res.json(result)
    //    })
    //})
    
    app.get('/books/book/:id' ,(req, res) => {
        let id = req.params.id;
        let sql = 'select * from book'
        let data = [id]
        db.base(spl, data, (result) => {
            if(err) throw err
            res.json(result[0])
        })
    })

    app.listen(3000, () => {
        console.log('running')
    })
```