title: "多线程之Condition"
date: 2015-09-07 10:52:21
tags: 
- java
- 多线程
- Condition
categories: Java基础
---

### 为什么使用多线程？

### 带来的问题

- 执行顺序：需要达到某种条件次线程阻塞或者继续执行。某些情况可以用回调模型来处理这个问题。
- 同步写：有时候并发写会造成数据出错，不一致，由于一般的写操作（如i++操作）是三个原子操作构成的：

        取i; 
        temp = i+1; 
        i = temp; 
        //应该合并成一个操作的三步如果被中间执行污染数据的代码，那就造成错误。
    
- 由于需要同步的原因，引入锁的概念。但是锁也需要最小刻度。因为锁有性能开销。

<!--more-->

### Condition的基本用法：解决执行条件/顺序问题

Condition解决的是线程通信问题，使用signal()提醒某些await()的线程.

直接上代码：

    package com.springapp.mvc;
    
    import java.util.PriorityQueue;
    import java.util.concurrent.locks.Condition;
    import java.util.concurrent.locks.Lock;
    import java.util.concurrent.locks.ReentrantLock;
    
    /**
     * Created by yp-tc-m-2795 on 15/9/6.
     */
    public class ConditionTest {
        private int                    queueSize = 10;
        private PriorityQueue<Integer> queue     = new PriorityQueue<>(queueSize);
        private Lock                   lock      = new ReentrantLock();
        private Condition              notFull   = lock.newCondition();
        private Condition              notEmpty  = lock.newCondition();
    
        public static void main(String[] args) {
            ConditionTest test     = new ConditionTest();
            Producer      producer = test.new Producer();
            Consumer      consumer = test.new Consumer();
    
            producer.start();
            consumer.start();
        }
    
        class Consumer extends Thread {
    
            @Override
            public void run() {
                consume();
            }
    
            private void consume() {
                while (true) {
                    lock.lock();
                    try {
                        while (queue.size() == 0) {
                            try {
                                System.out.println("队列空，等待数据");
                                notEmpty.await();
                            } catch (Exception e) {
                                e.printStackTrace();
                            }
                        }
                        queue.poll();                //每次移走队首元素
                        notFull.signal();
                        System.out.println("取走一个元素，队列剩余" + queue.size() + "个元素");
                        try {
                            Thread.sleep(800);
                        } catch (InterruptedException e) {
                            e.printStackTrace();
                        }
                    } finally {
                        lock.unlock();
                    }
                }
            }
        }
    
        class Producer extends Thread {
    
            @Override
            public void run() {
                produce();
            }
    
            private void produce() {
                while (true) {
                    lock.lock();
                    try {
                        while (queue.size() == queueSize) {
                            try {
                                System.out.println("队列满，等待有空余空间");
                                notFull.await();
                            } catch (InterruptedException e) {
                                e.printStackTrace();
                            }
                        }
                        queue.offer(1);        //每次插入一个元素
                        notEmpty.signal();
                        System.out.println("插入一个元素，队列剩余空间：" + (queueSize - queue.size()));
                        try {
                            Thread.sleep(800);
                        } catch (InterruptedException e) {
                            e.printStackTrace();
                        }
                    } finally {
                        lock.unlock();
                    }
                }
            }
        }
    }

 
在Condition中，用await()替换wait()，用signal()替换notify()，用signalAll()替换notifyAll()，传统线程的通信方式，Condition都可以实现，这里注意，Condition是被绑定到Lock上的，要创建一个Lock的Condition必须用newCondition()方法。

>***可以用signal()唤醒确定的线程，这是Object.notify()达不到的。***



---

传统的notify()/notifyAll()写法：不保证唤醒后的抢占结果的顺序。


    package com.springapp.mvc;
    
    /**
     * Created by yp-tc-m-2795 on 15/9/6.
     */
    public class NotifyTest {
        public static Object object = new Object();
    
        public static void main(String[] args) {
            Thread1 threadA = new Thread1("A");
            Thread1 threadB = new Thread1("B");
            Thread1 threadC = new Thread1("C");
            Thread2 thread2 = new Thread2();
    
            threadA.start();
            threadB.start();
            threadC.start();
    
            try {
                Thread.sleep(200);
            } catch (Exception e) {
                e.printStackTrace();
            }
    
            thread2.start();
        }
    
        static class Thread1 extends Thread {
    
            Thread1(String name) {
                super(name);
            }
    
            @Override
            public void run() {
                synchronized (object) {
                    System.out.println("加锁,然后释放锁");
                    try {
                        object.wait();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    object = "Thread1";
                    System.out.println("线程" + Thread.currentThread().getName() + "获取到了锁-----" + object);
                }
            }
        }
    
        static class Thread2 extends Thread {
            @Override
            public void run() {
                synchronized (object) {
                    object.notifyAll();
                    object = "Thread2";
                    System.out.println("线程" + Thread.currentThread().getName() + "调用了object.notify()---" + object);
                }
                System.out.println("线程" + Thread.currentThread().getName() + "释放了锁");
            }
        }
    }
