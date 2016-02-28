title: "java中foreach与iterator迭代器"
date: 2015-04-07 00:20:36
tags: 
- java
categories: Java基础
---
Iterator迭代器是java的一个接口，这是在JDK1.8的源代码，其中包含`boolean hasNext()`，`E next()`，`default void remove()`，`default void forEachRemaining(Consumer<? super E> action)`四个方法，两个已经是default关键字修饰的实现。

    package java.util;
    
    import java.util.function.Consumer;
    
    public interface Iterator<E> {
        boolean hasNext();
    
        E next();
    
        default void remove() {
            throw new UnsupportedOperationException("remove");
        }
    
        /**
         * @since 1.8
         */
        default void forEachRemaining(Consumer<? super E> action) {
            Objects.requireNonNull(action);
            while (hasNext())
                action.accept(next());
        }
    }

>注意:在默认实现里，迭代器不支持删除。如果需要支持，需要自己实现。例如在LinkedList里边，源代码如下：

    private class ListItr implements ListIterator<E> {
        private Node<E> lastReturned;
        private Node<E> next;
        private int nextIndex;
        private int expectedModCount = modCount;

        //...

        public boolean hasNext() {
            return nextIndex < size;
        }

        public E next() {
            checkForComodification();
            if (!hasNext())
                throw new NoSuchElementException();

            lastReturned = next;
            next = next.next;
            nextIndex++;
            return lastReturned.item;
        }

        

        

        public void remove() {
            checkForComodification();
            if (lastReturned == null)
                throw new IllegalStateException();

            Node<E> lastNext = lastReturned.next;
            unlink(lastReturned);
            if (next == lastReturned)
                next = lastNext;
            else
                nextIndex--;
            lastReturned = null;
            expectedModCount++;
        }

        public void set(E e) {
            if (lastReturned == null)
                throw new IllegalStateException();
            checkForComodification();
            lastReturned.item = e;
        }

        public void add(E e) {
            checkForComodification();
            lastReturned = null;
            if (next == null)
                linkLast(e);
            else
                linkBefore(e, next);
            nextIndex++;
            expectedModCount++;
        }

        public void forEachRemaining(Consumer<? super E> action) {
            Objects.requireNonNull(action);
            while (modCount == expectedModCount && nextIndex < size) {
                action.accept(next.item);
                lastReturned = next;
                next = next.next;
                nextIndex++;
            }
            checkForComodification();
        }

        final void checkForComodification() {
            if (modCount != expectedModCount)
                throw new ConcurrentModificationException();
        }
    }

下面是网上的其他解释，更能从本质上解释原因：
Iterator 是工作在一个独立的线程中，并且拥有一个 mutex 锁。 Iterator 被创建之后会建立一个指向原来对象的单链索引表，当原来的对象数量发生变化时，这个索引表的内容不会同步改变，所以当索引指针往后移动的时候就找不到要迭代的对象，所以按照 fail-fast 原则 Iterator 会马上抛出 java.util.ConcurrentModificationException 异常。
所以 Iterator 在工作的时候是不允许被迭代的对象被改变的。但你可以使用 Iterator 本身的方法 remove() 来删除对象， Iterator.remove() 方法会在删除当前迭代对象的同时维护索引的一致性。

- [集合迭代时对集合进行修改抛ConcurrentModificationException原因的深究以及解决方案][1]



  [1]: http://blog.csdn.net/izard999/article/details/6708738