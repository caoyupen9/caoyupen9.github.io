# 1.锁的底层实现
  java 虚拟机中的同步(Synchronization)基于进入和退出管理(Monitor)对象实现。同步方法，并不是由monitor enter 和 monitor exit 指令来实现同步的，而是由方法调用指令取运行时常量池中方法的 ACC_SYNCHRONIZED 标记来隐式实现的。

## 1.1 对象内存简图
![对象内存简图](https://i.loli.net/2019/07/11/5d26a6c5466ad58228.png "对象内存简图")

  对象头：存储对象的hashCode、锁信息或分代年龄或GC标记，类型指针指向对象的类元数据，JVM通过这个指针确定该对象是哪个类想实例等信息。
  实例变量：存放类属性数据信息，包括父类的属性信息。
  填充数据：由于虚拟机要求对象起始地址必须是8字节的整数倍。填充数据不是必须存在的，仅仅是为了字节对齐。
  当在对象上加锁时，数据是记录在对象头中。当执行synchronize同步方法或同步代码块时，会在对象中记录锁标记，锁标记指向的是monitor对象(也称为管程或监视器锁)的起始地址。每个对象都存在着一个 monitor 与之关联，对象与其 monitor 之间的关系有存在多种实现方式，如monitor可以与对象一起创建销毁或当线程试图获取对象锁时自动生成，但当一个monitor被某个线程持有后，他便处于锁定状态。
  在Java虚拟机(HotSpot)中，monitor是有ObjectMonitor实现的。
  ObjectMonitor中有两个队列， _WaitSet 和 _EntryList,以及 _Owner标记。其中 _WaitSet是用于管理等待队列(wait方法)线程的，
![对象内存简图](https://i.loli.net/2019/07/11/5d26ac5209ed875137.png "对象内存简图")
