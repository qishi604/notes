# UI bug

1. 今天出了个uibug，几乎花了一下午才找出来。bug是这样的：

一个自定义View明明设置了属性，加了点击事件、添加子view，结果渲染出来就是空白。由于代码比较多，继承层级也很深，找了半天没找出错误地方，只能一行行代码注释来调试。结果还是没找出来，只能硬着头皮，一行一行代码看。最终发现是由于**setContentView()调用了两次**导致。好吧，线上版本的代码也是如此~

2. 昨晚快提交代码的时候又来了个bug，就是ImageView#setSelected()不起作用。原因是文件冲突修改之后没注意，出现两个重复的布局~