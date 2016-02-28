title: "Java读取文本文件"
date: 2015-04-07 00:20:36
tags: 
- java
- 读取文件
categories: Java基础
---
Java IO系统里读写文件使用Reader和Writer两个抽象类，Reader中read()和close()方法都是抽象方法。Writer中 write()，flush()和close()方法为抽象方法。子类应该分别实现他们。
Java IO已经为我们提供了三个方便的Reader的实现类，FileReader,InputStreamReader和BufferedReader。其中最重要的类是InputStreamReader， 它是字节转换为字符的桥梁。你可以在构造器重指定编码的方式，如果不指定的话将采用底层操作系统的默认编码方式，例如`GBK`等。

### FileReader读txt文件例子

    @Test
    public void readByFileReader() throws IOException {
        try (FileReader reader = new FileReader(file)) {
            int ch = 0;
            while ((ch = reader.read()) != -1) {
                System.out.print((char) ch);
            }
        }
    }

其中read()方法返回的是读取得下个字符。
读取 3608 行数据耗时 **2.075**s

### InputStreamReader读txt文件例子

    @Test
    public void readByBufferedReader() throws IOException {
        try (BufferedReader bufferedReader = new BufferedReader(new FileReader(
                file))) {
            String lineTxt = null;
            while ((lineTxt = bufferedReader.readLine()) != null) {
                System.out.println(lineTxt);
            }
        }
    }
读取 3608 行数据耗时 **0.174**s