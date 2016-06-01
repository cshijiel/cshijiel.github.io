title: "Mockito初使用"
date: 2016-06-01 17:07:21
tags: 
- Test
- Mockito
- java
categories: 知识记录
---


作为一个研发工程师，自己的代码怎么可以不测试呢？

但是自己跑单测的时候，有的时候某些RPC服务不好配置 or 无线下测试环境，所以我们需要对这些难以实例化等的对象进行Mock（adj. 模拟的；仿制的；虚假的；不真实的）。



**这里我们使用Mockito。**



## Mockito简介

> Mockito is a mocking framework that tastes really good. It lets you write beautiful tests with a clean & simple API. Mockito doesn’t give you hangover because the tests are very readable and they produce clean verification errors. Read more about [features & motivations](https://github.com/mockito/mockito/wiki/Features-And-Motivations).

Mockito 是一个模拟框架，用起来非常好用。它允许你用干净简单(too simple)的API写漂亮的测试方法。Mockito不会给你遗留物(不侵入源代码，不造成别的影响的意思吧)，因为这些测试可读性非常好，产生的可验证的错误也非常简洁。阅读更多信息参考上述链接。

- Mockito是StackOverflow社区投票选出的最好的Java Mock 框架
- 不仅仅在策划工具中，在所有的类库中，Mockito也是排名前十的Java类库。



<!—more—>



## 简单配置

### maven

```xml
<!-- http://mvnrepository.com/artifact/org.mockito/mockito-all -->
<dependency>
    <groupId>org.mockito</groupId>
    <artifactId>mockito-all</artifactId>
    <version>1.10.19</version>
</dependency>
```



最新版本`2.0.2-beta`。

使用Gradle的同学可以自行搜索。



## 简单使用

添加完上述依赖后，就可以简单使用了



验证交互(调用方法等)

```java
import static org.mockito.Mockito.*;

// mock creation
List mockedList = mock(List.class);

// using mock object - it does not throw any "unexpected interaction" exception
mockedList.add("one");
mockedList.clear();

// selective, explicit, highly readable verification
verify(mockedList).add("one");
verify(mockedList).clear();
```



设置一个期望行为的调用

```java
// you can mock concrete classes, not only interfaces
LinkedList mockedList = mock(LinkedList.class);

// stubbing appears before the actual execution
when(mockedList.get(0)).thenReturn("first");

// the following prints "first"
System.out.println(mockedList.get(0));

// the following prints "null" because get(999) was not stubbed
System.out.println(mockedList.get(999));
```



## 常用/主要参考

- `mock()`/`@Mock`:创建一个Mock对象
  - 通过[`Answer`](http://mockito.github.io/mockito/docs/current/org/mockito/Mockito.html#field_summary)/[`ReturnValues`](http://mockito.github.io/mockito/docs/current/org/mockito/ReturnValues.html)/[`MockSettings`](http://mockito.github.io/mockito/docs/current/org/mockito/MockSettings.html) 随意指定这个对象怎样表现
  - 如果一个对象提供的行为（返回值等）不符合你的需求，可以自己写一个对Answer接口的实现
- [`spy()`](http://mockito.github.io/mockito/docs/current/org/mockito/Mockito.html#spy(T))/[`@Spy`](http://mockito.github.io/mockito/docs/current/org/mockito/Spy.html): 部分模拟，真实的方法被调用了，但是仍可以被验证和修改行为



## 自己的实践：数据源返回值的修改

在目前开发业务中，需要通过Atom组件根据id获取对应的字面，如果字面以"$_"开头，则执行其他逻辑。



### 构造一个Mock对象

此处的Atom组件数据源不可控，但可以在单测中使用，于是通过Spy(傀儡式mock)来修改对象返回值。



```java
// 从Spring Context中拿到需要spy的对象
AtomDriver atomDriver = this.applicationContext.getBean(atomDriverBeanName, AtomDriver.class);

final Map<Long, String> mapResult = new HashMap<Long, String>();
mapResult.put(wordid, "$_cxvafdslakjf");

// 以atomDriver为傀儡，在此基础上修改其返回值
AtomDriver spy = spy(atomDriver);
doAnswer(new Answer() {
  @Override
  public Object answer(InvocationOnMock invocationOnMock) throws Throwable {
    Map<Long, String> result = (Map<Long, String>) invocationOnMock.callRealMethod();
    result.putAll(mapResult);
    return result;
  }
}).when(spy).queryWordNameById(Matchers.anyCollection());
```



当然有人说，可以直接在返回值AtomDriver.queryWordNameById()那里修改 or 再包装一层，也是可以的，**但是使用Mockito可以做到无侵入**。





### 将Mock注入到Spring Context中

这时，就可以将mock对象交给Spring管理了，使用mock对象替换原来Spring管理的对象，可以做到自动使用mock对象装配到需要的地方。**有一个需要注意的地方，构造对象的scope如果不是单例模式，可能无效，待验证！**



注入到Spring容器的代码：

```java
// inject into spring context
ConfigurableApplicationContext applicationContext = (ConfigurableApplicationContext) this.applicationContext;
DefaultListableBeanFactory beanFactory = (DefaultListableBeanFactory) applicationContext.getBeanFactory();
if (beanFactory.containsBean(atomDriverBeanName)) {
  beanFactory.destroySingleton(atomDriverBeanName);
}
beanFactory.registerSingleton(atomDriverBeanName, spy); // spy为mock对象
```



**有更好的实践欢迎一起讨论。**
