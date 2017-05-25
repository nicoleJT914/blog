# 几个概念
npm ==> node包管理器

node ==> JS运行的环境/平台

amd, cmd, commonJS ==> 模块规范

require.js ==> 基于AMD的模块化的方案

webpack ==> JS预编译模块的方案

gulp ==> 前端自动化工具，优化前端工作流


# npm

- 怎么拉包？

进入当前目录，执行`npm install <packageName>`

在index.js中用`require('<packageName>')`加载包

- 以下两种写法的区别：
```
var maked = require('marked')
var sayHello = require('./a')
```
.代表当前目录，要获得同级文件，必须要用./

直接写文件名，则在当前目录中寻找node_modules目录，加载包

当前目录没有node_modules，则向上寻找，一直到根目录为止

- 如何执行index.js文件
使用命令`node index.js`

- 发布包的过程
进入目录，执行命令`npm init`，生成package.json文件

==> 新建index.js文件

==> 执行`npm login`登录

==> 发布包`npm publish`

- `npm install <packageName> --save`
--save命令可以在package.json中将此包加入dependencies

--save=dev命令可以在package.json中将此包加入devDependencies

dependencies在别人拉取包时会同时下载依赖的包,test3是拉取下来的test2发布的包

devDependencies通常用作测试，别人拉取包时并不会下载

# node程序的编写

和发布包大致相同

在index.js中加入`#!/usr/bin/env node`，表示用node环境运行此程序

在package.json中加入

```
"bin": {
  "ourDays": "./index.js"
},
```

表明全局下载后会在usr/local/bin中会以ourDays名字显示此程序，在终端输入`ourDays`即去加载对应包下的inde.js文件