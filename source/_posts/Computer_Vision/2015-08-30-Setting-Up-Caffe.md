---
date: 2015-08-30 00:00:00
title: "Set Up Caffe on Ubuntu14.04 64bit+NVIDIA GTX970M+CUDA7.0"
category: "Computer Vision"
tag: "Caffe"
---

## Table of Content ##

* Content
{:toc}

## Prerequisite ##

1. install NVIDIA GTX970M driver
2. install CUDA 7.0 Toolkit

Please refer to my previous blog [Installation of NVIDIA GPU Driver and CUDA Toolkit](https://joshua19881228.github.io/2015-08-29-Ndriver-Cuda/)

## Install OpenBLAS ##

1. download source code from [OpenBLAS official website](http://www.openblas.net/) and extract the archive
2. (optional) install gfortran by `sudo apt-get install gfortran`
3. change directory to the position of extracted folder the and compile `make FC=gfortran`
4. install by `make PREFIX=/your/path install`
5. add paths to envrionment: `PATH=/your/path/to/openblas/include:$PATH` and `LD_LIBRARY_PATH=/your/path/to/openblas/lib:$LD_LIBRARY_PATH` and export the pathes.

## Install Anaconda ##

1. download the script from [http://continuum.io/downloads](http://continuum.io/downloads)
2. change mode `sudo chmod +x Anaconda*.sh`
3. execute the installer by `bash Anaconda*.sh`
4. in ~/.bashrc add

```shell
LD_LIBRARY_PATH=your_anaconda_path/lib:$LD_LIBRARY_PATH
export LD_LIBRARY_PATH
```
NEVER put it in /etc !!! Otherwise, one may be in danger of unable to get into GUI.
5. config HDF5 version

```shell
cd /usr/lib/x86_64-linux-gnu
sudo ln -s libhdf5.so.7 libhdf5.so.10
sudo ln -s libhdf5_hl.so.7 libhdf5_hl.so.10
sudo ldconfig
```

## Install OpenCV ##
One can conveniently install OpenCV by run a shell script from a [Github repository](https://github.com/jayrambhia/Install-OpenCV)

1. download the script. For me, I use OpenCV 2.4.10.
2. change mode of the shell `sudo chmod +x opencv2_4_10.sh`
3. run the script `sudo ./opencv2_4_10.sh`. Note that one may need to modify the cmake settings, such as eliminating QT.

## Install a Set of Dpendencies ##
Following the guideline in [Caffe](http://caffe.berkeleyvision.org/installation.html), we can set up the dependencies by commond `sudo apt-get install libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libboost-all-dev libhdf5-serial-dev libgflags-dev libgoogle-glog-dev liblmdb-dev protobuf-compiler`

## Compile Caffe ##

1. get Caffe from github `git clone https://github.com/BVLC/caffe.git`
2. edit Makefile.config to set correct paths. Firstly create Makefile.config by `cp Makefile.config.example Makefile.config`. Then modify several paths. For me, I set blas to openblas and set blas path to /opt/OpenBLAS/include and /opt/OpenBLAS/lib where I install OpenBLAS; Python is set to Anaconda as well as its paths.
3. compile Caffe by `make -j` and `make pycaffe`
4. In addition, so far Caffe should be able to be compiled without any problem. However, when running exampls such as MNIST, some libs might be missing. My solution is to  add libraries to the system library cache. For example, create a file called cuda.conf in /etc/ld.so.conf.d/ and add the path  "/usr/local/cuda-7.0/lib64" to this file.