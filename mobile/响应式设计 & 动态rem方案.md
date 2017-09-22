## 响应式设计
响应式设计主要思想就是用一套代码兼容不同分辨率、不同设备  
主要是通过媒体查询来实现在检测到不同条件时，有不同的CSS样式  
对于媒体查询，可以分为PC优先和移动优先  
PC优先（比如bootstrap2）首先支持PC端，通过媒体查询兼容移动端：  
```css
/*pc端的CSS代码*/
@media (max-width: 800px) {
  /*移动端的CSS代码*/
}
```
移动端优先支持移动端：
```css
/*移动端的CSS代码*/
@media (min-width: 800px) {
  /*pc端的CSS代码*/
}
```
虽然响应式设计的概念很火，但是中国的网站用的并不多。  
对于页面设计复杂的页面，还是PC端使用定宽，移动PC两套代码比较好；对于布局较为简单的页面比如博客，可以尝试响应式布局，对用户更友好一些；  

## 动态rem方案
动态rem方案就是专门为移动端所制定的  
基本思想就是利用rem通过改变根元素<html>的font-size来改变不同分辨率下的缩放比例  

方法：  
- 获取屏幕宽度；
- 动态生成style标签；
- 设置<html>的font-size
```js
var clientWidth = document.documentElement.clientWidth;
var style = document.createElement('style');
style.innerHTML = 'html {font-size: ' + clientWdith/4 +'}'
document.head.appendChild(style);
```
为什么我们要除以4呢？  
比如说设计稿的宽度是640px，为了方便，希望设置1px <==> 1rem；  
但是这样html的font-size就会小于12px，出现bug，即浏览器会默认使用12px作为html的font-size  
因此，为了避免这个情况，一般我们都会保证html的font-size大于12px  
除以4的话，在写CSS时就讲实际设计稿的宽度再除以4就好了  
