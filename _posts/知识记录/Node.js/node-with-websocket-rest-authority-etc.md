title: "Node:WebSocket,REST,权限等"
date: 2015-08-05 12:20:36
tags: 
 - Node
 - REST
 - WebSocket
 - 权限
categories: Node.js
---
### 设计原则
好的原则可以让开发更规范，而且更好维护，易懂。现在主要想到的原则有：

 - 传输数据格式：JSON
 - 风格 or 规范：REST
 - 实时传输使用websocket
 - 更好地结合websocket与REST


### 关于REST
REST将每个URI看做一个资源，websocket同样可以借鉴这个想法。

在项目中可以把每一个URI看做一个资源集合。

例如：

    POST /api/workflow/changes
    [ {} , {} ]
    
单个元素的信息传递可以看做只含有一个元素的集合。

同样的，登录和注销这种也可以看做资源：

    POST /api/user/logins
    DELETE /api/user/logins
    
<!--more-->    

### 一个不大不小的问题：如何在express中调用socket.io
关于结合websocket与REST，可以试试NodeJS的事件机制传递消息，或者采用函数调用的方式。

> 后记： 
>最终解决方式是“setter”方式注入，获得可用对象后，操作对象，进行刷新前端页面等操作。

中间遇到的问题：为什么在index.js中使用exports.injectIO()导出出错？错误为undefined
解决信息来源：[nodejs中exports与module.exports的区别][1]

### 前端的数据更新

 - 增：OK 
 - 删：refresh 
 - 改：从未实现 
 - 查：OK

### 权限

 - 用户表 
 - 角色组 
 - 角色权限表
 - 权限表（id，url，description）

### 其他

 - 处理链中处理未登录状态的url权限，方式是URL资源白名单
 - 登录状态使用cookie+session+角色权限映射控制


  [1]: http://www.cnblogs.com/pigtail/archive/2013/01/14/2859555.html