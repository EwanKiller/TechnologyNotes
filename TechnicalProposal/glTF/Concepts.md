## Whats glTF ？

- glTF 全称：GL Transmission Format；

- glTF是一种3D内容的格式标准，由Khronos Group管理；（Khronos Group还管理着OpenGL系列、OpenCL等重要的行业标准）

- glTF的设计是**面向实时渲染应用**的，尽量提供可以直接传输给图形API的数据形式，不再需要二次转换；

- glTF对OpenGL ES、WebGL非常友好；

- glTF的目标是：3D领域的JPEG；

- 作为一个标准，自2015年10月发布（glTF 1.0）以来，已经得到了业界广泛的认可；

- glTF目前最新版本为2.0已于2017年6月正式发布；

## Why need glTF？

- 对于常用的Wavefont OBJ、Autodesk FBX，并不是针对实时渲染设计的，而是针对建模工具之间的数据交互使用，现在主流的开发引擎在导入FBX时，都需要一个import的过程去解析，并且转换为自己内部的对象，然后使用序列化机制，并且这些引擎都有自己的存储方式，并不会公开；

## glTF内包含了哪些数据？

- glTF包括场景定义数据，它使用SceneGraph，也就是一个树来定义场景；场景中的节点包含Transform矩阵（也就是位置、旋转、缩放）、父子关系的描述，以及包含的功能（mesh、camera等）；

- 包含mesh和skin数据，通过它们你可以加载静态模型和骨骼模型；

- 包含动画数据，通过定义场景中节点的运动来定义动画，这些节点可以用来组成骨架结构（Skeleton），也可以直接控制节点中的mesh做刚体运动；

- 包含Morph Target（也称作Blend Shape）模型动画数据；

- 包含材质定义；在glTF 1.0中，包含一个technique的定义，相对于Unity3D中的ShaderLab，或者D3D中的effect文件；

## glTF的物理结构

- .glTF(JSON) 
  - Node hierarchy，materials，cameras；作为整个标准的核心，描述场景结构，数据对应关系等；
- .bin 
  -  Geometry ：vertices and indices，Animation：key-frame，Skins：inverse-bind matrices；
  - 一个二进制文件，用来存储Vertex Buffer、Index Buffer等，这些数据可以直接通过OpenGL或者WebGL API直接上传到显示驱动，无需再做解析和转换；
- .glsl 
  - Shaders；
- .png .jpg 
  - Textures；