---
date: 2015-04-16 00:00:00
title: "VS2013配置Caffe卷积神经网络工具（64位Windows 7）——建立工程"
category: "Computer Vision"
tag: "Caffe"
---

2014年4月的时候自己在公司就将Caffe移植到Windows系统了，今年自己换了台电脑，想在家里也随便跑跑，本来也装了Ubuntu可以很方便的配置好，无奈在家的风格是“娱乐的时候抽空学习”，所以移植到Windows还是很有必要的。但是，公司禁止将公司内部资料带出，很多地方又都忘记了，周末磨了一天终于移植完，本篇为记录将Caffe移植至Windows7 x64系统下的一些关键步骤。下面介绍如何从源码建立VS2013工程。

## Table of Content ##

* Content
{:toc}

## 1. 下载源码

从[https://github.com/BVLC/caffe](https://github.com/BVLC/caffe)下载caffe源码，master或者dev版本都可以，本文中以dev为例

## 2. 建立工程

新建一个空的控制台VS2013工程，起名为caffe-dev。

File --> New --> Project

选择Win32 Console Application

选择工程路径，比如D:\CODE\CXX

给项目命名caffe-dev

单击OK后选择Empty project 

## 3. 将工程的Solution Platform改为x64

Build --> Configuration Manager --> Active solution platform --> new --> x64 --> OK 

## 4. 设置CUDA编译

Project --> Build Customizations... --> CUDA 7.0 --> OK 

## 5. 设置一些工程下的路径

注意Debug和Release下都要设置

在Project --> Properties --> Configuration Properties --> VC++ Directories --> Include Directories中添加openblas、opencv、boost和cuda，例如

```
D:\LIBS\boost_1_57_0;D:\LIBS\OpenBLAS\include
D:\LIBS\opencv\build\include
D:\LIBS\opencv\build\include\opencv
D:\LIBS\opencv\build\include\opencv2
C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v7.0\include
```

在Project --> Properties --> Configuration Properties --> VC++ Directories --> Library Directories中添加openblas、opencv、boost和cuda，例如

```
D:\LIBS\OpenBLAS\lib
D:\LIBS\opencv\build\x64\vc12\lib
D:\LIBS\boost_1_57_0\lib
C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v7.0\lib\x64
```

## 6. 拷贝源码

将压缩包内的include、src和tools文件夹拷贝到工程根目录下，同时把整理好的3rdparty文件夹也拷贝到工程目录下 

## 7. 添加第三方依赖库路径

添加caffe的路径和3rdparty中第三方依赖库的路径，同样Debug和Release都要设置

在Project --> Properties --> Configuration Properties --> C/C++ --> General --> Additional Include Directories中添加

```
../include;../src
../3rdparty/include
../3rdparty/include/hdf5
../3rdparty/include/lmdb
```

在Project --> Properties --> Configuration Properties --> Linker --> General --> Additional Library Directories中添加

```
../3rdparty/lib
```

在Project --> Properties --> Configuration Properties --> Linker --> Input --> Additional Dependencies中添加

```
opencv_core2410.lib
opencv_highgui2410.lib
opencv_imgproc2410.lib
cudart.lib
cublas.lib
curand.lib
gflags.lib
libglog.lib
libopenblas.dll.a
libprotobuf.lib
libprotoc.lib
leveldb.lib
lmdb.lib
libhdf5.lib
libhdf5_hl.lib
Shlwapi.lib
```

注意在Debug下应添加xxxd.lib形式的静态库

```
opencv_core2410d.lib
opencv_highgui2410d.lib
opencv_imgproc2410d.lib
cudart.lib
cublas.lib
curand.lib
gflagsd.lib
libglogd.lib
libopenblas.dll.a
libprotobufd.lib
libprotoc.lib
leveldbd.lib
lmdbd.lib
libhdf5.lib
libhdf5_hl.lib
Shlwapi.lib
```

## 8. 生成pb文件

利用protoc.exe和caffe.proto生成caffe.pb.h和caffe.pb.cc文件

在编译protobuf的时候会生成一个可执行文件protoc.exe，在工程的D:\CODE\CXX\caffe-dev\src\caffe\proto路径下有一个文件叫caffe.proto，可以将protoc.exe拷贝到该路径下，在命令行中进入D:\CODE\CXX\caffe-dev\src\caffe\proto路径，输入命令

```
protoc.exe caffe.proto --cpp_output=./
```

可以看到在该路径下会生成caffe.pb.h和caffe.pb.cc，在D:\CODE\CXX\caffe\include\caffe中新建一个文件夹proto，将生成的两个文件移动到D:\CODE\CXX\caffe\include\caffe\proto中

## 9. 准备mkstemp.h和mkstemp.cpp文件。

将mkstemp.h放在include/caffe/util/路径下，mkstemp.h中的内容为

```c++
#include <windows.h>
#include <io.h>
int mkstemp(char *templates);
```

将mkstemp.cpp放在src/caffe/util/路径下，mkstemp.cpp中的内容为

```c++
/* mkstemp extracted from libc/sysdeps/posix/tempname.c.  Copyright
    (C) 1991-1999, 2000, 2001, 2006 Free Software Foundation, Inc.

    The GNU C Library is free software; you can redistribute it and/or
    modify it under the terms of the GNU Lesser General Public
    License as published by the Free Software Foundation; either
    version 2.1 of the License, or (at your option) any later version.  */

#include "caffe/util/mkstemp.hpp"
#include <errno.h>
#include <fcntl.h>
#include <sys/stat.h>

static const char letters[] =
"abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789";

/* Generate a temporary file name based on TMPL.  TMPL must match the
    rules for mk[s]temp (i.e. end in "XXXXXX").  The name constructed
    does not exist at the time of the call to mkstemp.  TMPL is
    overwritten with the result.  */
int mkstemp (char *tmpl)
{
    int len;
    char *XXXXXX;
    static unsigned long long value;
    unsigned long long random_time_bits;
    unsigned int count;
    int fd = -1;
    int save_errno = errno;

    /* A lower bound on the number of temporary files to attempt to
        generate.  The maximum total number of temporary file names that
        can exist for a given template is 62**6.  It should never be
        necessary to try all these combinations.  Instead if a reasonable
        number of names is tried (we define reasonable as 62**3) fail to
        give the system administrator the chance to remove the problems.  */
#define ATTEMPTS_MIN (62 * 62 * 62)

    /* The number of times to attempt to generate a temporary file.  To
        conform to POSIX, this must be no smaller than TMP_MAX.  */
#if ATTEMPTS_MIN < TMP_MAX
    unsigned int attempts = TMP_MAX;
#else
    unsigned int attempts = ATTEMPTS_MIN;
#endif

    len = strlen (tmpl);
    if (len < 6 || strcmp (&tmpl[len - 6], "XXXXXX"))
    {
        errno = EINVAL;
        return -1;
    }

/* This is where the Xs start.  */
    XXXXXX = &tmpl[len - 6];

    /* Get some more or less random data.  */
    {
    SYSTEMTIME      stNow;
    FILETIME ftNow;

    // get system time
    GetSystemTime(&stNow);
    stNow.wMilliseconds = 500;
    if (!SystemTimeToFileTime(&stNow, &ftNow))
    {
        errno = -1;
        return -1;
    }

    random_time_bits = (((unsigned long long)ftNow.dwHighDateTime << 32)
                        | (unsigned long long)ftNow.dwLowDateTime);
    }
    value += random_time_bits ^ (unsigned long long)GetCurrentThreadId ();

    for (count = 0; count < attempts; value += 7777, ++count)
    {
        unsigned long long v = value;

        /* Fill in the random bits.  */
        XXXXXX[0] = letters[v % 62];
        v /= 62;
        XXXXXX[1] = letters[v % 62];
        v /= 62;
        XXXXXX[2] = letters[v % 62];
        v /= 62;
        XXXXXX[3] = letters[v % 62];
        v /= 62;
        XXXXXX[4] = letters[v % 62];
        v /= 62;
        XXXXXX[5] = letters[v % 62];

        fd = open (tmpl, O_RDWR | O_CREAT | O_EXCL, _S_IREAD | _S_IWRITE);
        if (fd >= 0)
    {
        errno = save_errno;
        return fd;
    }
        else if (errno != EEXIST)
    return -1;
    }

    /* We got out of the loop because we ran out of combinations to try.  */
    errno = EEXIST;
    return -1;
}
```

## 10. 将caffe的源文件添加到工程中

将D:\CODE\CXX\caffe-dev\src\caffe路径下除了test文件中的所有源文件添加到工程中，如果想要跑一些测试代码，也可以把test中的源文件加进去

将D:\CODE\CXX\caffe-dev\include\caffe\proto下的caffe.pb.cc添加到工程中

将D:\CODE\CXX\caffe-dev\tools下的caffe.cpp添加到工程中 

## 11. 修改一些代码

在include/caffe/common.hpp中添加`#include <direct.h>`

在include/caffe/util/io.hpp中添加`#include "mkstemp.hpp"`，并将`close`函数替换为`_close`，将`char* mkdtemp_result = mkdtemp(temp_dirname_cstr);`替换为`errno_t mkdtemp_result = _mktemp_s(temp_dirname_cstr, sizeof(temp_dirname_cstr)); `

将bnll_layer.cu中的`const float kBNLL_THRESHOLD = 50.;`修改为`#define kBNLL_THRESHOLD 50.`

将util/db.cpp中的`mkdir(source.c_str(), 0744)`修改为`_mkdir(source.c_str())`

将util/io.cpp中的`open`函数替换为`_open`，`close`函数替换为`_close`

将util/io.cpp中ReadProtoFromBinaryFile函数中的`int fd = open(filename, O_RDONLY);`替换为`int fd = open(filename, O_RDONLY | O_BINARY);`

在util/math_functions.cpp中添加

```
#define __builtin_popcount __popcnt 
#define __builtin_popcountl __popcnt
```

在common.cpp中添加`#include <process.h>`

将`pid = getpid();`替换为`pid = _getpid();` 

注释掉`::google::InstallFailureSignalHandler();`

将solver.cpp中的`snprintf`函数替换为`sprintf_s`

编译吧！！！！