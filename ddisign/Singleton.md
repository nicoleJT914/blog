## 单例模式
思想：保证一个类只有一个实例，并提供一个访问它的全局访问点

```js
var Singleton = function(name) {
  this.name = name,
  this.instance = null
}
Singleton.prototype.getName = function() {
   alert(this.name)
}
Singleton.getInstance = function(name) {
  if (!this.instance) {
    this.instance = new Singleton(name)
  }
  return this.instance
}

var a = Singleton.getInstance('nicole1')
var b = Singleton.getInstance('nicole2')
alert(a === b)
```
或写成闭包的形式
```js
var Singleton = function(name) { 
  this.name = name 
} 
Singleton.prototype.getName = function() { 
  alert(this.name) 
} 
Singleton.getInstance = (function(name) { 
  var instance = null 
  return function(name) { 
    if (!instance) { 
      instance = new Singleton(name) 
    } 
    return instance 
} })() 
var a = Singleton.getInstance('nicole1') 
var b = Singleton.getInstance('nicole2') 
alert(a === b)
```
存在问题：Singleton.getInstance导致类不透明

- 透明的单例模式：
```js
var CreateDiv = (function() {
  var instance
  var CreateDiv = function(html) {
    if (instance) {
      return instance
    }
    this.html = html
    this.init()
    return instance = this
  }
  CreateDiv.prototype.init = function() {
    var div = document.createElement('div')
    div.innerHTML = this.html
    document.body.appendChild(div)
  }
  return CreateDiv
})()
var a = new CreateDiv('nicole1')
var b = new CreateDiv('nicole2')
alert(a === b)
```
存在问题：使用IIFE和闭包，增加了程序的复杂度

- 用代理实现单例模式
```js
var CreateDiv = function(html) {
  this.html = html
  this.init()
}
CreateDiv.prototype.init = function() {
  var div = document.createElement('div')
  div.innerHTML = this.html
  document.body.appendChild(div)
}
var ProxySingletonCreateDiv = (function() {
  var instance
  return function(html) {
    if(!instance) {
      instance = new CreateDiv(html)
    }
    return instance
  }
})()
var a = new ProxySingletonCreateDiv('nicole1')
var b = new ProxySingletonCreateDiv('nicole2')
alert(a === b)
```
把负责管理单例模式的代码移除除去

- 惰性单例

惰性单例指的是在需要的时候才创建对象实例

管理单例模式的逻辑始终是：用一个变量来标志是否创建过对象，如果是则直接返回该对象

```js
var createLoginLayer = function(){
  var div = document.createElement( 'div' )
  div.innerHTML = '我是登录浮窗'
  div.style.display = 'none'
  document.body.appendChild( div )
  return div
}
var getSingle = function(fn) {
  var result
  return function(){
    return result || (result = fn.apply(null, arguments))
  }
}
var createSingleLoginLayer = getSingle(createLoginLayer)
var loginLayer = createSingleLoginLayer()
console.log(loginLayer)
```

一个实例：

渲染一个列表后，给列表绑定click事件，并用ajax向列表中动态添加数据。

不希望每次渲染都重新给列表绑定click事件，只需绑定一次。

```js
var getSingle = function(fn) {
  var result
  return function(){
    return result || (result = fn.apply(null, arguments))
  }
}
var bindEvent = getSingle(function() {
  document.getElementById('div').onclick = function() {
    alert('clicked')
  }
  return true
})
var render = function() {
  console.log('start to render')
  bindEvent()
}
render()
render()
render()
```
执行var bindEvent = getSingle()后，result已经在闭包中存在了，之后每次调用render()都是从var result下一行开始执行。

因此，调用了3次render()，但是div实际只被绑定了一个事件
