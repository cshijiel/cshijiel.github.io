title: "spring下应用@Resource, @Autowired 和 @Inject注解进行依赖注入的差别"
date: 2015-04-21 17:20:08
tags:
- Spring
- java
categories: 知识记录
---


## Overview

When using Spring there's the choice of 3 annotations for injecting resources with advantages and disadvantages : 
`@Autowired `
The original, "proprietary" annotation from Spring. 
- not standard, Spring proprietary 
+ resolves by type 
+ for optional dependencies  
`@Resource `
The  `JSR-250` annotation 
+ standard dependency injection 
- resolves by name, fallback by type 
- not for optional dependencies  
`@Inject `
The  `JSR-330` annotation 
+ resolves by type 
+ standard dependency injection 
- not for optional dependencies 

<!--more-->

---

## 总结
 `@Autowired` 和`@Inject`的报错信息完全相同，他们都是通过`AutowiredAnnotationBeanPostProcessor` 类实现的依赖注入，二者具有可互换性。 `@Resource`通过 `CommonAnnotationBeanPostProcessor` 类实现依赖注入，即便如此他们在依赖注入时的表现还是极为相近的，以下是他们在实现依赖注入时执行顺序的概括：

@Autowired and @Inject

- Matches by Type
- Restricts by Qualifiers
- Matches by Name

@Resource

- Matches by Name
- Matches by Type
- Restricts by Qualifiers (ignored if match is found by name)

‘@Resource’在依据name注入的时候速度性能表现的比 ‘@Autowired’ 和‘@Inject’优越，但这是微不足道的，不足以作为优先选择 ‘@Resource’的原因。我倾向于使用 ‘@Resource’是因为它配置起来更简洁。
```
@Resource(name="person")
@Autowired @Qualifier("person")
@Inject @Qualifier("person")
```
你也许会说使用字段  默认  名称作为注入时候的bean name，其他两种方式就会一样简洁：
```
@Resource private Party person;
@Autowired private Party person;
@Inject private Party person;
```
确实如此。但是当你需要重构代码的时候又如何呢？使用’@Resource‘方式只需简单修改name属性即可，而无需触及注入Bean的名称(注入Bean的时候同意使用接口名称)。所以我建议使用注解方式实现注入的时候遵循以下语法风格：

1. 在你的组件中明确限定bean名称而不是使用默认值[@Component("beanName")]。

2. 同时使用’@Resource‘和它的’name'属性 [@Resource(name="beanName")]。

3. 避免使用‘@Qualifier’注解，除非你要创建一系列类似beans的集合。例如，你也许需要建立一个set集合来存放一系列“规则”定义。这个时候可以选择‘@Qualifier'注解方式。这种方式使得将大量遵循相同规则的类放入集合中变得容易。

4. 使用如下配置限定需要尽心组件扫描的包： [context:component-scan base-package="com.sourceallies.person"]。这样做可以减小spring扫描很多无效的包的情况。

遵循以上原则能增强你的，注解风格的，spring配置的可读性和稳定性。 

[资料来源][1]


  [1]: http://my.oschina.net/swearyd7/blog/297681