# std API

```c++
/** std::enable_if_t ： 当条件满足时，类型才有效 */

// STRUCT TEMPLATE enable_if
template <bool _Test, class _Ty = void>
struct enable_if {}; // no member "type" when !_Test

template <class _Ty>
struct enable_if<true, _Ty> { // type is _Ty for _Test
    using type = _Ty;
};

template <bool _Test, class _Ty = void>
using enable_if_t = typename enable_if<_Test, _Ty>::type;  // 当_Test = true时，_Ty类型有效
```

```c++
/** std::thread : 线程 */
/** std::mutex : 互斥锁 */
#include <thread>
#include <mutex>
class NewThread {
public:
  int num;
  std::mutex _mutex;
  void addNum()
  {
    // 1.manully lock and unlock
    _mutex.lock();
    num++;
    _mutex.unlock();
    // 2. automatic unlock
    std::lock_guard<std::mutex> guard(_mutex);
    num++; 
  }
};
int main(int argc, const char * argv[])
{
  std::thread newThread(&NewThread::addNum, this);
  newThread.join(); // call creation thread will wait until new thread finished;
  newThread.detach(); // new thread will detach and call creation thread not wait;
}
```



