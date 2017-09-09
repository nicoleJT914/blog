- [什么是闭包，闭包有什么作用？举例说明  //看最下方答案](https://github.com/nicoleJT914/blog/issues/5)
- [apply和call的用法和区别](https://github.com/nicoleJT914/blog/issues/10)
- [bind函数的兼容性]()
- [setTimeout和requestAnimationFrame的区别？](https://github.com/nicoleJT914/blog/blob/master/HTML5/%E6%80%A7%E8%83%BD&%E9%9B%86%E6%88%90.md)  
- 写一个函数每隔1秒调用它自身，总共调用10次，要求可以自定义调用次数和延时时间。
```js
var count = 10;
var timerId = setInterval(function fn() {
  if (!count) {
    clearInterval(timerId)
  }else {
    console.log(count--)
  }
}, 1000)
```
