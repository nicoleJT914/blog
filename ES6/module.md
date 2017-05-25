# module
## ES6模块的设计思想
CommonJS： 运行时加载，只有运行时才能读取到模块对象及其方法

ES6: 编译时加载（静态加载），比commonJS的加载效率高

## ES6模块的特点
- 均采用严格模式
尤其注意：严格模式下禁止this指向全局对象，顶层的this指向undefined
- 一个模块就是一个独立的文件
模块内部的所有变量，外部无法获取，除非使用export输出该变量

## ES6模块语法
### export和import
export:规定模块的对外接口

import:输入其他模块提供的功能
- export输出变量的格式
```
// foo.js
// 1.
export var foo = 'nicole'
// 2.
var foo = 'nicole'
export {foo}
```
export输出的接口是动态更新的，可以取到模块内部实时的值。（commonJS无法动态更新）

export必须放到模块的顶层。

- import
```
// main.js
import {foo} from './foo'
// 对foo重命名
import {foo as bar} from './foo' 
// 不输出，仅执行
import 'loadsh'
// 同一个模块加载多次时，只执行一次
import {foo} from './foo'
import {bar} from './foo'
// 加载模块中所有方法
import * from './foo'
```
大括号内的变量名必须与foo.js对外接口的变量名相同

import命令具有提升效果，会提升到整个模块的头部，在编译阶段执行

因此不能使用只有在运行时才能得到结果的语法结构，比如表达式和变量

- export default
为模块指定默认输出，即不需知道所要加载的变量名或函数名
一个模块只有一个默认输出
```
export default function() {
  console.log('foo')
}
```

import命令可以为匿名函数指定任意名字
可以不使用大括号
```
import foo from './foo'
```

import同时输入默认方法和其他变量
```
// foo.js
export default function() {

}
function each() {

}
export { each }
// main.js
import _, {each} from './foo.js'
```

## 模块的继承
```
// foo模块继承bar模块
export * from 'bar'
export function foo() {
  console.log('foo')
}
```
export * 命令会忽略bar模块中的default方法
