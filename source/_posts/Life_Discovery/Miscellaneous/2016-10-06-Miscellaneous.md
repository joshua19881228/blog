---
date: 2016-10-06 00:00:00
title: "Miscellaneous [20161006]"
category: "Life Discovery"
tag: "Miscellaneous"
---

# First Post on 20161006 #

今天搞了一天[Gstreamer](https://gstreamer.freedesktop.org/)，早晨九点起来，就开始弄，不知不觉就到了午饭时间，吃了饭回来继续，不知不觉又到了晚饭时间。第一次搞这种编解码的库，真是头大，搞了这么整整一天，还是没什么结果。只是大概觉得gstreamer的工作原理就是把一堆element整合到一个pipeline里，然后一个多媒体文件就通过这个pipline把音频、视频解码出来，并送到对应的输出设备里。

说起来好像不太难，而且用gst命令也可以播放视频，但是如何把它们通过C代码融合到自己的程序里呢，同时又怎么将视频全屏显示也是个问题，网上很多例子都是用GTK，这个又是个新东西。在github上搜了很多，但是也没有发现一个简单易用的例子，真是挠头。看来还是得耐下心来看文档里。

# Update on 20161007 #

发现了一个很好的Gstreamer教程——[《Gstreamer Small Tutorial》](https://arashafiei.files.wordpress.com/2012/12/gst-doc.pdf)只有十页，读起来很顺畅，作者把Gstreamer的工作原理阐述得十分清晰，而且其中附有一个实例，对于理解有很大帮助。