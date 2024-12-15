---
title: java多线程编程
tags: [java]
categories: [java]
katex: true
mathjax: true
author: skyang
date: 2024-12-11 16:20:53
sticky:
mermaid:
wiki: java
topic:
---

## 多线程基础

并发：在同一时刻，有多个指令在单个cpu上交替执行。

并行：在同一时刻，有多个指令在多个cpu上同时执行。



## 多线程实现方式

### 继承Thread类方式实现

1. 定义一个类继承Thread
2. 重写子类run()方法
3. 创建子类对象并调用进程

```java
public class Mythread extends Thread{
    @Override
    public void run() {
        for(int i=0;i<100;i++){
            System.out.println(this.getName() + i);
        }
    }
}
```

```java
public class Main {
    public static void main(String[] args){
        Mythread th1 = new Mythread();
        Mythread th2 = new Mythread();

        th1.setName("thread1:");
        th2.setName("thread2:");

        th1.start();
        th2.start();
    }
}
```



### 实现Runnable接口实现

1. 自己定义一个类实现Runnable接口
2. 重写里面的run方法
3. 创建自己的类的对象
4. 创建个Thread类的对象，并开启线程

```java
public class Myrun implements Runnable{
    @Override
    public void run() {
        Thread th = Thread.currentThread();
        for(int i=0;i<100;i++)
            System.out.println(th.getName() + i);
    }
}
```

```java
public class Main {
    public static void main(String[] args){
        // 创建任务对象，表示多线程要执行的任务
        Myrun run= new Myrun();
        
        // 创建线程对象
        Thread th1 = new Thread(run);
        Thread th2 = new Thread(run);

        th1.setName("Thread1:");
        th2.setName("Thread2:");

        th1.start();
        th2.start();
    }
}

```

### 使用Callable接口和Furture接口进行实现

1. 创建一个类MyCallab1e实现callab1e按口
2. 重写call(是有返回值的，表示多线程运行的结果)
3. 创建MyCallable的对象（表示多线程要执行的任务）
4. 创建FutureTask的对象（作用管理多线程运行的结果）
5. 创建Thread类的对象，并启动（表示线程）

```java
public class Mycallable implements Callable {
    @Override
    public Object call() throws Exception {
        int sum = 0;
        for(int i=0;i<100;i++){
            System.out.println(i);
            sum = sum + i;
        }
        return sum;
    }
}
```

```java
public class Main {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        // 创建MyCallable对象，表示多线程要执行的任务
        Mycallable mc = new Mycallable();

        // 创建FureturTask对象，管理多线程运行的结果
        FutureTask<Integer> ft = new FutureTask<>(mc);

        Thread th = new Thread(ft);
        th.start();

        // 获取多线程运行的结果
        Integer result = ft.get();
        System.out.println(result);
    }
}
```



## Thread常用成员方法

![thread](https://gitee.com/skyang9/skyang-notebook/raw/master/img/image-20241211220310978.png)



优先级：1~10，默认为5，仅代表运行概率。


守护线程：当其他的非守护线程执行完毕之后：守护线程会陆续结束




## 线程生命周期

![image-20241212172045928](https://gitee.com/skyang9/skyang-notebook/raw/master/img/image-20241212172045928.png)

![image-20241212180947826](https://gitee.com/skyang9/skyang-notebook/raw/master/img/image-20241212180947826.png)



## 案例一

模拟使用三个线程表示三个售票窗口，一共出售1000张门票。

### Thread实现

注意点：

1. synchronized 应当锁一个相同的对象，可以锁一个静态成员变量 / 本类的class字节码文件。
2. synchronized应当放到循环之内，否则一个线程循环1000次后才会解锁。

```java
package org.example.juc;

public class Mythread extends Thread{
    static int ticket = 0;
    @Override
    public void run() {
        while(true) {
            synchronized (Mythread.class){
                if(ticket == 1000) break;
                try {
                    Thread.sleep(50);
                } catch (InterruptedException e) {
                    throw new RuntimeException(e);
                }

                ticket++;
                System.out.println(Thread.currentThread().getName() + "出售了门票" + ticket);
            }
        }
    }
}
```

### Runnable实现

期间的问题：由于线程切换速度较慢，所以出售100张票可能看不到线程切换的表现，因此将循环次数改为1000。

```java
public class Myrun implements Runnable{
    static int ticket = 0;
    
    @Override
    public void run() {
        while(true)
            if(method()) break;
    }

    private synchronized boolean method(){
        if(ticket == 1000)
            return true;

        try {
            Thread.sleep(50);
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }
        ticket++;
        System.out.println(Thread.currentThread().getName() + "出售了门票" + ticket);
        return false;
    }
}
```

## 案例二

### 普通实现

生产者消费者问题：一位顾客吃面，一位厨师做面，桌子上可以摆一碗面。
