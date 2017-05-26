# webpack学习笔记
## 模块化的发展
1. <script>
```
<script src="module1.js"></script>
<script src="module2.js"></script>
<script src="libraryA.js"></script>
<script src="module3.js"></script>
```
存在问题：
- 命名冲突
- 要严格按照脚本执行顺序加载脚本

2. CommonJs
各模块使用`require`方法加载依赖，返回exports接口

模块需要输出的变量添加为exports对象的属性或module.exports的值
```
require("module");
require("../file.js");
exports.doStuff = function() {};
module.exports = someValue;
```
利：
- 服务端模块可以被复用
- 很多包都基于CommonJs规范
- 简单易用

弊：
- 网络请求需要异步，同步加载不适用与客户端
- 没有多模块的并行加载

应用：
- node.js（服务端）
- broswerify

3. AMD
```
require(["module", "../file"], function(module, file) { /* ... */ });
define("mymodule", ["dep1", "dep2"], function(d1, d2) {
  return someExportedValue;
});
```
利：
- 异步加载
- 多模块并行加载

弊：
- 需要写的代码量较大
- 功能较单一

应用：
- require.js
- curl.js

4. ES6 Modules
```
import "jquery";
export function doStuff() {}
module "localModule" {}
```
利：
- 静态加载
- 特性将会被标准化

弊：
- 原生浏览器支持还需要一段时间
- 现有的模块较少

## 模块传输方式
1. 每个模块发一次请求

利：
- 只加载需要使用的模块

弊：
- 网络请求量大
- 由于请求的延时，用户体验不好

2. 所有模块，一次请求

利：
- 请求少，延时短
弊：
- 没有用的模块也会被加载

3. 分块加载

code splitting

将所有的模块拆分为多个块，每个快可以包含一个或多个模块，块异步加载

## webpack的目标
- Split the dependency tree into chunks loaded on demand
- Keep initial loading time low
- Every static asset should be able to be a module
- Ability to integrate 3rd-party libraries as modules
- Ability to customize nearly every part of the module bundler
- Suited for big projects

## webpack的特性
- Code Splitting
webpack has two types of dependencies in its dependency tree: sync and async. Async dependencies act as split points and form a new chunk. After the chunk tree is optimized, a file is emitted for each chunk.
- Loaders
webpack can only process JavaScript natively, but loaders are used to transform other resources into JavaScript. By doing so, every resource forms a module.
- Clever parsing
webpack has a clever parser that can process nearly every 3rd party library. It even allows expressions in dependencies such as: require("./templates/" + name + ".jade"). It handles the most common module styles: CommonJs and AMD.
- Plugin system
webpack features a rich plugin system. Most internal features are based on this plugin system. This allows you to customize webpack for your needs and distribute common plugins as open source.

## demo-1
根据文档创建了一个目录来学习webpack

初步尝试添加了jquery，和自己的JS文件

并用webpack的ugilifyJsPlugin对bundle.js文件进行了压缩

- `export`和`import`命令
语法见ES6学习笔记

- npm scripts
为了省去每次运行时输入一长段命令，可以在scripts里添加`"build": "webpack"`

运行时输入`npm run build`即可

npm script中除npm默认命令字段外，其他scripts中自定义的字段运行时必须使用命令`npm run <name>`

- .gitignore
git push时输入命令`echo '/node_modules/' > .gitignore`

可以避免node_modules文件夹上传到git上

- __dirname 和 './' 的区别
[What is the difference between __dirname and ./ in node.js?](https://stackoverflow.com/questions/8131344/what-is-the-difference-between-dirname-and-in-node-js)

简单来说，__dirname是执行的JS文件所在目录的绝对路径

而'./'是运行node命令时目录的绝对路径

比如有文件是如下路径：
```
/dir1
  /dir2
    pathtest.js
```
运行：
```
cd /dir1
node dir2/pathtest.js
```
此时__dirname 和 './' 分别代表：
```
. = /dir1
__dirname = /dir1/dir2
```



