# Android ANR

## 产生原因
应用程序响应超时。一般情况有以下两种原因：

-  输入事件（按键或屏幕触摸）在5s 内无响应
- `BroadcastReceiver.onReceive()` 运行在主线程，10s 内无法完成处理
- `Service` 各个生命周期函数在20s 内无法完成处理。这种情况比较少出现

## 典型问题场景

- UI线程存在耗时操作。如在UI线程进行网络请求，数据库操作，文件操作等
- UI线程等待子线程释放某个锁，从而无法处理用户输入
- 耗时的动画需要大量运算，导致CPU负载过重

## 检测 & 定位

### logcat 和 /data/anr/traces.txt

```shell
adb root
adb shell ls /data/anr
adb pull /data/anr/<filename>
```

### 开启StrictMode

### BlockCanary
BlockCanary主要用来监控应用主线程的卡顿。它的基本原理是利用主线程的消息队列处理机制，通过对比消息分发开始和结束的时间点来判断是否超过设定的时间，如果是，则判断为主线程卡顿。

当发生ANR时，可以通过结合Logcat日志和生成的位于手机内部存储的/data/anr/traces.tex文件进行分析和定位。



## 参考

- [ANR产生的原因及定位分析](https://juejin.im/entry/597026806fb9a06bcb7fc660)
- [Android ANR：原理分析及解决办法](https://www.jianshu.com/p/388166988cef)
- [理解Android ANR的触发原理](http://gityuan.com/2016/07/02/android-anr/)
- [Keeping your app responsive](https://developer.android.com/training/articles/perf-anr)
- [ANRs](https://developer.android.com/topic/performance/vitals/anr)