透明的边框
===

🤔问题
---
假设想给一个容器设置一个白色背景和一道半透明白色边框，body背景从半透明边框透出来，该如何实现呢？

:sailboat:思路
---

大部分人首先想到的代码可能是这样的：

```
.translucent-borders{
    border:2em solid rgba(255,255,255,0.5);
    background:white;
}
```

但渲染出来的结果却不如意:scream_cat:：

![BackgroundClip-BorderBox](https://github.com/fujiayishally/Daily-Cute-CSS/blob/master/1-translucent-borders/images/BackgroundClip-BorderBox.jpg)


边框去哪了？再看看去掉白色背景后的渲染结果：

![NoBackgroundColor](./images/NoBackgroundColor.JPG)

原来是背景色垫在边框下面，半透明的边框看不出来了。也就是说，**元素的背景色有效范围是边框外边沿**。

那么背景图的有效范围呢？把代码修改一下：

```
border:2em solid rgba(255,255,255,0.5);
background-image: linear-gradient(white 100%,transparent);
```
渲染结果和白色背景色时一样：

![BackgroundClip-BorderBox](./images/BackgroundClip-BorderBox.JPG)

🎉解决方案
---

这样解决办法就很明确了，让背景的边框渲染范围在边框内即可。但对于一个盒子而言，CSS2中无法实现。标准中明确表示过：

> Note that the background is always drawn behind the border, if any ... in [CSS2].
>
> 请注意，背景始终绘制在边框后面（如果有）。

针对这个问题，CSS3中添加了`background-clip` 属性，用于**确定背景绘制区域**。它有三个具值：
- `border-box`：(初始值)背景被绘制在（剪切到）边框内
- `padding-box` ：背景被绘制在（填充）填充框内。
- `content-box`：背景被绘制在（剪切到）内容框中

对比下三个效果：

|`border-box`|`padding-box`|`content-box`|
|:-:|:-:|:-:|
|![](./images/BackgroundClip-BorderBox.JPG)|![透明边框2](./images/BackgroundClip-PaddingBox.JPG)|![透明边框4](./images/BackgroundClip-ContentBox.JPG)|

⚠注意
---

CSS3背景中还有另外一个属性`background-origin`属性,用于**确定背景定位区域**。它也有三个具值：
- `padding-box`：（默认值）该位置相对于填充框。
- `border-box` ：该位置相对于边框。
- `content-box`：该位置相对于内容框。

给出如下代码：

```
background-color:#ffb6b9;
padding:1em;
border:1em solid hsla(0,0%,100%,0.5);
```

然后分别控制 `background-origin` 的值，对比下效果：

|`border-box`|`padding-box`（与默认时相同）|`content-box`|
|:-:|:-:|:-:|
|![](./images/BackgroundOrigin-BorderBox.JPG)|![透明边框2](./images/BackgroundOrigin-PaddingBox.JPG)|![透明边框4](./images/BackgroundOrigin-ContentBox.JPG)|

可以看出，`background-origin` 控制的是背景图的开始渲染位置（0,0）是相对于border、padding、还是content，而不是渲染范围。

🌵最后
---
> 这是 [半透明边框的代码](http://dabblet.com/gist/e70120a954256dcdd899566c7333752e)，这是 [背景定位例子代码](http://dabblet.com/gist/cd1ffbf4feb5cfc850fbe1cede1af118)
> 
> 本问题摘自《CSS揭秘》--Lea Verou的第一章，致谢
> 
> 点一下 [📖](https://www.w3.org/TR/css-backgrounds-3) 查看更多CSS3背景标准，致敬
