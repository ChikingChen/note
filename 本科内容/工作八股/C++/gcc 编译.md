# gcc 编译

一个文件夹下面三个文件分别为function.h function.cpp main.cpp

- function.h

  ```c++
  #ifndef FUNCTIONS_H
  #define FUNCTIONS_H
  
  // 函数声明
  int add(int a, int b);
  void printMessage(const char* message);
  
  #endif
  ```

- function.cpp

  ```c++
  #include "function.h"
  #include <iostream>
  
  // 实现加法
  int add(int a, int b) {
      return a + b;
  }
  
  // 实现打印消息
  void printMessage(const char* message) {
      std::cout << message << std::endl;
  }
  ```

- main.cpp

  ```c++
  #include "function.h"
  #include <iostream>
  
  int main() {
      int result = add(3, 5);  // 调用其他文件中的函数
      printMessage("Result:");
      std::cout << result << std::endl;
      return 0;
  }
  ```

function.h中定义了两个函数头而没有指明函数的具体实现形式

function.cpp中指明了函数的具体实现形式

main.cpp定义了main函数

- 编译命令

  ```
  g++ -c function.cpp
  g++ -c main.cpp
  ```

  将function.cpp main.cpp编译为function.o main.o，.o文件即为目标文件，即我们常认为的二进制文件

- 链接命令

  ```c++
  g++ -o main main.o function.o
  ```

  使用-o命令将.o文件链接为可执行文件

  - main.o不可直接链接为执行文件但可以编译为目标文件

    主要聚焦于函数的实现形式

    在main.cpp文件编译过程中我们将function.h文件展开即可得到函数头，而没有定义函数头的具体实现形式，将main.cpp文件编译为.o文件之后，.o文件中函数头描述为函数描述符，但因为没有具体的实现形式不能直接链接为可执行文件

## argc argv

argc为argv中成员个数

argv中的第一条为当前可执行文件位于计算机中的绝对位置，其后为命令行参数逐个排布

