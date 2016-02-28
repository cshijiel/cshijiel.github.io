title: "Java5新特征之foreach语句使用总结"
date: 2015-04-07 00:20:36
tags: 
- foreach
- java
categories: Java基础
---
### Java5新特征之foreach语句使用总结
`java.util.List`继承了`java.lang.Iterable`接口.

jdk api文档中是这样描述Iterable接口的：实现这个接口允许对象成为 "`foreach`" 语句的目标。不过咋一看Iterable接口并没啥特别之处，只是定义了一个迭代器而已。

  public interface Iterable&lt;T> {
 
    /**
    * Returns an iterator over a set of elements of type T.
    *
    * @return an Iterator.
    */
    Iterator&lt;T> iterator();
  }

究竟是如何实现foreach的呢，想想可能是编译器做了优化，就看了下最终编译成的字节码

  public class Iterable_eros {
  
 
  
 List&lt;String> strings;
 
  
 public void display(){
  
 for(String s : strings){
  
 System.out.println(s);
  
 }
  
 }
  
 
  }

相应的字节码为：

  public void display (){
  line0  : aload_0 
       getfield java.util.List my.lang.Iterable_eros.strings
       invokeinterface java.util.Iterator java.util.List.iterator() 1
       astore_2 
       goto line30
  line13  : aload_2 
       invokeinterface java.lang.Object java.util.Iterator.next() 1
       checkcast java.lang.String
       astore_1 
  line23  : getstatic java.io.PrintStream java.lang.System.out
       aload_1 
  line27  : invokevirtual void java.io.PrintStream.println(java.lang.String)
  line30  : aload_2 
       invokeinterface boolean java.util.Iterator.hasNext() 1
       ifne line13
  line39  : return 