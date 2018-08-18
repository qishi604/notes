# Smali

## 关键字

- `.field private isFlag:z` 	定义变量
- `.method` 	函数
- `.parameter` 	方法参数
- `.prologue` 		方法开始
- `.line 12`		此方法位于第12行
- `invoke-super`		调用父函数
- `const/high16 v0, 0x7f03` 		把0x7f03赋值给v0
- `invoke-direct`		调用函数
- `return-void`		函数返回void
- `.end method`		函数结束标志
- `new-instance`		创建实例
- `input-object`		对象赋值
- `iget-object`		调用对象
- `invoke-static`		调用静态函数

## 数据类型

### 基本数据类型

- `B` 		byte
- `C	`	char
- `D	`	double (64bits)
- `F	`	float
- `I` 		int
- `J`		long (64 bits)
- `S	`	short
- `V`	void
- `Z	`	boolean

### 对象类型

- `Lxxx/yyy/zzz;`		object

`L` 表示这是一个对象

`xxx/yyy` 是该对象所在的包

`zzz` 是对象名称

`;` 标识对象名称结束

### 数组类型

- `[XXX` 		array

`[I` 表示一个`int`型的一维数组，相当于`int[]`
增加一个维度增加一个`[`, 如`[[I`表示`int[][]`
数组每一个维度最多255个
对象数组表示也是类似，如String数组表示为`[Ljava/lang/String`

## smali example

