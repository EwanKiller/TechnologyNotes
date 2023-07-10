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



## /system/lib64中放入so

```bash
adb root // 获取root权限,重启后需要重新获取root

adb remount // 重新挂载
// 出现 `Disabling verity for /system`
adb disable-verity // 禁用verity
adb reboot // 禁用后需要重启

// so在手机中，需要cp复制
adb shell cp <source.so in phone> /system/lib64
// so在自己PC上，需要push推送
adb shell push <souce.so in PC> /system/lib64

```

