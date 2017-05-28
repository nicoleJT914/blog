# let 和 const 命令

不存在变量提升

const实际上保证的，并不是变量的值不得改动，而是变量指向的那个内存地址不得改动。

如果真的想将对象冻结，应该使用Object.freeze方法:
```
const foo = Object.freeze({});
// 常规模式时，下面一行不起作用；
// 严格模式时，该行会报错
foo.prop = 123;
```
ES6声明变量的六种方法：
- var
- funtion
- let
- const
- import
- class

let命令、const命令、class命令声明的全局变量，不属于顶层对象的属性
```
let b = 1;
window.b // undefined
```

# 变量的解构赋值
## 数组的解构赋值
基本：`let [a, b, c] = [1, 2, 3];`

数组嵌套：
- 格式相同
```
let [foo, [[bar], baz]] = [1, [[2], 3]];
foo // 1
bar // 2
baz // 3
```

- 使用剩余参数...args
```
let [head, ...tail] = [1, 2, 3, 4];
head // 1
tail // [2, 3, 4]
```
剩余参数一定是不定数量的数组形式，且一定是最后一个命名参数

- 不完全结构
```
let [a, [b], d] = [1, [2, 3], 4];
a // 1
b // 2
d // 4
```
解构赋值只能为变量指定直接值

解构不成功，变量的值就等于undefined

等号的右边不是数组将会报错

## 对象的解构赋值
```
let { bar, foo } = { foo: "aaa", bar: "bbb" };
foo // "aaa"
bar // "bbb"

let { baz } = { foo: "aaa", bar: "bbb" };
baz // undefined
```
对象的解构与数组有一个重要的不同。

数组的元素是按次序排列的，变量的取值由它的位置决定

而对象的属性没有次序，变量必须与属性同名，才能取到正确的值。

```
let { foo: baz } = { foo: "aaa", bar: "bbb" };
baz // "aaa"
foo // error: foo is not defined
```
对象的解构赋值的内部机制，是先找到同名属性，然后再赋给对应的变量。

真正被赋值的是后者，而不是前者。

## 字符串的解构赋值
```
const [a, b, c, d, e] = 'hello';
a // "h"
b // "e"
c // "l"
d // "l"
e // "o"

let {length : len} = 'hello';
len // 5
```
类似数组的对象都有一个length属性，可以对这个属性解构赋值

## 数值和布尔值的解构赋值
```
let {toString: s} = 123;
s === Number.prototype.toString // true

let {toString: s} = true;
s === Boolean.prototype.toString // true

let { prop: x } = undefined; // TypeError
let { prop: y } = null; // TypeError
```
解构赋值的规则是，只要等号右边的值不是对象或数组，就先将其转为对象

由于undefined和null无法转为对象，所以对它们进行解构赋值，都会报错

## 函数参数的解构赋值
```
function add([x, y]){
  return x + y;
}

add([1, 2]); // 3
```
## 应用
- 交换变量的值
```
let x = 1;
let y = 2;

[x, y] = [y, x];
```
- 从函数返回多个值
```
// 返回一个数组

function example() {
  return [1, 2, 3];
}
let [a, b, c] = example();

// 返回一个对象

function example() {
  return {
    foo: 1,
    bar: 2
  };
}
let { foo, bar } = example();
```





