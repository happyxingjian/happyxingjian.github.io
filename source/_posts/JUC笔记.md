---
title: JUC笔记
date: 2021-11-04 01:14:36
sticky: 1
categories: 
  - 多线程
tags:
  - java
---
# Java JUC 简介

> 5.0提供的 java.util.concurrent 包，在此包中增加了在并发编程中很常用的实用工具类，用于定义类似于线程的自定义子系统，包括线程池，异步IO,和轻量级任务框架。提供可调的、灵活的线程池。还提供了设计用于多线程上下文中Collection实现等。

# volatile关键字（内存可见性）

内存可见性问题：当多个线程操作共享数据时，彼此不可见。

volatile关键字：当多个线程操作共享数据时，可以保证内存中数据可见。相较于sychronized是一种轻量级的同步策略

注意：
1、volatile不具备“**互斥性**”
2、volatile不能保证变量的“**原子性**”

# 原子变量（CAS算法）

原子变量：jdk1.5后java.util.concurrent.atomic包下提供常用的原子变量。比如AtomicInteger

该包下的类的特性:

- 类的变量都是volatile修饰，保证内存可见性。
- 使用CAS(Compare-And-Swap)算法保证数据的原子性。
  - CAS是硬件对于并发操作共享数据的支持
  - CAS包含了3个操作数：内存值V，预估值A,更新值B，当且仅当V==A,V=B,否则不做任何操作。

# ConcurrentHashMap（锁分段机制）

对于多线程操作，介于HashMap与Hashtable之间，内部采用“**锁分段**”机制，代替Hashtable的“独占锁”，进而提高性能。

默认分为16段Segment。

# CountDownLatch（闭锁）

它是一个同步辅助类，在完成一组正在其他线程中执行的操作之前，它允许一个或者多个线程一直等待。

- 闭锁可以延迟线程的进度直到其到达最终状态。闭锁可以确保某些活动直到其他活动都完成才继续执行。
- 确保某个计算在其需要的所有资源都被初始化之后才继续执行。
- 确保某个服务在其所依赖的所有服务都已经启动之后才启动
- 等待某个操作所有参与者都就绪再继续执行。

# 实现Callable接口

一、实现Callable接口相较于实现Runable接口的方式：

- 可以有返回值
- 可以抛出异常

二、执行Callable方式，需要FutureTask 实现类的支持，用于接收运算后结果。FutureTask是Future接口的实现类。

```java
// 启动线程方式
ThreadDemo td = new ThreadDemo();
FutureTask<Integer> result = new FutureTask<>(td);
new Thread(result).start();
result.get()//获取线程运算后结果，它会一直等到上述线程执行完毕后才执行get,所以也能实现闭锁。

// 实现Calllable
class ThreadDemo implements Callable<Integer>{
    public Integer call() throws Exception{
        return null;
    }
}
```



# Lock（同步锁）

用于解决多线程安全问题的方式

- sychronized:隐式锁
  - 同步代码块
  - 同步方法
-  同步锁Lock（jdk1.5以后）：显示锁   通过lock()方法上锁，通过unlock()方法释放锁

# Condition控制线程通信

- Condition接口描述了可能会与锁有关联的条件变量。这些变量在用法上与使用Object.wait()访问的隐式监视器类似，但提供了更强大的功能。需要特别指出的是，**单个Lock可能与多个Condition对象关联**。为了避免兼容问题，Condition中的方法名称与对应的Object版本中不同
- 在Condition对象中，与wait、notify、notifyAll对应的分别是**await、signal、signalAll**。
- Condition实例实质上被绑定到一个锁上。要为特定Lock获得Condition实例，请使用其newCondition()方法。

# ReadWriteLock（读写锁）

- 写写/读写 需要“互斥”
- 读读  不需要“互斥”

# 线程池

线程池：提供了一个线程队列，队列中保存着所有等待状态的线程。避免了创建销毁的额外开销，提高了响应速度。

线程池体系结构：

- java.util.concurrent.Executor 接口: 负责线程的使用和调度的**根接口**。
  - ExecutorService 子接口:线程池的主要接口
    - **ThreadPoolExecutor** 实现类：
    - ScheduledExecutorService 子接口：负责线程的调度
      - ScheduledThreadPoolExecutor实现类:继承了ThreadPoolExecutor，实现ScheduledExecutorService

工具类：Executors

- ExecutorService newFixedThreadPool():创建固定大小的线程池
- ExecutorService newCachedThreadPool():缓存线程池，线程池的数量不固定，可以根据需求自动地更改数量。
- ExecutorService newSingleThreadExecutor():创建只有一个线程的线程池。
- ScheduledExecutorService newScheduledThreadPool():创建固定大小的线程池，可以延迟或者定时地执行任务。

# ThreadLocal

定义：提供**线程局部**变量，一个线程局部变量在多个线程中分别有独立的值(**副本**)

特点：简单(开箱即用)、快速(无额外开销)、安全(线程安全)

场景：多线程场景（资源持有、线程一致性、并发计算、线程安全等场景）

实现原理：Java中用**哈希表**实现

应用范围：几乎所有提供多线程特征的语言

模型：

![ThreadLocal模型](https://gitee.com/gaoshoufengmu/pic/raw/master/java%E6%A0%B8%E5%BF%83/ThreadLocal%E6%A8%A1%E5%9E%8B.jpg)

ThreadLocal 常用API

```java
ThreadLocal<T>() // 构造函数
initialValue() // 初始化
get() / set()   // 访问器
remove()   // 回收
```















