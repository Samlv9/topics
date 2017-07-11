浅谈 Web Animation 动画技术
--------------------------------------------------------------------------------
#### 基本概念
简单而言，动画是指许多的静态画面，以一定的速度连续播放来产生的视觉上的运动。

**逐帧动画** 
<center>[<img src="https://raw.githubusercontent.com/Samlv9/topics/master/WebAnimation/frameByframe.gif"/>](https://raw.githubusercontent.com/Samlv9/topics/master/WebAnimation/frameByframe.gif)</center><center><small><u>_逐帧动画_</u></small></center>

**精灵图(Sprite)**
<center>[<img width="600px" src="https://raw.githubusercontent.com/Samlv9/topics/master/WebAnimation/monster.png"/>](https://raw.githubusercontent.com/Samlv9/topics/master/WebAnimation/monster.gif)</center><center><small><u>_精灵图(Sprite sheet)_</u></small></center>

```
.element {
    width: 190px;
	height: 240px;
	background: url(./monster.png) no-repeat 0 0;
	background-position: 0px 0px;
	animation: sprite-movie 2s steps(10, end) infinite;
}

@keyframes sprite-movie {
    100% { background-position: -1900px 0px; }
}
```

演示地址：[Monster](https://samlv9.github.io/topics/WebAnimation/FrameByFrame2.html) | [Monster-Not-Overflow](https://samlv9.github.io/topics/WebAnimation/FrameByFrame.html)

**矢量动画**
<center>[<img width="500" src="https://raw.githubusercontent.com/Samlv9/topics/master/WebAnimation/scalar.png"/>](https://raw.githubusercontent.com/Samlv9/topics/master/WebAnimation/scalar.png)</center><center><small><u>_矢量动画_</u></small></center>

#### 动画结构
<center>[<img src="https://raw.githubusercontent.com/Samlv9/topics/master/WebAnimation/timeline.png"/>](https://raw.githubusercontent.com/Samlv9/topics/master/WebAnimation/timeline.png)</center><center><small><u>_时间轴_</u></small></center>

- 时间轴
- 图层
- 帧
- 元件

**帧频(Frame Per Seconds)**
是指每秒渲染画面的数量。

#### 缓动(Tween)
- 缓动对象(`Target`)
- 计时器(`Timer`)
- 插值函数(`Timing Function`)
- 关键帧属性值(`Keyframe Values`)

**同步频率(RAF)**
<center>[<img width="500" src="https://raw.githubusercontent.com/Samlv9/topics/master/WebAnimation/raf.png"/>](https://raw.githubusercontent.com/Samlv9/topics/master/WebAnimation/raf.png)</center><center><small><u>_Request Animation Frame_</u></small></center>

相关文档：[Better Performance With requestAnimationFrame](https://dev.opera.com/articles/better-performance-with-requestanimationframe/)

Q: 为什么应该使用 `requestAnimationFrame`，而不是 `setInterval` 或者是 `setTimeout`？

**高精度时间戳(Timestamp)**
```
/// 最早的方法：
+(new Date) || (new Date).getTime(); // 毫秒级别

/// IE9+
Date.now(); // 毫秒级别

/// Performance API
performance.now() || performance.webkitNow(); // 微秒级别（千分之一毫秒）。
```
Q: 为什么需要使用 `performance.now()`代替 `Date.now()`。

**插值函数(Timing Function)**
插值函数用于根据时间输出，计算出当前时间对应的输出值的一个比例(ratio)。其主要作用是实现动画**缓入缓出**，控制动画在不同的时间段变化的比例不同。比如：**弹簧效果**。

<center>[<img src="https://raw.githubusercontent.com/Samlv9/topics/master/WebAnimation/linearFN.png"/>](https://raw.githubusercontent.com/Samlv9/topics/master/WebAnimation/linearFN.png)</center><center><small><u>_插值函数_</u></small></center>

```
/// Linear 插值函数的实现：
/// t: time
/// b: begin
/// c: change
/// d: duration
function Linear( t, b, c, d ) {
	return b + c * (t / d);
}
```

其他函数：`Back`, `Elastic`, `Bounce`, `Circ`, `Sine`, `SlowMo`....
演示地址：[CustomEase](https://greensock.com/customease), [EaseVisualizer](https://samlv9.github.io/topics/WebAnimation/EaseVisualizer.html)

**Bezier 曲线**
一次的贝塞尔曲线，就是一条线段：
<img width="300" src="https://raw.githubusercontent.com/Samlv9/topics/master/WebAnimation/bezier-01.png" />

<center>[<img src="https://raw.githubusercontent.com/Samlv9/topics/master/WebAnimation/Bézier_1_big.gif"/>](https://raw.githubusercontent.com/Samlv9/topics/master/WebAnimation/Bézier_1_big.gif)</center><center><small><u>_一次 Bezier 曲线_</u></small></center>

二次贝塞尔曲线：
<img width="350" src="https://raw.githubusercontent.com/Samlv9/topics/master/WebAnimation/bezier-02.png" />

<center>[<img src="https://raw.githubusercontent.com/Samlv9/topics/master/WebAnimation/Bézier_2_big.gif"/>](https://raw.githubusercontent.com/Samlv9/topics/master/WebAnimation/Bézier_2_big.gif)</center><center><small><u>_二次 Bezier 曲线_</u></small></center>
三次贝塞尔曲线：
<img width="400" src="https://raw.githubusercontent.com/Samlv9/topics/master/WebAnimation/bezier-03.png" />

<center>[<img src="https://raw.githubusercontent.com/Samlv9/topics/master/WebAnimation/Bézier_3_big.gif"/>](https://raw.githubusercontent.com/Samlv9/topics/master/WebAnimation/Bézier_3_big.gif)</center><center><small><u>_三次 Bezier 曲线_</u></small></center>

相关文档:  [A Primer on Bézier Curves](https://pomax.github.io/bezierinfo/)

**三次 Bezier 曲线在 CSS 中的应用**
<center>[<img src="https://raw.githubusercontent.com/Samlv9/topics/master/WebAnimation/cubic-bezier.png"/>](https://raw.githubusercontent.com/Samlv9/topics/master/WebAnimation/cubic-bezier.png)</center><center><small>`cubic-bezier( P1<x,y>, P2<x,y> )`</small></center>

Q: 为什么要限定 `P1<x,y>`、`P2<x,y>` 的定义域为 `[0, 1]` 区间？

**WAAPI**
```
/// document.body.animate(keyframes, [options]);
document.body.animate([
	{ width: "100px" }
	{ width: "200px" }
], 
{
	duration: 1000
});
```
相关文档：[Web Animation Codelabs](https://github.com/web-animations/web-animations-codelabs)

**TweenMax**
```
TweenMax.to(div, 1, { width: "100px" });
```

相关文档：[GSAP](https://greensock.com/)

**缓动队列**
```
var timeline = new TimelineMax({ paused: true });
timeline.add(new TweenMax({...}));
...
timeline.restart();
```
演示地址：[TimelineMax](https://samlv9.github.io/topics/WebAnimation/GSAP_TweenMax.html)

#### 复杂效果 
**色彩变换(color-transform)**
<center>[<img src="https://raw.githubusercontent.com/Samlv9/topics/master/WebAnimation/rgb_calc.png"/>](https://raw.githubusercontent.com/Samlv9/topics/master/WebAnimation/rgb_calc.gif)</center><center><small><u>_RGB 颜色模型_</u></small></center>

```
/*< red=[0, 255] | green=[0, 255] | blue=[0, 255] >*/
function splitRGB( color ) {
	var red = (color >> 16) & 0xFF;
	var green = (color >> 8) & 0xFF;
	var blue = color & 0xFF;
	
	return { red, green, blue };
}

function combineRGB( red, green, blue ) {
	return (red << 24) | (green << 16) | blue;
}
```

**HSL 与 HSV**
<center>[<img width="640" src="https://raw.githubusercontent.com/Samlv9/topics/master/WebAnimation/hsl_hsv.png"/>](https://raw.githubusercontent.com/Samlv9/topics/master/WebAnimation/hsl_hsv.png)</center>

<center>[<img width="320" src="https://upload.wikimedia.org/wikipedia/commons/b/b3/HSL_color_solid_dblcone_chroma_gray.png"/>](https://raw.githubusercontent.com/Samlv9/topics/master/WebAnimation/HSL_color_solid_dblcone_chroma_gray.png)[<img width="320" src="https://upload.wikimedia.org/wikipedia/commons/0/00/HSV_color_solid_cone_chroma_gray.png"/>](https://upload.wikimedia.org/wikipedia/commons/0/00/HSV_color_solid_cone_chroma_gray.png)</center>
<center><small><u>_HSV / HSL 颜色模型_</u></small></center>

相关文档：
- [HSL and HSV](https://en.wikipedia.org/wiki/HSL_and_HSV)
- [RGB](https://en.wikipedia.org/wiki/RGB_color_model)

演示地址：
- [Grayscale](https://samlv9.github.io/topics/WebAnimation/GrayScale.html)
- [Color Animation](https://samlv9.github.io/topics/WebAnimation/ColorAnimation.html)

**滤镜(filter)**
- 模糊滤镜(Blur Filter)
- 发光滤镜(Glow Filter)
- 投影滤镜(DropShadow Filter)
- **高阶:**置换滤镜(Replacement Filter)

**混合模式(blend-mode)**
<center>[<img width="550" src="https://raw.githubusercontent.com/Samlv9/topics/master/WebAnimation/blendmode.png"/>](https://raw.githubusercontent.com/Samlv9/topics/master/WebAnimation/blendmode.png)</center><center><small><u>混合模式</u></small></center>

- 正常(Normal)
- 变暗(Darken)
- 变亮(Lighten)
- 叠加(Add)
- 差异(Difference)
- 发色(Invert)

演示地址：[高光梦幻效果](https://samlv9.github.io/topics/WebAnimation/BlendMode.html)

#### 动画文件
**SWF文件结构**
一个普通的 SWF 文件，由文件头(header)以及一系列的 Tags 组成。
<center>[<img src="https://raw.githubusercontent.com/Samlv9/topics/master/WebAnimation/swftags.png"/>](https://raw.githubusercontent.com/Samlv9/topics/master/WebAnimation/swftags.png)</center><center><small><u>_SWF文件结构_</u></small></center>

#### 关于性能
- _如何触发 GPU 渲染？_
	```css
/* 如果一个元素包含任何 CSS 3D 样式，则浏览器会使用 GPU 来渲染该元素。*/
.element { transform: translateZ(0); }
```
		
- 设置 [will-change](https://developer.mozilla.org/en-US/docs/Web/CSS/will-change) 属性，让浏览器对将要进行动画的内容做预处理。
	```css
	/* 浏览器可以在 transform 和 opacity 属性动画之前，先进行一些相应优化工作。*/
	/* #WARN: Safari 无效 */
	.element { will-change: transform, opacity; }
	```

#### 动画调试
- [Test Animate.css](https://daneden.github.io/animate.css/)

--------------------------------------------------------------------------------
####<center style="font-size:60px;margin:60px 0">THANKS</center>