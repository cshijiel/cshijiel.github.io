title: 模板方法模式--设计模式
date:  2016-08-22 19:48:22
tags:
- java
- 设计模式

categories: Java基础

---

### 模板方法概述

在一个复杂的系统里，有很多类似的业务。整体业务有很多步骤，例如产品内部运营系统，包括销售创建、运营审核上线、商户推广等步骤。在很多业务中，有一些步骤是相同的，有一些步骤是不同的。例如销售创建部分，创建的内容类型等可能有很多种，而审核和推广则是相同的。

模板方法适合解决这种场景的问题。模板中定义一个流程的骨架，具体的差异化的步骤在子类中实现。通常基类中的某些步骤是抽象的。

定义如下：
> 模板方法模式：定义一个操作中算法的框架，而将一些步骤延迟到子类中。模板方法模式使得子类可以不改变一个算法的结构即可重定义该算法的某些特定步骤。



> Template Method Pattern:  Define the skeleton of an algorithm in an  operation, deferring some steps to subclasses. Template Method lets  subclasses redefine certain steps of an algorithm without changing the  algorithm's structure.

### 模式中的角色

1. 抽象类(基类，AbstractClass): 定义模板方法，定义算法骨架（包括业务流程信息等）。
2. 具体类(SubClass): 实现抽象类中的抽象方法，通常是差异化业务部分。

### 模式的结构和实现

#### 模式结构

模板方法模式结构比较简单，其核心是抽象类和其中的模板方法的设计：

![模板方法模式典型的结构](http://cshijiel.qiniudn.com/16-8-22/27086173.jpg)



其中，`AbstractFlow`是角色中的抽象类，`AbstractFlowImpl`和`AnotherAbstractFlowImpl`是具体类。



<!—more—>



### 模式实现

#### 抽象类

在这个抽象类中定义了总的流程信息方法，这个方法中包括一些基本步骤，这些基本步骤也是一些方法。有些方法在不同的业务中是不变的，可以在这个抽象类中直接实现；有的方法随不同的业务不同而不同，这些方法可以有一个默认实现，还可以直接定义成抽象方法（Abstract method）,强制要求在子类实现，建议使用抽象方法。



在访问控制方面，通常流程信息方法是**public**的，具体的基本步骤是**protect**的。

```java
package com.roc.TemplatePattern;

/**
 * 抽象基类,定义了一些没有实现的细节步骤和共同的流程信息
 *
 * @author Roc
 * @title DesignPatterns
 * @date 16/8/22
 */
public abstract class AbstractFlow {

    // 延迟到子类实现的一个步骤, 权限访问符是protected,不对外公开,可被子类修改.
    // abstract强制子类实现
    protected abstract void step1();

    protected abstract int step2();

    // hook,钩子,可以做一些额外的处理.通常基类有一个空实现.不是必须的.
    protected void hook(String msg) {
        System.out.println("AbstractFlow#" + msg);
    }

    // 对外公开的执行方法,里面是流程详细步骤的组合.
    public void exe() {
        System.out.println("do some thing");
        step1();
        step2();
        hook("Hello");
        System.out.println("do some thing finish!");
    }
}
```

在抽象类中，模板方法exe()定义了算法的框架，在模板方法中调用基本方法以实现完整的算。其中step1()和step2()都是抽象的，需要子类实现。其中hook()方法可以做一些额外的事情，如果整个流程还不足以完成整体业务，则可以在子类中覆盖hook()进行补充。

#### 实现类：

实现类对基类中的抽象方法和业务特定的方法进行实现 or 重写。



```java
package com.roc.TemplatePattern;

/**
 * 一个实现类,实现需要差异化的细节步骤
 *
 * @author Roc
 * @title DesignPatterns
 * @date 16/8/22
 */
public class AbstractFlowImpl extends AbstractFlow {

    @Override
    protected void step1() {
        System.out.println("AbstractFlowImpl1#step1");

    }

    @Override
    protected int step2() {
        System.out.println("AbstractFlowImpl1#step2");
        return 0;
    }

    // 可以覆盖,但不是必须的.
    @Override
    protected void hook(String msg) {
        System.out.println("AbstractFlowImpl1#" + msg);
    }
}
```



### 简单测试

测试类如下：

```java
package com.roc.TemplatePattern;

/**
 * 执行入口
 *
 * @author Roc
 * @title DesignPatterns
 * @date 16/8/22
 */
public class Client {
    public static void main(String[] as) {
        AbstractFlow flow = new AbstractFlowImpl();
        flow.exe();

        System.out.println("\n==========================\n");

        flow = new AnotherAbstractFlowImpl();
        flow.exe();
    }
}
```

其中`AnotherAbstractFlowImpl`是类似`AbstractFlowImpl`的一个实现类。



```java
package com.roc.TemplatePattern;

/**
 * 另一个实现类,实现需要差异化的细节步骤
 *
 * @author Roc
 * @title DesignPatterns
 * @date 16/8/22
 */
public class AnotherAbstractFlowImpl extends AbstractFlow {
    @Override
    protected void step1() {
        System.out.println("AnotherAbstractFlowImpl#step1");

    }

    @Override
    protected int step2() {
        System.out.println("AnotherAbstractFlowImpl#step2");
        return 0;
    }
}
```



执行结果：

```json
do some thing
AbstractFlowImpl1#step1
AbstractFlowImpl1#step2
AbstractFlowImpl1#Hello
do some thing finish!

==========================

do some thing
AnotherAbstractFlowImpl#step1
AnotherAbstractFlowImpl#step2
AbstractFlow#Hello
do some thing finish!
```



### 相关代码

https://github.com/cshijiel/DesignPatterns



### 参考信息

http://meigesir.iteye.com/blog/1506484

http://blog.csdn.net/lovelion/article/details/8299794

http://design-patterns.readthedocs.io/zh_CN/latest/behavioral_patterns/strategy.html

https://www.gitbook.com/book/quanke/design-pattern-java/details

http://blog.csdn.net/yanbober/article/details/45501715









---

##### 一个小问题：

如果将基类AbstractFlow#exe方法的hook方法调用改成this.hook("Hello")，那么结果会是怎样显示呢？

![code diff](http://cshijiel.qiniudn.com/16-8-22/63797067.jpg)



