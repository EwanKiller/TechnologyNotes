# API注释

- setVertexBytes 虽然可以用以设置小于4k的顶点数据并且不用声明MTLBuffer来进行绑定提前设定好的数据结构，但是它会清理掉之前全部手动绑定的，如下代码，如果先调用（2），再调用（1）将会报错，因为（1）之前设定的buffer 绑定被（2）清理掉了；
```c++
[renderEncoder setVertexBuffer:_uniformBuffer offset:0 atIndex:1]; （1）
[renderEncoder setVertexBytes:triangleVertices length:sizeof(triangleVertices) atIndex:0]; （2）
```

# Whats MTKView？

- A specialized view that creates, configures, and displays Metal objects.

## Declaration

- **iOS, Mac Catalyst, tvOS**

  ```objective-c
  @interface MTKView : UIView
  ```

- **macOS**

  ```objective-c
  @interface MTKView : NSView
  ```

## Overview

- Framework : MetalKit ;
- Provides MTLRenderPassDescriptor ;
- Can create depth and stencil textures for you and any intermediate textures needed for antialiasing ;

## Configuring the Drawing Behavior

- Support three drawing modes:
  - **Timed updates** : The view redraws its contents based on an internal timer. Use this mode for games and other animated content that’s regularly updated.
  - **Draw notifications** :  The view redraws itself when something invalidates its contents , Use this mode for apps with a more traditional workflow, where updates happen when data changes, but not on a regular timed interval.
  - **Explicit drawing** :The view redraws its contents only when you explicitly call the [`draw`](https://developer.apple.com/documentation/metalkit/mtkview/1535943-draw?language=objc) method , Use this mode to create your own custom workflow.

## Apple developer document link

https://developer.apple.com/documentation/metalkit/mtkview?language=objc



# Whats CADisplayLink?

- A timer object that allows your application to synchronize its drawing to the refresh rate of the display.

## Declaration

```objective-c
@interface CADisplayLink : NSObject
```

## Overview

- Framework : Core Animation ;
- initializes a new display link , providing a target object and a selector to be called when the screen is updated.
- our application adds it to a run loop using the [`addToRunLoop:forMode:`](https://developer.apple.com/documentation/quartzcore/cadisplaylink/1621323-addtorunloop?language=objc) method

- `CADisplayLink` should not be subclassed.
- The [`duration`](https://developer.apple.com/documentation/quartzcore/cadisplaylink/1621292-duration?language=objc) property provides the amount of time between frames at the [`maximumFramesPerSecond`](https://developer.apple.com/documentation/uikit/uiscreen/2806814-maximumframespersecond?language=objc).
- You can define the number of frames per second by setting the [`preferredFramesPerSecond`](https://developer.apple.com/documentation/quartzcore/cadisplaylink/1648421-preferredframespersecond?language=objc) property.
- When your application finishes with a display link, it should call [`invalidate`](https://developer.apple.com/documentation/quartzcore/cadisplaylink/1621293-invalidate?language=objc) to remove it from all run loops and to disassociate it from the target.

## Apple developer document link

https://developer.apple.com/documentation/quartzcore/cadisplaylink?language=objc

## Metal-Cpp
https://developer.apple.com/metal/cpp/
