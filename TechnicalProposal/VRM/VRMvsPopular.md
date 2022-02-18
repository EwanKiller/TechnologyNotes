## VRM希望解决常规工作流中的问题

- The output data is depend on how creators make the 3D model and what modeling tools are used
  - The coordinate system, scale, initial pose, and the way to express expressions are all different…

- Needless to say, the way bones put into the 3D model is also different ; 

- 解决不同DDC软件和引擎之间坐标系，缩放，初始化姿态，骨骼点位置等等不同的问题；

- Each company has its own specifications for handling the 3D model data, which are way more complex than usual. Also, the necessary information about their file formats is not fully provided ;
  - Whether the FBX file, which is compatible with various software, is viable for an application and which version of the application can process the FBX file are main issues concerned by most of the users ;

- 解决不同公司之间有不同描述3D模型数据的问题；

- There is not enough necessary information available from the viewpoint of using the 3D model data as Avatar ;
  - For example, in the first-person view, how to get the right viewpoint position, how to exclude head due to the view blocking issue and so on ;

- 解决如何在Avatar中找到视点的位置；

- Support three types of shaders [MToon](https://vrm.dev/en/docs/univrm/shaders/shader_mtoon/), [Unlit](https://vrm.dev/en/docs/univrm/shaders/univrm_unlit/), [PBR](https://vrm.dev/en/docs/univrm/shaders/univrm_standard/)；
  - 支持卡通，无光照，基于物理的渲染三种shader，能够给不具备Shader开发或者该Shader已经能够满足需求的用户直接使用，而不需要学习这部分知识，直接按规则调参即可；

## VRM Features (for Developers)

- Right-handed Y-Up coordinate system ➡️ [Coordinate](https://vrm.dev/en/docs/univrm/programming/univrm_coordinate/) ;

- Metric unit is meter ;

- Humanoid bones ➡️ compatible with humanoid motion and motion capture ;

- T-pose as the initial posture (towards +Z-axis) ➡️ can be used directly for Third-Person-Shooter mode ;

- The initial pose has no rotation and scale ;

- The mesh is aligned with the Humanoid Skeleton in the initial pose (only skin’s bind matrices are not included) ;

- Expression/eye gaze manipulation are in BlendShapeProxy ➡️ [BlendShapeProxy](https://vrm.dev/en/docs/univrm/programming/univrm_use_blendshape/) ;

- Spring physics setup on clothes, hair and others ➡️ no interference with other physics

- VR settings are available ➡️ [FirstPerson](https://vrm.dev/en/docs/univrm/programming/univrm_use_firstperson/) ;

- Licenses settings such as permission to use the avatar, permissions for violent and sexual activities, etc. are defined ;

## Developer Web

- https://vrm.dev/en/docs/vrm/vrm_development/

## Runtime Import with UniVRM

- UniVRM can load VRM into Unity as a GameObject instead of a prefab. The imported GameObject is the same as the instantiated GameObject from Prefab.

## Runtime Export with UniVRM[ ](https://vrm.dev/en/docs/vrm/vrm_development/#runtime-export-with-univrm)

- UniVRM can export the VRM model at runtime in Unity. The export function can be used for applications such as character maker tool.

## VRM的限制

1. 如果需要从零制作一个VRM格式的资源，依然是常规的DDC工具先制作FBX模型和动画，然后将FBX模型和动画导入Unity，通过VRM Plugin For Unity来Retarget Bone，替换VRM Plugin预制的Material，然后再通过Plugin导出VRM格式资源；
2. VRM希望统一模型创建的坐标系，缩放，初始化姿势等，但是对于正规的美术工作流，会根据实际业务需求，统一规定坐标系，缩放比例，初始化姿势，骨骼点位置等，并且很多时候为了特殊需求需要做变动；
3. 关于VR中viewport位置的问题，往往很多时候需要根据业务设置，并不是一个恒定不变点，这一点实际上很难用VRM统一；
4. VRM有卡通，无光照，基于物理的渲染三种Shader，但是除非该Shader完全满足业务需求效果，否则的话由于Shader这种需求变化较大的资源，并不太适合统一；