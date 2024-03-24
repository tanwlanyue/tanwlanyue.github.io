---
title: C++单例模式跨DLL
date: 2024-03-25 02:26:46
categories:
---

**C++单例模式跨DLL是不是就是会出问题?**

在DLL里有个单例类, 在调用dll的地方写instance调用多次, 就出现了内存分配问题. 这个问题在直接读写dll类内的static变量也有. 如果不在dll外使用instance() 而是dll对外接口都封装成静态函数, 就没有问题. 我独立建一个项目, 写一个简单的dll是不会有这个问题的, 那么又是什么情况造成这一现象的?

<!-- more -->

---

static变量的唯一性是动态库级别的，不同库包含**同一截代码**的话就会有多份static实例。出现问题的情况往往是，这截代码是实现在头文件里的，所以被不同模块引用时就生成了多份代码。

解决办法就是按照一般DLL导出接口的规则，在获取static变量的地方：

1. 对于实现单例的dll，使用dllexport。对于MSVC来说就是__declspec(dllexport)，对于GCC/clang就是__attribute__ ((dllexport))
2. 对于引用单例的dll，使用dllimport。对于MSVC/clang来说就是__declspec(dllimport)，对于GCC/clang就是__attribute__ ((dllimport))

这样就只有一份了。

作者：PaintDream
链接：https://www.zhihu.com/question/425920019/answer/2254241454
来源：知乎
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
