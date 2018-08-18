# App 启动过程

1. 通过 `Binder` 进程间通信进入到 `ActivityMangerService` 进程，调用 `ActivityManagerService.startActivity`

2. `ActivityManagerService` 调用 `ActivityStack.startActivityMayWait` 来做准备要启动的 Activity 的相关信息
3. ActivityStack 通知 ApplicationThread 要进行 Activity 启动调度
4. AMS 调用 ApplicationThread.scheduleLaunchActivity , 通知相应的进程执行启动 Activity
5. ApplicationThread 把启动Activity 的操作转发给 ActivityThread，ActivityThread 通过 ClassLoader 加载相应的 Activity 类，然后把它启动起来


## 参考

- [Android应用程序的Activity启动过程](https://blog.csdn.net/luoshengyang/article/details/6685853)

