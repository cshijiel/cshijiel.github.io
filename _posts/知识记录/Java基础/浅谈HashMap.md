title: "浅谈HashMap"
date: 2015-07-21 08:13:30
tags: 
- java
- HashMap
- 数据结构
categories: Java基础
---
导言：

数组在O(1)时间复杂度可以定位元素(读)，但是随机插入元素(写)效率比较低。而链表则相反，随机插入元素效率很高，但是随机读取效率低。

那有没有，读取和插入效率都很高的数据结构呢？

答案当然是有，HashMap就是一种读取和插入效率都很高的数据结构。

### 基础特性
HashMap的几个必要的基础特性：

 - hashCode()
 - KV键值对
 - 数组为基础，辅助以链表的数据结构

因为是散列函数，实际的键在数组中是分散存在的，占有一定的比例，如果比例高的时候容易出现键一样值不一样的情况（在该处形成链表），所以有初始容量和加载因子两个参数。

通常，默认加载因子是0.75，这是在时间和空间上的一种折中。

<!--more-->

### 存储节点
HashMap中的Key-Value都是存储在Entry数组中的。

    static class Entry<K,V> implements Map.Entry<K,V> {
        final K key;
        V value;
        // 指向下一个节点
        Entry<K,V> next;
        final int hash;
    
        //...other
    }
    

### 常用用法

    clear()--清空HashMap
    boolean containsKey(Object key)
    boolean containsValue(Object value)
    getEntry(Object key)
    
    entrySet()/values()/keySet()
    
    get(Object key)
    put(Object key,Object value)



### 新的理解和问题
HashMap是一个数组，每个元素都是一个链表。由链表组成的数组。
HashMap属于Key-Value型数据结构。Key与元素在数组中的位置有关，插入顺序与元素在链表中的位置有关。引出问题：从Key到数组序号是怎么映射的，有哪些问题（怎样减少重复而且均匀、重复之后怎么办）被引入，又是怎么解决的。关键词：均匀不重复、Key+key.hashCode()查找元素。
HashMap的基础是数组，怎样扩容，扩容是的元素重排是怎样的。
