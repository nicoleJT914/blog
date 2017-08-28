主要就是三大方面：`transform`,`transition`,`animation`  
### `transform`
对元素进行变形操作，旋转、平移、缩放、倾斜等  
- rotate  
`transform:rotate(<angle>)`
- scale  
`transform:scale(<number>[, <number>]);`  
number负数表示先翻转、后缩放  
- translate
`transform:translate(<translation-value>[, <translation-value>]);`  
- skew  
`transform:skew(<angle> [,<angle>]);`  
- perspective
```
transform:perspective(length);
perspective:<length>;
```
perspective属性的存在与否决定是2D transform还是3D transform.  

### `transition`
transform呈现的是一种变形结果，而Transation呈现的是一种过渡，通俗点说就是一种动画转换过程，如渐显、渐弱、动画快慢等。  
transform和transition可以结合使用  
`transirion-property`,`transition-duration`,`transition-delay`,`transition-timing-function`  

### `animation`
`animation-name`,`animation-duration`,`animation-timin-function`,`animation-delay`,`animation-iteration-count`,`animation-direction`,`animation-fill-mode:forwards/backwards`,`animation-play-state:running/paused`  
必须与@keyframes结合使用，用来定义动画序列  
`backface-visibility:visible/hidden`背景是否隐藏
