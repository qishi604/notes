# 使用 Chrome 开发者工具进行 JavaScript 问题定位与调试

[from](http://www.ibm.com/developerworks/cn/web/1410_wangcy_chromejs/)

## Chrome 开发者工具介绍

1. Element 标签页： 用于查看和编辑当前页面中的 HTML 和 CSS 元素。
2. Network 标签页：用于查看 HTTP 请求的详细信息，如请求头、响应头及返回内容等。
3. Source 标签页：用于查看和调试当前页面所加载的脚本的源文件。
4. TimeLine 标签页： 用于查看脚本的执行时间、页面元素渲染时间等信息。
5. Profiles 标签页：用于查看 CPU 执行时间与内存占用等信息。
6. Resource 标签页：用于查看当前页面所请求的资源文件，如 HTML，CSS 样式文件等。
7. Audits 标签页：用于优化前端页面，加速网页加载速度等。
8. Console 标签页：用于显示脚本中所输出的调试信息，或运行测试脚本等。

## 开启运行时错误自动暂停功能，准确定位出错脚本位置

Source 标签页 "pause on exception"

## 设置条件断点
与 Java 调试类似，Chrome 开发者工具提供了断点设置、删除与断点存储等基本功能。同时，开发者工具也提供了设置条件断点的功能，使开发者可以控制该断点只有在满足某一条件时才会被触发。

## 修改 JavaScript 文件中的代码
这是 Chrome 开发者工具提供的一种非常实用的功能，即使开发人员可直接对开发者工具的 Source 标签页中的代码进行修改，并将其保存，使浏览器在下次执行该段脚本时，直接加载最新修改的版本。目前的 Firebug 及 IE 自带的开发者工具都不支持对脚本的直接修改，导致在 Firefox 或 IE 中调试脚本时，如果需要对代码进行修改，需要先去修改脚本源文件，再同步至应用服务器，再清理浏览器缓存，最终再次打开应用程序时，才会看到代码修改后的效果。可见 Chrome 开发者工具提供的这一功能，大大提供了开发者调试脚本的效果。

## 使用控制台打印变量值或方法的返回结果
当断点被触发进入到调试模式时，我们可以将当前任意存在的变量或方法输入到控制台中，按下回车后，控制台便会返回相关的结果。该功能可使开发人员方便了解程序运行至断点处时各个所需要变量或方法的返回值。