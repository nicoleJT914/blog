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
