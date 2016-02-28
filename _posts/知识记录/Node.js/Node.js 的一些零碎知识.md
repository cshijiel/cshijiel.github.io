title: "Node.js 的一些零碎知识"
date: 2015-04-07 00:20:36
tags: 
- Node
- NPM
categories: Node.js
---
### 1. NPM
npm 全称是 Node Package Manager，相当于Java世界中的Maven。

操作办法，首先确认安装了node.js和npm模块，然后在node项目文件夹创建package.json，执行npm install会安装（npm install xxx）package内包含的库。例如：

    {
      "name": "ShareConnect",
      "version": "0.0.0",
      "private": true,
      "scripts": {
        "start": "node ./bin/www"
      },
      "dependencies": {
        "body-parser": "~1.12.0",
        "cookie-parser": "~1.3.4",
        "debug": "~2.1.1",
        "ejs": "~2.3.1",
        "express": "~4.12.2",
        "morgan": "~1.5.1",
        "serve-favicon": "~2.2.0",
        "uuid-js": "~0.7.5",
        "redis": "~0.12.1",
        "connect": "~2.9.0"
      }
    }
    
还有这样一篇文章：[如何使用NPM来管理你的Node.js依赖][1]

### 2. 项目结构

MVC分层结构经久考验，在此之上沉淀的技术资源也很丰富，Node.js同样可以设计出优雅高效的分层结果。

Connet中间件在MVC中合理的使用。

>借助Connect可以自由定制中间件的优势，可以自行提升性能或是设计出适合自己需要的项目。Connect自身提供了路由功能，在此基础上，可以轻松搭建MVC模式的框架，以达到开发效率和执行效率的平衡。以下是笔者项目中采用的目录结构，清晰地划分目录结构可以帮助划分代码的职责，此处仅供参考。

    ├── Makefile // 构建文件，通常用于启动单元测试运行等操作
    ├── app.js // 应用文件
    ├── automation  // 自动化测试目录
    ├── bin  // 存放启动应用相关脚本的目录
    ├── conf  // 配置文件目录
    ├── controllers  // 控制层目录------------------------C，Controller，控制器
    ├── helpers  // 帮助类库
    ├── middlewares  // 自定义中间件目录
    ├── models  // 数据层目录-----------------------------M，Model，模型
    ├── node_modules  // 第三方模块目录
    ├── package.json  // 项目包描述文件
    ├── public  // 静态文件目录
    │   ├── images  // 图片目录
    │   ├── libs  // 第三方前端JavaScript库目录
    │   ├── scripts  // 前端JavaScript脚本目录
    │   └── styles  // 样式表目录
    ├── test  // 单元测试目录
    └── views  // 视图层目录-----------------------------V，View，视图
    
    
### 3. 关于本项目基本的设计
可以一个图来解释：
![UDP-返回-Post-set][2]






  [1]: http://www.infoq.com/cn/articles/msh-using-npm-manage-node.js-dependence
  [2]: http://note.wiz.cn/api/document/files/unzip/66e5c3f6-8482-11e1-a525-00237def97cc/6d3b0fec-73aa-4846-a15d-ff78e296af74.32632/index_files/f76767cb-d24f-4ef4-a718-cc3264db2dde.png