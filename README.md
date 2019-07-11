# 1.锁的底层实现
java 虚拟机中的同步(Synchronization)基于进入和退出管理(Monitor)对象实现。同步方法，并不是由monitor-enter 和 monitor-exit 指令来实现同步的，而是由方法调用指令取运行时常量池中方法的 ACC_SYNCHRONIZED 标记来隐式实现的。

## 1.1 对象内存简图
![对象内存简图](https://i.loli.net/2019/07/11/5d26a6c5466ad58228.png "对象内存简图")
对象头：存储对象的hashCode、锁信息或分代年龄或GC标记，类型

![对象内存简图](https://i.loli.net/2019/07/11/5d26ac5209ed875137.png "对象内存简图")
