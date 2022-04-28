# 通过hello_xr demo分析Open XR_SDK_Source的开发方式



## 代码调用结构

- main.cpp - call android_main - call CreatePlatformPlugin

  - platformplugin.h - Impl CratePlatformPlugin
    - platformplugin_factory.cpp - declare CreatePlatformPlugin_Android // 平台相关信息
      - platformplugin_android.cpp - Impl CreatePlatformPlugin_Android -> struct AndroidPlatformPlugin -> return IPlatformPlugin ; // [Android平台相关信息](#platformplugin_android)
- main.cpp - call android_main - call CreateGraphicsPlugin
  - graphicplugin.h - Impl CreateGraphicsPlugin
    - graphicplugin_factory.cpp - declare  CreateGraphicsPlugin_OpenGLES // RHI相关信息
      - graphicplugin_opengles.cpp - Impl CreateGraphicsPlugin_OpenGLES -> struct OpenGLESGraphicsPlugin -> return IGraphicsPlugin ; // [OpenGLES相关信息](#graphicplugin_opengles)
- main.cpp - call android_main - call CrateOpenXrProgram
  - openxr_program.cpp - Impl CrateOpenXrProgram -> struct OpenXrProgram -> return IOpenXrProgram ; // OpenXR Program相关信息
- main.cpp - call android_main - [InitializeOpenXrLoader](#初始化OpenXR Loader)
- main.cpp - call program -> CreateInstance
  - openxr_program.cpp -> Impl CreateInstance -> call CreateInstanceInternal
    - openxr.program.cpp -> call LogLayersAndExtensions - call LoaderXrEnumerateInstanceExtensionProperties
      - loader_core.cpp -> Impl [LoaderXrEnumerateInstanceExtensionProperties](#LoaderXrEnumerateInstanceExtensionProperties)
        - loader_core.cpp -> call RuntimeInterface::LoadRuntime // 重点！加载runtime的地方；
          - runtime_interface.cpp -> call RuntimeManifestFile::FindManifestFiles  
            - manifest_file.cpp -> Impl RuntimeManifestFile::FindManifestFiles















## OpenXR重要解析点

<a name="platformplugin_android"></a>

```c++
// platformplugin_android.cpp

XrInstanceCreateInfoAndroidKHR instanceCreateInfoAndroid;

std::vector<std::string> GetInstanceExtensions() const override { return {XR_KHR_ANDROID_CREATE_INSTANCE_EXTENSION_NAME}; }

XrBaseInStructure* GetInstanceCreateExtension() const override { return (XrBaseInStructure*)&instanceCreateInfoAndroid; }

```

- 获取Open XR的extension的name以及Android平台的插件的创建信息数据结构；

<a name="graphicplugin_opengles"></a>

```c++
// graphicplugin_opengles.cpp

XrGraphicsBindingOpenGLESAndroidKHR m_graphicsBinding{XR_TYPE_GRAPHICS_BINDING_OPENGL_ES_ANDROID_KHR};

std::vector<std::string> GetInstanceExtensions() const override { return {XR_KHR_OPENGL_ES_ENABLE_EXTENSION_NAME}; }
```

- 获取OpenXR的OpenGLES的图形绑定信息以及extension的name；

<a name="初始化OpenXR Loader"></a> 

```c++
// main.cpp

PFN_xrInitializeLoaderKHR initializeLoader = nullptr;
if (XR_SUCCEEDED(xrGetInstanceProcAddr(XR_NULL_HANDLE, "xrInitializeLoaderKHR", (PFN_xrVoidFunction*)(&initializeLoader)))) 
{
  // 用来初始化loader时传入的设置信息
  XrLoaderInitInfoAndroidKHR loaderInitInfoAndroid;
  memset(&loaderInitInfoAndroid, 0, sizeof(loaderInitInfoAndroid));
  loaderInitInfoAndroid.type = XR_TYPE_LOADER_INIT_INFO_ANDROID_KHR;
  loaderInitInfoAndroid.next = NULL;
  loaderInitInfoAndroid.applicationVM = app->activity->vm;
  loaderInitInfoAndroid.applicationContext = app->activity->clazz;
	// 初始化openxr loader
  initializeLoader((const XrLoaderInitInfoBaseHeaderKHR*)&loaderInitInfoAndroid);
  ...
}
```

- xrGetInstanceProcAddr函数的声明在openxr.h中，实现在loader_core.cpp中，即libopenxr_loader.so中；
- 此处通过传入"xrInitializeLoaderKHR"，可以获取到函数LoaderXrGetInstanceProcAddr的指针，即最后一行等价于`LoaderXrGetInstanceProcAddr((const XrLoaderInitInfoBaseHeaderKHR*)&loaderInitInfoAndroid)`；

- 实际上初始化的操作如下：

  ```c++
  XrResult LoaderInitData::initialize(const XrLoaderInitInfoBaseHeaderKHR* info) {
      if (info->type != XR_TYPE_LOADER_INIT_INFO_ANDROID_KHR) {
          return XR_ERROR_VALIDATION_FAILURE;
      }
      auto cast_info = reinterpret_cast<XrLoaderInitInfoAndroidKHR const*>(info);
  
      if (cast_info->applicationVM == nullptr) {
          return XR_ERROR_VALIDATION_FAILURE;
      }
      if (cast_info->applicationContext == nullptr) {
          return XR_ERROR_VALIDATION_FAILURE;
      }
      _data = *cast_info;
      jni::init((jni::JavaVM*)_data.applicationVM);
      _data.next = nullptr;
      _initialized = true;
      return XR_SUCCESS;
  }
  ```




<a name="LoaderXrEnumerateInstanceExtensionProperties"></a>

```c++
// loader_core.cpp

static XRAPI_ATTR XrResult XRAPI_CALL
LoaderXrEnumerateInstanceExtensionProperties(const char *layerName, uint32_t propertyCapacityInput, uint32_t *propertyCountOutput,XrExtensionProperties *properties) XRLOADER_ABI_TRY {
    bool just_layer_properties = false;
    LoaderLogger::LogVerboseMessage("xrEnumerateInstanceExtensionProperties", "Entering loader trampoline");

    // "Independent of elementCapacityInput or elements parameters, elementCountOutput must be a valid pointer,
    // and the function sets elementCountOutput." - 2.11
    if (nullptr == propertyCountOutput) {
        return XR_ERROR_VALIDATION_FAILURE;
    }

    if (nullptr != layerName && 0 != strlen(layerName)) {
        // Application is only interested in layer's properties, not all of them.
        just_layer_properties = true;
    }

    std::vector<XrExtensionProperties> extension_properties = {};
    XrResult result;

    {
        // Make sure the runtime isn't unloaded while this call is in progress.
        std::unique_lock<std::mutex> loader_lock(GetGlobalLoaderMutex());

        // Get the layer extension properties
        result = ApiLayerInterface::GetInstanceExtensionProperties("xrEnumerateInstanceExtensionProperties", layerName,
                                                                   extension_properties);
        if (XR_SUCCEEDED(result) && !just_layer_properties) {
            // If not specific to a layer, get the runtime extension properties
            result = RuntimeInterface::LoadRuntime("xrEnumerateInstanceExtensionProperties");
            if (XR_SUCCEEDED(result)) {
                RuntimeInterface::GetRuntime().GetInstanceExtensionProperties(extension_properties);
            } else {
                LoaderLogger::LogErrorMessage("xrEnumerateInstanceExtensionProperties",
                                              "Failed to find default runtime with RuntimeInterface::LoadRuntime()");
            }
        }
    }

    if (XR_FAILED(result)) {
        LoaderLogger::LogErrorMessage("xrEnumerateInstanceExtensionProperties", "Failed querying extension properties");
        return result;
    }

    // If this is not in reference to a specific layer, then add the loader-specific extension properties as well.
    // These are extensions that the loader directly supports.
    if (!just_layer_properties) {
        for (const XrExtensionProperties &loader_prop : LoaderInstance::LoaderSpecificExtensions()) {
            bool found_prop = false;
            for (XrExtensionProperties &existing_prop : extension_properties) {
                if (0 == strcmp(existing_prop.extensionName, loader_prop.extensionName)) {
                    found_prop = true;
                    // Use the loader version if it is newer
                    if (existing_prop.extensionVersion < loader_prop.extensionVersion) {
                        existing_prop.extensionVersion = loader_prop.extensionVersion;
                    }
                    break;
                }
            }
            // Only add extensions not supported by the loader
            if (!found_prop) {
                extension_properties.push_back(loader_prop);
            }
        }
    }

    auto num_extension_properties = static_cast<uint32_t>(extension_properties.size());
    if (propertyCapacityInput == 0) {
        *propertyCountOutput = num_extension_properties;
    } else if (nullptr != properties) {
        if (propertyCapacityInput < num_extension_properties) {
            *propertyCountOutput = num_extension_properties;
            LoaderLogger::LogValidationErrorMessage("VUID-xrEnumerateInstanceExtensionProperties-propertyCountOutput-parameter",
                                                    "xrEnumerateInstanceExtensionProperties", "insufficient space in array");
            return XR_ERROR_SIZE_INSUFFICIENT;
        }

        uint32_t num_to_copy = num_extension_properties;
        // Determine how many extension properties we can copy over
        if (propertyCapacityInput < num_to_copy) {
            num_to_copy = propertyCapacityInput;
        }
        bool properties_valid = true;
        for (uint32_t prop = 0; prop < propertyCapacityInput && prop < extension_properties.size(); ++prop) {
            if (XR_TYPE_EXTENSION_PROPERTIES != properties[prop].type) {
                properties_valid = false;
                LoaderLogger::LogValidationErrorMessage("VUID-XrExtensionProperties-type-type",
                                                        "xrEnumerateInstanceExtensionProperties", "unknown type in properties");
            }
            if (properties_valid) {
                properties[prop] = extension_properties[prop];
            }
        }
        if (!properties_valid) {
            LoaderLogger::LogValidationErrorMessage("VUID-xrEnumerateInstanceExtensionProperties-properties-parameter",
                                                    "xrEnumerateInstanceExtensionProperties", "invalid properties");
            return XR_ERROR_VALIDATION_FAILURE;
        }
        if (nullptr != propertyCountOutput) {
            *propertyCountOutput = num_to_copy;
        }
    } else {
        // incoming_count is not 0 BUT the properties is NULL
        return XR_ERROR_VALIDATION_FAILURE;
    }
    LoaderLogger::LogVerboseMessage("xrEnumerateInstanceExtensionProperties", "Completed loader trampoline");
    return XR_SUCCESS;
}
```

