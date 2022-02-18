## 场景图/场景树

- 一种常用的场景对象组织方式；
- 把场景中的对象，按照一定的规则（通常是空间关系）组织成一棵树，树上的每个node代表场景中的一个对象；
- 每个node可以有零到多个child node，但只能有一个parent node；
- 每个node都包含一个空间定义，通过一个4x4的矩阵表示；
- 该node所在的空间就叫“local space”，每个child node的空间定义在parent node的local space中，当渲染某个node的Mesh时，需要计算Model Matrix，也就是需要把顶点从Local Space转换到World Space的矩阵，即`nodeA.modelMatrix = NodeA.parent.modelMatrix * nodeA.localMatrix`；这里会以递归去计算，直到SceneGraph的root node；



## Mesh的数据组织

- 图形API普遍都支持下面三种Mesh的拓扑结构：
  - Triangle List ： 每三个顶点组成一个三角形；
  - Triangle Strip ：每增加一个顶点，和前面的两个顶点组成一个三角形（共享前面两个顶点）；
  - Triangle Fan ：每增加两个顶点，和第0个顶点组成一个三角形（共享第0个顶点）；
- OpenGL支持两种向GPU提交Mesh数据的方式：
  - DrawArrays方式
    - 把Mesh中的三角形的所有顶点数据，按照上面那三种方式之一排列形成一个顶点数组，就可以让GPU去绘制所有这些三角形；
  - DrawElement方式
    - 一个Mesh中很多三角形是互相连接在一起，所以会共用一条边，则这条边的两个顶点可以共享，可以引入“vertex index”；所有的顶点放在一个数组中，然后另外使用一个index buffer通过数组下标来引用；

## 顶点数据格式

- 一个顶点可能有多项数据，在OpenGL API中成为Attribute，该数据有两种排列方式被支持，但是效率上有所差别：
  - Struct of arrays
    - 把顶点位置，法线等各自定义一个结构体的数组提交给GPU；
  - Array of structs
    - 把每个顶点定义一个结构体：

```
struct Vertex {

    vec3 pos;

    vec4 normal;

    vec2 uv;

}
```

- Array of structs方式的Vertex Buffer在内存布局上是连续的，GPU访问起来更加高效，并且Vertex Buffer保持4字节对齐，有助于提高GPU的内存访问效率；