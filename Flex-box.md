# Flex-box

[flex-box](http://www.w3.org/TR/css-flexbox-1/#overview)

[android-flex-box](https://github.com/google/flexbox-layout)

## 容器属性
### flex-direction 定义主轴方向

* row (默认值) 左-右
* row-reverse 右-左
* column 上-下
* column-reverse 下-上

### flex-wrap 如何换行

* nowrap (默认值) 不换行
* wrap 换行，上-下
* wrap-reverse 换行，下-上

### flex-flow

`flex-direction` `flex-wrap` 简写

默认值为：`row nowrap`

### justify-content 定义主轴对齐方式（水平）

* flex-start (default, 左对齐)
* flex-end (右对齐)
* center (居中)
* space-between (两端对齐)
* space-around (项目两侧间隔相等，项目之间的间隔会比项目与边框的间隔大)


### align-items 定义交叉轴对齐方式 （竖直）

* flex-start 交叉轴起点对齐
* flex-end 交叉轴终点对齐
* center 交叉轴中点对齐
* baseline 项目的第一行文字的基线对齐
* stretch （默认值）如果项目未设置高度或设置为auto，将占满整个容器的高度

### align-content 定义多跟轴线对齐方式

如果项目只有一根轴线，该属性不起作用

* flex-start
* flex-end
* center
* space-between
* space-around
* stretch

## 项目属性

### order 排序，小-大，默认为0

### flex-grow 放大比例，默认为0（不放大）

如果项目的flex-grow都为1，则它们将等分剩余空间

### flex-shrink 定义缩小比例，默认为1

如果项目`flex-shrink`为0，不缩小。

### flex-basis

定义了在分配多余空间之前，项目占据的主轴空间，默认值为auto


### flex
`flex-grow` `flex-shrink` `flex-basis`的简写，默认值为0 1 auto，后两个属性可选

### align-self

允许单个项目有与其他项目不一样的对齐方式，可覆盖`align-items`. 默认值为auto，表示继承父元素的`align-items`属性

