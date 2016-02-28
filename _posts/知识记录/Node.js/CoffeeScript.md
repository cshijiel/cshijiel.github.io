title: "CoffeeScript"
date: 2015-08-24 08:20:36
tags: 
- CoffeeScript
- JavaScript
categories: 知识记录
---

> CoffeeScript 是一门编译到 JavaScript 的小巧语言. 在 Java 般笨拙的外表下, JavaScript
> 其实有着一颗华丽的心脏. CoffeeScript 尝试用简洁的方式展示 JavaScript 优秀的部分.
> 
> CoffeeScript 的指导原则是: “她仅仅是 JavaScript”. 代码一一对应地编译到 JS, 不会在编译过程中进行解释.
> 已有的 JavaScript 类库可以无缝地和 CoffeeScript 搭配使用, 反之亦然. 编译后的代码是可读的, 且经过美化,
> 能在所有 JavaScript 环境中运行, 并且应该和对应手写的 JavaScript 一样快或者更快.

优点：

 1. 抛弃诞生时的C/Java语法，改用类似Ruby/Python的语法—-更适合函数式+动态语言内核。 
 2. 语法糖&ECMAScript

总结就是，屏蔽掉不优雅的地方。

<!--more-->

安装：

    sudo npm install -g coffee-script

#### 语言手册：
一些基础：CoffeeScript使用显式的空白来区分代码块，换行就可以了。不需要花括号来包裹代码块。在函数，if表达式，switch，和try/catch当中使用缩进。传入参数不需要圆括号表明函数被执行。

#### 函数

    square = (x) -> x * x
    cube   = (x) -> square(x) * x

带有默认值参数的函数

    fill = (container, liquid = "coffee") ->
      "Filling the #{container} with #{liquid}..."
      
编译后

    var fill;
    
    fill = function(container, liquid) {
      if (liquid == null) {
        liquid = "coffee";
      }
      return "Filling the " + container + " with " + liquid + "...";
    };      
    
#### 对象和数组

    song = ["do", "re", "mi", "fa", "so"]
    
    singers = {Jagger: "Rock", Elvis: "Roll"}
    
    bitlist = [
      1, 0, 1
      0, 0, 1
      1, 1, 0
    ]
    
    kids =
      brother:
        name: "Max"
        age:  11
      sister:
        name: "Ida"
        age:  9
        
#### if, else, unless 和条件赋值

    mood = greatlyImproved if singing
    
    if happy and knowsIt
      clapsHands()
      chaChaCha()
    else
      showIt()
    
    date = if friday then sue else jill

注意if and unless 意义的变化。

>待完成。