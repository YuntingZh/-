---
description: https://www.zhihu.com/question/21451211
---

# 如何系统的学习 Unity 3D 中的 shader 编写（nvidia cg 编程）

![](<.gitbook/assets/image (1).png>)

Shader（着色器）实际上就是一小段程序，它负责将输入的Mesh（网格）以指定的方式和输入的贴图或者颜色等组合作用，然后输出。绘图单元可以依据这个输出来将图像绘制到屏幕上。输入的贴图或者颜色等，加上对应的Shader，以及对Shader的特定的参数设置，将这些内容（Shader及输入参数）打包存储在一起，得到的就是一个Material（材质）。之后，我们便可以将材质赋予合适的renderer（渲染器）来进行渲染（输出）了。

所以说Shader并没有什么特别神奇的，它只是一段规定好输入（颜色，贴图等）和输出（渲染器能够读懂的点和颜色的对应关系）的程序。而Shader开发者要做的就是根据输入，进行计算变换，产生输出而已。

Shader大体上可以分为两类，简单来说

* 表面着色器（Surface Shader） - 为你做了大部分的工作，只需要简单的技巧即可实现很多不错的效果。类比卡片机，上手以后不太需要很多努力就能拍出不错的效果。
* 片段着色器（Fragment Shader） - 可以做的事情更多，但是也比较难写。使用片段着色器的主要目的是可以在比较低的层级上进行更复杂（或者针对目标设备更高效）的开发。

因为是入门文章，所以之后的介绍将主要集中在表面着色器上。

{% embed url="https://www.youtube.com/watch?v=9aOtie1DKCc" %}

RP渲染管线流程包括下面几个步骤：顶点处理、面处理、光栅化、像素处理等。

`Unity2018`中引入了可编程渲染管线（`Scriptable Render Pipline`），简称`SRP`，是一种在`Unity`中通过`C#`脚本配置和执行渲染的方式。

为什么需要`SRP`?\
`Unity`内置的渲染管道只有`Forward`和`Deferred`两种。

Forward Shading 原理：每个作用于物体的像素光单独计算一次，drawCall随着物体与光照数量增加而成倍增加 优点：不受硬件限制 缺点：光照计算开销成倍增加随着光源和物体数量增加。 每个物体接受光照数量有限。

Deferred Shading 原理：物体颜色、法线、材质等信息先渲染到G-Buffer中，光照最后单独渲染，避免每个物体多个光照批次的问题 优点：作用于每个物体光照数量不再受到限制，光照计算不随着物体增加而增加 缺点：移动设备需要支持OpenGL3.0。 不支持MSAA。 半透明物体仍然使用前向渲染。&#x20;

{% embed url="https://linxinfa.blog.csdn.net/article/details/108049048?depth_1-utm_source=distribute.pc_relevant.none-task-blog-2~default~CTRLIST~Rate-1-108049048-blog-115710935.pc_relevant_antiscanv2&spm=1001.2101.3001.6650.1&utm_medium=distribute.pc_relevant.none-task-blog-2~default~CTRLIST~Rate-1-108049048-blog-115710935.pc_relevant_antiscanv2&utm_relevant_index=2" %}
讲的非常好很细致，快去看
{% endembed %}



**Note: All shaders in Unity are written in language called "ShaderLab." Shaderlab is a wrapper for HLSL/Cg that lets Unity cross compile shader code for many platforms and expose properties to the inspector.**
