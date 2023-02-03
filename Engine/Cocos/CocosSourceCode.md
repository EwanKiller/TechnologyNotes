# Cocos source code analysis

- Cocos version 3.7.0

## Entry flow

- 以Android平台为例；

### Init阶段

- 从`Project/java/package_name/AppActivity`开始为Android的起点，`AppActivity`继承自`CocosActivity`；
- 在`CocosActivity`的`onCreate()`中：
    - `onLoadNativeLibrary`函数是为了加载cocos native库，即加载`libcocos.so`；
    - `onCreateNative`函数是一个jni函数，这里就是调向native code的入口；该函数的实现在`/native/cocos/platform/android/jni/JniCocosEntry.cpp`中；同时该cpp中会看到`android_main`函数，此函数为引擎native code的入口点；
    - 这里主要关注`androidPlatform->run`函数，此处进入loop循环，然后调用`source->process(_app,source) -> handleCmdProxy -> handleAppCommand -> case APP_CMD_INIT_WINDOW -> cocos_main`，最后的`cocos_main`函数，实际在`Classes/Game.cpp`中的宏`CC_REGISTER_APPLICATION(Game)`中实现，这里实际是创建了`Game`对象，并且接着调用了`Game::init()`函数；这里继续会调到`/native/cocos/application/BaseGame.cpp`中的`BaseGame::init()`函数，接着会调用`cc::CocosApplication::init()`,进来后首先会调用`/native/cocos/engine/Engine.cpp`中的`Engine::init()`函数，这里创建了`Scheduler`,`gfxDevice`等对象；
    - 这里的native class继承关系：`Game : BaseGame : CocosApplication : BaseApplication`；

    - 在`CocosApplication::init()`中还有一个重要的作用就是加载js层使用的函数或类对象与native层对应的绑定代码：
        - 首先会先调用`jsb_register_all_modules()`把全部定义了绑定对于js代码的native代码加载进来，然后调用`se->start()`，这里其实就是调用了v8引擎来进行绑定；
        - `se->start()`的实现在`native/cocos/bingdings/jswrapper/v8/ScriptEngine.cpp`中，最终会调用该cpp内的`callRegisteredCallback()`来逐个按照v8的要求进行绑定，重点关注:
        ```c++
        for (auto cb : _registerCallbackArray) {
            ok = cb(_globalObj);
            CC_ASSERT(ok);
            if (!ok) {
                break;
            }
        }
        ```
        - 这里`cb(_globalObject)`实际会挨个执行`native/cocos/bindings/auto/`文件夹下的全部cpp中的`bool(*function)(se::Object* obj)`；

    - 当`CocosApplication::init()`执行完成，我们回到`BaseGame::init()`中，接着会调用封装的v8函数读取js源码：`runScript("jsb-adapter/web-adapter.js");`和`runScript("main.js");`;