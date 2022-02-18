# render pipeline in OpenGL

## Vertex processing

- 为vertex shader阶段提供 ***vertex buffer***
  - 计算position,normal,lighting,animation,etc;
  - 将vertex从world space转换到view space;
- 可选的 ***primitive Tessellation stages***（曲面细分阶段）
  - 把patch的vertex data细分成更小的primitive(base primitive),即把vertex变得更加精细;
  - 可以通过displacement mapping(置换贴图)，把经过Tessellation变得更加平滑后的mesh，根据贴图的高度信息，使vertex具有了高度偏移量，模型的细节更加真实;
- 可选的 ***Geometry Shader***
  - 不同于Tessellation在已有图元的基础上，更加精细而是通过新增vertex，把原有图元变成新的图元;

## vertex post-processing

- ***Transform Feedback***，可以将上一阶段输出的vertex data重新写回vertex buffer object,这样就拿到了经过verter shader处理过的vertex data;
- ***Primitive Assembly*** 图元装配，能够将上一阶段输出的图元拼装成一系列三角形，用以下一阶段检测哪些像素点位于三角形内;

## Rasterization

- ***Primitive clipping***
  - 检测哪些像素点位于三角形图元内;
- ***perspective divide***
  - from view space to screen space

## Fragment shader

- Pixel shading 即对每一个pixel的颜色，纹理，光照，阴影等进行设置，而颜色，纹理坐标，灯光信息在vertex shader阶段得到了;
  - per vectex lighting
    根据三角形的三个顶点，通过数学公式计算出三角形内的一个点受三个点各部分的影响程度;
  - per pixel lighting
    对每一个顶点进行光照信息计算，利用光源向量，法线向量，射线向量;

## Per-sample Operations

- Merge阶段会做一系列test,如blend test,stencil test,depth test等;

# vertex buffer object and vertex array object in OpenGL

- vertex buffer obejct(VBO)
  - VBO中记录了如坐标，纹理，颜色等信息
- vertex array object(VAO)
  - VAO记录了定点属性列表，通过bind来确定如何读取VBO中的数据
- 链接
  - <https://www.zhihu.com/question/30095978>