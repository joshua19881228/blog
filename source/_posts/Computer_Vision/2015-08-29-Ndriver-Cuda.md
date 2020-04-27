---
date: 2015-08-29 00:00:00
title: "Installation of NVIDIA GPU Driver and CUDA Toolkit"
category: "Computer Vision"
tag: "Caffe"
---

## Table of Content ##

* Content
{:toc}

## Disable default driver ##

1. modify `/etc/modprobe.d/blacklist.conf` by adding following commond at the end of the file:`blacklist nouveau`
2. modify `/etc/default/grub` file by adding `rdblacklist=nouveau nouveau.modeset=0`

## Install NVIDIA GPU Driver ##

1. type `ctrl+alt+F1` to tty and log in.
2. shut down lightdm `sudo service lightdm stop`
3. add source `sudo add-apt-repository ppa:xorg-edgers/ppa`,`sudo apt-get update`.
4. install driver. Note that the version of the driver must be correspondent to the GPU card. One can find the correct version in NVIDIA official website. For example, my GPU is GTX970M, then I should use the following commend: `sudo apt-get install nvidia-352` and `sudo apt-get install nvidia-352-uvm`
5. reboot

## Install CUDA Toolkit ##

1. download CUDA 7.0 .run file from NVIDIA website. Note that the CUDA version should be correspendent to gcc version and driver version.
2. change .run file mode by `chmod +x *.run`
3. install CUDA by `sudo ./*.run`. Note to skip the installation of driver.
4. add necessary environment path by `PATH=/path/to/cuda/bin:$PATH` and `LD_LIBRARY_PATH=/path/to/cuda/lib64:$LD_IBRARY_PATH` and export them. Alternatively, one can modify /etc/profile to set the environment paths. For example, add `export PATH="/path/to/cuda/bin:$PATH"` and in commond line type `source /etc/profile`
5. verify the installation. Change directory to CUDA sample and make the samples. Run a sample `sudo ./deviceQuery` to verify the installation.