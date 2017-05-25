# 前端模块化
## 谈谈对前端模块化的理解？
模块的由来：

嵌入网页的JS代码越来越庞大，越来越像桌面程序，需要一个团队去分工协作，进行管理和测试等等，为了更好的管理网页的业务逻辑，产生了模块化编程的理念。

模块的定义：
模块就是实现特定功能的相互独立的一组方法。

模块的意义：
- 能更好的管理网页的业务逻辑
- 可维护性：良好设计的模块会尽量与外部的代码撇清关系，以便于独立对其进行改进和维护
- 可复用性：我们可以按照自己的需求去使用模块，别人也可以使用我们所设计的模块代码
- 解决命名冲突，解决各模块依赖管理

## 谈谈对AMD, CMD, UMD, commonJS的理解？
以上均为模块规范
### commonJS
- CommonJS即为服务器端模块的规范，nodejs是参照CommonJS规范实现的
- 根据CommonJS规范，一个单独的文件就是一个模块。加载模块使用require方法，该方法读取一个文件并执行，最后返回文件内部的exports对象。
```
// foo.js
foo.message = "hi";
foo.say = function (){
    console.log("hello");
};
// main.js
var foo = require('./foo.js');
```
### AMD
#### AMD的诞生
- CommonJS对客户端模块不适用，因为加载模块需要从服务端读取模块，有耗时
- 加载模块同步的话，加载完成之前都不能继续下面的操作，会造成浏览器“假死”状态
- AMD采用“异步加载asynchronous”,允许指定回调函数
#### define()
```
// 依赖项（foo 和 bar）被映射为函数的参数
define(['foo', 'bar'], function ( foo, bar ) {
    // 返回一个定义了模块导出接口的值

    var myModule = {
        doStuff:function(){
            console.log('Yay! Stuff');
        }
    }

    return myModule;
});
```
#### AMD的理念：依赖前置，依赖先执行
优：依赖早运行可尽早发现错误，带来更好的用户体验

缺：可能造成浪费，执行的依赖可能会没有用到

#### require.js ==> 基于AMD规范的模块化方案
#### require.js的诞生背景
最原始的做法：所有要引入的模块放在同一个文件中
如main.js:
```
<script src="a.js"></script>
<script src="b.js"></script>
<script src="c.js"></script>
<script src="d.js"></script>
```
存在问题：
- 加载脚本时，会停止渲染网页。脚本数很多时，会使浏览器较长时间内失去响应
- 必须严格保证脚本的执行顺序，依赖性越大的脚本，放在越后面。难以维护

require.js解决的问题：
- 实现js文件的异步加载，避免网页失去响应
- 管理模块之间的依赖性，便于代码的编写和维护

#### require最佳实践
- require.js加载的模块必须按照AMD规范来写。若要加载的模块不符合AMD规范，须在require.config()中进行配置
- require.js每加载一个模块，就发出一个HTTP请求。r.js是require.js的一个优化工具，当模块部署完毕以后，可以用其将多个模块合并在一个文件中，减少HTTP请求数。

### CMD
通过 exports 暴露接口，通过 require 引入依赖。

### UMD
兼容了AMD和commonJS，同时还支持老式的“全局”变量规范
```
(function (root, factory) {
    if (typeof define === 'function' && define.amd) {
        // AMD
        define(['jquery'], factory);
    } else if (typeof exports === 'object') {
        // Node, CommonJS之类的
        module.exports = factory(require('jquery'));
    } else {
        // 浏览器全局变量(root 即 window)
        root.returnExports = factory(root.jQuery);
    }
}(this, function ($) {
    //    方法
    function myFunc(){};
 
    //    暴露公共方法
    return myFunc;
}));
```

## ES6 module
见ES6学习笔记
