---
date: 2018-04-19 00:00:00
title: "采坑记录"
category: "Coding"
tag: ["Dynamic Library", "Windows"]
---

最近在给封装一个动态库，需要支持古老的windows xp系统。而我的开发系统是windows 10，使用visual studio 2013作为IDE。

一通谷歌百度之后，我采用了曝光度最高的方法。具体来说包括两个步骤：

1. 在工程设置里，配置属性->常规->平台工具集，选择 Visual Studio 2013 - Windows XP (v120_xp)
2. 在工程设置里，配置属性->C/C++->代码生成->运行库，选择MT/MTd。分别对应于release和debug模式。

最初并没有发现这样做有什么问题，后来写了一个接口函数，release模式下没有发现问题，但是debug模式下调用该接口的函数在出栈时一直崩溃，错误如下

![Error](https://raw.githubusercontent.com/joshua19881228/my_blogs/master/Coding/20180419/error.png "Error")

因为对这一块儿实在不熟悉，就抱着死马当活马医的态度，把所有MT/MTd都改成了MD/MDd，又把所有依赖库和自己的库编译了一遍。在目标测试机上安装，居然成功了。

在网上搜了一些解释：

[Debug Assertion Failed! Expression: __acrt_first_block == header](https://stackoverflow.com/questions/35310117/debug-assertion-failed-expression-acrt-first-block-header)

>As this is a DLL, the problem might lie in different heaps used for allocation and deallocation (try to build the library statically and check if that will work).
>
>The problem is, that DLLs and templates do not agree together very well. In general, depending on the linkage of the MSVC runtime, it might be problem if the memory is allocated in the executable and deallocated in the DLL and vice versa (because they might have different heaps). And that can happen with templates very easily, for example: you push_back() to the vector inside the removeWhiteSpaces() in the DLL, so the vector memory is allocated inside the DLL. Then you use the output vector in the executable and once it gets out of scope, it is deallocated, but inside the executable whose heap doesn't know anything about the heap it has been allocated from. Bang, you're dead.
>
>This can be worked-around if both DLL and the executable use the same heap. To ensure this, both the DLL and the executable must use the dynamic MSVC runtime - so make sure, that both link to the runtime dynamically, not statically. In particular, the exe should be compiled and linked with /MD[d] and the library with /LD[d] or /MD[d] as well, neither one with /MT[d]. Note that afterwards the computer which will be running the app will need the MSVC runtime library to run (for example, by installing "Visual C++ Redistributable" for the particular MSVC version).
>
>You could get that work even with /MT, but that is more difficult - you would need to provide some interface which will allow the objects allocated in the DLL to be deallocated there as well. For example something like:
>
> ```
>__declspec(dllexport) void deallocVector(std::vector<std::string> &x);
>
>void deallocVector(std::vector<std::string> &x) {
>    std::vector<std::string> tmp;
>    v.swap(tmp);
>}
>```
>
>(however this does not work very well in all cases, as this needs to be called explicitly so it will not be called e.g. in case of exception - to solve this properly, you would need to provide some interface from the DLL, which will cover the vector under the hood and will take care about the proper RAII)
>
>EDIT: the final solution was actually was to have all of the projects (the exe, dll and the entire googleTest project) built in Multi-threaded Debug DLL (/MDd) (the GoogleTest projects are built in Multi-threaded debug(/MTd) by default)

说实话，对计算机原理的理解十分欠缺，遇到稍微专业一些的问题只能照着网上的一些方法试一试，如果成了也就不会再深入研究了，如果不成也不知道为什么不成，只能再去试别的方法。:-(