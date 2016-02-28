title: "JavaScript 中的回调函数"
date: 2015-04-07 00:20:36
tags: 
- Node
categories: 知识记录
---
在Node.js中使用异步的方式读取一个文件：

    /**
     * Created by chen on 1月14日.
     */
    var fs = require('fs');
    fs.readFile('readfile.js','utf-8',function(err,data){
        if(err){
            console.error(err);
        }else{
            console.log(data);
        }
        console.log('21212123.')
    });
    console.log('end.');
    
运行结果如下：

    end.
    /**
     * Created by chen on 1月14日.
     */
    ....

>基于多线程的模型也有相应的解决方案，如轻量级线程（`lightweight thread`）等。事件驱动的单线程异步模型与多线程同步模型到底谁更好是一件非常有争议的事情，因为尽管消耗资源，后者的吞吐率并不比前者低。

Node.js也提供了同步读取文件的API：

    /**
     * Created by chen on 1月14日.
     */
    
    /***另一种的读取方式***/
    //readfilesync.js
    
    var fs = require('fs');
    var data =  fs.readFileSync('index.js','utf-8');
    console.log(data);
    console.log('end.');

运行结果与前面不同，如下所示：

    $ node readfilesync.js
    Contents of the file.
    end.
    
同步读取文件的方式比较容易理解，将文件名作为参数

同步式读取文件的方式比较容易理解，将文件名作为参数传入 `fs.readFileSync` 函数，阻塞等待读取完成后，将文件的内容作为函数的返回值赋给 `data` 变量，接下来控制台
输出 `data` 的值，最后输出 `end.` 。
异步式读取文件就稍微有些违反直觉了， `end.` 先被输出。要想理解结果，我们必须先知道在 `Node.js` 中，异步式 `I/O` 是通过回调函数来实现的。 `fs.readFile` 接收了三个参数，第一个是文件名，第二个是编码方式，第三个是一个函数，我们称这个函数为**回调函数**。
`JavaScript` 支持匿名的函数定义方式，譬如我们例子中回调函数的定义就是嵌套在 `fs.readFile` 的参数表中的。这种定义方式在 `JavaScript` 程序中极为普遍。

`fs.readFile` 调用时所做的工作只是将异步式 `I/O` 请求发送给了操作系统，然后立即返回并执行后面的语句，执行完以后进入事件循环监听事件。当 `fs` 接收到 `I/O` 请求完成的
事件时，事件循环会主动调用回调函数以完成后续工作。因此我们会先看到 `end.`，再看到
`file.txt` 文件的内容。