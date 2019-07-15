多重边框
===

🤔问题
---
怎么让一个元素现实出多个边框的效果呢？心里面的第一反应是多个元素嵌套层叠，给出视觉效果上的“多边框”。但这并不是真正的多边框不是吗？

:sailboat:思路
---
既然是边框的问题，就让“边框”来解决。`border`边框属性不支持逗号分隔的多个值，所以只能实现一层。之外还能对元素边界产生视觉的效果的元素还有 [盒子模型的`outline`](https://www.w3.org/TR/css-ui-3/#outline-props) 以及 用作[边框效果的`box-shadow`](https://www.w3.org/TR/css-backgrounds-3/#the-box-shadow)。

### `outline`

`outline`通常通常用于突出某个可视对象，标准语法如下：

```
<outline>:	[ <outline-color> || <outline-style> || <outline-width> ]
```

其实 `outline` 和 `border` 的属性设置是很相似的，它也可以选择样式，同样不支持逗号分割多值。此外，`outline` 还受偏移属性 `outline-offset` 控制。那么如果需要更多层的边框怎么办呢？你可以考虑一下`box-shadow`。

### `box-shadow`
`box-shadow`可以产生一个或多个下拉阴影框，标准语法如下：

```
<shadow> = inset？ &&  <length> {2,4}  &&  <color> ？
```

第二部分的 `<length>` 值的个数是{2,4}，表示在[水平偏移，垂直偏移，模糊半径，传播距离]中从左到右选择两到四个赋值。**传播距离**是往往被忽略的一个值。现在设想如果一个阴影不偏移，不模糊，有一个传播距离，那么呈现出来的效果不就是一个边框吗？而且这个“边框”是可以多个的。

当然 `box-shadow` 并非完美无缺的，它只能用来模拟实线边框，如果需要虚线效果，它是没辙的。

下面我们来具体看看两个属性实现多重边框的效果吧。


🎉解决方案
---

为了便于比较，每个盒子都设置了一样的通用样式：

```
width:200px;
height:100px;
box-sizing:border-box;
text-align:center;
background-color:rgba(253,234,171,0.5);
```

### `outline`方案



```
border:10px dashed rgba(0,0,255,0.3);
outline:10px solid rgba(0,255,0,0.3);
```
效果图：

![outline](https://github.com/fujiayishally/Daily-Cute-CSS/blob/master/2-multiple-borders/images/outline.JPG)

### `box-shadow`方案

#### 外阴影

```
border:10px dashed rgba(0,0,255,0.3);
box-shadow: 0 0 0 10px rgba(0,255,0,0.3),
            0 0 0 20px rgba(255,0,0,0.3);
```

效果图：

![box-shadow](https://github.com/fujiayishally/Daily-Cute-CSS/blob/master/2-multiple-borders/images/box-shadow.JPG)

#### 内阴影

```
border:10px dashed rgba(0,0,255,0.3);
box-shadow: 0 0 0 10px rgba(0,255,0,0.3) inset,
            0 0 0 20px rgba(255,0,0,0.3) inset;
```

效果图：

![box-shadow-inset](https://github.com/fujiayishally/Daily-Cute-CSS/blob/master/2-multiple-borders/images/box-shadow-inset.JPG)

⚠注意
---

看起来还不错是不是？不过有些地方还是需要注意的。

### 圆角问题

`outline` 不一定会贴合 `border-radius` 属性产生的圆角，这个被CSS工作组认为是一个bug，未来可能会修复。但目前而言，需要根据实际情况谨慎测试使用。`box-shadow` 就不会有这个问题。看看加上圆角后的不同效果：

|`outline`|外阴影|内阴影|
|:-:|:-:|:-:|
|![outline](https://github.com/fujiayishally/Daily-Cute-CSS/blob/master/2-multiple-borders/images/outline-radius.JPG)|![box-shadow](https://github.com/fujiayishally/Daily-Cute-CSS/blob/master/2-multiple-borders/images/box-shadow-radius.JPG)|![box-shadow-inset](https://github.com/fujiayishally/Daily-Cute-CSS/blob/master/2-multiple-borders/images/box-shadow-inset-radius.JPG)|


### 布局影响

#### `outline`

[标准文档](https://www.w3.org/TR/css-ui-3/#outline-props)中给出了`outline` 和 `border` 的区别，其中一点就是

> Outlines do not take up space.(`outline`不占空间)

对此进一步的解析是：

> the outline is always on top, and doesn’t influence the position or size of the box, or of any other boxes. Therefore, displaying or suppressing outlines does not cause reflow.（`outline` 一直渲染在元素顶层，不影响自身盒子的位置、大小，也不影响其他盒子。所以，它的显示与否并不会引起回流）

#### `box-shadow`

[标准文档](https://www.w3.org/TR/css-backgrounds/#the-box-shadow)中也给出了相关细节：

> Shadows do not influence layout and may overlap other boxes or their shadows. （阴影不会影响布局，它可以与其他框或阴影重叠）

#### 对比
为了看出两者在布局上不同，我给圆角中的例子外层又套了一层等大小的父容器：

```
width:200px;
height:100px;
border:1px dashed black;
margin:50px;
```

看看效果：

|`outline`|外阴影|内阴影|
|:-:|:-:|:-:|
|![outline2](https://github.com/fujiayishally/Daily-Cute-CSS/blob/master/2-multiple-borders/images/outline-radius2.JPG)|![box-shadow2](https://github.com/fujiayishally/Daily-Cute-CSS/blob/master/2-multiple-borders/images/box-shadow-radius2.JPG)|![box-shadow-inset2](https://github.com/fujiayishally/Daily-Cute-CSS/blob/master/2-multiple-borders/images/box-shadow-inset-radius2.JPG)|

### `box-shadow`的渲染

既然阴影可以有多层，那么阴影顺序是怎样的呢？

> The shadow effects are applied front-to-back: the first shadow is on top and the others are layered behind. （阴影之间第一个位于**顶部**，其他在它**后面**）
> 
> ...the outer box-shadows of an element are drawn immediately below the background of that element...(外阴影绘制在元素背景下方)
> 
> ... the inner shadows of an element are drawn immediately above the background of that element.(内阴影绘制在元素背景上方)

也就是说，阴影是层层叠加的，传播距离的长度需要递增，否则会被挡住。外阴影和内阴影不是连续的，中间隔着背景和边框。

还有一点要注意的是，外阴影不会触发鼠标点击或悬停，也不会触发滚动或增加可滚动区域的大小。弥补办法是：

1. 通过 margin（外阴影）和padding（内阴影）模拟边框所占空间进行弥补，
2.  如果对鼠标悬停有要求，应该选择内阴影而不是外阴影
   


🌵最后
---
> 这是 [本例代码](http://dabblet.com/gist/b1a1cd82180a38f20c612d6b8d7619b4)以供参考
> 
> 本问题摘自《CSS揭秘》--Lea Verou的第二章，致谢
> 
> 点[📖](https://www.w3.org/TR/css-backgrounds/) 查看更多CSS3背景标准，点[📖](https://www.w3.org/TR/css3-ui) 查看更多用户界面模块标准，致敬
