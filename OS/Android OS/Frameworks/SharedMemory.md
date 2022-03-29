# Android SharedMemory

## 创建

```java
// 创建一个SharedMemory，内存长度为8
int memorySize = 8;
SharedMemory sharedMemory = SharedMemory.create("Memory Name", memorySize);

// 通过mapReadWrite函数map一个可以读写的ByteBuffer
ByteBuffer buffer = sharedMemory.mapReadWrite();

// 在ByteBuffer中放入一个 int类型的4
buffer.putInt(4);

// 默认创建的SharedMemory的权限为读写，写入完成后改为只读，防止被改写
sharedMemory.setProtect(OsConstants.PROT_READ);

// map得到的ByteBuffer使用完后，umap释放
SharedMemory.umap(buffer);
```

## 传递

### 定义AIDL文件

```java
import android.os.SharedMemory;

interface IAidlInterface {
    SharedMemory getSharedMemory();
}
```

### 通过Binder跨进程

1. Service中实例化Binder对象，通过onBind函数返回对象；
   ```java
   pubic class BinderS extens Service {

       private static final IAidlInterface binder = new IAidlInterface.Stub() {
           @Override
           public SharedMemory getSharedMemory() throws RemoteException {
   	    // 创建Shared Memory并返回
           }
       };

       @Override
       public IBinder onBind(Intent intent) {
           return binder.asBinder();
       }
   }
   ```
2. Activity通过bindService的onServiceConnected函数，拿到Service的Binder代理对象；
   ```java
   public class BinderC implements ServiceConnection {
       private IAidlInterface binderProxy = null;
       @Override
       public void onServiceConnected(ComponentName name, IBinder service) {
           binderProxy = IAidlInterface.Stub.asInterface(service);
       }
   }
   ```
3. 通过调用AIDL定义的接口对SharedMemory进行操作；
   ```java
   // Binder Service端
   SharedMemory sharedMemory = binder.getSharedMemory();
   // Binder C端
   SharedMemory sharedMemory = binderProxy.getSharedMemory();
   ```

## 读取

以Binder C端读取为例：

```java
// 获取SharedMemory
SharedMemory sharedMemory = binderProxy.getSharedMemory();

// 通过mapReadOnly函数map一个只读的ByteBuffer
ByteBuffer buffer = sharedMemory.mapReadOnly();

// 根据写入的规则读出结果
int result = buffer.getInt();
Log.d("SharedMemory", "SharedMemory value is " + result);

// umap释放掉map出来的ByteBuffer
SharedMemory.umap(buffer);
```
