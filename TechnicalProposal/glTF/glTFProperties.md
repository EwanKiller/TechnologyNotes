## **资源(Asset)**

- 每个glTF必须包含一个asset属性。一个合法的glTF文件，在顶层属性中，也就这么一个必须要有的属性。

```
{

 "asset": {

 "version": "2.0",

 "generator": "collada2gltf@f356b99aef8868f74877c7ca545f2cd206b9d3b7",

 "copyright": "2017 (c) Khronos Group"

 }

}
```

## **索引和命名(Indices and Names)**

- glTF文件顶层有bufferView和buffers两个数组类型的object。bufferView通过其下层的buffer来引用buffers中对应的buffer。如

```
{

 "buffers": [

 {

 "byteLength": 1024,

 "uri": "path-to.bin"

 }

 ],

 "bufferViews": [

 {

 "buffer": 0,

 "byteLength": 512,

 "byteOffset": 0

 }

 ]

}
```

- 该例中，bufferView和buffers都只有一个元素。bufferView对象的buffer属性为0，代表了bufferView引用了buffers中下标为0的元素。
- glTF文件顶层object都可以有一个name属性，用来显示。name属性可以缺失，也不保证唯一。

## **坐标和单位(Coordinate System and Units)**

- glTF采用右手坐标系

- 长度单位是米

- 角度单位为弧度

- 正向旋转为逆时针

- 颜色采用sRGB格式



## **场景(Scenes)**

- glTF可以包含0个或多个Scene，保存在scenes数组里。

- 另外有一个scene属性用来指定加载时要显示的那个scene。scene属性可以为空。

Nodes and Hierarchy

glTF中可以定义nodes，其中包含了场景中要渲染的实体。

- nodes 可以包含name属性

- nodes 可以包含坐标变换参数

- nodes 有父子结构。一个父节点用children数组对象保存儿子节点的下标（笔者认为这种方式不直观）

```
{

 "nodes": [

 {

 "name": "Car",

 "children": [1, 2, 3, 4]

 },

 {

 "name": "wheel_1"

 },

 {

 "name": "wheel_2"

 },

 {

 "name": "wheel_3"

 },

 {

 "name": "wheel_4"

 } 

 ]

}
```

Transformations

一个node可以定义一个本地空间变换参数，有两种方式：

1. 定义一个matrix对象
2. 定义TRS(translation, rotation, scale)对象

两种方式原则上可以相互转换。

## **二进制存储(Binary Data Storage)**

- Buffer 是二进制blob，可以包含geometry、animation和skin。

- Buffers 存储在buffers数组对象中

- Buffer 数据小端存储

- 一个buffer view可以包含定点索引(vertex indices)或属性(attributes)，但一个buffer view只能包含一种数据。

- buffers和bufferView不包含类型信息。

- 内容较大的数据(mesh、skins、annimations)应存放在buffers对象中，从而可以被accessor获取到。

## **几何信息(Geometry)**

- 一个node可包含一个mesh

- 一个mesh包含一个primitives数组对象

- 一个primitives可包含一个attributes对象

- attributes对象可包含以下属性：POSITION, NORMAL, TANGENT, TEXCOORD_0, TEXCOORD_1, COLOR_0, JOINTS_0, and WEIGHTS_0

- mesh可以用node.mesh属性进行实例化。相同的mesh可以被多个node引用，只是变换矩阵不同而已。如：

```
{

 "nodes": [

 {

 "mesh": 11

 },

 {

 "mesh": 11,

 "translation": [

 -20,

 -1,

 0

 ] 

 }

 ]

}
```

## **纹理(Texture Data)**

- Texture 可被定义在textures, images, samplers三种不同的对象中

## **材质(Materials)**

glTF中的核心材质系统通过一下信息通道支持金属/粗糙 PBR 工作流：

- 基础色

- 金属度

- 粗糙度

- 烘焙的环境光遮蔽

- Normal Map (tangent space, +Y up)

- 发射

## **相机(Cameras)**

- Camera定义从视图(view)坐标转换到剪辑(clip)坐标的投影矩阵。
- 相机(Cameras)保存在cameras数组对象中。
- 每个camera有一个type属性，代表投射类型，其值为perspective 或 orthographic。
- 每个camera有一个perspective 或 orthographic属性对象。

## **动画(Animations)**

- glTF通过节点(node)变换的关键帧动画支持关节(articulated)动画和蒙皮(skinned)动画。关键帧数据存储在缓冲区(buffers)中，并使用访问器在动画中引用。gltf2.0也以类似的方式支持实例化变形目标(Morph Targets)的动画。
- 所有动画都存储在"animations"数组中。动画定义为一组通道(channels)和一组采样器(samplers)，这些采样器使用关键帧数据和插值(interpolation)方法（采样器属性）指定访问器。

## **Specifying Extensions**

- glTF定义了一种扩展机制，允许用新功能扩展基本格式。任何glTF对象都可以具有可选的extensions属性。如下例所示：

```
{

 "material": [

 {

 "extensions": {

 "KHR_materials_common": {

 "technique": "LAMBERT"

 }

 }

 }

 ]

}
```

- glTF中使用的所有扩展必须列在顶级extensionsUsed数组对象中，例如

```
{

 "extensionsUsed": [

 "KHR_materials_common",

 "VENDOR_physics"

 ]

}
```

- 加载或呈现资产所需的所有glTF扩展必须列在顶级扩展所需的数组中，例如

```
{

 "extensionsRequired": [

 "WEB3D_quantized_attributes"

 ]

}
```

- extensionsRequired是extensionsUsed的一个子集。extensionsRequired中的所有值也必须存在于extensionsUsed中。

## **GLB File Format Specification**

glTF提供了两个也可以一起使用的交付选项(delivery options)：

- gltf json指向外部二进制数据（几何体、关键帧、皮肤）和图像。

- gltf json使用数据uri嵌入base64编码的二进制数据和图像。

由于采用了base64编码，glTF需要单独的请求或额外的空间。Base64编码需要额外的处理来解码，这增加了文件大小（对于编码资源，增加了约33%）。虽然gzip减轻了文件大小的增加，但是解压和解码仍然增加了大量的加载时间。

为了解决这个问题，引入了一种二进制格式的glTF。在二进制glTF中，glTF资产（JSON、.bin和images）可以存储在二进制blob中。