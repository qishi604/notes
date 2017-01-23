# Component 生命周期

[官方文档](https://facebook.github.io/react/docs/react-component.html)

## Mounting

这些方法会在`component`创建并且插入到DOM时调用

* `constructor()`
* `componentWillMount`
* `render()`
* `componentDidMount()`

## Updating

`props` 或者 `state` 的更新都可以触发更新。这些方法会在`component`重新渲染的时候调用

* `componentWillReceiveProps()`
* `shouldComponentUpdate()`
* `componentWillUpdate()`
* `render()`
* `componentDidUpdate()`

## Unmonting

这个方法会在`component`将要被从DOM中移除时调用

* `componentWillUnmount()`

## 其他APIs

* `setState()`
* `forceUpdate()`

## Class Properties

* defaultProps
* displayName
* propTypes

## Instance Properties

* props
* state
