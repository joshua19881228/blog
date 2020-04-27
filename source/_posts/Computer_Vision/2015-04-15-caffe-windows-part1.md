---
date: 2015-04-15 00:00:00
title: "VS2013配置Caffe卷积神经网络工具（64位Windows 7）——准备依赖库"
category: "Computer Vision"
tag: "Caffe"
---

2014年4月的时候自己在公司就将Caffe移植到Windows系统了，今年自己换了台电脑，想在家里也随便跑跑，本来也装了Ubuntu可以很方便的配置好，无奈在家的风格是“娱乐的时候抽空学习”，所以移植到Windows还是很有必要的。但是，公司禁止将公司内部资料带出，很多地方又都忘记了，周末磨了一天终于移植完，本篇为记录将Caffe移植至Windows7 x64系统下的一些关键步骤。第一步先看看这老些依赖库怎么搞。

在真正开始编译各依赖库之前，需要准备一些必备工具：

首先当然是VS2013，下载地址：https://www.visualstudio.com/

其次是CMake工具，下载地址：http://www.cmake.org/download/

## Table of Content ##

* Content
{:toc}

## 1. Boost

下载地址：[http://www.boost.org/](http://www.boost.org/)

编译方法：

1. 运行Visual Studio Tools中的VS2013 x64 Cross Tools Command Prompt终端工具
2. 从终端进入boost库所在目录，如D:\LIBS\boost_1_57_0
3. 运行bootstrap.bat生产64位的bjam.exe
4. 输入命令进行编译，更正一下，msvc-12.0才是vs2013哈

静态库：bjam --build-type=complete toolset=msvc-9.0 toolset=msvc-12.0 threading=multi link=static address-model=64

共享库：bjam --build-type=complete toolset=msvc-9.0 toolset=msvc-12.0 threading=multi link=shared address-model=64

## 2. OpenCV

下载地址：[http://opencv.org/downloads.html](http://opencv.org/downloads.html) 本文中使用的是2.4.10版本

编译方法：

下载后的安装包中有已编译好的库，可直接引用，如D:\LIBS\opencv\build\x64\vc12

## 3. OpenBlas

下载地址：[http://sourceforge.net/projects/openblas/files/](http://sourceforge.net/projects/openblas/files/)

编译方法：

OpenBlas库在windows上编译起来比较复杂，这里给出的下载地址是一个已编译好的压缩包OpenBLAS-v0.2.14-Win32.zip (12.1 MB)，直接提供了./bin ./include ./lib路径

## 4. CUDA

下载地址：[https://developer.nvidia.com/cuda-downloads](https://developer.nvidia.com/cuda-downloads)

编译方法：

与OpenCV类似，安装好后直接有已编译好的库。如C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v7.0\include和C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v7.0\lib\x64 

以上是依赖库比较常用，最好将该它们的include和lib加入到Windows的环境变量中，方便后续的库的编译

## 5. GFlags

下载地址：[https://github.com/gflags/gflags](https://github.com/gflags/gflags)

编译方法：

1. 启动CMake(cmake-gui)工具
2. 设置source code路径和build路径
3. 单击Configure按钮，并选择并选择Visual Studio 12 2013 Win64编译器编译器
4. 更新完成后，勾选中BUILD_SHARED_LIBS和BUILD_STATIC_LIBS
5. 单击Generate按钮，生成VS工程 
6. 打开刚刚生成的VS工程，build其中的ALL_BUILD工程，注意选择x64模式，并分别生成Debug和Release下的库 
7. 编译成功后，在工程路径下会生成bin、include、lib三个文件夹 

## 6. GLog

下载地址：[https://github.com/google/glog](https://github.com/google/glog)

编译方法：

该工程中包含VS工程google-glog.sln，打开直接编译即可，同样注意Solution Platform选择x64模式，并分别生成Debug和Release下的库

## 7. LevelDB

下载地址：[https://github.com/bureau14/leveldb](https://github.com/bureau14/leveldb)

这里没有选择官方的[https://github.com/google/leveldb](https://github.com/google/leveldb)是由于官方版本移除了CMake的依赖项，自己写起来比较复杂

编译方法：

与编译GFlags方法类似，唯一需要注意的地方是将CMakeLists.txt中第82行的-DSNAPPY注释掉，否则需要依赖Snappy库，其实并不绝对需要，为了简单起见将此库去掉。另外Leveldb依赖于boost库，如果没有将boost库添加至环境变量，可能需要手动进行设置。

## 8. LMDB

下载地址：[https://gitorious.org/mdb/mdb/archive/462dc097451834477b597447af69c5acc93182b7.tar.gz](https://gitorious.org/mdb/mdb/archive/462dc097451834477b597447af69c5acc93182b7.tar.gz)

编译方法：

1. 解压压缩包到某路径，例如D:\CODE\CXX\mdb-mdb
2. 在VS2013中新建工程，FILE --> New --> Project From Existing Code.. 
3. 选取源码所在路径，并给工程起名 
4. 单击next按钮后选择Project type为Console application project 
5. 将Solution Platform修改为x64模式
6. 注意将工程的输出改为静态库，右键单击工程 --> property --> Configuration Properties --> General --> Project Default --> Configureation Type --> Static library (.lib)
7. 其中一个.c文件中包含了unistd.h，为了解决这个问题需要准备三个文件 unistd.h、getopt.h、getopt.c。unistd.h可以考[http://stackoverflow.com/questions/341817/is-there-a-replacement-for-unistd-h-for-windows-visual-c](http://stackoverflow.com/questions/341817/is-there-a-replacement-for-unistd-h-for-windows-visual-c)解决。另外两个可以从[http://ieng6.ucsd.edu/~cs12x/vc08install/getopt9.zip](http://ieng6.ucsd.edu/~cs12x/vc08install/getopt9.zip)下载
8.  最后编译即可

## 9. ProtoBuf

下载地址：[https://github.com/google/protobuf](https://github.com/google/protobuf)

编译方法：

压缩包里有一个叫vsprojects的文件夹，其中有现成的VS工程，可以用来直接编译，也需要注意将Solution Platform修改为x64模式

## 10. HDF5

下载地址：[http://www.hdfgroup.org/ftp/HDF5/current/src/CMake/hdf518-CMakeWindows.zip](http://www.hdfgroup.org/ftp/HDF5/current/src/CMake/hdf518-CMakeWindows.zip)

编译方法：

解压后，在VS2013 x64 Cross Tools Command Prompt终端工具中运行build-VS2013-64.bat即可。

## 整理头文件和库文件

将5-10的头文件和编译后的库统一整理到一个3rdparty文件夹下，其中包含两个文件夹include和lib

include文件夹下包含gflags、glog、google、hdf5、leveldb、lmdb六个文件。gflags的头文件来自于生成的VS工程目录中的include文件夹；glog的头文件来自于VS工程目录中的src\windows文件夹；google中是protobuf的头文件，来自于压缩包中的src\google文件夹；hdf5来自于压缩包中的CMake\hdf5-1.8.14\src文件夹，保留.h文件即可；leveldb的头文件来自于压缩包的include文件夹；lmdb的头文件来自于压缩包中的libraries\liblmdb文件夹，保留.h文件即可

lib文件夹中的.lib文件直接从编译好的工程目录下拷贝即可，注意debug模式下的.lib文件的文件名修改为xxxd.lib形式

至此，caffe需要的各项依赖库已经准备完毕，后续会上一些图，看起来直观一些。
