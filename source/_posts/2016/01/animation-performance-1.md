---
title: 动画性能（一）
date: 2016-01-18 02:39:39
tags:
- animation
- performance
---

作为一个前端，没有流畅的动画的话，那只能是个切图的...直入主题了，好困的...
## 图层
用过PS的切图仔肯定知道射鸡师们给出的psd是由一个个图层组成的。不过不知道你是否留意到，你所写的前端页面也是由一个个图层堆叠而成的。  
浏览器在渲染一个页面时，会将页面分为很多个图层，图层有大有小，每个图层上有一个或多个节点。  
在渲染DOM的时候，浏览器所做的工作实际上是：  

1. 获取DOM后分割为多个图层 
2. 对每个图层的节点计算样式结果（Recalculate style--样式重计算） 
3. 为每个节点生成图形和位置（Layout--回流和重布局） 
4. 将每个节点绘制填充到图层位图中（Paint Setup和Paint--重绘） 
5. 图层作为纹理上传至GPU 
6. 符合多个图层到页面上生成最终屏幕图像（Composite Layers--图层重组）

**Chrome子开发者工具台中勾选“show layer borders”标记(在 “rendering” 标题下)，它会高亮屏幕上的层。**

### 图层创建的标准
下面为chrome启发式创建图层的条件：

* 3D 或透视变换(perspective transform) CSS 属性
* 使用加速视频解码的 `<video>` 元素
* 拥有 3D (WebGL) 上下文或加速的 2D 上下文的 `<canvas>` 元素
* 混合插件(如 Flash)
* 对自己的 opacity 做 CSS 动画或使用一个动画变换的元素
* 拥有加速 CSS 过滤器的元素
* 元素有一个包含复合层的后代节点(换句话说，就是一个元素拥有一个子元素，该子元素在自己的层里)
* 元素有一个 z-index 较低且包含一个复合层的兄弟元素(换句话说就是该元素在复合层上面渲染)

**需要注意的是，如果图层中某个元素需要重绘，那么整个图层都需要重绘。**

## 参考文献
* [前端性能优化（CSS动画篇）](http://www.tuicool.com/articles/NBbQjy3)
* [Accelerated Rendering in Chrome](http://www.html5rocks.com/zh/tutorials/speed/layers/)
* [High Performance Animations](http://www.html5rocks.com/en/tutorials/speed/high-performance-animations/)