---
title: H2O 生成[中等]
date: 2020-11-12 10:25:54
tags: 
 - 多线程
categories: 
 - 多线程
cover: https://wei-picgo.oss-cn-beijing.aliyuncs.com/img/20201112171920.png
---

# H2O 生成

![image-20201112171918197](https://wei-picgo.oss-cn-beijing.aliyuncs.com/img/20201112171920.png)

## 分析

> 题目看起来复杂，思路很清晰，3个为一组 必须执行生产两个 h 1个 o ，顺序可以变

## 代码

### java

> 信号量

```java
class H2O {

    private Semaphore h = new Semaphore(2);
    private Semaphore o = new Semaphore(2);
    public H2O() {
        
    }

    public void hydrogen(Runnable releaseHydrogen) throws InterruptedException {
        h.acquire(1);
        releaseHydrogen.run();
        o.release(1);
    }

    public void oxygen(Runnable releaseOxygen) throws InterruptedException {
        o.acquire(2);
        releaseOxygen.run();
        h.release(2);
    }
}
```