# system/lib相关

## 把system/lib下的xxx.so变为公共库

官方文档：https://source.android.google.cn/devices/tech/config/namespaces_libraries

```bash
# Android的系统库中分为: 
1.系统标准公共库(/system/etc/public.libraries.txt)
2.芯片厂商公共库(/vendor/etc/public.libraries.txt)
3.系统设备厂商公共库(/system/etc/public.libraries-companyName.txt)

# 类1不建议修改，类2在Android 8以上有额外限制，需要给so加标记，类3比较容易修改；

# 类3修改方法
1.在/system/etc/下加入public.libraries-ewan.txt；
2.so的名字必须为xxx.ewan.so；
3.32位和64位so分别在/system/lib/和/system/lib64/下；
```

