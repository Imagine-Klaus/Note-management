[TOC]
## Canvas学习笔记
###  1.表格折线图插件
- **echarts** 

### 2. 虚线

> 1.0 ctx.setLineDash( 数组 )<br>
> 2.0 ctx.getLineDash()<br>
> 3.0 ctx.lineDashOffset = 值<br>

**注:**数组中存储的数字就是分别表示 实线部分与空白部分的长度 [ 10 ]


### 3. 绘制形状

    -> 矩形
        ctx.rect( x, y, width, heigth )     描边, 需要 stroke 或 fill
        ctx.strokeRect( x, y, w, h )
        ctx.fillRect( x, y, w, h )
        ctx.clearRect( x, y, w, h )         清除该矩形区域的内容

    -> 清除整个画布
        ctx.clearRect( 0, 0, cas.width, cas.height );

        cas.width = cas.width;

    -> 圆弧
        ctx.arc( x, y, r, startAngle, endAngle, clockwise )
        ctx.arcTo() 了解

### 4. 弧度制

- 转换为弧度
        
        function toRadian(angle){
              return  angle*Math.PI/180;
        }

- 转换为角度

        function toAngle(radian){
               return radian*180/Math.PI
        }        


### 9. 绘制文字
    ctx.fillText( 文本内容, x, y )
    ctx.strokeText( 文本内容, x, y );

    常用的属性
    ctx.font = '30px 黑体'

    ctx.textAlign       left, center, right.    start, end
    ctx.textBaseline    top, middle, bottom.    hanging, ideographics, alphabetic



