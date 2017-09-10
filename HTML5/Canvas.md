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

## Canvas & SVG
[SVG 与 Canvas：如何选择](https://msdn.microsoft.com/zh-cn/library/gg193983(v=vs.85).aspx)  

Canvas | SVG
------ | ------
基于像素（动态 .png） | 基于形状
单个 HTML 元素 | 多个图形元素，这些元素成为 DOM 的一部分
仅通过脚本修改 | 通过脚本和 CSS 修改
事件模型/用户交互颗粒化 (x,y) | 事件模型/用户交互抽象化 (rect, path)
图面较小时、对象数量较大 (>10k)（或同时满足这二者）时性能更佳 | 对象数量较小 (<10k)、图面更大（或同时满足这二者）时性能更佳

### 从性能方面选择
![xingneng](https://i-msdn.sec.s-msft.com/dynimg/IC628910.png)  
随着屏幕大小的增大，画布将开始降级，因为需要绘制更多的像素。  
随着屏幕上的对象数目增多，SVG 将开始降级，因为我们正不断将这些对象添加到 DOM 中。  

### 高保真度的复杂矢量文档
使用SVG，矢量图特性

### 增强的 Web 图形
SVG 另外还常用于简单图像，无论是应用程序还是网页中的图像，大图像还是小图像。由于 SVG 要加载到 DOM 中，或者创建图像前至少要进行解析，所以性能会稍微有所下降，但相比于呈现网页的成本（大约几毫秒），这种下降是极其微小。

### 像素操作
使用 Canvas 时可以获得快速的绘图速度，且不需要保留相应信息。  

### 实时数据
Canvas 非常适合输出实时数据  
通过使用 Canvas API，可以在不影响 DOM 的情况下快速绘制（和擦除）这些对象。  

![diff](https://i-msdn.sec.s-msft.com/dynimg/IC628922.png)  

Canvas是使用JavaScript程序绘图(动态生成)，SVG是使用XML文档描述来绘图。  
从这点来看：SVG更适合用来做动态交互，而且SVG绘图很容易编辑，只需要增加或移除相应的元素就可以了。  
同时SVG是基于矢量的，所有它能够很好的处理图形大小的改变。  
Canvas是基于位图的图像，它不能够改变大小，只能缩放显示；所以说Canvas更适合用来实现类似于Flash能做的事情(当然现在Canvas与Flash相比还有一些不够完善的地方)。
