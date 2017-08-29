## 清除浮动
解决浮动时造成的父元素高度塌陷问题  
- 在父元素末尾添加一个空标签，使其`clear:both`。
- 利用BFC清除浮动。父元素生成BFC，就可包含浮动元素。比如`overfolw:hidden`等
- 各框架通用方法：
```css
.clear::after {
  content: "";
  display: table;
  clear: both;
}
```
## BFC
block format context，格式化块级上下文。是CSS渲染过程中独立的布局环境，在BFC内部的浮动元素不会影响到外部，在BFC外部也不会影响到BFC内的布局。  
可以生成BFC的元素：`float`,`absolute/fixed`,`overflow`不是visible，`display:inline-block`。  
### BFC的应用场景
- BFC阻止垂直外边距折叠
- BFC包含浮动，可防止父元素高度塌陷
- BFC可以使行内不围绕浮动元素  

参考：  
- [深入理解BFC和Margin Collapse](http://www.w3cplus.com/css/understanding-bfc-and-margin-collapse.html)
