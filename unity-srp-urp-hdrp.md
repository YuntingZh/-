# Unity SRP URP HDRP

![](<.gitbook/assets/image (6).png>)

1.Build-In Render 内置渲染器（默认）兼容太多，反而不能面面俱到，效果不好

2.Scriptable Render Pipline 可编程渲染管线技术，是Unity提供的新渲染系统，可用C#脚本定制Unity的渲染过程，但自己定制渲染管线对编程要求很高，难度大，所以Unity提供里2个预制的管线，基本上涵盖了我们所有的需求，使用时不需要太底层的技术要求！

2.1 High Definition Render Pipleline 高清管线流程，专注于高端图形渲染，针对高端硬件配置，像PC、XBox 和Playstation，其面向高逼真度的游戏、图形demo和建筑渲染、超写实效果，以及所需的最佳图形效果。同时针对高端图形处理时，它要比内置渲染器要快得多，但如果用来做low poly风格的作品就有点杀鸡用牛刀了，纯属浪费资源和时间；但要想得到完成利用HDRP的完美表现能力，需要大量的贴图，漫反射贴图、高光贴图、金属贴图、平滑贴图、AO贴图、法线贴图、凹凸贴图、高度贴图...So,要做HDRP流程需要非常长的时间和庞大的制作团队，还有充足的预算！建议5人以下的小团队慎入！

2.2 Universal Render Pipleline 通用管线流程（URP前身为Lightweight Render Pipeline --- LWRP轻量级渲染管线），专注于性能，URP是选了不会错的渲染管线，它被设计为能够在任何平台上都能提供最好的性能，所以除非有特殊需求只能在HDRP或者SRP解决的，其他都应该选择URP，不管是移动端、主机、PC等，URP都能提供高性能的渲染，目前URP能做的也非常多，它拥有很多HDRP相同的功能，但为了在所有平台达到更好的性能，其做了一定的缩减，但这并不意味着URP做不出漂亮的游戏；

这两种管线流程都利用里Unity新的可编程渲染管线技术，Unity正在把他们变成新的标准，shader graph，visual effect graph这些新功能都是他们的专属，这两个都是创作shader和粒子特效很棒的工具，也支持VR，不过HDRP做逼真风格的VR性能要求非常高！因为Unity实际上把所有东西渲染2次（因为有两个镜头），所以延迟渲染现在只有HDRP支持，不过对URP的支持官方已经在路上。

两者最大的区别是对光照的支持，HDRP提供高级和丰富的光照功能，比如实时全局光照（RealTime GI），能够模拟光线反射、体积光、能模拟光穿过空气中的粒子，还有重头戏的光线跟踪，一种新的光线反射和阴影渲染技术，其原理是跟踪光线在场景中放射的路径，模拟光线在真实世界里与物体交互的效果，这技术对硬件性能非常高，但能产生非常逼真的效果，可以直接用来做电影级别的预渲染作品。

另外一个区别是Shader，HDRP提供一系列高端的shader特效，例如高度、细节和parallax Maps，分别用于纹理的位移、细节和深度模拟，它还支持子面散射，用于模拟光线穿过很薄的物体，比如皮肤和衣物，它提供了高级的shader，像是stacklit，能够让你同时使用多个材质的属性，比如子面散射、彩虹色、各向异性和模糊参数化。

对于后处理效果，两者不相伯仲，HDRP独占的最重要效果，包括AO（环境光遮蔽）、自动曝光（模拟人眼适应不同光线条件的能力）、屏幕空间反射（模拟基于屏幕上可见物体的反射）；URP的AO支持已经在做了，但好东西不是都在HDRP，2D光照和阴影就是URP独占的，所以如果你在做2D游戏，就选URP！

另外两个管线都支持一个很Cool的特效---相机堆叠，让你能够同时用多个相机渲染。&#x20;

如何系统的学习 Unity 3D 中的 shader 编写（nvidia cg 编程）[https://www.zhihu.com/question/21451211](https://www.zhihu.com/question/21451211)

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
