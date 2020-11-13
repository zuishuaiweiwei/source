---
title:  java 线程间通信手段
date: 2020-11-11 10:25:54
tags: 
 - 多线程
categories: 
 - 多线程
---
# java 线程间通信手段

### AtomicInteger

> 线程安全的进行 int 值的修改，因为使用了 unsafe 的本地方法，直接调用 cpu 的 cas 指令，核心方法

 ```java
     //如果修改前的值为 expect，那么修改为 update，否则重试
 	public final boolean compareAndSet(int expect, int update) {
         return unsafe.compareAndSwapInt(this, valueOffset, expect, update);
     }
 ```

### Semaphore

> 信号量，初始化为开始为几个信号量，线程可以从 Semaphore 申请许可，申请一个值减 1 ，为 0 则申请失败 ，从新获取许可。天然带重试，阻塞功能。核心方法

```
    //申请1许可 值减1
    public void acquire() throws InterruptedException {
        sync.acquireSharedInterruptibly(1);
    }
    //释放1许可，值加1
    public void release() {
        sync.releaseShared(1);
    }
    //获取失败不会重试，一般配合 if 判断使用
    public boolean tryAcquire() {
        return sync.nonfairTryAcquireShared(1) >= 0;
    }
```

### ReentrantLock

> 可重入锁，synchronized 的代替类，比 synchronized 灵活，使用略麻烦，可以创建不同数量的 Condition 实现等待通知机制,Condition 一般使用都是 while 判断条件，条件不满足就睡眠 ，等待唤醒
>
> ，核心方法

```java
    //获取 Condition
    public Condition newCondition() {
        return sync.newCondition();
    }
    // 获取锁
    public void lock() {
        sync.lock();
    }
    //释放锁
    public void unlock() {
        sync.release(1);
    }

```



