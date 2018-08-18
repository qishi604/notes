# Android Binder

## Binder 是什么

- 通常意义下，Binder 指的是一种通信机制。我们说的 AIDL 使用 Binder 进行通信，指的就是Binder 这种 IPC机制
- 对于 server 进程来说，Binder 指的是Binder 本地对象
- 对于Client 进程来说，Binder 指的是Binder 代理对象，它只是Binder 本地对象的一个远程代理。对这个Binder 代理对象的操作，会通过驱动最终转发到 Binder 本地对象上去完成。对于使用者而言，它无需关心是代理对象还是本地对象
- 对于传输过程而言，Binder 是可以跨进程传递的对象。Binder 驱动会对具有跨进程传递能力的对象做特殊处理，自动完成代理对象和本地对象的转换。

## 理解 java 层的Binder 

- `IBinder` 是一个接口，代表一种跨进程传输的能力。只要实现这个接口，就能将这个对象进行跨进程传递。这是驱动底层支持
- `IInterface` 代表远程 server 对象具有什么能力。具体来说就是 aidl 里面的接口
- `Binder` java 层的 Binder 类，代表的就是 Binder 本地对象。
- `BinderProxy` 类是 Binder 类的一个内部类，它代表远程进程的Binder 对象的本地代理。这两个类都继承 IBinder ，因而都具有跨进程传输的能力。
- `Stub` 在使用AIDL的时候，编译工具会给我们生成一个Stub的静态内部类；这个类继承了Binder, 说明它是一个Binder本地对象，它实现了IInterface接口，表明它具有远程Server承诺给Client的能力；Stub是一个抽象类，具体的IInterface的相关实现需要我们手动完成，这里使用了策略模式。

## 参考

[Binder学习指南](http://weishu.me/2016/01/12/binder-index-for-newer/)