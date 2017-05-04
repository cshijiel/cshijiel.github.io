title: "使用Optional处理NullPointerException"
date: 2017-01-04 21:29:19
tags:
- Optional
- NullPointerException
categories: Java基础
---
## 一、 Java中的null

在java中，使用`null`表示引用的默认值(`null`是Java中的关键字)。引用在声明的时候不指向任何实例，此时值为null。

```java
Object o1; // null
Object o2 = null; // null
Object o3 = new Object(); // not null
```

除此之外，在找不到对应资源的时候，除了抛异常，还可能返回`null`。

例如在`java.util.HashMap#get`中，

```java
// 在没有输入key对应的value时，返回null
public V get(Object key) {
    Node<K,V> e;
    return (e = getNode(hash(key), key)) == null ? null : e.value;
}
```

**了解更多**

> 关于`null`的更多信息：[Java中有关Null的9件事](http://www.importnew.com/14229.html)

综上几点，可以说`null`无处不在，因此导致到处需要判空操作 or 处理空指针异常（NullPointerException）。

<!--more-->

## 二、 Optional是怎样处理null值的？

业务中，我们可能有这样的场景：
一个Map：
```java
Map<String, Object> subMap = new HashMap<>();
subMap.put("subkey", "subValue");
Map<String, Map> map = new HashMap<>();
map.put("key", subMap);
```
然后获取嵌套Map中的某个值：

```java
Object res = null;
if (map.containsKey("key")) {
    Map valueMap = map.get("key");
    if (valueMap.containsKey("subkey")) {
        res = valueMap.get("subkey");
    }
}
if (res == null) {
    res = "Empty";
}
System.out.println(res);
```

Or

```java
try {
    res = map.get("key").get("subkey");
} catch (Exception e) {
    res = "Empty";
}
```

总是不如Groovy等一些编程语言自带的安全的属性/方法访问操作符。

`user?.getUsername()?.toUpperCase();`

使用Optional处理NullPointerException，可以达到类似的效果：

```java
Object res = optional.map(o -> o.get("key"))
        .map(o -> {
            Map omap = ((Map) o);
            return omap.get("subkey");
        }).orElse("Empty");
System.out.println(res);
```

到此为止，最有用的特性之一已经讲完了。

## 三、 Optional其他方法

![Optional全部方法](http://cshijiel.qiniudn.com/17-1-4/18091537-file_1483533846622_ecc7.png)

除`java.util.Optional#map`之外，还有`java.util.Optional#flatMap`。
