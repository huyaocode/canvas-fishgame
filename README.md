# canvas学习

<canvas>元素可被用来通过脚本（通常是JavaScript）绘制图形。它可以用于绘制图形，制作照片，创建动画，甚至可以进行实时视频处理或渲染。

### canvas标签上的两个属性
 - height
   - canvas高度，默认150px
 - width
   - 宽度，默认为 300

注意：canvas的宽高如果用CSS设置的话会导致canvas拉升变形。


### 渲染上下文
<canvas> 元素创造了一个固定大小的画布，它公开了一个或多个渲染上下文，其可以用来绘制和处理要展示的内容。<canvas> 元素有一个叫做`getContext()`的方法，这个方法是用来获得渲染上下文和它的绘画功能，`getContext()`只有一个参数，上下文的格式，对2D图像则传入`'2d'`。
```js
var canvas = document.getElementById('tutorial');
var ctx = canvas.getContext('2d');
```

### 绘制矩形
 - 填充的矩形
   - `ctx.fillRect(x, y, width, height)`
 - 矩形的边框
   - `ctx.strokeRect(x, y, width, height)`
 - 清除指定的矩形区域，让该区域透明
   - `clearRect(x, y, width, height)`

### 绘制路径
路径是通过不同颜色和宽度的线段或曲线相连形成的不同形状的点的集合。一个路径，甚至一个子路径，都是闭合的。本质上，路径是由很多子路径构成，这些子路径都是在一个列表中，所有的子路径（线、弧形、等等）构成图形。

1. 首先，你需要创建路径起始点。
2. 然后你使用画图命令去画出路径。
3. 之后你把路径封闭。
4. 一旦路径生成，你就能通过描边或填充路径区域来渲染图形。

 - `beginPath()`
   - 新建一条路径，生成之后，图形绘制命令被指向到路径上生成路径。
   - 每次这个方法调用之后，列表清空重置，然后我们就可以重新绘制新的图形。
 - `closePath()`
   - 闭合路径之后图形绘制命令又重新指向到上下文中。
   - 如果图形是已经闭合了的，即当前点为开始点，该函数什么也不做。
 - `stroke()`
   - 通过线条来绘制图形轮廓。
 - `fill()`
   - 通过填充路径的内容区域生成实心的图形。

> 当前路径为空，即调用`beginPath()`之后，或者canvas刚建的时候，第一条路径构造命令通常被视为是`moveTo()`，无论实际上是什么。出于这个原因，你几乎总是要在设置路径之后专门指定你的起始位置。


### 移动笔触
`moveTo(x, y)`并不能画出任何东西，相当于把笔抬起来，放到另一个点。

当canvas初始化或者beginPath()调用后，你通常会使用moveTo()函数设置起点。我们也能够使用moveTo()绘制一些不连续的路径。

### 直线
`lineTo(x, y)`
绘制一条从当前位置到指定x以及y位置的直线。开始点也可以通过moveTo()函数改变。

路径闭合：
 - 路径使用填充（fill）时，路径自动闭合
 - 使用描边（stroke）则不会闭合路径。可以使用`closePath()`解决

### 圆弧
 - `arc(x, y, radius, startAngle, endAngle, anticlockwise)`
   - 画一个以（x,y）为圆心的以radius为半径的圆弧（圆），从startAngle开始到endAngle结束，按照anticlockwise给定的方向（默认为顺时针）来生成。
 - `arcTo(x1, y1, x2, y2, radius)`
   - 根据给定的控制点和半径画一段圆弧，再以直线连接两个控制点。

注：`弧度=(Math.PI/180)*角度`。

### 二次贝塞尔曲线及三次贝塞尔曲线
 - `quadraticCurveTo(cp1x, cp1y, x, y)`
   - 绘制二次贝塞尔曲线，cp1x,cp1y为一个控制点，x,y为结束点。
 - `bezierCurveTo(cp1x, cp1y, cp2x, cp2y, x, y)`
   - 绘制三次贝塞尔曲线，cp1x,cp1y为控制点一，cp2x,cp2y为控制点二，x,y为结束点。

![](img/Canvas_curves.png)

