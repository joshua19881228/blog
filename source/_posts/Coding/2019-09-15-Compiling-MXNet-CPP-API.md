---
date: 2019-09-15 00:00:00
title: "Compile MXNet CPP API on Windows10 Using VS2015"
category: "Coding"
tag: ["MXNet"]
---

In order to deploy MXNet based vision engine to projects develped in C++, we need to compile MXNet CPP API. Though the instruction of how to compile is well illustrated in [_Build from Source_](http://mxnet.incubator.apache.org/versions/master/install/windows_setup.html#build-from-source) and [_Build the C++ package_](http://mxnet.incubator.apache.org/versions/master/install/c_plus_plus.html), I still confronted some difficulties. This blog records some tips for compling MXNet CPP API.

1. Modify Source Code

   By following the instruction, I could easily complie and get the libmxnet. However, when compling cpp-package, the `op.h` file can not be generated correctly. In [_issues#14116_](https://github.com/apache/incubator-mxnet/issues/14116), [Vigilans](https://github.com/Vigilans) provided a solution.

   > Here: [https://github.com/apache/incubator-mxnet/blob/master/include/mxnet/tuple.h#L744](https://github.com/apache/incubator-mxnet/blob/master/include/mxnet/tuple.h#L744)
   >
   > ```C++
   > namespace dmlc {
   > /*! \brief description for optional TShape */
   > DMLC_DECLARE_TYPE_NAME(optional<mxnet::TShape>, "Shape or None");
   > MLC_DECLARE_TYPE_NAME(optional<mxnet::Tuple<int>>, "Shape or None");
   > // avoid low version of MSVC
   > #if !defined(_MSC_VER) // <----------- Here !
   > template<typename T>
   > struct type_name_helper<mxnet::Tuple<T> > {
   >  static inline std::string value() {
   >      return "tuple of <" + type_name<T>() + ">";
   >  }
   > };
   > #endif
   > }  // namespace dmlc
   > ```
   >
   > So the specialization of mxnet::tuple<T> was disabled for Visual Studio in the first place!
   > I removed the #if block, recompile, then everything works fine.

2. Set the Environment Variables

   In my own case, I only needed to set `OpenBLAS_HOME` and `OpenCV_DIR`. Both of the can be set by `set` command or `-D` in cmake config.

3. Use CMake to generate VS solution

   `cmake -G "Visual Studio 14 2015 Win64" -DUSE_CUDA=0 -DUSE_CUDNN=0 -DUSE_NVRTC=0 -DUSE_OPENCV=1 -DUSE_OPENMP=1 -DUSE_BLAS=open -DUSE_LAPACK=0 -DUSE_DIST_KVSTORE=0 -DUSE_CPP_PACKAGE=1 -DCMAKE_INSTALL_PREFIX=install ..`

   Above command can be used to generate a solution without GPU support. By modifying config `-DUSE_CUDA` and `-DUSE_CUDNN`, we can generate a solution with GPU support.

4. Generate `op.h`

   After generating libmxnet, we should run `python OpWrapperGenerator.py libmxnet.dll` to generate `op.h`. Note to place `libmxnet.dll`, `libopenblas.dll` and `libopencv_world.dll` together with `OpWrapperGenerator.py`.

5. No `mxnet_static.lib`

   The cpp example project failed to link to `mxnet_static.lib`, which was actually named as `libmxnet.lib`. I modified the name of the static library. I believe the project settings can be fixed to cope with this problem.
