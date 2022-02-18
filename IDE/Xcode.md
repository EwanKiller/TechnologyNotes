# 记录一些 Xcode使用过程中的点

## 快捷键

- control + command + up arrow : 切换 .h与 .m文件
- control + command + left mouse key : 进入定义
- shift + command + o : 快速搜索函数名
- shift + command + F : 导航栏搜索
- command + F : 文件内搜索
- command + 1 : 选择导航栏-目录结构
- command + 0 : 关闭导航栏
- Command + a and contro + i ：自动排版

## 如何在Xcode中使用Eigen数学库

```
brew install eigen  // 安装eigen
brew list eigen // 查询eigen的安装路径
```

- 打开Xcode中的 project.xcodeproj，找到Header Search Paths，在里面填上brew 安装的eigen库的路径，一般来说是    /usr/local/Cellar/eigen/3.3.9/include/

## 如何安装和配置opencv

```
brew install opencv // 安装opencv
brew list opencv  // 查询opencv安装路径
```

- 打开Xcode中的 project.xcodeproj，找到Header Search Paths，在里面填上brew 安装的opencv库的路径，一般来说是 /usr/local/Cellar/opencv/4.5.2_4/include/opencv4/

- 找到Library Search Paths，一般来说填写 /usr/local/Cellar/opencv/4.5.2_4/lib

```
pkg-config --libs opencv4  // 得到所有的Linker Flags,粘贴内容设置其他链接器标志（Other Linker Flags）
```

```
-lopencv_gapi -lopencv_stitching -lopencv_alphamat -lopencv_aruco -lopencv_bgsegm -lopencv_bioinspired -lopencv_ccalib -lopencv_dnn_objdetect -lopencv_dnn_superres -lopencv_dpm -lopencv_face -lopencv_freetype -lopencv_fuzzy -lopencv_hfs -lopencv_img_hash -lopencv_intensity_transform -lopencv_line_descriptor -lopencv_mcc -lopencv_quality -lopencv_rapid -lopencv_reg -lopencv_rgbd -lopencv_saliency -lopencv_sfm -lopencv_stereo -lopencv_structured_light -lopencv_phase_unwrapping -lopencv_superres -lopencv_optflow -lopencv_surface_matching -lopencv_tracking -lopencv_highgui -lopencv_datasets -lopencv_text -lopencv_plot -lopencv_videostab -lopencv_videoio -lopencv_viz -lopencv_wechat_qrcode -lopencv_xfeatures2d -lopencv_shape -lopencv_ml -lopencv_ximgproc -lopencv_video -lopencv_dnn -lopencv_xobjdetect -lopencv_objdetect -lopencv_calib3d -lopencv_imgcodecs -lopencv_features2d -lopencv_flann -lopencv_xphoto -lopencv_photo -lopencv_imgproc -lopencv_core
```


- 找到Building Settings -> Other Linker Flags，添加Linker Flags

## 如何在Xcode中使用Eigen数学库

```
brew install eigen 
```

然后在打开Xcode中的 project.xcodeproj，找到Header Search Paths，在里面填上brew 安装的eigen库的路径，一般来说是    /usr/local/Cellar/eigen/3.3.9/include/