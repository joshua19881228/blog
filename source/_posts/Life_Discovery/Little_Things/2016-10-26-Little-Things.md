---
date: 2016-10-26 00:00:00
title: "Little Things [20161026]"
category: "Life Discovery"
tag: "Little Things"
---

没什么新鲜事，应该督促自己读paper了。

一切事情都不能太自以为是，不然是要被打脸的。总部同事以前就怀疑过SVM预测会占用大量CPU资源，但是我们却觉得2000*100的矩阵向量乘应该是轻量级运算，但是经过排查果然还是这个计算出了问题。

在make之前一定要make clean。各种调试代码，一个小时以后，最后却是通过make clean解决了，一切代码又恢复原状，无用功啊！