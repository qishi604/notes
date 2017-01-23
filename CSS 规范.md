# CSS 规范

## 声明顺序

相关的属性应当归为一组，并按照下面的顺序排列：

1. Positioning
2. Box model
3. Typographic
4. Visual

``` css
.declaration-order {
  /* Positioning */
  position: absolute;
  top: 0;
  right: 0;
  z-index: 1000;
  
  /* Box model */
  display: block;
  float: left;
  width: 100px;
  height: 100px;
  
  /* Typography */
  font: normal 13px "Helvetica Neue", sans-serif;
  line-height: 1.5;
  color: #333;
  text-align: center;
  
  /* Visual */
  background-color: #f44444;
  border: 1px solid #444;
  border-radius: 3px;
  
  /* Misc */
  opacity: 1;
}
``` 

## 简写

### Background 

```
background-color: #000;
background-image: url(images/bg.gif);
background-repeat: no-repeat;
background-position: top right;
```

简写成：

```
background: #000 url(images/bg.gif) no-repeat top right;
```

### Font 

```
font-style: italic;
font-weight: bold;
font-size: .8em;
line-height: 1.2;
font-family: Arial, sans-serif;
```

简写成：

```
font: italic bold .8em/1.2 Arial, sans-serif;
```

### Border

```
border-width: 1px;
border-style: solid;
border-color: #000;
```

简写成：

```
border: 1px solid #000;
```

### Margin and Padding

```
margin-top: 10px;
margin-right: 5px;
margin-bottom: 10px;
margin-left: 5px;
```

简写成：

```
margin: 10px 5px 10px 5px;
```
