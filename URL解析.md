![URL](http://ww2.sinaimg.cn/mw690/6941baebgw1essgl9bomkj20db01cwep.jpg)
```js
var parser = document.createElement('a')
parser.href = "http://site.com:81/path/page?a=1&b=2#hash"

parser.protocol // 'http:'
parser.host     // 'site.com:81'
parser.hostname // 'site.com'
parser.port     // '81'
parser.pathName // '/path/page'
parser.search   // '?a=1&amp;b=2'
parser.hash     // '#hash'
parser.origin   // 'http://site.com:81'
```
[url.js](https://gist.github.com/jlong/2428561)

- 网易笔试题：

写一个输出函数QuerySearch(),参数为key，输出其对应的value
```js
var url = "http://site.com:81/path/page?a=1&b=2#hash"
function parser(url, key) {
  var search = url.split('?')[1]
  var index = search.indexOf(key + '=')
  if (index == -1) return;
  var start = index + key.length + 1
  var end = search.indexOf('&', start)
  if (end == -1) end = search.indexOf('#', start)
  return earch.substring(start, end)
}
var val = parser(url, 'b')
```
```js
var url = "http://site.com:81/path/page?a=1&b=2#hash"
function parser(url, key) {
  var a = document.createElement('a')
  a.href = url
  var search = a.search
  var index = search.indexOf(key + '=')
  return search.charAt(index + key.length + 1)
}
var val = parser(url, 'a')
```
- 获取 url 中的参数
1. 指定参数名称，返回该参数的值 或者 空字符串
2. 不指定参数名称，返回全部的参数对象 或者 {}
3. 如果存在多个同名参数，则返回数组
```js
function getUrlParam(sUrl,sKey){
    var result = {};
    sUrl.replace(/\??(\w+)=(\w+)&?/g,function(a,k,v){
        if(result[k] !== undefined){
            var t = result[k];
            result[k] = [].concat(t,v);
        }else{
            result[k] = v;
        }
    });
    if(sKey === undefined){
        return result;
    }else{
        return result[sKey] || '';
    }
}
```
