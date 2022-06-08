# C/Cpp Coding tips

## 线程tid 进程pid查看

- 进程pid

    ```c++
    #include <iostream>
    #include <csignal>
    
    int main(int argc, char *argv[]) {
        std::cout << "pid:" << getpid() << std::endl;
        return 0;
    }
    ```

- 线程tid(进程内唯一，在不同进程不唯一)

  ```c++
  #include <iostream>
  #include <pthread.h>
  int main() {
      std::cout << "tid:" << pthread_self << std::endl;
      return 0;
  }
  ```
  

- 线程pid（系统内唯一）

  ```c++
  #include <iostream>
  #include <sys/syscall.h>
  #include <csignal>
  
  int main() {
      std::cout << "pid:" << syscall(SYS_gettid) << std::endl;
      return 0;
  }
  ```

  
