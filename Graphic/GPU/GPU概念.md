# GPU概念和基础架构

## 名词解释

- GPC:Graphic Process Cluster（图形处理簇）
- TPC:Texture Process Cluster（纹理处理簇）
- Thread:线程
- SM:Stream Multiprocessor（流式多处理器）
- Warp:多个线程为一组称为Warp
- SP:Streaming Processor（流处理器）
- Core:执行数学运算的核心单元
- ALU:Arithmetic and logic unit（逻辑运算处理单元）
- FPU:Float point unit（浮点运算单元）
- SFU:Special function unit（特殊函数单元）
- ROP:Render output unit（渲染输出单元）
- Load/Store Unit:加载存储单元
- L1 Cache:L1缓存
- L2 Cache:L2缓存
- Shared Memory:共享内存
- Register File:寄存器
- Poly Morph Engine:多边形引擎
- Execution Context:执行上下文

## 核心组件结构与关系

### 包含关系
- GPC
  - TPC
    - SM
      -  Poly Morph Engine
         -  Vertex Fetch
         -  Tessellator
         -  Viewport Transform
         -  Attribute Setup
         -  Stream Output
      -  Core
         -  ALU
         -  FPU
         -  Execution Context
         -  Detch
         -  Decode
      -  L1 Cache
         -  Instruction L1
         -  Data L1
      -  Shared memory
      -  SP
      -  SFU
      -  Instruction Fetch/Dispatch
    - TEX  