# UC浏览器真乃移动版IE毒瘤

## 1. 在UC浏览器上对flex和transform的支持性不好

- media query 没有覆盖，反而变成了某种奇怪的基准点（？？）

- `@media (max-width: 992px)`  里的 `justify-content: flex-end`
居然影响到了 `media (max-width: 576px)` 里 `justify-content: space-between` 的正确显示。 

> 最后的解决方案是，**写了个media (max-width: 577px) ，大一个像素，
在里面专门对造成影响的元素做一个 justify-content: flex-start 这样的重置变成默认值。**
显示就正常了。

## 2. 省流量模式下不支持border-radius
- 解决方式: **加moz前缀**

## 3. div的class名为phone,被UC的广告屏蔽了
- 换类名

## 4. uc浏览器可以识别空格
- 需要手动过滤

## 5. 关于flex的话
- 不支持`justify-content`:`space-around`
- 不支持`flex-wrap`
