title: "shell-export"
date: 2015-04-07 00:20:36
tags: 
- shell
- export
categories: Linux基础
---
在shell中，有的时候会重用一部分代码，例如变量或者函数等，就会用到export。

使用例子如下：
首先是父进程：`father.sh`

    #! /bin/bash
    echo "this is the father"
    FILM="A Few Good Men"
    echo "I like the film : $FILM"
    
    tick()
    {
            echo $1;
            return $1;
    }
    
    tick fdjk
    
    #call the child script
    export FILM
    export -f tick
    ./child.sh
    echo "back to father"
    echo "and the film is : $FILM"
    exit
  
然后是子进程：child.sh  

    #! /bin/bash
    echo "called from father...i am the child"
    echo "filem name is : $FILM"
    FILM="Die Hard"
    echo "changing film to :$FILM"
    
    tick fdafds
    
    exit



----


## shell 与 export命令

用户登录到Linux系统后，系统将启动一个用户shell。在这个shell中，可以使用shell命令

或声明变量，也可以创建并运行shell脚本程序。运行shell脚本程序时，系统将创建一个子shell。

此时，系统中将有两个shell，一个是登录时系统启动的shell，另一个是系统为运行脚本程序创建

的shell。当一个脚本程序运行完毕，脚本shell将终止，返回到执行该脚本之前的shell。

 

从这种意义上来说，用户可以有许多 shell，每个shell都是由某个shell（称为父shell）派生的。

在子shell中定义的变量只在该子shell内有效。如果在一个shell脚本程序中定义了一个变量，

当该脚本程序运行时，这个定义的变量只是该脚本程序内的一个局部变量，其他的shell不能引用它，

要使某个变量的值可以在其他shell中被改变，可以使用export命令对已定义的变量进行输出。

 

export命令将使系统在创建每一个新的shell时，定义这个变量的一个拷贝。

这个过程称之为变量输出。


## export 功能说明：设置或显示环境变量。
**语　　法**：export [-fnp][变量名称]=[变量设置值]
**补充说明**：在shell中执行程序时，shell会提供一组环境变量。export可新增，修改或删除环境变量，供后续执行的程序使用。export的效力仅限于该次登陆操作。
**参　　数**：
　`-f` 　代表[变量名称]中为函数名称。
　`-n` 　删除指定的变量。变量实际上并未删除，只是不会输出到后续指令的执行环境中。
　`-p` 　列出所有的shell赋予程序的环境变量。


----

## 其他
shell返回值只能是整数型数字，上边的例子会报错：

    return: fdafds: numeric argument required
    
参考信息：[Shell 函数返回值][1]


  [1]: http://blog.csdn.net/yasi_xi/article/details/8546758