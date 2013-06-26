---
layout: post
title: 摄像头互动入门指南
---

{{ page.title }}
================

这篇文章最初是QQ群中回答网友提问的记录，之后发表在豆瓣小组以及[hudo.it 社区](http://www.hudo.it/index.php/topic,16.0.html) 中

------------

OSC是什么？ 
---
答：   
OSC首先是一套互联网通讯的协议，凡是可以连入网络的设备（PC、mac、iPhone）都可以用。但你并不需要去了解它到底是怎么一回事。 
因为各种开发软件都有各自支持OSC的库。
processing 中就是oscP5，openframeworks就是ofxOSC. 

你要做的是将数据准备好，写几句话，通过OSC库的功能发出去就可以了。另外一边则是接收信息，接收完了就可以随便做事情了。 

OSC的重要性在于它是一个**工业标准**，支持它的工具、语言足够多。

通过OSC做到了数据输入 与 数据可视化 的分离。 

Tuio、OpenCV、OpenFrameworks和reacTIVision分别有什么关系？ 
---
答：   
Tuio是一个协议，你不喜欢可以另起门户自己定义一个，需要说明的一点是它是基于OSC的。 
引用
对于如何用ofxOsc发送Tuio信息感兴趣的，可以看我在CamServer中的 send_tuio_msg() 函数

OpenCV是C/C++的一套函数库，基本上开源的摄像头相关的软件/插件都是基于OpenCV做的，当前版本是2.4.3。 
引用
有人为Processing写了OpenCV的库，但是功能很有限，并且只支持OpenCV1.0，不建议大家使用。
事实上OpenCV官方也支持Java，所以可以直接在Processing中调用原版本的OpenCV。
另外插一句，Processing就是Java，Java能用的库Processing都能用。

OpenFrameworks和Processing是同等类型的东西，一系列C++的库，并且包含对OpenCV 1.0的简单封装。 
引用
如果想通过当代的主流的C++来进行Creative Coding，那么我建议学习Cinder，因为它是白富美。至于OpenFrameworks是啥我就不多说了。

reacTIVision是一个可以直接运行的服务端软件，它的特点是可以识别特定的图案，并且通过网络(OSC/Tuio)将物件的信息发给客户端。客户端可以是PD/processing/openFrameworks/FLASH等。 

我做的CamServer是和reacTIVision类似的服务端软件。通讯也是基于OSC。 
相同类型的软件还有一个叫CCV（Community Core Vision），CCV是基于OpenFrameworks开发的，跨平台，代码非常庞大，甚至说臃肿。 
CCV当前版本是1.5，代码存在很多bug，非常丑陋。
CCV的下一代是2.0，大量重构，代码还是存在很多bug，还是非常丑陋。

reacTIVision的图片能控制视频和音频呀，很神奇哈，怎么做到的？ 
---
答：  
这东西并不能直接控制视频和音频，背后的过程是：   

- 特定的图片->摄像头->reacTIVision->信息（ID、坐标、旋转角度）    
- reacTIVision 通过 OSC 将信息传至客户端    
- 客户端收到信息->根据坐标和旋转角度可以做任何事情，到了这一步，具体实现什么内容和其实与摄像头是无关的

reacTIVision识别用的黑白图片好难看啊，怎么来的？ 
---
![](http://reactivision.sourceforge.net/thumbs/reactivision02.png)

答：  
图像不是随便就有的，这张图就像是二维码，程序可以瞬间识别它的身份ID，即知道这是哪张图，无论旋转放缩都不会认错。所以显而易见的还有一个专门用于生成这种图片的软件，并且每张图片都伴有一个身份ID，reacTIVision 在初始化时会读取身份ID的列表，需要注意的是对于特定的应用ID的个数是有限的，然后就一切顺其自然了。

这其实和ArToolkit里的方块图片起的是同样的作用。 

![](http://www.hitl.washington.edu/artoolkit/images/nakaohome.jpg)



