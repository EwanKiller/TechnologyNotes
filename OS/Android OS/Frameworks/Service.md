# Service

## 与Service通过Binder进行双向通信

### 检查设置

1. 设置Service的AndroidManifest的service属性和intent-filter属性：
```
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
  package="com.ewan.service">

  <application
    ...
    <service
      android:enabled="true"
      android:exported="true"
      ...
      <intent-filter>
        <category android:name="android.intent.category.DEFAULT"/>
      </intent-filter>
    </service>
  </application>

</manifest>
```

2. 设置Client端的AndroidMainifest的qureies属性：
```
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
  package="com.ewan.client">

  <queries>
    <package android:name="com.ewan.service.ExampleService"/>
  </queries>

</manifest>
```

### Aidl代码

1. IS2CAidlInterface.aidl

```java
package com.ewan.ipccommon;
// Service主动向Client发送消息时使用的Aidl
interface IS2CAidlInterface {
    void S2C(String content);
}
```

2. IC2SAidlInterface.aidl

```java
package com.ewan.ipccommon;

// Declare any non-default types here with import statements
import com.ewan.ipccommon.IS2CAidlInterface;
// Client向Service发送消息时使用的Aidl
interface IC2SAidlInterface {
    // 把Service主动发消息给自己的Stub传过去
    void SetClientStub(IS2CAidlInterface stub);
    void C2S(String content);
}
```

### Service端代码

```java
package com.ewan.service;

public class ExampleService extends Service {

  // 存Service主动向Client发送消息的stub对象，由Client绑定到Service时发送过来  
  private IS2CAidlInterface clientStub;

  private final IC2SAidlInterface ipc = new Stub() {

    @Override
    public void SetClientStub(IS2CAidlInterface stub) throws RemoteException {
        // 获取Service能够主动发消息给Client的Stub
        clientStub = stub;
    }

    @Override
    public void C2S(String content) throws RemoteException {
        // todo: 处理Client主动发来的消息
    }
  };

  @Override
  public IBinder onBind(Intent intent) {
      // 返回给Client能够主动发消息使用的Stub
    return ipc.asBinder();
  }

  private void SendMessageToClient() {
        // example:调用stub对象发送消息给Client
        clientStub.S2C(content);
  }
}
```

### Client端代码

```java
package com.ewan.client;

public class ExampleActivity extends Activity {
    // 存Client主动向Service使用的Stub对象，由Client绑定到Service时通过onServiceConnected函数返回获得
    private IC2SAidlInterface serverStub;

    // 声明Service主动发送消息给Client的Stub对象
    private final IS2CAidlInterface.Stub clientStub = new Stub() {
        @Override
        public void S2C(String content) throws RemoteException {
            //todo: 处理Service主动发送过来的消息
        }
    };

    private final ServiceConnection connection = new ServiceConnection() {
        @Override
        public void onServiceConnected(ComponentName name, IBinder service) {
            // 通过这里获取到Client主动发送消息给Service的Stub对象
            serverStub = IC2SAidlInterface.Stub.asInterface(service);
            try {
                // 把Service主动发送消息给Client的Stub对象传给Service
                serverStub.SetClientStub(clientStub);
            } catch (RemoteException e) {
                e.printStackTrace();
            }
        }

        @Override
        public void onServiceDisconnected(ComponentName name) {
        serverStub = null;
        }
    };

    private void SendMessageToService() {
        // example: 调用stub对象的方法进行消息发送
        serverStub.C2S("Hello, Service");
    }

    // 绑定到Service
    private void BindTargetService() {
        Intent intent = new Intent();
        intent.setClassName("com.ewan.service", "com.ewan.service.ExampleService");
        context.bindService(intent, connection, BIND_AUTO_CREATE);
    }
}
```

