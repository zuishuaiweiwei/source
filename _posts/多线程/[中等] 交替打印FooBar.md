---
title: 交替打印FooBar[中等]
date: 2021-11-12 10:25:54
tags: 
 - 多线程
categories: 
 - 多线程
cover: https://wei-picgo.oss-cn-beijing.aliyuncs.com/img/20201112141146.png
---

# 交替打印FooBar

![image-20201112141129371](https://wei-picgo.oss-cn-beijing.aliyuncs.com/img/20201112141146.png)

## 分析

> 思路简单，还是考验基本的线程间通信手段

## 代码

### java

```java
class FooBar {
    private int n;
    private ReentrantLock lock = new ReentrantLock(true);
    private Condition n1 =  lock.newCondition();
    private Condition n2 =  lock.newCondition();
    private volatile boolean b = true;

    public FooBar(int n) {
        this.n = n;
    }

    public void foo(Runnable printFoo) throws InterruptedException {
        
        for (int i = 0; i < n; i++) {
            // one.acquire();
            lock.lock();
            if(!b){
                n1.await();
            }
            printFoo.run();
            // two.release();
            b = !b;
            n2.signal();
            lock.unlock();
        }
    }

    public void bar(Runnable printBar) throws InterruptedException {
        
        for (int i = 0; i < n; i++) {
            lock.lock();
            // two.acquire();
            if(b){
                n2.await();
            }
            printBar.run();
            // one.release();
            b=!b;
            n1.signal();
            lock.unlock();
        }
    }
}
```
