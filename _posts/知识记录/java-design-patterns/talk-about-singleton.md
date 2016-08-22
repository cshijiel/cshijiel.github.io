title: 也谈单例模式
date: 2015-10-08 16:53:42
tags:
- java
- 设计模式

categories: Java基础

---

单例模式(`Singleton`)也叫单态模式。

#### 用途

有时候需要内存中仅有一个某对象就足够了，出现多个就会出现内存浪费甚至逻辑错误。典型的有数据库连接池等，一个连接池就够了。

#### 常见实现

##### 急加载（Eager Initialization）
Initialization On Demand Holder

	public class Singleton {
		// 这个实例要不要不可变(final),一般不需要
		private static Singleton singleton = new Singleton();
	
		// 私有化构造器
		private Singleton() {
		}
	
		public static Singleton getInstance() {
			return singleton;
		}
	}

<!--more-->


##### DCL（Double-Checked Locking）
支持延迟加载

	public class Singleton {
		private static Singleton singleton = null;
	
		private Singleton() {
		}
	
		public static Singleton getInstance() {
			if (null == singleton) {
				synchronized (Singleton.class) {
					if (null == singleton) {
						singleton = new Singleton();
					}
				}
			}
			return singleton;
		}
	}

但是，DCL仍然会出现失败的情况，
DCL失败的原因：
关键在于这一句`singleton = new Singleton();`，并不是原子操作，大致会做下面几件事：

- 开辟内存空间；

- 引用指向内存空间（非null的一步）；

- 初始化内存空间；

但是，由于Java编译器允许处理器乱序执行（out-of-order），以及JDK1.5之前JMM（Java Memory Medel）中Cache、寄存器到主内存回写顺序的规定，初始化内存空间和连接引用与内存空间的执行顺序是不一定的，所以按照现在的顺序，有可能出现拿走一个为初始化的对象，造成很难fix的异常。
下面是优化版本。

##### 双判空模式DCL with Volatile

	public class Singleton {
		public static volatile Singleton singleton = null;
	
		public static Singleton getInstance() {
			Singleton s = singleton;
			if (singleton == null) {
				synchronized (Singleton.class) {
					s = singleton;
					if (singleton == null) {
						singleton = s = new Singleton();
					}
				}
			}
			return singleton;
		}
	}



##### 静态内部私有类（Static Private Inner Class）

	public class Singleton {
		private static class HelperSingleton {
			public static Singleton singleton = new Singleton();
		}
	
		public static Singleton getInstance() {
			return HelperSingleton.singleton;
		}
	}

##### 枚举单例模式

	public enum EnumSingle {
		INSTANCE
	}