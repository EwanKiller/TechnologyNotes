# 在VR中使用Single pass时默认的post process效果会出错

- 解决方案：https://docs.unity3d.com/cn/2020.3/Manual/SinglePassStereoRendering.html

# Unity Bake Lightmap 
- https://zhuanlan.zhihu.com/p/157992819

# Hololens2 中使用Custom shader
1. UNITY_VERTEX_INPUT_INSTANCE_ID 用在顶点着色器输入/输出结构中定义实例 ID，将引入SV_InstanceID用以区分当前Instance；并且对于输出结构来说是可选的，只有当需要在Fragment shader中访问Instance属性时才需要；
2. UNITY_VERTEX_OUTPUT_STEREO 用在顶点着色器输出结构中，表示将输出适用于双目渲染的顶点数据；
3. UNITY_SETUP_INSTANCE_ID() 根据 GPU 当前渲染的眼睛来计算Unity 着色器内置变量 unity_StereoEyeIndex 和 unity_InstanceID 并设置为正确值，必须用在Vertex shader的最前面，对于Fragment shader来说是可选项，当Fragment shader需要用到Instance 属性时；
4. UNITY_INITIALIZE_OUTPUT 用以初始化变量的各个分部为0；
5. UNITY_INITIALIZE_VERTEX_OUTPUT_STEREO 表示vertex shader阶段将初始化适用于双目渲染的顶点数据；
6. 对于在Hololens 2 中使用Custom shader主要保证：
  1. 在vertex shader的输入结构中使用`UNITY_VERTEX_INPUT_INSTANCE_ID `定义实例ID；
  2. 在vertex shader的输出结构中使用 `UNITY_VERTEX_OUTPUT_STEREO `来表示将输出适用于双目渲染的顶点数据；
  3. 在Vertex shader中使用`UNITY_SETUP_INSTANCE_ID`来设置使用SV_Instance ID内置变量；
  4. 在Vertex shader中使用`UNITY_INITIALIZE_VERTEX_OUTPUT_STEREO`来初始化要用于双目渲染的数据；
### 说明参考来源
1. [GPU 实例化 - Unity 手册 ](https://docs.unity3d.com/cn/current/Manual/GPUInstancing.html)
2. [单通道实例化渲染 - Unity 手册](https://docs.unity3d.com/cn/current/Manual/SinglePassInstancing.html)
3. [HoloLens 单通道立体渲染 - Unity 手册](https://docs.unity3d.com/cn/current/Manual/SinglePassStereoRenderingHoloLens.html)


# 好的教程
https://catlikecoding.com/unity/tutorials/