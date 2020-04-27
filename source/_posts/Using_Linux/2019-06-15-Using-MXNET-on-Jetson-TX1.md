---
date: 2019-06-15 00:00:00
title: "Using MXNET on Jetson TX1"
category: ["Using Linux"]
---

The main steps can be found on [mxnet install instruction](https://mxnet.apache.org/versions/master/install/index.html?platform=Devices&language=Python). When following the steps, I've confronted with some errors.

**1. nvcc path error**

set the correct path using environment path, or modify USE_CUDA_PATH in config.mk

**2. Error 137**

It's because of lack of memory when compiling. We need to add a swap file in size 2G

```bash
dd if=/dev/zero of=/swapfile bs=1k count=2048000
mkswap /swapfile
swapon /swapfile
```

**3. openblas link error**

install openblas-dev by `sudo apt-get install libopenblas-dev`

**4. compile error when installing numpy**

install python-dev by `sudo apt-get install python-dev`

**5. gluoncv dependencies**

```
sudo apt-get install libfreetype6-dev
sudo apt-get install pkg-config
sudo apt-get install libpng12-dev
sudo apt-get install libjpeg-dev
```

**6. image.io error when testing GluonCV**

Need to compile libmxnet.so with OpenCV. Modify USE_OPENCV in config.mk. set `USE_OPENCV = 1`
