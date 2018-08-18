# python 简单教程

## 参考

- [廖雪峰python教程](http://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/001431675624710bb20e9734ef343bbb4bd64bcd37d4b52000)

- [python 官方文档](https://docs.python.org/3/)

## 数据类型

```python 
a = 1
f = 12.3
```

## 字符串

```python
s = 'python'

# 格式化
'%d - %2d' % (3, 5) 
# >>> 3 - 05

```

## 条件

```
if age > 20:
  print('2000')
elif age > 30: 
  print('3000')
else:
  print('444')
```

## 循环

`for in`

`while`

```
for x in [1, 2, 3, 4]: 
  print(x)
  
while n > 10: 
  print (n)
  n -= 1
```

## 函数定义

东西比较多，详细参考[函数定义](http://www.liaoxuefeng.com/wiki/0014316089557264a6b348958f449949df42a6d3a2e542c000/001431752945034eb82ac80a3e64b9bb4929b16eeed1eb9000)

### 语法：

```
def log(msg): 
  print(msg)
```

### 默认参数

```
def power(x, n=2): 
  sum = 1
  while n > 0:
    n = n - 1
    sum *= x
  return s
  
power(3)
power(3, 3)
```
### 可变参数
可变参数允许你传入0个或任意个参数

```
def calc(*numbers):
  s = 1
  for n in numbers:
    s += n
  return s
```

### 关键字参数
关键字参数允许你传入0个或任意个含参数名的参数，这些关键字参数在函数内部自动组装为一个dict

```
def person(name, age, **kw):
    print('name:', name, 'age:', age, 'other:', kw)

>>> person('Michael', 30)
name: Michael age: 30 other: {}

>>> person('Bob', 35, city='Beijing')
name: Bob age: 35 other: {'city': 'Beijing'}
>>> person('Adam', 45, gender='M', job='Engineer')
name: Adam age: 45 other: {'gender': 'M', 'job': 'Engineer'}
```

如果要限制关键字参数的名字，就可以用命名关键字参数，例如，只接收city和job作为关键字参数。这种方式定义的函数如下：

```
def person(name, age, *, city, job):
    print(name, age, city, job)
```

## 定义类

和普通的函数相比，在类中定义的函数只有一点不同，就是第一个参数永远是实例变量`self`，并且，调用时，不用传递该参数。除此之外，类的方法和普通函数没有什么区别，所以，你仍然可以用默认参数、可变参数、关键字参数和命名关键字参数。

```
class Music(objcect):
  pass
```

```
class Music(objcect):
  def __init__(self, id, source):
    self.id = id
    self.source = source
  
  def toString(self):
    print('id %s, source %s', % (self.id, self.source)
```