```
# class name, also determines file path when dumped
# 类名
.class public Lcom/packageName/example;
 
# inherits from Object (could be activity, view, etc.)
# note class structure is L<class path="">;
# 基类
.super Ljava/lang/Object;
 
# original java file name
# 原始java文件名
.source "example.java"
 
 
# these are class instance variables
# 类属性
.field private someString:Ljava/lang/String;
 
# finals are not actually used directly, because references
# to them are replaced by the value itself
# primitive cheat sheet:
# V - void, B - byte, S - short, C - char, I - int
# J - long (uses two registers), F - float, D - double
.field public final someInt:I  # the :I means integer
.field public final someBool:Z # the :Z means boolean
 
# Do you see how to make arrays?
# char[]
.field public final someCharArray:[C
# String[]
.field private someStringArray:[Ljava/lang/String;
 
 
# this is the <init> of the constructor
# it calls the <init> of it's super, which in this case
# is Ljava/lang/Object; as you can see at the top
# the parameter list reads: ZLjava/lang/String;I
# Z - boolean
# Ljava/lang/String; - java String object
#   (semi-colon after non-primitive data types)
# I - integer
#   (no semi-colon)
# also notice this constructor returns V, which means void
# 构造函数，返回类型为void
.method public constructor <init>(ZLjava/lang/String;I)V
 # declare how many variable spaces we will need
 # we can have: v0, v1, v2, v3, v4 and v5 as variables.
 # smali/baksmali by default uses .registers
 # but you can change this by using --use-locals
 # apktool uses --use-locals and --sequential-labels
 .locals 6
 
 # these are not always present and are usuaully taken
 # out by optimization/obfuscation but they tell us
 # the names of Z, Ljava/lang/String; and I before
 # when it was in Java
 .parameter "someBool"
 .parameter "someInt"
 .parameter "exampleString"
  
 # the .prologue and .line directives can be mostly ignored
 # sometimes line numbers are useful for debugging errors
 .prologue
 .line 10
  
 # p0 means parameter 0
 # p0, in this case, is like "this" from a java class.
 # we are calling the constructor of our mother class.
 # what would p1 be?
 invoke-direct {p0}, Ljava/lang/Object;-><init>()V
  
 # store string in v0
 const-string v0, "i will not fear. fear is the mind-killer."
  
 # store 0xF hex value in v0 (or 15 in base 10)
 # this destroys previous value string in v0
 # variables do not have types they are just registers
 # for storing any type of value.
 # hexadecimal is base 15 is used in all machine languages
 # you normally use base 10
 # read up on it:
 # http://en.wikipedia.org/wiki/Hexadecimal
 const/4 v0, 0xF
  
 # create instance of StringBuilder in v1
 new-instance v1, Ljava/lang/StringBuilder;
  
 # initialize StringBuilder with v2
 # notice it returns V, or void
 const-string v2, "the spice must flow"
 invoke-direct {v1, v2}, Ljava/lang/StringBuilder;-><init>(Ljava/lang/String;)V
  
 # append p1, which is our first paramater and is boolean
 # therefore we use append(Z)
 # notice how append returns a StringBuilder
 invoke-virtual {v1, p1}, Ljava/lang/StringBuilder;->append(Z)Ljava/lang/StringBuilder;
  
 # use move-result-object to store previous result in v1
 move-result-object v1
 # non-objects use move-result or move-result-wide
 
 # append v2 to our StringBuilder
 # notice how this append takes a string and not Z
 const-string v2, "some random string"
 invoke-virtual {v1, v2}, Ljava/lang/StringBuilder;->append(Ljava/lang/String;)Ljava/lang/StringBuilder;
 move-result-object v1
   
 # call toString() on our StringBuilder
 # if you use Java you know that most objects have toString()
 invoke-virtual {v1}, Ljava/lang/StringBuilder;->toString()Ljava/lang/String;
 move-result-object v1
  
 # send our new string to the log.
 # this can be used to debug and can be picked up with ddms, logcat
 # or log collector. as an exercise look up what the d() function does
 # in the android developer documentation.
 const-string v0, "Tag"
 invoke-static {v0, v1}, Landroid/util/Log;->d(Ljava/lang/String;Ljava/lang/String;)I
 move-result v0
  
 # get the current time in milliseconds
 # J denotes a float or wide return value
 invoke-static {}, Ljava/lang/System;->currentTimeMillis()J
 move-result-wide v2
 # note!! since it is a wide value, it takes up v2 AND v3
 
 # so we must use v4 next
 # try to reuse variables if possible.
 const-wide/16 v4, 0x300 # this takes up v4 and v5
 div-long/2addr v2, v4   # divide v2 by v4
 long-to-int v2, v2      # convert v2 to an integer
  
 # since i wrote this in my head, there was no Java
 # compiler to add the .line's in the right places
 # but normally they would relate to actual Java lines
 # these are often removed with proguard optimization
 .line 12
  
 # store p1 as an instance variable (someBool) for this class
 # in java this may look like this.someBool = p1;
 iput-boolean p1, p0, Lcom/packageName/example;->someBool:Z
  
 .line 14
  
 # do the same for p3 and someInt
 iput p3, p0, Lcom/packageName/example;->someInt:I
  
  
 # get the value from p0.someInt
 iget v0, p0, Lcom/packageName/example;->someInt:I
  
 # now we will invoke a static method.
 # {} means empty parameters then the full package name followed by ->
 # then the method and it's return value. everything must be there.
 invoke-static {}, Lcom/packageName/example;->someMethod()Ljava/lang/String;
  
 # for different types of invoke-*, try this:
 # http://www.netmite.com/android/mydroid/dalvik/docs/dalvik-bytecode.html
 # invoke-virtual and direct take the class instance as a first parameter.
  
 .line 16
 return-void # meditate on the void.
.end method
 
# try and figure this one out
.method public static someMethod()Ljava/lang/String;
 # could i have used fewer variables?
 .locals 4
  
 new-instance v0, Ljava/lang/Long;
  
 invoke-static {}, Ljava/lang/System;->currentTimeMillis()J
 move-result-wide v1
  
 invoke-direct {v0, v1, v2}, Ljava/lang/Long;-><init>(J)V
  
 invoke-static {v0}, Ljava/lang/String;->valueOf(Ljava/lang/Object;)Ljava/lang/String;
 move-result-object v1
  
 # notice use of return-object and not just return
 return-object v1
.end method</class>
```



## Reference

[smali语法宝典](http://www.jianshu.com/p/ba9b374346dd)