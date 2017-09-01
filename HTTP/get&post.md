## GET和POST的区别  
通信的4种方法：GET, POST, PUT, DELETE（查、改、增、删）  

- 根据HTTP规范，GET用于信息获取，应该是安全和幂等的，安全指获取信息而不会修改信息，幂等指的是对一个URL的多个请求应该返回同样的结果  
- get请求的数据会放在URL中，post则放在body中  
- HTTP对get可提交的数据量并么有限制，与URL的长度有关，虽然URL的长度也无限制，但是浏览器和操作系统一般会对其做出限制  
- post比get的安全性高。由于get的提交数据在URL中，页面被浏览器缓存，别人就有可能从历史记录中看到你提交的信息。另外，get还有可能导致CSRF。

一些闲话：
- get和post都是明文传输，如果被抓包，都不安全，所以还是应该用HTTPS

- get相比post的优势：  
请求中的URL可以被缓存  
请求中的URL可以被存在书签里，或者历史里，或者快速拨号里面，或者分享给别人。  
请求中的URL可以被手动输入；  
请求中的URL可以被搜素引擎收录  

- 可以重复的交互，比如取个数据，跳个页面， 用GET.不可以重复的操作， 比如创建一个条目/修改一条记录， 用POST, 因为POST不能被缓存，所以浏览器不会多次提交。

资料：  
- [浅谈HTTP中Get与Post的区别](http://www.cnblogs.com/hyddd/archive/2009/03/31/1426026.html#3762804)  
- [post 相比get 有很多优点，为什么现在的HTTP通信中大多数请求还是使用get？](https://www.zhihu.com/question/31640769)
- [https://www.zhihu.com/question/28557115](https://www.zhihu.com/question/28557115) 
