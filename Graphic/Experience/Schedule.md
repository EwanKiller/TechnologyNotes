## 学习计划

1. 基于Metal搭建一个IOS系统的输入系统;  （完成）
2. Metal的OC与C++,swift混编，先搭建一个iOS app的框架用来展示各个demo；（完成）
   1. https://developer.apple.com/tutorials/app-dev-training/creating-a-storyboard-app
3. 数学计算用到的函数尽量自己写，了解运算过程，遇到问题再对照Eigen；（完成）
4. 实现 Demo 漫游摄像机 ，可移动观察Cube  Sphere基本图元 ，对着Cocos的camera；（ing）
5. 场景树 - 强化矩阵变换的应用，3D空间对象之间的变化关系；
6. 纹理如何导入（软解PNG的过程用别人的库）；
7. 上纹理 + 天空盒 skybox；
8. 上光照；
9. render target；
10. 上阴影与高级光照技法（bloom或者延迟渲染等）；
11. 2D渲染管线；skia graphic engine，web里的 pixi；

## MathLib

- Vector

  - ```c++
    class Vector3f // vector(x,y,z)
    {
    public:
        float x,y,z;
        
        Vector3f();
        Vector3f(const float a, const float b, const float c);
        ~Vector3f();
        Vector3f& operator=(const Vector3f& vec);
        Vector3f& operator+(const Vector3f& vec);
        Vector3f& operator-(const Vector3f& vec);
        void normalize() const;
    };
    ```

- Matrix

  - ```c++
    class Matrix4x1f // 4行1列的矩阵
    {
    public:
        float col[4];
        
        Matrix4x1f();
        ~Matrix4x1f();
        Matrix4x1f(float x, float y, float z, float w);
        Matrix4x1f& operator>>(const float item[]);
    };
    ```

  - ```c++
    class Matrix4x4f // 4行4列的矩阵
    {
    public:
        // [row][column]
        float item[4][4];
        Matrix4x4f();
        /** column matrix but write in as row matrix for float array*/
        Matrix4x4f& operator>>(const float item[]);
        
        static void multiply(const Matrix4x4f &lhs, const Matrix4x1f &rhs, Matrix4x1f &outResult);
        static void multiply(const Matrix4x4f &lhs, const Matrix4x4f &rhs, Matrix4x4f &outResult);
    };
    ```

- Transform

  - ```c++
    class AffineTransform // 仿射变换
    {
        static void Translate(const Vector3f& offset, Vector3f& pos);
        static void RotateAxisX(const float angle, Vector3f& pos);
        static void RotateAxisY(const float angle, Vector3f& pos);
        static void RotateAxisZ(const float angle, Vector3f& pos);
        static void Scale(const Vector3f& factor, Vector3f& pos);
    };
    ```

    

## 关于在Metal工程中目前要做的东西

1. 实现一套自己的vector和matrix类；
2. 漫游摄像机本质上是ViewMatrix的变化，应该在Camera类中实现 输入cube顶点数据，由camera类完成MVP变化后输出，然后在放入Metal的MTLBuffer中，由Metal的渲染流程进行渲染绘制；

