关于XMLHttpRequest[你真的会使用XMLHttpRequest吗？](https://segmentfault.com/a/1190000004322487#articleHeader5)讲得很全了，基本上所有内容都涵盖到了，因为时间目前只能粗略了解下。
## XMLHttpRequest
XMLHttpRequest是一个浏览器提供的对象，它提供了一个通过 URL 来获取数据的简单方式，并且不会使整个页面刷新。

XMLHttpRequest的属性：
### onreadystatechange
当readyState属性改变时会调用
### readyState
请求的5种状态：
- 0：`open()`还未被调用
- 1：`send()`还未被调用
- 2(已获取响应头):send()方法已经被调用, 响应头和响应状态已经返回
- 3(正在下载响应体)：响应体下载中; `responseText`中已经获取了部分数据
- 4(请求完成)：整个请求过程已经完毕
### `response`
响应实体的类型由`responseType`来指定， 可以是 ArrayBuffer， Blob， Document， json， 或者是字符串。如果请求未完成或失败，则该值为 null。
### `responseText`
响应为文本，请求未成功或还未发送时为 null。
### `status`
该请求的响应状态码

XMLHttpRequest的方法：
- `abort()`
如果请求已经被发送,则立刻中止请求
- `open(method, url, async)`
初始化一个请求。

async默认为true，异步。
- `send()`
发送请求.
- `setRequestHeader(header, value)`
给指定的HTTP请求头赋值

## html5的XMLHttpRequest
老版本的XMLHttpRequest的属性：  
- `xhr.readyState`  
- `xhr.status`  
- `xhr.statusText` 
- `xhr.responseText`  

老版本的缺点：  
- 只支持文本数据的传送，无法用来读取和上传二进制文件  
- 传送和接收数据，没有进度信息  
- 同源限制

新版本特点：  
- 可以设置HTTP请求的时限  
```js
xhr.timeout = 3000
xhr.ontimeout = function(event) {
  alert('请求超时')
}
```
- 可以使用FormData对象管理表单数据  
```js
var formData = new formDate()
formData.append('id', 123456)
xhr.send(formData)
```
- 可以上传文件
`formData.append('files[]', files[i])`
- 可以请求不同于域名下的数据 
CORS 
- 可以获取服务端的二进制数据
`xhr.responseType = 'blob'`
- 可以获得数据传输的进度
```js
// 下载
xhr.onprogress = fn
// 上传
xhr.upload.onprogress = fn
```

## 手写ajax
```js
var httpRequest = new XMLHttpRequest()
httpRequest.onreadstatechange = callback
httpRequest.open('GET', url+'?'+query)
httpRequest.send()

// httpRequest.open('POST',url)
// httpRequest.setRequestHeader('Content-Type','application/x-www-form-urlencoded')
// httpRequest.send(query)

function callback() {
  if(httpRequest.readyState === 4) {
    if(httpRequest.status === 200) {
      var response = JSON.parse(httpRequest.responseText)
      // ...do something
    } else {
      // ...do something
    }
  }
}
```
用Promise写ajax
```js
function ajax(url) {
  return new Promise(function(resolve, reject) {
    var httpRequest = new XMLHttpRequest()
    httpRequest.onreadstatechange = function() {
      if (httpRequest.readyState === 4) {
        if (httpRequest.status === 200) {
          resolve(httpRequest.responseText)
        }else {
          reject(new Error(httpRequest.responseText))
        }
      }
    }
    httpRequest.open('GET', url+'?'+query)
    httpRequest.send()
  })
}
ajax(url).then(function success(data) {
  //...
}).catch(function failure(error) {
  //...
}
```
## ajax封装
```js
function ajax(opts) {
  var httpRequest = new httpRequest()
  httpRequest.onreadstatechange = showRes
  var data = opts.data
  // 解析data
  if (data) {
    var query = ''
    for (var key in data) {
      if (data.hasOwnProperty(key)) {
        query += key+'='+data[key]+'&'
      }
    }
    query = query.substr(0, query.legnth-1)
  }
  // makerequest
  if (opts.type.toLowerCase() === 'get') {
    httpRequest.open('GET', opts.url+'?'+query, true)
    httpRequest.send()
  }else {
    httpRequest.open('POST', opts.url, true)
    httpRequest.setRequestHeader('Content-Type','application/x-www-form-urlencoded')
    httpRequest.send(query)
  }
  function showRes() {
    if (httpRequest.readyState === 4) {
      if (httpRequest.status === 200) {
        var res = httpRequest.responseText
        opts.success(res)
      }else {
        opts.error()
      }
    }
  }
}
// usage
ajax({
  url: 'http://...',
  type: 'GET',
  data: {},
  success: function(ret) {},
  error: function() {}
})
```
## 同源策略
协议、域名、端口相同  
CSS,JS加载不存在同源策略  
[跨域全面总结](https://mp.weixin.qq.com/s?__biz=MzAxODE2MjM1MA==&mid=2651552471&idx=1&sn=794c182bd4504930650beae83fc020aa&chksm=8025ad16b7522400df25d0156c4327123f21118e82ee71c5c6235513a2455819a3bb0e023f75&scene=0&key=41d2cdafcf8079a6ba98eaa9f7187baf50ddf3b8b2b6219810671c55d4a4c17098d6a9a0eeb95d262b6d503088bf73a031247853e4a96dc2b8f6a75c2f73eea838dcf0d85b9b2eac7e245084042d2bce&ascene=0&uin=MTM4NTE3MDA2MQ%3D%3D&devicetype=iMac+MacBookAir7%2C1+OSX+OSX+10.10.5+build(14F2009)&version=12020810&nettype=WIFI&fontScale=100&pass_ticket=5m46QDn4qW%2F12AzWoN95sBUiocqGgMWRQWlNAiCfUh28SwAc0zYx5MxDSnZIzPAK)

## 跨域  
[浏览器同源政策及其规避方法](http://www.ruanyifeng.com/blog/2016/04/same-origin-policy.html)
### cookie
cookie只有同源网页才能共享  

若一级域名相同，而二级域名不相同，可以通过设置相同的`document.domain`来跨域  

http://w1.example.cpm/a.html 和 http://w2.example.com/b.html 可以共同设置`document.domain='example.com'`  

即可通过`document.cookie`来共享cookie了  

另外，上述方式也可通过服务器设置cookie时实现`Set-Cookie:key=value; domain=example.com; path=/`,这样二三级域名不用再各自进行设置了

！这种方法只适合cookie和iframe，localStorage和indexDB要通过postMessage来进行跨域  
### iframe
若父窗口与iframe不是同源  

父窗口获取iframe`document.getElementById('myIframe').contentWindow.document`  

iframe获取父窗口`window.parent.document.body`都会报错  

若一级域名相同，可采用`document.domain`的方法  

若是完全不同源  
#### fragment identifier  
`http://example.com/x.html#fragment`#后面是片段标识符  

只改变片段标识符，网页不会刷新  

父窗口可以把信息写进iframe的片段标识符里  
```js
// 父窗口
var src = OrigURL + '#' + data
document.getElementById('myIframe').src = src
// 子窗口
window.addEventListener('change', function() {
  var message = window.location.hash
})
// 子窗口改变父窗口的片段标识符
parent.location.href = target + '#' + hash
```
#### window.name  
`window.name`的特点是：无论是否同源，只有在同一个窗口里，一个网页设置`window.name`，另一个网页就可以进行读取  

子窗口写入要传递的信息`window.name = 'hi'`  

父窗口创建一个iframe元素，将其src指向子窗口的src  

父窗口监听iframe的load事件，就可以通过`iframe.contentWindow.name`来读取信息  

完成后将iframe关闭，释放内存，保证安全  

这种方法的优点是，window.name容量很大，可以放置非常长的字符串；缺点是必须监听子窗口window.name属性的变化，影响网页性能。

#### window.postMessage（cross-docuement messaging）

html5引入的新的API，`window.postMessage`允许跨窗口通信，无论两个窗口是否同源  
`window.postMessage(data, origin(协议+域名+端口/*))`  

通过`message`监听事件  
`window.addeventListener('message', function() {})`  

message事件的属性：  
`message.source`:发送消息的窗口  
`message.origin`:发送的网址  
`message.data`:消息内容  

通过postMessage也可读写其他窗口的localStorage  

### Ajax
#### JSONP  
JSONP的基本思想就是使用<script>标签向服务器请求JSON数据，服务器将JSON数据放在一个指定回调函数的参数中返回  
JSONP只能发送`get`请求！

Jquery的`$.ajax()`使用的就是JSONP来实现跨域，详细的可以看看以下这篇文章  
[说说JSON和JSONP，也许你会豁然开朗，含jQuery用例](http://www.cnblogs.com/dowinning/archive/2012/04/19/json-jsonp-jquery.html)
- CORS(cross-origin resouce sharing)
它允许浏览器向跨源服务器，发出XMLHttpRequest请求，从而克服了AJAX只能同源使用的限制。  

#### websocket
websocket是一种通信协议，不实行同源策略，只要服务器支持，就可以跨域通信  

websocket请求带有origin，因此服务器可以根据origin字段决定是否许可本次通信  

#### CORS(cross-origin resource sharing)

它允许浏览器向跨源服务器，发出XMLHttpRequest请求，从而克服了AJAX只能同源使用的限制。

只要服务器实现了CORS接口，就可以跨源通信  

[跨域资源共享 CORS 详解](http://www.ruanyifeng.com/blog/2016/04/cors.html)
