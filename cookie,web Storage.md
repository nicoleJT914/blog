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
```
document.cookie = "yummy_cookie=choco"; 
document.cookie = "tasty_cookie=strawberry"; 
console.log(document.cookie); 
// logs "yummy_cookie=choco; tasty_cookie=strawberry"
```
- 客户端操作cookie
封装的cookie方法：
```
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

## web storage


参考：
- [详说 Cookie, LocalStorage 与 SessionStorage](http://jerryzou.com/posts/cookie-and-web-storage/)
- [HTML5本地存储：SessionStorage, LocalStorage, Cookie](http://harttle.com/2015/08/16/localstorage-sessionstorage-cookie.html)
- [What is the difference between localStorage, sessionStorage, session and cookies?](https://stackoverflow.com/questions/19867599/what-is-the-difference-between-localstorage-sessionstorage-session-and-cookies)
- 阮一峰
- MDN
