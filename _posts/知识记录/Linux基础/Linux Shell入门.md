title: "Linux Shell入门"
date: 2015-04-07 00:20:36
tags: 
- Linux
- Shell
categories: Linux基础
---
## Shell简介：什么是Shell，Shell命令的两种执行方式

Shell本身是一个用C语言编写的程序，它是用户使用Unix/Linux的桥梁，用户的大部分工作都是通过Shell完成的。Shell既是一种命令语言，又是一种程序设计语言。

可以说，shell使用的熟练程度反映了用户对Unix/Linux使用的熟练程度。

**Shell有两种执行命令的方式：**

- 交互式（Interactive）：解释执行用户的命令，用户输入一条命令，Shell就解释执行一条。
- 批处理（Batch）：用户事先写一个Shell脚本(Script)，其中有很多条命令，让Shell一次把这些命令执行完，而不必一条一条地敲命令。

Shell脚本和编程语言很相似，**也有变量和流程控制语句**，但Shell脚本是解释执行的，不需要编译，Shell程序从脚本中一行一行读取并执行这些命令，相当于一个用户把脚本中的命令一行一行敲到Shell提示符下执行。

>*Shell初学者请注意，在平常应用中，建议不要用 root 帐号运行 Shell 。作为普通用户，不管您有意还是无意，都无法破坏系统；但如果是 root，那就不同了，只要敲几个字母，就可能导致灾难性后果。*

## 几种常见的Shell
Unix/Linux上常见的Shell脚本解释器有bash、sh、csh、ksh等，习惯上把它们称作一种Shell。我们常说有多少种Shell，其实说的是Shell脚本解释器。

其中：`sh`
sh 由Steve Bourne开发，是Bourne Shell的缩写，sh 是Unix 标准默认的shell。

第一个Shell脚本（Shell的`HelloWorld`）
打开文本编辑器，新建一个文件，扩展名为sh（sh代表shell），扩展名并不影响脚本执行，见名知意就好，如果你用php写shell 脚本，扩展名就用php好了。

输入一些代码：

    #!/bin/bash
    echo "Hello World !"
    
“`#!`” 是一个约定的标记，它告诉系统这个脚本需要什么解释器来执行，即使用哪一种Shell。echo命令用于向窗口输出文本。

**作为可执行程序**

将上面的代码保存为test.sh，并 cd 到相应目录：

    chmod +x ./test.sh  #使脚本具有执行权限
    ./test.sh  #执行脚本
注意，一定要写成`./test.sh`，而不是test.sh。运行其它二进制的程序也一样，直接写test.sh，linux系统会去PATH里寻找有没有叫test.sh的，而只有/bin, /sbin, /usr/bin，/usr/sbin等在PATH里，你的当前目录通常不在PATH里，所以写成test.sh是会找不到命令的，**要用./test.sh告诉系统说，就在当前目录找。**

通过这种方式运行bash脚本，第一行一定要写对，好让系统查找到正确的解释器。

这里的"系统"，其实就是shell这个应用程序（想象一下Windows Explorer），但我故意写成系统，是方便理解，既然这个系统就是指shell，那么一个使用/bin/sh作为解释器的脚本是不是可以省去第一行呢？是的。

再看一个例子。下面的脚本使用 **read** 命令从 stdin 获取输入并赋值给 PERSON 变量，最后在 stdout 上输出：

    #!/bin/bash
    # Author : mozhiyan
    # Copyright (c) http://see.xidian.edu.cn/cpp/linux/
    # Script follows here:
    echo "What is your name?"
    read PERSON
    echo "Hello, $PERSON"
    
运行脚本：

    chmod +x ./test.sh
    $./test.sh
    What is your name?
    mozhiyan
    Hello, mozhiyan
    $
    
## Shell变量：Shell变量的定义、删除变量、只读变量、变量类型

**定义变量**

定义变量时，变量名不加美元符号（$），如：

    variableName="value"
注意，变量名和等号之间不能有空格，这可能和你熟悉的所有编程语言都不一样。同时，变量名的命名须遵循如下规则：

