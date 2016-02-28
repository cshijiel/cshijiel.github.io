title: "Thread类与多线程"
date: 2015-04-07 00:20:36
tags: 
- 多线程
- Thread
categories: Java基础
---
首先是ThreadA类，继承了Thread，重写了run().

    package cn.wiz.roc.thread;
    
    public class ThreadA extends Thread {
    	public ThreadA(String name) {
    		super(name);
    	}
    
    	public synchronized void run() {
    		try {
    			for (int i = 0; i < 100; i++) {
    				System.out.println(this.getName() + ":" + i);
    				if (i % 3 == 0)
    				    //每隔3的时候线程休眠1000ms
    					Thread.sleep(1000);
    			}
    		} catch (InterruptedException e) {
    			e.printStackTrace();
    		}
    	}
    }
    
ThreadB类似：

    package cn.wiz.roc.thread;
    
    public class ThreadB extends Thread {
    	public ThreadB(String name) {
    		super(name);
    	}
    
    	public synchronized void run() {
    		try {
    			for (int i = 0; i < 100; i++) {
    				System.out.println(this.getName() + ":" + i);
    				// i能被4整除时，休眠100毫秒
    				if (i % 4 == 0)
    					Thread.sleep(1500);
    			}
    		} catch (InterruptedException e) {
    			e.printStackTrace();
    		}
    	}
    }


SleepTest.java    

    package cn.wiz.roc.thread;
    
    public class SleepTest {
    	public static void main(String[] args) {
    		ThreadA t1 = new ThreadA("A");
    		ThreadB t2 = new ThreadB("B");
    		t1.start();
    		t2.start();
    	}
    }



其中一段执行结果：

    ...
    A:16
    A:17
    A:18
    A:19
    A:20
    A:21
    B:17
    B:18
    B:19
    B:20
    ...

>对于多线程环境，`start()`方法会通过底层的API执行重写的`run()`方法，使该方法获得竞争CPU计算的能力。在方法的执行过程中，原来的调用环境依次往下执行，不会等待`run()`方法执行结束。
>run()的过程里，可以调用`Thread.sleep(1000);`使得线程休眠。当然，Thread还有其他几个方法，在此不再赘述。
--Roc

---
## Thread.sleep()和Thread.currentThread().sleep()区别

> 线程可以用继承**Thread类**或者实现**Runnable接口**来实现.
> 
> Thread.sleep()是Thread类的方法,只对当前线程起作用,睡眠一段时间.
> 
> 如果线程是通过继承Thread实现的话这2个方法没有区别；
> 
> 如果线程是通过实现Runnable接口来实现的,则不是Thread类,不能直接使用Thread.sleep()
> 
> 必须使用Thread.currentThread()来得到当前线程的引用才可以调用sleep(),
> 
> 所以要用Thread.currentThread().sleep()来睡眠...


