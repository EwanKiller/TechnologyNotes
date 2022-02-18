# cocos creator 指南

## cocos creator 如何使用HTTP请求服务

  - 由旧的 `cc.loader.getXMLHttpRequest()`换成了`getXMLHttpRequest()`

## cocos creator 键盘事件如何使用

```typescript
    systemEvent.on(SystemEvent.EventType.KEY_Down,this.onPressDown,this);
    systemEvent.off(SystemEvent.EventType.KEY_Down,this.onPressDown,this);

    private onPressDown(event: EventKeyboard): void {
        switch(event.keyCode){
            xxx
        }
    }
```

## cocos creator 怎么声明可以拖拽对象进去的变量

```typescript
    @property(Node)
    //@ts-ignore    (如果在严格模式下编程，必须加这行,或者你在声明的时候初始化好)
    private cube1: Node = null;
```

## cocos creator web调试时关于onDestroy()

- web调试的时候，onDestroy()并不会在直接关闭网页的时候调用

## cocos creator 如何保存数据到本地

```typescript
     import {sys} from 'cc'

     sys.localStorage.setItem(key,value); // 保存
     sys.localStorage.getItem(key); // 读取
     sys.localStorage.remove(key); // 删除指定
     sys.localStorage.clear();    // 清除全部
```

## Cocos Creator 中如何使用Protobuf

1. 安装protobufjs到指定项目，在项目文件夹中打开Terminal
```
npm install --save protobufjs
```

2. 修改项目的配置文件tsconfig.json
```JSON
{
  /* Base configuration. Do not edit this field. */
  "extends": "./temp/tsconfig.cocos.json",

  "compilerOptions": {
      "allowSyntheticDefaultImports": true, // 需要开启
  }
}
```

3. 导入protobufjs模块
```typescript
import protobuf from 'protobufjs';

const root = protobuf.Root.fromJSON(/* ... */);
```

- 官方参考链接：
- https://docs.cocos.com/creator/3.0/manual/zh/scripting/modules/example-protobufjs.html

## JSB 2.0使用总结

### 安装教程

1. Fork source code from git engine-native；

2. Terminal in code folder

   ```
   npm install -g gulp  // 安装gulp
   npm install    // 安装依赖环境
   gulp init    // 编译
   ```

3. Open Cocos engine launcher editor - preference - Engine Manager；

4. Uncheck use built-in TS engine and uncheck use built-in native engine；

5. Select native engine source code path for Custom native engine；

### gulp init error

1. Error : gulp:ReferenceError: primordials is not defined

   - Solution : node 版本太新，官方解释需要降级至v12之前版本

     ```
     npm i -g n
     sudo n v11
     node --version
     ```

2. Error : (node:51177) ExperimentalWarning: queueMicrotask() is experimental. TypeError: gulp.series is not a function 

   - Solution : gulp版本太低，需要更新到4.0以上；

     ```
     npm install gulpjs/gulp.git#4.0 --save-dev
     ```

### 在引擎现有class中自定义func

1. declare a custom func and implement

   ```c++
   // FileUtils.h
   public:
       static void ewanTest();
   
   // FileUtils.cpp
   void FileUtils::ewanTest() {
       SE_LOGD("ewan test \n");
   }
   ```

2. use cocos macro SE_DECLARE_FUNC to declare a func , then implement it in .cpp and use cocos macro SE_BIND_FUNC to bind it , final to register it

   ```c++
   // jsb_cocos_auto.h
   SE_DECLARE_FUNC(js_engine_FileUtils_ewanTest);
   
   // jsb_cocos_auto.cpp
   static bool js_engine_FileUtils_ewanTest(se::State& s)
   {
       cc::FileUtils* cobj = SE_THIS_OBJECT<cc::FileUtils>(s);
       cobj->ewanTest();
       return true; 
   }
   SE_BIND_FUNC(js_engine_FileUtils_ewanTest);
   
   bool js_register_engine_FileUtils(se::Object* obj)
   {
       auto cls = se::Class::create("FileUtils", obj, nullptr, nullptr);
       ...
       cls->defineFunction("ewanTest", _SE(js_engine_FileUtils_ewanTest));  // register custom func
       ...
   }
   ```

3. call custom func by jsb in cocos creator

   ```javascript
   // test.ts
   start () {
       jsb.fileUtils.ewanTest();
   }
   ```

- 官网文档链接：
  - https://docs.cocos.com/creator/3.1/manual/zh/advanced-topics/engine-customization.html

## Animation system

### Animation

1. 动画编辑器
   - 选中挂有Animation\SkeletonAnimation组件的对象，按Ctrl/Cmd + E进入；
   - 动画编辑器设置时间帧：https://docs.cocos.com/creator/3.2/manual/zh/editor/animation/animation-event.html；
     - 编辑器内绑定事件名，代码里做事件的实现；
2. 动画状态
   - 播放时间
     - 累计播放时间 animState .time
     - 进度时间 animState.current(readOnly)
   - 循环模式
     - forward play
       - Normal
       - Loop
       - PingPong
     - reverse play
       - Reverse
       - LoopReverse
       - PingPongReverse
   - 循环次数
     - animState.repeatCount
   - 播放控制
     - readOnly
       - isPlaying
       - isPause
       - isMotionless
     - func
       - play
       - pause
       - resume
       - stop

### SkeletonAnimation

- 组件上的 useBakedAnimation负责控制切换两种类型；
- prefab 中全部默认使用预烘焙系统，以达到最佳性能。建议只在明显感到预烘焙系统的表现力无法达标的情况下，再使用实时计算系统；
- 支持运行时无缝切换；

### 蒙皮算法

- LBS（线性混合蒙皮）
  - 骨骼信息以 3x4 矩阵形式存储，直接对矩阵线性插值实现蒙皮，有体积损失等典型已知问题。
- DQS（双四元数蒙皮）
  - 骨骼信息以双四元数形式插值，对不含有缩放变换的骨骼动画效果更精确自然，但出于性能考虑，对所有缩放动画有近似简化处理。
- 引擎默认使用 LBS；
  - 可以通过修改引擎 skeletal-animation-utils.ts 的 updateJointData 函数引用与 cc-skinning.chunk 中的头文件引用来切换蒙皮算法；
- 对蒙皮动画质量有较高追求尝试启用 DQS
  - 但因 GLSL 400 之前都没有 fma 指令，如 cross等操作在某些 GPU 上无法绕过浮点抵消问题，误差较大，可能引入部分可见瑕疵；

## 挂点系统（socket）

## 动态instancing

## 实现3D换装

- 保证全部动画使用同一套骨骼
- 替换SkinnedMeshRender的Mesh蒙皮即可

## 编辑器模式下直接执行代码

```typescript
const { ccclass, executeInEditMode } = _decorator;

@ccclass('EditorComponent')
@executeInEditMode
export class EditorComponent extends Component {
}
```



