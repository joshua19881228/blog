---
date: 2016-08-19 00:00:00
title: "Reading Note: Factorized Convolutional Neural Networks"
category: ["Computer Vision"]
tag: ["Reading Note"]
---

**TITLE**: Factorized Convolutional Neural Networks

**AUTHER**: Min Wang, Baoyuan Liu, Hassan Foroosh

**ASSOCIATION**: Department of EECS, University of Central Florida, Orlando

**FROM**: [arXiv:1608.04337](http://arxiv.org/abs/1608.04337)

### CONTRIBUTIONS ###

1. A new implementation of convolutional layer is proposed and only involves single in-channel convolution and linear channel projection.
2. The network using such layers can achieves similar accuracy with significantly less computaion.

### METHOD ###

**Convolutional Layer with Bases**

<img class="img-responsive center-block" src="https://raw.githubusercontent.com/joshua19881228/my_blogs/master/Computer_Vision/Reading_Note/figures/alg2.jpg" alt="" width="640"/>

When $b = k^2$, this layer is equivalent to the standard convolutional layer. The number of multiplication required for this layer is $hwbm(k^2 + n)$, which means that by reducing b and increasing k, we create a layer that achieves large convolutional kernel while maintaining low complexity.

**Convolutional Layer as Stacked Single Basis Layer**

<img class="img-responsive center-block" src="https://raw.githubusercontent.com/joshua19881228/my_blogs/master/Computer_Vision/Reading_Note/figures/alg3.jpg" alt="" width="640"/>

One assumption is that the number of output channels is the same as the number of input channels $m = n$, which is the case of that in ResNet. The modified layer can be considered as stacking multiple convolutional layers with single basis. Residual learning is also introduced in thie modified layer, which solves the problem of losing useful information caused by single basis.

**Topological Connections**

<img class="img-responsive center-block" src="https://raw.githubusercontent.com/joshua19881228/my_blogs/master/Computer_Vision/Reading_Note/figures/alg4.jpg" alt="" width="640"/>

A $n$-dimensional topological connections between the input and output channels in convolutional layer is proposed. Each output channel is only connected with its local neighbors rather than all input channels.


### ADVANTAGES ###

1. It is an interesting method of speeding up CNN as the auther claims that the network achieves accuracy of GoogLeNet while consuming 3.4 times less computaion.