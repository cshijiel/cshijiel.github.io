title: "YAML格式简介"
date: 2015-04-07 00:20:36
tags: 
- YAML
categories: 知识记录
---
## 1. 概念
YAML是一种人们可以轻松阅读的数据序列化格式，并且它非常适合对动态编程语言中使用的数据类型进行编码。YAML是`YAML Ain't Markup Language`简写，和GNU("GNU's Not Unix!")一样，YAML是一个递归着说“不”的名字。不同的是，GNU对UNIX说不，YAML说不的对象是XML。YAML不是XML。它可以用作数据序列，配置文件，log文件，Internat信息和过滤。

## 2. 优点
- YAML的可读性好
- YAML和脚本语言的交互性好
- YAML使用实现语言的数据类型
- YAML有一个一致的信息模型
- YAML易于实现

## 3. YAML的适用范围
由于实现简单，解析成本很低，YAML特别适合在脚本语言中使用。列一下现有的语言实现：Ruby，Java，Perl，Python，PHP，OCaml，JavaScript。除了Java，其他都是脚本语言.
YAML比较适合做序列化。因为它是宿主语言数据类型直转的。
YAML做配置文件也不错。比如Ruby on Rails的配置就选用的YAML。对ROR而言，这很自然，也很省事。 
**由于兼容性问题，不同语言间的数据流转建议现在不要用YAML.**

## 4. YAML不足
YAML和XML不同，没有自己的数据类型的定义，而是使用实现语言的数据类型。这一点，有可能是出奇制胜的地方，也可能是一个败笔。如果兼容性保证的不好的话，YAML数据在不同语言间流转会有问题。
假如兼容性没问题的话，YAML就太完美了。**轻巧，敏捷，高效，简便，通用**。

## 5. JYaml简介
**`JYAML`**是`YAML`的`Java`实现。
YAML使用实现语言的数据类型。我们看一下一些JYaml支持的Java数据类型：
原始数据和封装类（比如int，java.lang.Integer）
JavaBean兼容对象（Structure支持）
Collection （sequence支持）
List
Set
Map （map支持）
Arrays （sequence支持）
BigInteger 和BigDecimal
Date
注：我把我个人认为较好的文章推荐如下，方便我们共同学习和交流。
参考文献：
1 《YAML 简介》 http://www.ibm.com/developerworks/cn/xml/x-cn-yamlintro/index.html
2 《YAML 对 XML 的改进》http://www.ibm.com/developerworks/cn/xml/x-matters/part23/
3 http://jyaml.sourceforge.net/
4 http://www.yaml.org/