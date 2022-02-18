# Graphic Concepts

## OpenGL DX11 与 Metal DX12 Vulkan

- 后三个更加偏向底层，可以对硬件做更多更底层的操作
- 后三个在渲染阶段中让尽可能可以并行运算的部分并行运算

## Pipeline 的含义

- 前一个的输出作为下一个输入，在Render Pipline的中，即上一个阶段输出的数据，当作下一个阶段的输入数据，依次线性的执行下去

## Forward Rendering vs Deferred Rendering

- Forward rendering
- Deferred rendering

## Metal空间坐标系的讨论

https://stackoverflow.com/questions/58702023/what-is-the-coordinate-system-used-in-metal

- NDC: +Y is up. Point(-1, -1) is at the bottom left corner.
- Framebuffer coordinate: +Y is down. Origin(0, 0) is at the top left corner.
- Texture coordinate: +Y is down. Origin(0, 0) is at the top left corner.

## 2D和3D渲染的区别点

1. 2D关系的shader可以群举

## 可见性 着色

1.可见性

2.着色
