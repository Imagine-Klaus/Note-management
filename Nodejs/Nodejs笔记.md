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