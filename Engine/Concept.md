# 工程的构成

- source file ：源代码 各种资源数据等等
- config file ： 组织负责source file之间的调用关系
- build file：输出打包成对应平台的运行文件

# Cocos如何对接原生iOS

application - windows - view- Metal上下文 

# 操作系统内置的图形库？

UIKit framework-> UIKit graphic -> core graphic（内置图形库） -> Metal（与硬件沟通）

# 为什么不用游戏引擎做app

图形API的update分三种：

1. timing update - 不管有没有内容一直不停的绘制
2. dirty update - 有数据状态改变的时候才会去调用
3. custom update - 必须手动去调用绘制

# GFX的意思

graphic effect



