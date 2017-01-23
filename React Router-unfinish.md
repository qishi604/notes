# React Router 

from [React Router 使用教程](http://www.ruanyifeng.com/blog/2016/05/react_router.html)

[github](https://github.com/ReactTraining/react-router)

## 1. 基本用法

安装命令： 

`$ npm install -S react-router`

使用时，路由器`Router`就是`React`的一个组件。

```
import { Router } from 'react-router';
render(<Router/>, document.getElementById('app'));
```

`Router`组件本身只是一个容器，真正的路由要通过`Route`组件定义。

```
import { Router, Route, hashHistory } from 'react-router';

render((
  <Router history={hashHistory}>
    <Route path="/" component={App}/>
  </Router>
), document.getElementById('app'));
```

上面代码中，用户访问根路由`/`（比如`http://www.example.com/`），组件`APP`就会加载到`document.getElementById('app')`。
你可能还注意到，`Router`组件有一个参数`history`，它的值hashHistory表示，路由的切换由URL的hash变化决定，即URL的`#`部分发生变化。举例来说，用户访问`http://www.example.com/`，实际会看到的是`http://www.example.com/#/`。
`Route`组件定义了URL路径与组件的对应关系。你可以同时使用多个`Route`组件。

```
<Router history={hashHistory}>
  <Route path="/" component={App}/>
  <Route path="/repos" component={Repos}/>
  <Route path="/about" component={About}/>
</Router>
```

上面代码中，用户访问`/repos`（比如`http://localhost:8080/#/repos`）时，加载`Repos`组件；访问`/about`（`http://localhost:8080/#/about`）时，加载`About`组件。

## 2. 嵌套路由

`Route`组件还可以嵌套。

```
<Router history={hashHistory}>
  <Route path="/" component={App}>
    <Route path="/repos" component={Repos}/>
    <Route path="/about" component={About}/>
  </Route>
</Router>
```

上面代码中，用户访问`/repos`时，会先加载`App`组件，然后在它的内部再加载`Repos`组件。

## 通配符

`path` 属性可以使用通配符。

```
<Route path="/hello/:name">
// 匹配 /hello/michael
// 匹配 /hello/ryan

<Route path="/hello(/:name)">
// 匹配 /hello
// 匹配 /hello/michael
// 匹配 /hello/ryan

<Route path="/files/*.*">
// 匹配 /files/hello.jpg
// 匹配 /files/hello.html

<Route path="/files/*">
// 匹配 /files/ 
// 匹配 /files/a
// 匹配 /files/a/b

<Route path="/**/*.jpg">
// 匹配 /files/hello.jpg
// 匹配 /files/path/to/file.jpg
```

通配符的规则如下。

```
1）:paramName
:paramName匹配URL的一个部分，直到遇到下一个/、?、#为止。这个路径参数可以通过this.props.params.paramName取出。
（2）()
()表示URL的这个部分是可选的。
（3）*
*匹配任意字符，直到模式里面的下一个字符为止。匹配方式是非贪婪模式。
（4） **
** 匹配任意字符，直到下一个/、?、#为止。匹配方式是贪婪模式。
```

`path`属性也可以使用相对路径（不以/开头），匹配时就会相对于父组件的路径，可以参考上一节的例子。嵌套路由如果想摆脱这个规则，可以使用绝对路由。
路由匹配规则是从上到下执行，一旦发现匹配，就不再其余的规则了。

```
<Route path="/comments" ... />
<Route path="/comments" ... />
```

上面代码中，路径`/comments`同时匹配两个规则，第二个规则不会生效。
设置路径参数时，需要特别小心这一点。

```
<Router>
  <Route path="/:userName/:id" component={UserPage}/>
  <Route path="/about/me" component={About}/>
</Router>
```

上面代码中，用户访问`/about/me`时，不会触发第二个路由规则，因为它会匹配`/:userName/:id`这个规则。因此，带参数的路径一般要写在路由规则的底部。
此外，URL的查询字符串`/foo?bar=baz`，可以用`this.props.location.query.bar`获取。