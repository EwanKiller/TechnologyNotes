- Whats VRM

  - VRM is a [GLTF-based, cross-platform file format that can handle the humanoid character/avatar (3D model data).](https://www.khronos.org/gltf/)/ VRM是基于GLTF，跨平台的一种文件格式，可以处理人形的3D模型数据；

  - “VRM” is a file format for handling 3D humanoid avatar (3D model) data for VR applications. / VRM是一种能够为VR应用处理3D人形模型的数据格式；

  - In addition, a standard implementation ([UniVRM](https://github.com/vrm-c/UniVRM)) in c# that can import and export VRM file in [Unity](https://unity3d.com/) is released as open source./ 可以通过Unity使用插件Uni VRM来导入和导出VRM格式文件；

  - VRM is not just a 3D model data. It is designed for being able to be used right away once being loaded into applications./ VR M不只是一种3D模型数据，它设计的目的是为了在不同应用之间仅仅通过单文件进行加载；

- 特点

  - The same avatar (VRM model) data can be used in any application that supports VRM. / 任何VRM模型都可以在支持该格式的应用中使用；

  - It is based on [glTF2.0](https://www.khronos.org/gltf/). Anyone is free to use it./ 它基于glTF2.0，任何人都可以自由使用；

  - all data including textures and materials is compacted as one file. To import the VRM model into applications, only one single file is needed./ 所有数据如纹理材质都被打包进一个文件，导入应用时仅需要一个文件；

  - Facial expressions can be manipulated via the controller./ 表情可以通过控制器操作；

  - Support three types of shaders [MToon](https://vrm.dev/en/docs/univrm/shaders/shader_mtoon/), [Unlit](https://vrm.dev/en/docs/univrm/shaders/univrm_unlit/), [PBR](https://vrm.dev/en/docs/univrm/shaders/univrm_standard/) / 支持卡通，无光照，基于物理的渲染三种类型的shader；

  - Standardize eye gaze control to handle the different types of models [eye gaze controlled by Bone](https://vrm.dev/en/docs/univrm/lookat/lookat_bone/), [eye gaze controlled by BlendShape](https://vrm.dev/en/docs/univrm/lookat/lookat_blendshape/), [eye gaze controlled by UV](https://vrm.dev/en/docs/univrm/lookat/lookat_uv/) / 三种标准化的眼神视线控制方法；

  - An implementation of spring physics that does not rely on the physical engine is provided for setting up the model’s hairs, clothes and so forth. [SpringBone](https://vrm.dev/en/docs/univrm/springbone/univrm_secondary/)；/ 不依赖于物理引擎实现的头发衣服之类的弹性物理效果；

- 用处

  - Create your own avatar with the character maker in VRM format. / 在支持VRM格式的角色编辑器里创建你自己的角色； 

  - Use your own avatar to host a live streaming event and show up on your friends’ live channels；/ 使用你自己的形象来直播或者在你朋友的直播频道展示；

  - After the live streaming, start a VR game to explore the VR world with your own avatar；/ 直播外，也可以使用你自己的形象探索VR世界；

  - After the game, move to the chat room in the VR world. Use the same avatar character to hang out with friends；/ 跟你的朋友在VR世界里闲逛；

  - Tomorrow join the VR study group in VR. Of course, the same avatar appearance；/ 加入VR学习小组使用同样的角色形象；