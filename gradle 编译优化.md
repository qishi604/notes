# gradle 编译优化

## module build.gradle 添加

```gradle
android{
...
   tasks.whenTaskAdded { task ->
        if (task.name.contains("lint")
//如果instant run不生效，把clean这行干掉
                ||task.name.equals("clean")
             //如果项目中有用到aidl则不可以舍弃这个任务
                ||task.name.contains("Aidl")
//用不到测试的时候就可以先关闭
                ||task.name.contains("mockableAndroidJar")
                ||task.name.contains("UnitTest")
                ||task.name.contains("AndroidTest")
//用不到NDK和JNI的也关闭掉
                || task.name.contains("Ndk")
                || task.name.contains("Jni")
        ) {
                     task.enabled = false
        }
    }
}
```

## 参考

- [编译时间从33.8秒降到4.5秒我只多做了一件事](http://www.jianshu.com/p/6603bd624539)
- [【译】我每周在构建Gradle时是如何节约出5小时的](http://www.jianshu.com/p/f9b0592383b8)
- [构建神器Gradle](http://jiajixin.cn/2015/08/07/gradle-android/)