[贝塞尔曲线效果展示网站](http://cubic-bezier.com)

### Path2D 对象
`Path2D`用来缓存或记录绘画命令，这样你将能快速地回顾路径。（新浏览器）

`Path2D()`会返回一个新初始化的Path2D对象（可能将某一个路径作为变量——创建一个它的副本，或者将一个包含SVG path数据的字符串作为变量）。
```js
new Path2D();     // 空的Path对象
new Path2D(path); // 克隆Path对象
new Path2D(d);    // 从SVG建立Path对象
```
`Path2D.addPath(path [, transform])​`
添加了一条路径到当前路径（可能添加了一个变换矩阵）
```js
var rectangle = new Path2D();
rectangle.rect(10, 10, 50, 50);

var circle = new Path2D();
circle.moveTo(125, 35);
circle.arc(100, 35, 25, 0, 2 * Math.PI);

ctx.stroke(rectangle);
ctx.fill(circle);
```

### 添加色彩
 - `fillStyle = color`
   - 设置图形的填充颜色
 - `strokeStyle = color`
   - 设置图形轮廓的颜色

注意: 一旦您设置了 strokeStyle 或者 fillStyle 的值，那么这个新值就会成为新绘制的图形的默认值。

### 透明度 
可以设置颜色的时候使用`rgba()`做单位，也可以直接`ctx.globalAlpha = 0.2;`

### 线型 Line stylesEdit
 - lineWidth = value
   - 设置线条宽度。
 - lineCap = type
   - 设置线条末端样式。
   - 分为：`butt`，`round` 和 `square`
 - lineJoin = type
   - 设定线条与线条间接合处的样式。
   - 分为：`miter`, `round`, `bevel`
 - miterLimit = value
   - 设定外延交点与连接点的最大距离，如果交点距离大于此值，连接效果会变成了 bevel。
 - getLineDash()
   - 返回一个包含当前虚线样式，长度为非负偶数的数组。
 - setLineDash(segments)
   - 接受一个数组，来指定线段与间隙的交替
 - lineDashOffset = value
   - 设置虚线样式的起始偏移量。

### 渐变Gradients
 - `createLinearGradient(x1, y1, x2, y2)`
   - 表示渐变的起点 (x1,y1) 与终点 (x2,y2)
 - `createRadialGradient(x1, y1, r1, x2, y2, r2)`
   - 前三个定义一个以 (x1,y1) 为原点，半径为 r1 的圆，后三个参数则定义另一个以 (x2,y2) 为原点，半径为 r2 的圆。


### 图案样式 Patterns
`createPattern(image, type)`
该方法接受两个参数。Image 可以是一个 Image 对象的引用，或者另一个 canvas 对象。Type 必须是下面的字符串值之一：repeat，repeat-x，repeat-y 和 no-repeat。
```js
  // 创建新 image 对象，用作图案
  var img = new Image();
  img.src = 'https://mdn.mozillademos.org/files/222/Canvas_createpattern.png';

  img.onload = function() {
    // 创建图案
    var ptrn = ctx.createPattern(img, 'repeat');
    ctx.fillStyle = ptrn;
    ctx.fillRect(0, 0, 150, 150);
  }
```

### 阴影 Shadows
 - `shadowOffsetX = float`
   - shadowOffsetX 和 shadowOffsetY 用来设定阴影在 X 和 Y 轴的延伸距离，它们是不受变换矩阵所影响的。负值表示阴影会往上或左延伸，正值则表示会往下或右延伸，它们默认都为 0。
 - `shadowOffsetY = float`
   - shadowOffsetX 和 shadowOffsetY 用来设定阴影在 X 和 Y 轴的延伸距离，它们是不受变换矩阵所影响的。负值表示阴影会往上或左延伸，正值则表示会往下或右延伸，它们默认都为 0。
 - `shadowBlur = float`
   - shadowBlur 用于设定阴影的模糊程度，其数值并不跟像素数量挂钩，也不受变换矩阵的影响，默认为 0。
 - `shadowColor = color`
   - shadowColor 是标准的 CSS 颜色值，用于设定阴影颜色效果，默认是全透明的黑色。

### 绘制文本
 - `fillText(text, x, y [, maxWidth])`
   - 在指定的(x,y)位置填充指定的文本，绘制的最大宽度是可选的.
 - `strokeText(text, x, y [, maxWidth])`
   - 在指定的(x,y)位置绘制文本边框，绘制的最大宽度是可选的.

文本样式
 - `font = value`
   - 当前我们用来绘制文本的样式. 这个字符串使用和 CSS font 属性相同的语法. 默认的字体是 `10px sans-serif`。
 - `textAlign = value`
   - 文本对齐选项. 可选的值包括：start, end, left, right or center. 默认值是 start。
 - `textBaseline = value`
   - 基线对齐选项. 可选的值包括：top, hanging, middle, alphabetic, ideographic, bottom。默认值是 alphabetic。
 - `direction = value`
   - 文本方向。可能的值包括：ltr, rtl, inherit。默认值是 inherit。

预测量文本宽度
`measureText()`
将返回一个 TextMetrics对象的宽度、所在像素，这些体现文本特性的属性。