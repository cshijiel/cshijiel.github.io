title: "java.lang.Thread#start源代码解读"

date: 2016-03-10 22:05:29

tags:

- java
- 源代码

---

在需要多线程的场景中，最基础的创建线程的方式是

``` java
Runnable task = () -> {
	// TODO: 16/3/10  
};
new Thread(task).start();
```

那`new Thread(task).start()`到底发生了什么呢？



<!--more-->



------------

### 简单分析`java.lang.Thread#start`的源代码

这是一个`synchronized`修饰的方法：

``` java
/**
 * Causes this thread to begin execution; the Java Virtual Machine
 * calls the <code>run</code> method of this thread.
 * <p>
 * The result is that two threads are running concurrently: the
 * current thread (which returns from the call to the
 * <code>start</code> method) and the other thread (which executes its
 * <code>run</code> method).
 * <p>
 * It is never legal to start a thread more than once.
 * In particular, a thread may not be restarted once it has completed
 * execution.
 *
 * @exception  IllegalThreadStateException  if the thread was already
 *               started.
 * @see        #run()
 * @see        #stop()
 */
public synchronized void start() {
	/**
	 * This method is not invoked for the main method thread or "system"
	 * group threads created/set up by the VM. Any new functionality added
	 * to this method in the future may have to also be added to the VM.
	 *
	 * A zero status value corresponds to state "NEW".
	 */
	if (threadStatus != 0)
		throw new IllegalThreadStateException();

	/* Notify the group that this thread is about to be started
	 * so that it can be added to the group's list of threads
	 * and the group's unstarted count can be decremented. */
	group.add(this);

	boolean started = false;
	try {
		start0();
		started = true;
	} finally {
		try {
			if (!started) {
				group.threadStartFailed(this);
			}
		} catch (Throwable ignore) {
			/* do nothing. If start0 threw a Throwable then
			  it will be passed up the call stack */
		}
	}
}
```

注意到有以下特点：

- 一个thread对象只能启动一个任务。该方法是`synchronized `的，目的是为了保证多线程并发下也只有一个线程可以执行该方法，即--It is never legal to start a thread more than once.并且有状态`java.lang.Thread#threadStatus`(volatile修饰)的保证。
- 调用时同时有两个线程执行：一个返回到调用该方法的主线程，另一个执行任务的`run()`方法。
- 调用`run()`方法是通过`java.lang.Thread#start0()`方法，这是一个**native**本地方法。

**疑问：**既然“一个thread对象只能启动一个任务”，那线程池是怎样实现的？

**挖个坑，待填！**



### 线程池的原理