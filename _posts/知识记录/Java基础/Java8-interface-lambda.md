title: "Java 8 中的函数式接口与Lambda表达式"
date: 2015-10-01 14:21:44
tags: 
- java
- interface
- lambda
- 函数式编程
categories: Java基础
---
### 函数式接口（Functional Interface）
函数式接口是Java 8 对一类特殊类型的接口的称呼。

### Java 8 之前"已有"的函数式接口

	java.lang.Runnable
	java.util.concurrent.Callable
	java.security.PrivilegedAction
	java.util.Comparator
	java.io.FileFilter
	java.io.file.pathMatcher
	...

<!--more-->

### 新定义的函数式接口

`java.util.function`中定义了几组类型的函数式接口以及针对基本数据类型的子接口：

	Predicate
	传入一个参数，返回一个bool结果，方法为boolean test(T t)
	
	Consumer
	传入一个参数，无返回值，纯消费。方法为 void accept(T t)
	
	Function
	传入一个参数，返回一个结果，方法为R apply(T t)
	
	Supplier
	无参数传入，返回一个结果，方法为T get()

这是`java.util.function`下的所有函数式接口，可以根据名称看到大致的用途：
![java.util.function下的所有函数式接口](http://7d9owd.com1.z0.glb.clouddn.com/images/95484557-79ee-4ac6-b453-f4b61786f1c1.png)

可以根据需要使用。



**`Predicate`的用法：**

	import java.util.function.Predicate;

	/**
	 * Created by Roc on 15/9/30.
	 */
	public class Kik {
		public static void main(String[] args) {
			System.out.println("TestPredicate");
			String input = "test";
			testFunction((string) -> input.equals(string));
		}


		public static void testFunction(Predicate predicate) {
			System.out.println(predicate.test("test"));
		}
	}



>**双冒号调用**：静态调用，适合简单函数体。



