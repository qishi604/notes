# 常用css样式

### reset

```css
html,body,h1,h2,h3,h4,h5,h6,div,dl,dt,dd,ul,ol,li,p,blockquote,pre,hr,figure,table,caption,th,td,form,fieldset,legend,input,button,textarea,menu{ margin:0;padding:0}
header,footer,section,article,aside,nav,hgroup,address,figure,figcaption,menu,details{display:block;}
table{border-collapse:collapse;border-spacing:0;}
caption,th{text-align:left;font-weight:normal;}
html,body,fieldset,img,iframe,adbr{border:0;}
i,cite,em,var,address,dfn{font-style:normal;}
[hidefocus],summary{outline:0;}
li{list-style:none;}
h1,h2,h3,h4,h5,h6,small{font-size:100%;}
sup,sub{font-size:83%;}
pre,code,kbd,samp{font-family:inherit;}
q:before,q:after{content:none;}
textarea{overflow:auto;resize:none;}
label,summary{cusor:default;}
a,button{cursor:pointer;}
h1,h2,h3,h4,h5,h6,em,strong,b{font-weight:bold;}
del,ins,u,s,a,a:hover{text-decoration:none;}
body,textarea,input,button,select,keygen,legend{font:12px/1.14 arial,\5b8b\4f53;color:#333;outline:0;}
body{background:#fff;}
a,a:hover{color:#333;}

* {box-sizing: border-box;}

```

### Clearfix

```css

.clearfix, .container, .cf{
	*zoom:1;
}

.clearfix:before, .container:before, .cf:before, .clearfix:after, .container:after, .cf:after{
	display:table;
	content:" ";
	line-height:0;
}

.clearfix:after, .container:after, .cf:after{
	clear:both;
}
```

### 居中

#### 水平垂直居中

```css
/* 父元素position不为static */
.parent {
  position: relative;
}
/*宽高已知*/
.center-vh {
  position: absolute;
  width: 200px;
  height:200px;
  top: 50%;
  left: 50%;
  margin: -100px;
}

/*宽高未知, 这个全能*/
.center-vh {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
}

```

#### 水平居中

```css
/* inline- 元素 */
.center-h {text-align: center;}

/* block 元素, 宽度固定 */
.center-h {margin: 0 auto;}

/* 如果是多行，可以设置为inline-block */


```

#### 垂直居中

```css

/* 单行 inline- 元素, 设置height 和 line-height 相等 */
.center-v {
  height: 40px;
  line-height: 40px;
  white-space: nowrap;
}

/* 多行 inline-元素，可以利用vertical-align: middle; */
.center-table {
  display: table;
  height: 250px;
  background: white;
  width: 240px;
  margin: 20px;
}
.center-table p {
  display: table-cell;
  margin: 0;
  background: black;
  color: white;
  padding: 20px;
  border: 10px solid white;
  vertical-align: middle;
}

/* block 元素参考 水平垂直居中 */

```

### 文字限制

#### 行数显示 
```css 

.single-line {
  display: -webkit-box;
  overflow: hidden;
  word-break: break-all;
  text-overflow: 'ellipsis'!important;
  -webkit-line-clamp: 1; /* 用这个限制行数, 1 是1行，2是2行 */
  -webkit-box-orient: vertical;
}
```

#### 单词换行

```css 
.text-multi {
  /* 根据单词换行 */
  word-wrap: break-word;
  /* 超过父元素长度换行 */
  word-break: break-all;
}
```


### 列表项布局

#### 横向

```css 

.list {
  @import clearfix;
}

.list-item-left: {
  float: left;
}

.list-item-right: {
  float: right;
  
}
```


#### 竖向 

竖向比较简单，就不说了，如果是item可以左右滑动，可以让用float:left;或者display:inline-block;

### 按钮点击效果

```css
.btn {
 height: 48px;
 line-height: 48px;
 border-radius: 24px;
 box-shadow: 0 3px #009405;
 text-align:center;
 color: white;
 background-color: #56c65e;
}

/* 点击效果 */
.btn:active {
  background-color: #009405;
}
```

