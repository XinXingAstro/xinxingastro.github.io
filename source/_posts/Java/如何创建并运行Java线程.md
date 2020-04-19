---
title: 如何创建并运行Java线程
tags:
  - null
  - null
comments: true
mathjax: false
typora-copy-images-to: ../../images
typora-root-url: ../../../source
date: 2017-08-06 14:43:07
categories: Java
---

创建并运行Java线程的两种方法。

<!-- more -->

---

## 方法1: 实现Runnable接口

先来看一下Runnable接口，非常简单，里面只有一个run()方法：

```java
public interface Runnable {
    public abstract void run();
}

```

实现了Runnble接口的类的对象是被用来创建线程的，start这个线程会导致对象的run方法在这个线程中被调用。

最常使用的方法是在Thread的构造方法中传入一个实现了Runnable接口的匿名内部类，如下所示：

```java
public static void main(String[] args) {
    Thread myThread = new Thread(new Runnable() {
        public void run() {
            System.out.println("myThread is running");
        }
    });
    myThread.start();
}
```

当新建的线程很多的时候，为了区分线程，需要给线程起个名字，可以在Thread构造方法中直接传入新建线程的名字，然后使用`Thread.currentThread().getName()`来得到这个线程的名字。

```java
public static void main(String[] args) {
    Thread myThread = new Thread(new Runnable() {
        public void run() {
            System.out.println(Thread.currentThread().getName());
        }
    }, "myThread");
    myThread.start();
}
```

注意：在启动线程时要使用`Thread.start()`，不能直接调用`Thread.run()`。使用start()方法JVM会新建一个线程然后使用新建的线程执行run()方法，如果直接调用run()方法，JVM不会新建线程，相当于在当前线程中执行Thread对象中的run()方法。

## 方法2: 创建Thread的子类

Thread类实现了Runnable接口，我们可以创建Thread类的子类，然后重写run()方法，然后通过新建该类的对象来新建线程。

```java
static class MyThread extends Thread {
    @Override
    public void run() {
        System.out.println("this is MyThread");
    }
}
public static void main(String[] args) {
    Thread thread = new MyThread();
    thread.start();
}
```

也可以创建一个Thread的匿名子类对象来新建线程。

```java
public static void main(String[] args) {
    Thread thread = new Thread() {
        @Override
        public void run() {
            System.out.println("this is myThread");
        }
    };
    thread.start();
}
```

## 方法3: 实现Callable接口

`java.util.concurrent.Callable`接口里面只有一个方法，跟Runnable接口很类似，Callable接口源码如下：

```java
public interface Callable<V> {
    V call() throws Exception;
}
```

使用这种方法创建线程分以下几步：

1.创建一个类实现Callable接口中的call()方法.

```java
public static class MyThread implements Callable<Integer> {
    @Override
    public Integer call() throws Exception {
        System.out.println(Thread.currentThread().getName() + " is running");
        return 1;
    }
}
```

2.新建Callable对象，使用Callabe对象作为传入参数新建FutureTask对象，FutureTask对Callable的call()方法的返回值进行了封装，然后使用FutureTask对象作为参数新建Thread对象，然后使用Thread.start()方法启动线程。

```java
import java.util.concurrent.Callable;
import java.util.concurrent.ExecutionException;
import java.util.concurrent.FutureTask;

public class ThreadExample02 {
    public static class MyThread implements Callable<Integer> {

        @Override
        public Integer call() throws Exception {
            System.out.println(Thread.currentThread().getName() + " is running");
            return 1;
        }
    }

    public static void main(String[] args) {
        Callable myThread = new MyThread();
        FutureTask<Integer> futureTask = new FutureTask<Integer>(myThread);
        Thread thread = new Thread(futureTask, "myThread");
        thread.start();
        try {
            System.out.println("子线程返回值 ：" + futureTask.get());
        } catch (InterruptedException e) {
            e.printStackTrace();
        } catch (ExecutionException e) {
            e.printStackTrace();
        }
    }
}
```

## 三种方法对比

使用时间Runnable，Callable接口的方法：

- 优势：由于只实现了接口，所以还可以继承其他类。多个Thread可以共享同一个target对象，适合多个相同线程来处理同一份资源的情况，可以将CPU、代码和数据分开，形成清晰的模型。
- 劣势：编程复杂，访问当前线程必须使用Thread.currentThread()方法。

使用继承Thread类的方法：

- 优势：编程简单，访问当前线程直接使用this。
- 劣势：由于线程类已经继承了Thread类，所以不能再继承其他类。