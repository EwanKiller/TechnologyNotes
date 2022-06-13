# adb tool

```bash

// 查询包名
adb shell pm list packages 

// 查询app启动activity,输出log中cmp=<packageName/ActivityName>
adb shell monkey -p <package name> -v -v -v 1    

// 启动app
adb shell am start -n <packageName/ActivityName>

// 强制停止应用
adb shell am force-stop <packageName>
```
