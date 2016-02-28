title: "JDK1.8 中 接口和抽象类的异同"
date: 2015-04-07 00:20:36
tags: 
- Java8
- 翻译
- 接口
- 抽象类
categories: Java基础
---
在最新版本Java1.8中的变化里， Stephen Colebourne 告诉Hartmut Schlosser，“我认为最重要的语言上的改变不是Lambdas表达式，而是在接口上的静态和默认方法”。ColeBoune补充道，“默认方法功能的增加减少了许多使用抽象类的原因”。当我读到这里时，我意识到Colebourne是正确的，而且我通常用抽象类的许多场景可以通过JDK1.8的默认方法（default method）替换为JDK1.8接口。

在网上论坛和博客中讨论Java接口和抽象类异同的例子不胜枚举。这些讨论包括但不限于， [JavaWorld 's Abstract classes vs. interfaces][1] , StackOverflow 's [When do I have to use interfaces instead of abstract classes][2]? , [Difference Between Interface and Abstract Class][3] , 10 [Abstract Class and Interface Interview Questions Answers in Java][4] , 作为曾经有用的信息，许多现在已经过时了，可能会成为让Java体验更加混乱的部分。

在我思考Java1.8中接口和抽象类尚存在的不同，我决定去看看[Java Tutorial][5]关于这部分说过的信息。这个引导（Tutorial）已经更新到JDK 8 而且关于抽象方法和类的讨论有一个章节，称作 抽象类 vs 接口。这个章节指出在JDK 8 中接口和抽象类的异同点。着重强调的不同点是数据成员和方法的作用域（访问权限）：

- 抽象类允许非静态和非final的域，允许方法成为public,static和final
- 所有的接口方法本质为public的。

在Java Tutorial接着列出了许多应该考虑抽象类的场景，和许多应该考虑接口的场景。不出意料，这些源于前面提到的不同、分歧，主要处理你是否**需要域和方法成为private,protected,non-static,或者 not-final（偏向抽象类）**，或者你需要**把重点放在类型而不是考虑实现的能力（偏向接口）**。

因为Java语序一个类实现多个接口但只能继承一个类，接口可能被视为有利于多继承的实现。多亏JDK 8 的默认方法，这些接口甚至能提供默认的实现。

那么，一个很自然的问题是：当一个类实现了两个接口，而这两个接口描述了一个具有相同特点的默认方法，Java怎么处理。答案是，这会报一个编译错误。这是一个屏幕截图，NetBeans8上运行报的这个错误。

![编译报错][6]

### 结论

JDK 8 争议性的把抽象类的优点带给接口。造成的影响是今天有大量的抽象类通过默认方法可能被替换为接口。


> 翻译文章来源： [Abstract Class Versus Interface in the JDK 8 Era][7]


 [1]: http://www.javaworld.com/article/2077421/learn-java/abstract-classes-vs-interfaces.html&amp;usg=ALkJrhhe1JDAtAAePiPSeFh1a_fDBKdqQg
 [2]: http://stackoverflow.com/questions/16781329/when-do-i-have-to-use-interfaces-instead-of-abstract-classes
 [3]: http://javapapers.com/core-java/abstract-and-interface-core-java-2/difference-between-a-java-interface-and-a-java-abstract-class/&amp;usg=ALkJrhgKQek5dM2xZ98sZRLN8Csq6zJ_bQ
 [4]: http://javarevisited.blogspot.com/2013/04/10-abstract-class-and-interface-interview-question-java-answers.html&amp;usg=ALkJrhiFXycVbilpdqLhlKeppz5q2_eHdA
 [5]: http://docs.oracle.com/javase/tutorial/&amp;usg=ALkJrhj-rqreHzT3CKkBki9MFnUE9WRFwg
 [6]: http://a3ab771892fd198a96736e50.javacodegeeks.netdna-cdn.com/wp-content/uploads/2014/03/netBeans8CompilerErrorMultipleInterfacesSameDefaultMethodSignatures.png
 [7]: http://www.javacodegeeks.com/2014/04/abstract-class-versus-interface-in-the-jdk-8-era.html