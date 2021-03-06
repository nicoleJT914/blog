面试经常问的一道题：cookie，localStorage和sessionStorage的区别？

先给出答案：
![QQ20170811-094126.png](http://upload-images.jianshu.io/upload_images/4952363-0eb4bb7c8bc4ba53.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## cookie
服务端发送到浏览器并保存在浏览器上的小型的文本文件，包含若干键值对，它会在下一次请求时被携带发送到服务端。

问题：由于每次请求时都会携带cookie数据，会造成性能上的负担，特别是在移动环境下。

### 创建cookie
服务器收到HTTP请求后，在响应头加一个`Set-Cookie`头部来发送cookie信息。
```
HTTP/1.0 200 OK
Content-type: text/html
Set-Cookie: yummy_cookie=choco
Set-Cookie: tasty_cookie=strawberry
```
客户端保存cookie信息后，每次发送请求都会携带cookie信息
```
GET /sample_page.html HTTP/1.1
Host: www.example.org
Cookie: yummy_cookie=choco; tasty_cookie=strawberry
```
- 会话期cookie在关闭浏览器后自动删除，持久cookie可以设置有效时间/过期时间
- secure属性表示只能在HTTPS下发送cookie，HttpOnly表示cookie不能被js读取
- domain和path指定了cookie的作用域，即需要发送cookie的URL集合
![image.png](http://upload-images.jianshu.io/upload_images/4952363-5f5e0b977f9f5bb2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
path为/，则所有的子文件夹都可以匹配到
- js可以通过`document.cookie`来访问cookie
```js
document.cookie = "yummy_cookie=choco"; 
document.cookie = "tasty_cookie=strawberry"; 
console.log(document.cookie); 
// logs "yummy_cookie=choco; tasty_cookie=strawberry"
```
- 客户端操作cookie
封装的cookie方法：
```js
function getCookie(key) {
  var s = document.cookie;
  var index = s.indexOf(key + '=');
  if (index == -1) return;
  var start = index + key.length + 1;
  var end = s.indexOf(';', index);
  if (end == -1) end = s.length;
  return s.substring(start, end);
}
```
jquery-cookie插件
```
$.cookie('name')
```

cookie的安全使用：
- 使用HTTPonly
- 当发送数据非常敏感时，使用Secure（仅使用HTTPS发送）
- 谨慎设置domain,path属性，最小化授权

## cookie和session的区别
HTTP是无状态的协议，所以服务端需要记录用户的状态时，就需要用某种机制来识具体的用户，这种机制叫做session

session是在服务器，实现session需要session_id，session_id存在cookie中。

## web storage
- 两种机制：local storage（除非用户清除浏览数据，一直存在）和session storage（会话期间有效）
```js
localStorage.clear();
localStorage.setItem('key', 'value');
localStorage.getItem('key'); 
localStorage.removeItem('key');
```
- 与Cookie一样，它们也受同源限制。某个网页存入的数据，只有同源下的文档才能读取。(摘自阮一峰)
- 当本地存储的数据发生变化时，会触发storage事件。

storage通过window对象触发，可以共享被改动存储的任何一个文档。可以通过这种机制，实现多个窗口之间的通信。

storage事件不会在制造变化的文档内指派，只在其他同源文档里可用。

## 应用场景(SO)
localStorage and sessionStorage are perfect for persisting non-sensitive data needed within client scripts between pages (for example: preferences, scores in games).

cookies should not be used to store large amounts of information.Typically cookies are used to store identifying tokens for authentication, session and advertising tracking. The tokens are typically not human readable information in and of themselves, but encrypted identifiers linked to your application or database.

As session data is completely controlled by your application (server side) it is the best place for anything sensitive or secure in nature.

localStorage, sessionStorage and cookies are all subject to "same-origin" rules which means browsers should prevent access to the data except from the domain that set the information to start with.

参考,学习：
- [详说 Cookie, LocalStorage 与 SessionStorage](http://jerryzou.com/posts/cookie-and-web-storage/)
- [HTML5本地存储：SessionStorage, LocalStorage, Cookie](http://harttle.com/2015/08/16/localstorage-sessionstorage-cookie.html)
- [What is the difference between localStorage, sessionStorage, session and cookies?](https://stackoverflow.com/questions/19867599/what-is-the-difference-between-localstorage-sessionstorage-session-and-cookies)
- [COOKIE和SESSION有什么区别？](https://www.zhihu.com/question/19786827/answer/28752144?utm_medium=social&utm_source=wechat_session) 
- 阮一峰
- [MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Web_Storage_API/Using_the_Web_Storage_API)
- [静态资源（JS/CSS）存储在localStorage有什么缺点？为什么没有被广泛应用？](https://www.zhihu.com/question/28467444) 
- [阮一峰indexDB](http://javascript.ruanyifeng.com/bom/indexeddb.html)
