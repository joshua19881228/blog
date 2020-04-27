---
date: 2017-07-19 00:00:00
title: "Reading Note: MobileNets: Efficient Convolutional Neural Networks for Mobile Vision Applications"
category: ["Computer Vision"]
tag: "Reading Note"
---

**TITLE**:  MobileNets: Efficient Convolutional Neural Networks for Mobile Vision Applications

**AUTHOR**: Andrew G. Howard, Menglong Zhu, Bo Chen, Dmitry Kalenichenko, Weijun Wang, Tobias Weyand, Marco Andreetto, Hartwig Adam

**ASSOCIATION**: Google

**FROM**: [arXiv:1704.04861](https://arxiv.org/abs/1704.04861)

## CONTRIBUTIONS ##

1. A class of efficient models called MobileNets for mobile and embedded vision applications is proposed, which are based on a streamlined architecture that uses depthwise separable convolutions to build light weight deep neural networks
2. Two simple global hyper-parameters that efficiently trade off between latency and
accuracy are introduced.

## MobileNet Architecture ##

The core layer of MobileNet is depthwise separable filters, named as Depthwise Separable Convolution. The network structure is another factor to boost the performance. Finally, the width and resolution can be tuned to trade off between latency and accuracy.

### Depthwise Separable Convolution ###

Depthwise separable convolutions which is a form of factorized convolutions which factorize a standard convolution into a depthwise convolution and a $1 \times 1$ convolution called a pointwise convolution. In MobileNet, the depthwise convolution applies a single filter to each input channel. The pointwise convolution then applies a $ 1 \times 1 $ convolution to combine the outputs the depthwise convolution. The following figure illustrates the difference between standard convolution and depthwise separable convolution.

![Difference between Standard Convolution and Depthwise Separable Convolution](https://raw.githubusercontent.com/joshua19881228/my_blogs/master/Computer_Vision/Reading_Note/figures/Reading_Note_20170719_MobileNet_0.png "Difference between Standard Convolution and Depthwise Separable Convolution")

The standard convolution has the computation cost of 

$$ D_{k} \cdot D_{k} \cdot M \cdot N \cdot D_{F} \cdot D_{F} $$

Depthwise separable convolution costs

$$ D_{k} \cdot D_{k} \cdot M \cdot D_{F} \cdot D_{F} + M \cdot N \cdot D_{F} \cdot D_{F} $$

### MobileNet Structure ###

The following table shows the structure of MobileNet

![MobileNet Structure](https://raw.githubusercontent.com/joshua19881228/my_blogs/master/Computer_Vision/Reading_Note/figures/Reading_Note_20170719_MobileNet_1.png "MobileNet Structure")

### Width and Resolution Multiplier ###

The Width Multiplier is used to reduce the number of the channels. The Resolution Multiplier is used to reduce the input image of the network.

## Comparison ##

![Comparison](https://raw.githubusercontent.com/joshua19881228/my_blogs/master/Computer_Vision/Reading_Note/figures/Reading_Note_20170719_MobileNet_2.png "Comparison")
