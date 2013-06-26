---
layout: post
title: Nike 运动会跑步机互动项目的心得分享
---

{{ page.title }}
================
本文曾连载于[hudo.it 社区](http://www.hudo.it/index.php/topic,21.0.html)

[现场视频1](http://v.youku.com/v_show/id_XMjk2MTc3NzU2.html)

[现场视频2](http://v.youku.com/v_show/id_XMjk2MTMyNjg0.html)

[Processing 代码](https://github.com/vinjn/Processing_sketch/tree/master/SuperNature_Nike_FaceOff)

------------

## HelloWorld

不同的程序之间要分离，保证一个程序出问题，其他程序不受影响<br>
使用osc进行通信<br>
基于数据的可视化程序，即使没有数据输入时，也应该有显示（因为是活动现场应用）<br>
考虑各种可能出错的环节<br>
Processing 效率不够的地方果断用 openFrameworks 来重写<br>

## 关于网络编程

* 使用osc，Processing 用oscP5，C++使用ofxOsc
* arduino的以太网模块很好用，能够以网页服务器的形式运作，用浏览器就可以查看信息。<br>
用Processing访问的时候必须注意，通过不停地new Client的方式去获取信息，会使得内存很快就用尽，以致程序无响应。<br>
幸好Processing提供了另一个方法 loadStrings

        String[] lines = loadStrings("http://192.168.1.100");
        println(lines);
    
* 网络消息无法发送时的解决思路

        1 网线是否连接，是否断掉 
        2 MAC/IP是否有冲突 
        3 Processing中的ip地址是否输入正确

* 当涉及的电脑比较多时，可以在显示器或机箱上黏上贴纸，指明ip地址。

## 适用于一切与硬件打交道的项目

就是，尽快制作一个对硬件的模拟器，模仿硬件发送同样格式的osc消息。（建议用Processing写，足够快）<br>
好处是，可以不用考虑硬件的实际状态<br>
拿这次Nike活动为例，要测试视觉效果必须启动跑步机，缓缓将速度提升<br>
如果用模拟器，那么你可以随意设置，得到任意的速度<br>
而负责效果的程序，是察觉不到这种真真假假的<br>
想使用Kinect进行快速开发也可以用这个方法<br>
一个专门的程序用于读取Kinect数据，并把关节点的坐标以osc消息发送<br>
同时可以保存到本地文件，以便在没有Kinect的情况下，伪装出一组组的坐标<br>
这样，就不用每回都要在Kinect之前张牙舞爪了，不论是调试还是开发都很方便<br>

## 状态机 State Machine（上）

将程序分解成一个个状态

以这个项目为例，分成

* idle--待机画面，显示logo
* intro--跑步前，显示参赛队伍/人员的名称
* countdown--倒计时，5-4-3-2-1-Start
* gaming--3分钟的跑步
* rank--排名统计
* winner--冠军颁奖

每个状态用一个整数来标示，从0开始（Processing用枚举不方便，否则应该用枚举）<br>
用一个变量int state 来表示状态本身

    final int idle = 0;
    final int intro = 1;
    final int countdown = 2;
    final int gaming = 3;
    final int rank = 4;
    final int winner = 5;
    
    int state = idle;//初始化为idle状态

每个状态在Processing中都用一个tab(标签页）来维护<br>
每个tab里写这么一些函数：

    --------------idle------------
    void idle_setup();
    void idle_draw();
    --------------intro-----------
    void intro_setup();
    void intro_draw();

状态切换时需要调用

    xx_setup();
    
状态进行时需要调用

    xx_draw();

## tab（标签页）的使用

当程序的行数达到一定规模（几百上千行）时，tab的意义就浮现出来了

* 主tab，也就是默认就有的那个tab里，放置setup()和draw()，尽量简短些
* 特定的功能都在各自的tab中实现，比如声音相关的函数都放在名为minim的tab里

        minim_setup();
        minim_draw();

主tab负责调用这些子tab里的 xx_setup()和 xx_draw() 即可

类似的，network/tuio/osc 相关的函数也应该放在名为network的tab里

如果学过C++，这里tab的作用其实很像namespace（命名空间）

