# Web 开发技巧

* Height:100%

想让`height:100%;`生效，可以设置

```css
.match-parent {
  display:absolute;
  top:0;
  bottom:0;
  overflow:auto;
  height:100%;
}
```

* 英文换行

```css
.text-multi {
  word-wrap: break-word;
  word-break: break-all;
}
```

* ios native 滚动效果

```css
.ios-scroll {
  overflow: auto;
  -webkit-overflow-scrolling: touch;
}

```

* 移动端禁止选中内容

```css
.disable-select {
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
}
```

* 移动端取消touch高亮效果

```css
html {
  -webkit-tap-highlight-color: rgba(0,0,0,0);
}
```

* Ios 禁止保存或拷贝图像

```css
img {
  -webkit-touch-callout: none;
}

```

* input placeholder color

```css
::-webkit-input-placeholder { /* WebKit, Blink, Edge */
    color:    #909;
}
:-moz-placeholder { /* Mozilla Firefox 4 to 18 */
   color:    #909;
   opacity:  1;
}
::-moz-placeholder { /* Mozilla Firefox 19+ */
   color:    #909;
   opacity:  1;
}
:-ms-input-placeholder { /* Internet Explorer 10-11 */
   color:    #909;
}
```


## 参考

- [移动端web开发技巧](http://ljinkai.github.io/2015/06/06/mobile-web-skill/)
- [learn layout](http://zh.learnlayout.com/)
- [MDN CSS](https://developer.mozilla.org/zh-CN/docs/Web/CSS/)