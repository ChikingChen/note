# C++ 头文件

在C++执行文件相同目录的头文件中定义类、命名空间等等，在执行文件中使用双引号引起即可使用

## include防范

```c++
// my_class.h
#ifndef MY_CLASS_H // include guard
#define MY_CLASS_H

namespace N
{
    class my_class
    {
    public:
        void do_something();
    };
}

#endif /* MY_CLASS_H */
```

头文件在源文件中展开后若未引用过则定义MY_CLASS_H，若已经定义过则不满足ifndef MY_CLASS_H

