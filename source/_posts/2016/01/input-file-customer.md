---
title: input-file定制样式
date: 2016-01-22 00:44:27
tags:
- input-file
---
前端文件上传是个永恒的话题，不管是使用原先的form提交还是使用fileReader直接ajax提交Base64编码，最终的最终都离不开一个元素——input:file  
这玩意长得丑不说，还在个家浏览器上长得不一样，堪称又丑又不自觉的典范。  
躲是躲不掉的，那么作为一个注重美观的FE来说，改如何满足自己和客户呢？

## 透明配合绝对定位
使用`opacity:0;`使得input:file按钮处于透明状态，同时使用绝对定位使得他完全覆盖在模拟的上传按钮上。这样input:file接受click事件响应文件上传，模拟的按钮处于一层透明的薄膜下起显示作用

### 不兼容IE8/9/10
IE8/9/10下点击按钮的左侧类似输入框的东东无法弹出文件选择框，并且会出现光标...  
因为input:file包括一个文本区域和一个浏览按钮，，在IE8/9/10中点击文本区域，是不会弹出文件选择框的，并且在所有的IE版本中即使将它设置为透明，依然会有光标的出现。

解决方案：**使用`font-size`属性把input:file的浏览按钮撑得足够大以至于完全覆盖显示区域**
<!-- more -->
## 隐藏input:file，手动触发click
如题所述，在点击模拟的按钮的时候通过js间接触发input:file的click事件来触发上传。

### IE8不干了
IE8出于安全的考虑不允许提交手动触发过click事件的

解决方案：**利用lable标签与表单元素的关联触发效果，使用label来模拟按钮，for属性指向input:file的id值**

同时input[type=file]的display属性不能为none，否则就不会起作用。

## 参考文献
* [关于input[type=file]那些事：如何定制样式](http://fedvic.com/2015/11/15/inputfile/customCSS/index.html)