---
layout:     post
title:      "wait和sleep"
subtitle:   "Java多线程wait()和sleep()的区别"
date:       2018-08-23 11:40:00
author:     "TenzT"
header-img: "img/post-bg-2015.jpg"
tags:
    - JavaSE
    - 并发
---

# 简介
Java中sleep和wait都可以使进程进入的等待状态，文本对两者进行对比

# 区别
1. sleep是Thread的静态方法，在线程中调用该方法会使线程进入睡眠。wait()是Object的成员方法。
2. sleep没有释放锁，而wait则是释放锁之后进入等待队列
3. 退出方式：sleep在睡眠指定时间自动唤醒，而wait则是等待其他线程notify/notifyAll来唤醒
4. wait、notify/notifyAll必须在同步控制块、同步方法中使用。

# 补充
- wait被用于线程间通信，而sleep一般来说被用于在执行时引入暂停。前者用于线程同步，后者线程同步
- Thread.sleep()让当前线程进入不可运行状态一段时间。线程继续保持它所获取的监视器——即如果线程当前处于同步块或方法中，则没有其他线程可以进入此块或方法。
- object.wait()让当前线程进入不可运行状态，如sleep()一样，但不同的是wait方法从一个对象调用，而不是从一个线程调用；我们称这个对象为“锁定对象（lockObj）”
- notify/notify之后，不是马上释放锁，必须等到所在同步块执行完之后才释放锁。

# 扩展：
wait/notify的机制可以参照操作系统的“管程”。线程进入临界区需要先获取互斥锁，然后设定特定条件，满足条件则线程继续运行，否则释放锁，等待被唤醒。
![](https://raw.githubusercontent.com/TenzT/TenzT.github.io/master/img_markdown/concurrency/waitTest.png)
