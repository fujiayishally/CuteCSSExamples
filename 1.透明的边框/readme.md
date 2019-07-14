透明的边框
===

![效果图](/1.透明的边框/translucent-borders.JPG)

问题
---
假设想给一个容器设置一个白色背景和一道半透明白色边框，body背景从半透明边框透出来，该如何实现呢？

思路
---

```
.translucent-borders{
    border:2em solid hsla(0,0%,100%,0.5);
    background:white;
}
```

<style>
.container{
    /* width:20em; */
    /* height:13em; */
    /* margin:0 auto; */
    /* background:url(https://fujiayishally.github.io/images/header.jpg) no-repeat; */
    /* background-position:cover;  */
     height: 500px;

  width: 500px;

  background: #1F1D20;

  background-image: linear-gradient(#757575 1px, transparent 1px), linear-gradient(90deg, #757575 1px, transparent 1px);

  background-size: 25px 25px;
}
.translucent-borders{
    /* border:10px solid hsla(0,0%,100%,0.5);
    background:green;

    //styling
    width:10em;
    margin:0 auto; */
     margin-right: auto;

  width: 250px;

  height: 125px;

  background-image: linear-gradient(45deg, #84ECEF 10%, #F8F62F 60%, #FDC018);
}
</style>
<div class="container">
    <div class="translucent-borders"></div>
</div>


解决方案
---



> 本问题摘自《CSS揭秘》--Lea Verou的第一章