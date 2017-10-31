# 1.新语义标签
## 1.1 语义标签
- `header`
- `nav`
- `section`
- `aside`
- `footer`
- `article`
- `main`
- `progress`
    + `<progress max="100" value="50"><progress>`
- `meter`
    + `<meter max="100" min="0" high="80" low="40" value="0" id="one" optimum='90'  ></meter>`

## 1.2 没有语义的

- `div`

## 1.3 常见的布局
> div 进行布局，往内填充元素
```html
<!-- H5经典布局: -->
	<header>
		ul>li*4>a[href=#]{导航$}  
	</header>  
	<nav></nav>  
	<main></main>  
		<aside></aside> 侧栏 
		<article></article> 
	<footer></footer>
	<section></section> -- 相当于div 
```

## 1.4 html5新语义标签的兼容问题 
- a.导入`html5shiv.js` 框架: 
```html
    <!--[if lt IE 9]>
            <script type="text/javascript" src='js/html5shiv.min.js'></script>
    <![endif]--> 
    // 快捷键: cc:ie6  (不能写成cc:ie8) 
```
- b. 在不支持HTML5新标签的浏览器里，会将这些新的标签解析成行内元素(inline)对待，
```js
    <!--[if lt IE 9]>
        <script type="text/javascript">
            alert("我执行了");
            window.onload = function(){
                var createHeader = document.createElement("header");
                document.body.appendChild(createHeader);
            }
        </script>
<![endif]--> 

//必须设置为block
```
# 2. 表单
## 2.1 新输入类型
- file
- number 
- url 
- email
- search
- date-picker
- password
    + 可以添加最大字长属性 `maxlength="8"`
- range
- tel 
## 2.2 新表单元素

## 2.3 新表单属性01
## 2.4 新表单属性02