- 首个字符必须为字母（a-z，A-Z）。
- 中间不能有空格，可以使用下划线（_）。
- 不能使用标点符号。
- 不能使用bash里的关键字（可用help命令查看保留关键字）。

变量定义举例：

    myUrl="http://see.xidian.edu.cn/cpp/linux/"
    myNum=100
    
**使用变量**
使用一个定义过的变量，只要在变量名前面加美元符号（`$`）即可，如：

    your_name="mozhiyan"
    echo $your_name
    echo ${your_name}
变量名外面的花括号是**可选的**，加不加都行，加花括号是为了帮助解释器识别变量的边界，比如下面这种情况：

    for skill in Ada Coffe Action Java 
    do
        echo "I am good at ${skill}Script"
    done

如果不给skill变量加花括号，写成

    echo "I am good at $skillScript"`

解释器就会把$skillScript当成一个变量（其值为空），代码执行结果就不是我们期望的样子了。

>推荐给所有变量加上花括号，这是个好的编程习惯。

**重新定义变量**

**只读变量**

    readonly myUrl
    
**删除变量**

    unset variable_name
    
**变量类型**

运行shell时，会同时存在三种变量：

1. **局部变量**
局部变量在脚本或命令中定义，仅在当前shell实例中有效，其他shell启动的程序不能访问局部变量。

2. **环境变量**
所有的程序，包括shell启动的程序，都能访问环境变量，有些程序需要环境变量来保证其正常运行。必要的时候shell脚本也可以定义环境变量。
3. **shell变量**
shell变量是由shell程序设置的特殊变量。shell变量中有一部分是环境变量，有一部分是局部变量，这些变量保证了shell的正常运行

## Shell特殊变量：Shell $0, $#, $*, $@, $?, $$和命令行参数

## Shell替换：Shell变量替换，命令替换，转义字符

## Shell运算符：Shell算数运算符、关系运算符、布尔运算符、字符串运算符等
    if [ $a -le $b ]
    then
       echo "$a -le $b: a is less or  equal to b"
    else
       echo "$a -le $b: a is not less or equal to b"
    fi

## Shell字符串    
- 单引号（原样输出）
- 双引号（可以转义、变量等）
- 拼接字符串
- 获取字符串长度
- 提取字符串
- 查找字符串

## Shell数组：shell数组的定义、数组长度
- 定义数组
- 读取数组
- 获取数组的长度

## Shell echo命令

## shell printf命令：格式化输出语句

## Shell if else语句
if 语句通过关系运算符判断表达式的真假来决定执行哪个分支。Shell 有三种 if ... else 语句：

- if ... fi 语句；
- if ... else ... fi 语句；
- if ... elif ... else ... fi 语句。

## Shell case esac语句
>类似于其他语言的`switch...case`语句。

## Shell for循环
与其他编程语言类似，Shell支持for循环。

for循环一般格式为：

    for 变量 in 列表
    do
        command1
        command2
        ...
        commandN
    done
**列表**是一组值（数字、字符串等）组成的序列，每个值通过空格分隔。每循环一次，就将列表中的下一个值赋给变量。

in 列表是可选的，如果不用它，for 循环使用命令行的位置参数。

例如，顺序输出当前列表中的数字：

    for loop in 1 2 3 4 5
    do
        echo "The value is: $loop"
    done
运行结果：

    The value is: 1
    The value is: 2
    The value is: 3
    The value is: 4
    The value is: 5

显示主目录下以 .bash 开头的文件：

    #!/bin/bash
    for FILE in $HOME/.bash*
    do
       echo $FILE
    done
运行结果：

    /root/.bash_history
    /root/.bash_logout
    /root/.bash_profile
    /root/.bashrc
    
## Shell while循环
    while command
    #该语言使用do和done来进行控制循环代码块
    do 
       Statement(s) to be executed if command is true
    done
    
## Shell until循环
until 循环执行一系列命令直至条件为 true 时停止。until 循环与 while 循环在处理方式上刚好相反。一般while循环优于until循环，但在某些时候，也只是极少数情况下，until 循环更加有用。

until 循环格式为：
    
    until command
    do
       Statement(s) to be executed until command is true
    done
command 一般为条件表达式，如果返回值为 false，则继续执行循环体内的语句，否则跳出循环。

例如，使用 until 命令输出 0 ~ 9 的数字：
    
    #!/bin/bash
    a=0
    until [ ! $a -lt 10 ]
    do
       echo $a
       a=`expr $a + 1`
    done
运行结果：

    0
    1
    2
    3
    4
    5
    6
    7
    8
    9

##　Shell break和continue命令

##　Shell函数：Shell函数返回值、删除函数、在终端调用函数
>函数可以让我们将一个复杂功能划分成若干模块，让程序结构更加清晰，代码重复利用率更高。像其他编程语言一样，Shell 也支持函数。Shell 函数必须先定义后使用。

Shell 函数的定义格式如下：

    function_name () {
        list of commands
        [ return value ]
    }
如果你愿意，也可以在函数名前加上关键字 function：

    function function_name () {
        list of commands
        [ return value ]
    }
函数返回值，可以显式增加`return`语句；如果不加，会将最后一条命令运行结果作为返回值。

Shell 函数返回值只能是整数，一般用来表示函数执行成功与否，0表示成功，其他值表示失败。如果 return 其他数据，比如一个字符串，往往会得到错误提示：“numeric argument required”。

如果一定要让函数返回字符串，那么可以先定义一个变量，用来接收函数的计算结果，脚本在需要的时候访问这个变量来获得函数返回值。

先来看一个例子：

    #!/bin/bash
    # Define your function here
    Hello () {
       echo "Url is http://see.xidian.edu.cn/cpp/shell/"
    }
    # Invoke your function
    Hello
运行结果：

    $./test.sh
    Hello World
    $
调用函数只需要给出函数名，不需要加括号。

再来看一个带有return语句的函数：

    #!/bin/bash
    funWithReturn(){
        echo "The function is to get the sum of two numbers..."
        echo -n "Input first number: "
        read aNum
        echo -n "Input another number: "
        read anotherNum
        echo "The two numbers are $aNum and $anotherNum !"
        return $(($aNum+$anotherNum))
    }
    funWithReturn
    # Capture value returnd by last command
    ret=$?
    echo "The sum of two numbers is $ret !"
运行结果：

    The function is to get the sum of two numbers...
    Input first number: 25
    Input another number: 50
    The two numbers are 25 and 50 !
    The sum of two numbers is 75 !
函数返回值在调用该函数后通过 $? 来获得。

再来看一个函数嵌套的例子：

    #!/bin/bash
    
    # Calling one function from another
    number_one () {
       echo "Url_1 is http://see.xidian.edu.cn/cpp/shell/"
       number_two
    }
    
    number_two () {
       echo "Url_2 is http://see.xidian.edu.cn/cpp/u/xitong/"
    }
    
    number_one
运行结果：

    Url_1 is http://see.xidian.edu.cn/cpp/shell/
    Url_2 is http://see.xidian.edu.cn/cpp/u/xitong/
像删除变量一样，删除函数也可以使用 unset 命令，不过要加上 .f 选项，如下所示：
$unset .f function_name
如果你希望直接从终端调用函数，可以将函数定义在主目录下的 .profile 文件，这样每次登录后，在命令提示符后面输入函数名字就可以立即调用。

## Shell输入输出重定向：Shell Here Document，/dev/null文件

## Shell文件包含
像其他语言一样，Shell 也可以包含外部脚本，将外部脚本的内容合并到当前脚本。

Shell 中包含脚本可以使用：

    . filename
或

    source filename
两种方式的效果相同，简单起见，一般使用点号(.)，但是注意点号(.)和文件名中间有一空格。

例如，创建两个脚本，一个是被调用脚本 subscript.sh，内容如下：

    url="http://see.xidian.edu.cn/cpp/view/2738.html"

一个是主文件 main.sh，内容如下：

    #!/bin/bash
    . ./subscript.sh
    echo $url

执行脚本：

    $chomd +x main.sh
    ./main.sh
    http://see.xidian.edu.cn/cpp/view/2738.html
    $

>注意：被包含脚本不需要有执行权限。