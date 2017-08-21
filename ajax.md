Q:
- 手写一个ajax，不依赖第三方库
- 跨域实现的方式；跨域的机制

K：
- XMLHttpRequest
- 状态码
- 跨域

跨域：
JSONP
后端：HTTP header

同源策略
跨域条件

可以跨域的三个标签
<img src=xxx> ==》防盗链处理
<link hred=xxx> ==> 加载CSS
<script>

<img>用于打点统计，统计网站可能是其他域，如百度的站长统计
<link><script>可以使用CDN，(bootCDN)，CDN的也是其他域
<script>可以用与JSONP

跨域注意事项
所有跨域请求都必须经过信息提供方允许
如果未经允许即可获取，那是浏览器同源策略出现漏洞

JSONP
加载链接http://baidu.com/index.html
不一定真有index.html那个文件
<script src='http://baidu.com/api.js'>
也不一定真的有api.js
api.js是一个callback(para)
callback返回的就是我们想要的数据

服务器端设置http header
需要服务端来做

存储
cookie,sessionStorage,localStorage区别





## XMLHttpRequest
XMLHttpRequest是一个JS对象，它提供了一个通过 URL 来获取数据的简单方式，并且不会使整个页面刷新。

XMLHttpRequest的属性：
### onreadystatechange
当readyState属性改变时会调用
### readyState
请求的5种状态：
- 0(UNSET)：`open()`还未被调用
- 1(OPENED)：`send()`还未被调用
- 2(HEADERS_RECEIVED已获取响应头):send()方法已经被调用, 响应头和响应状态已经返回
- 3(LOADING正在下载响应体)：响应体下载中; responseText中已经获取了部分数据
- 4(DONE请求完成)：整个请求过程已经完毕
### responseText

