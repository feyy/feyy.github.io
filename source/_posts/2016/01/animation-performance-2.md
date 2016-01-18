---
title: 动画性能（二）
date: 2016-01-19 01:12:13
tags:
- animation
- performance
---
水了好几天的日记了，居然忘了最重要的YY环节...
## 动画帧和帧率
动画是由一帧一帧构成的。帧可以理解为是动画过程的一张张照片。  
动画的性能(视觉上的卡顿程度)与帧率有关，也就是1s钟内有多少帧。60fps表示1s钟有60帧。  
我们一般使用的显示器刷新率是60Hz，也就是1s钟刷新60次。对于人眼来说就是1s钟看到了60张照片，由于人眼的视觉暂留使得两张照片得以连贯过渡，从而形成动画。说成帧率的话也就是60fps，因而60fps是所有web动画追求的极致。  
通常来说动画如果低于30fps将无法接受。强调动画性能在某种程度上就是获得更高的帧率。

## 帧绘制
那么在web中，每一帧的绘制流程是怎样的呢？

1. javascript执行（function call）
2. 计算需要被加载到节点上的样式结果（Recalculate style--样式重计算） 
3. 为每个节点生成图形和位置（Layout--回流和重布局） 
4. 将每个节点填充到图层中（Paint Setup和Paint--重绘） 
5. 组合图层到页面上（Composite Layers--图层重组）

![frame-flow](http://www.html5rocks.com/en/tutorials/speed/high-performance-animations/devtools-waterfall.jpg)
动画性能优化的核心也就是**缩小每帧绘制所需要的时间，尽量在1/60s内完成，从而达到60fps**。  
上面的步骤有些是可以省略的，优化点就是去省略或者压缩每个点所需的时间。  
如变换（ transform ）和透明度（ opacity ）的改变就不会触发2、3、4过程。
<!-- more -->
## 触发样式重计算
CSS类的改变会触发样式重计算。后面的流程当然一个不可能少，因而开销很大。
## CSS属性分类
不同CSS属性的改变会触发不同的帧绘制流程。
### 触发重布局的属性
当你改变他时，会需要重新布局（这也意味着需要重新计算其他被影响的节点的位置和大小）。被影响的DOM树越大（可见节点），重绘所需要的时间就会越长。

* 盒模型相关属性：width, height, padding, margin, display, border-width, border, min-height
* 定位属性和浮动：top, bottom, left, right, position, float, clear
* 改变内部文字结构：text-align, overflow-y, font-weight, overflow, font-family, line-height, vertival-align, white-space, font-size

### 只触发重绘的属性
color, border-style, border-radius, visibility, text-decoration, background, background-image, background-position, background-repeat, background-size, outline-color, outline, outline-style, outline-width, box-shadow

**手机就算重绘也很慢**  
在重绘时，这些节点会被加载到GPU中进行重绘，这对移动设备如手机的影响还是很大的。因为CPU不如台式机或笔记本电脑，所以绘画巫妖的时间更长。而且CPU与GPU之间的有较大的带宽限制，所以纹理的上传需要一定时间
### 触发图层重组的属性
opacity, transform  
可见使用opacity和transform(translate, rotate, scale...)创建动画是开销最小的方式。

## JS动画
前面提到过显示器刷新率多为60Hz，那么要获得更流畅的动画体验，动画的刷新时间和显示器刷新应该吻合。纯CSS动画由浏览器内部机制去保证，那么js该如何做呢。  
一个新的方法`requestAnimationFrame`, 该方法告诉浏览器JavaScript想发起一个动画帧，然后在动画帧绘制之前，需要做一些动作，这样浏览器可以根据需要来优化自己的mainloop机制和调用时间点，以达到较好地平衡效果。

## YY
为了实现高效的动画我们应该这样来：

* 为复杂动画元素创建独立的渲染层
* 按照帧绘制流程优先选用影响最小的CSS属性(opacity, transform)
* js动画应使用`requestAnimationFrame`来控制绘制时机
* js动画的每帧计算时间应该小于1/60s

## 参考文献
* [前端性能优化（CSS动画篇）](http://www.tuicool.com/articles/NBbQjy3)
* [Accelerated Rendering in Chrome](http://www.html5rocks.com/zh/tutorials/speed/layers/)
* [High Performance Animations](http://www.html5rocks.com/en/tutorials/speed/high-performance-animations/)
* [Chromium渲染主循环(mainloop)和requestAnimationFrame](http://blog.csdn.net/milado_nju/article/details/8101188)