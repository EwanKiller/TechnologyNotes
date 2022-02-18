# 教程地址

https://vulkan-tutorial.com/Development_environment#page_MacOS

## Xcode 配置问题

1. 报错如下：

```
dyld: Library not loaded: /usr/local/opt/glfw/lib/libglfw.3.dylib
  Referenced from: /Users/durdukolk/Library/Developer/Xcode/DerivedData/Graphic-dutmtfjytmmfetggltqeajfkggms/Build/Products/Debug/Graphic
  Reason: no suitable image found.  Did find:
    /usr/local/opt/glfw/lib/libglfw.3.dylib: code signature in (/usr/local/opt/glfw/lib/libglfw.3.dylib) not valid for use in process using Library Validation: mapped file has no cdhash, completely unsigned? Code has to be at least ad-hoc signed.
    /usr/local/Cellar/glfw/3.3.4/lib/libglfw.3.3.dylib: code signature in (/usr/local/Cellar/glfw/3.3.4/lib/libglfw.3.3.dylib) not valid for use in process using Library Validation: mapped file has no cdhash, completely unsigned? Code has to be at least ad-hoc signed.
```

解决方案：https://stackoverflow.com/questions/67049987/xcode-gives-error-when-i-add-libraries-dyld-library-not-loaded

