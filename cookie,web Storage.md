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
