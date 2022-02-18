# 记录Android OpenGL ES学习的知识点

## OpenGL与OpenGL ES的关系

OpenGL ES是OpenGL规范的一种形式，适用于嵌入式设备，Android支持多版本的OpenGL ES API：

- OpenGL ES 1.0/1.1 - 此API规范受Android1.0及更高版本的支持；
- OpenGL ES 2.0 - 此API规范受Android2.2（API 8）及更高版本的支持；
- OpenGL ES 3.0 - 此API规范受Android4.3（API 18）及更高版本的支持；
- OpenGL ES 3.1 - 此API规范受Android5.0（API 21）及更高版本的支持； 

[attention]要使设备支持OpenGL ES 3.0 API，则需要使用由设备制造商提供的此图形管线的一个实现；

## Android支持OpenGL的方式

Android通过其框架API和原生开发套件（NDK）来支持OpenGL；

Android框架下有两个基本类，用于通过OpenGL ES API来创建和操作图形：GLSurfaceView和GLSurfaceView.Renderer；

### GLSurfaceView

此类是一个View，可以使用OpenGL API调用在其中绘制和操控对象，其功能类似于SurfaceView。可以通过创建GLSurfaceView的实例并将Renderer添加到其中来使用此类；
不过，如果要捕获触屏事件，则应该通过拓展GLSurfaceView类来实现触摸监听程序![](../GLSurfaceView/TouchEvent.md)；

