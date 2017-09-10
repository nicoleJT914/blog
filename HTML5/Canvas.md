- Canvas上画个圆，可以用诸如getElementById()之类的方法获取吗？  
不可以，canvas是通过js绘制的图形，图形是一个一个像素画上去的，不可以获取到。但是svg是基于XML格式，内部是一个个节点，可以用DOM操作获取节点。

- canvas上的图像的获取
toDataURL()方法获取图像的Base64编码

### Canvas优化
[Canvas的优化-MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Canvas_API/Tutorial/Optimizing_canvas)
- 在离屏canvas上预渲染相似的图形或重复的对象
如果每一帧里有好多复杂的画图运算，请考虑创建一个离屏canvas，将图像在这个画布上画一次（或者每当图像改变的时候画一次），然后在每帧上画出视线以外的这个画布。
- 避免浮点数的坐标点，用整数取而代之
避免渲染子像素
- 不要在用drawImage时缩放图像
- 使用多层画布去画一个复杂的场景
比如一层是背景层，用来绘制不太改变的元素；一个游戏层，用来绘制不断更新的元素
- 用CSS设置大的背景图
使用静态的<div>元素，结合background 特性，将它置于画布元素之后，避免在每一帧在画布上绘制大图。
- 渲染画布中的不同点，而非整个新状态
- 有动画，请使用window.requestAnimationFrame() 而非window.setInterval()

### 基本语法
<canvas> 标签只有两个属性:width和height，默认大小为300px×150px（宽×高）

兼容：在<canvas>标签中提供了替换内容（文字或图片）。支持<canvas>的浏览器将会忽略在容器中包含的内容，并且只是正常渲染canvas。  

渲染上下文（The rendering context）  
getContext(format)：用来获得渲染上下文和它的绘画功能getContext()只有一个参数  
format：上下文的格式  

绘制矩形  
HTML中的元素canvas只支持一种原生的图形绘制：矩形。所有其他的图形的绘制都至少需要生成一条路径。  
```js
fillRect()
strokeRect()
clearRect()
```

绘制路径  
```js
beginPath()
closePath()
stroke()
fill()
```
生成路径的第一步叫做beginPath()。本质上，路径是由很多子路径构成，这些子路径都是在一个列表中，所有的子路径（线、弧形、等等）构成图形。而每次这个方法调用之后，列表清空重置，然后我们就可以重新绘制新的图形。  

当调用fill()函数时，所有没有闭合的形状都会自动闭合，所以你不需要调用closePath()函数。  
但是调用stroke()时不会自动闭合。  

`moveTo()`:将笔触移动到指定的坐标x以及y上  
当canvas初始化或者beginPath()调用后，通常会使用moveTo()函数设置起点。  

`lineTo()`:绘制一条从当前位置到指定x以及y位置的直线  

`arc(x, y, radius, startAngle, endAngle, anticlockwise)`,`arcTo(x1, y1, x2, y2, radius)`  
角度单位是弧度，不是度数。角度与弧度的js表达式:radians=(Math.PI/180)*degrees。

