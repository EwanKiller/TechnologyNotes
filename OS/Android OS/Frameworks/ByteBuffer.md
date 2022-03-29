# ByteBuffer

## 创建

```java
// buffer创建大小
int bufferSize = 32;

// heapByteBuffer创建在JVM中的堆区(Java的byte[])
ByteBuffer heapByteBuffer = ByteBuffer.allocate(bufferSize);

// directByteBuffer是操作系统的本地代码创建的内存区(c/c++的byte[])
ByteBuffer directByteBuffer = ByteBuffer.allocateDirect(bufferSize);
```

- HeapByteBuffer的创建速度更快，但是I/O效率不如DirectByteBuffer，所以当创建后会频繁进行I/O操作尽量使用DirectByteBuffer，如果需要频繁创建后仅少量I/O使用HeapByteBuffer更好

## 写入

```java
ByteBuffer buffer = ByteBuffer.allocate(32);

buffer.putInt(6);
Log.d("ByteBuffer","position:" + buffer.position()); // position: 4
buffer.putChar(4, 'c');
Log.d("ByteBuffer","position:" + buffer.position())
```

- put(value)和put(index, value)的区别在于，前者会移动buffer.position而后者不会移动；

## 读取

```java
int a = buffer.getInt();
char b = buffer.getChar(4);
Log.d("ByteBuffer", "a:" + a + " /b:" + b); // a:6 /b:c
```

- get()和get(index)的区别在于直接按index位开始读取对应的值;

## 重要参数

1. limit()
   代表实际可读的byte位数；
2. capacity()
   代表byte数组的最大容量；
3. position()
   代表当前数组的读写位置；
4. flip() vs rewind()
   都会把position设置为0，mark设置为-1；不同点在于flip会把当前位置设置给limit，rewind则是把capacity设置给limit；
