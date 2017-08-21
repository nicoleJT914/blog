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
