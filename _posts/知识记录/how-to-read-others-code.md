title: "怎样阅读别人的代码？"
date: 2015-12-28 17:40:46
tags:
- IDEA
- Debug
- 阅读代码
---
### 背景:
最近接到一个任务, 在原有的项目基础上进行开发, 增加一个新功能, 有之前的类似功能对照.看起来挺简单的, 然后立刻准备开发, 但是碰壁一头雾水, 工程太复杂了, 涉及的子工程比较多, 类/配置文件等特别多.

### 简要:
然后开始从网上搜索--"怎样阅读别人的代码", 总结了下:
收集相关的资料, 阅读代码，追踪数据流，仿写，测试。

<!--more-->


### 详细步骤:
1. **收集相关背景资料。** Wiki，PPT，MRD等都是重要的相关资料，阅读代码前可以先看看这些。从这里可以了解项目的背景，目的，甚至大致的结构等。
2. **阅读代码。**阅读代码可以借助IDE，这里推荐IDEA，当然哪一个熟练用哪一个了。如果有可以运行的测试用例的话（一般都有），那就debug测试用例，追踪数据流。这里一定要有耐心，不要急着跳过，尤其是第一遍，在关键位置最好是进入方法内部看一下，在关键的位置打上断点，中间使用expression等方法，辅助debug。在debug过程中，注意阅读注释，简要记录重要的对象和方法，对象是怎么样构造的和变化的。
*这两步都是尽可能多的掌握有价值的信息，目的是为了下一步写出代码。*
3. **仿写。** 准备的这么多，终于写代码了。其实代码是不急着写的，了解清楚了，花不了多久就能写完。写的时候注意别人的规范，力求代码规范一致。
4. **测试。**编写测试用例，很可能一次通过不了，还是通过上面阅读代码时的debug方法，一点点追踪定位问题，解决了就OK了。

---
### IDEA debug技巧
主要看图，看图一目了然。
![IDEA debug面板](http://7d9owd.com1.z0.glb.clouddn.com/images/c65226c6-90aa-4b2e-b754-98778ae6bbd7.png)

断点的设定和eclipse一样，只要点一下就可以，下面是我设定的几个断点，再下面的三个窗口是用来调试代码的，这个和eclipse类似。

#### 调试常用的快捷键

	F9            resume programe 恢复程序
	Alt+F10       show execution point 显示执行断点
	F8            Step Over 相当于eclipse的f6      跳到下一步
	F7            Step Into 相当于eclipse的f5就是  进入到代码
	Alt+shift+F7  Force Step Into 这个是强制进入代码
	Shift+F8      Step Out  相当于eclipse的f8跳到下一个断点，也相当于eclipse的f7跳出函数
	Atl+F9        Run To Cursor 运行到光标处
	ctrl+shift+F9   debug运行java类
	ctrl+shift+F10  正常运行java类
	alt+F8          debug时选中查看值

来源：http://my.oschina.net/nyp/blog/383673

### 相关资料：
- [如何阅读别人的源代码？](http://blog.csdn.net/ilyfeng1314/article/details/7452326)
- [如何阅读别人的代码？](https://www.zhihu.com/question/21186887)
- [怎么阅读别人的项目代码？](http://www.cnblogs.com/snidget/articles/1951121.html)
- [mac下idea的使用之代码调试debug篇](http://ylq365.iteye.com/blog/1953854)
