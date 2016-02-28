title: "java多线程中的volatile"
date: 2015-04-07 00:20:36
tags: 
- volatile
categories: Java基础
---
>**volatile：**
英['vɒlətaɪl] 美[ˈvɑlətl, -ˌtaɪl]
adj. 易变的，不稳定的;（液体或油）易挥发的;爆炸性的;快活的，轻快的
[例句]A volatile political system makes these reforms hard to achieve.
而反复无常的政治体系又让这些方面的改革无法实施。
--xx翻译

# 一、volatile简述
Java 语言提供了一种稍弱的同步机制,即 volatile 变量.用来确保将变量的更新操作通知到其他线程,保证了新值能立即同步到主内存,以及每次使用前立即从主内存刷新. 当把变量声明为volatile类型后,编译器与运行时都会注意到这个变量是共享的.

# 二、 volatile线程安全？
volatile 变量对所有线程是立即可见的,对 volatile 变量所有的写操作都能立即反应到其他线程之中,换句话说:volatile 变量在各个线程中是一致的,所以基于 volatile 变量的运算是线程安全的.  

这句话论据貌似没有错,论点确实错的.

# 三、 valotile 为什么是线程不安全的？

    public class VolatileTest{
        
        public static volatile int  i;
    
        public static void increase(){
            i++;
        }
    }

编译（`javap -c -l VolatileTest.class`）后，

    public class VolatileTest {
      public static volatile int i;
     
      public VolatileTest();
        Code:
           0: aload_0       
           1: invokespecial #1                  // Method java/lang/Object."<init>":()V
           4: return        
        LineNumberTable:
          line 1: 0
     
      public static void increase();
        Code:
           0: getstatic     #2                  // Field i:I, 把i的值取到了操作栈顶,volatile保证了i值此时是正确的. 
           3: iconst_1      
           4: iadd                              // increase,但其他线程此时可能已经把i值加大了好多
           5: putstatic     #2                  // Field i:I ,把这个已经out of date的i值同步回主内存中,i值被破坏了.
           8: return        
        LineNumberTable:
          line 6: 0
          line 7: 8
    }

从这个角度说 volatile 并不完全是线程安全的.

# 四、 常见的用法
在Java内存模型中，有main memory，每个线程也有自己的memory (例如寄存器)。为了性能，一个线程会在自己的memory中保持要访问的变量的副本。这样就会出现同一个变量在某个瞬间，在一个线程的memory中的值可能与另外一个线程memory中的值，或者main memory中的值不一致的情况。 
一个变量声明为volatile，就意味着这个变量是随时会被其他线程修改的，因此不能将它cache在线程memory中。以下例子展现了volatile的作用：

    public class StoppableTask extends Thread {
        private volatile boolean pleaseStop;

        public void run() {
            while (!pleaseStop) {
                // do some stuff...
            }
        }
    
        public void tellMeToStop() {
            pleaseStop = true;
        }
    }

假如pleaseStop没有被声明为volatile，线程执行run的时候检查的是自己的副本，就不能及时得知其他线程已经调用tellMeToStop()修改了pleaseStop的值。 

Volatile一般情况下不能代替sychronized，因为volatile不能保证操作的原子性，即使只是i++，实际上也是由多个原子操作组成：read i; inc; write i，假如多个线程同时执行i++，volatile只能保证他们操作的i是同一块内存，但依然可能出现写入脏数据的情况。如果配合Java 5增加的atomic wrapper classes，对它们的increase之类的操作就不需要sychronized。 