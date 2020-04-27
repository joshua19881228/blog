---
date: 2016-07-04 00:00:00
title: "Reading Note: Learning Deconvolution Network for Semantic Segmentation"
category: ["Computer Vision"]
tag: ["Reading Note", "Semantic Segmentation"]
---

**TITLE**: Learning Deconvolution Network for Semantic Segmentation

**AUTHER**: Hyeonwoo Noh, Seunghoon Hong, Bohyung Han

**ASSOCIATION**: Department of Computer Science and Engineering, POSTECH, Korea

**FROM**: [arXiv:1505.04366](http://arxiv.org/abs/1505.04366)

### CONTRIBUTIONS ###

1. A multi-layer deconvolution network is designed and learned, which is composed of deconvolution, unpooling, and rectified linear unit (ReLU) layers.
2. Instance-wise segmentations are merged for final sematic segmentation, which is free from scale issues.

### METHOD ###

The main steps of the method is as follows:

1. Object proposals are genereated by alogrithms such as EdgeBox.
2. ROI extracted based on object proposals are sent to the Deconvolution Network. The outputs are instance-wise segmentations.
3. instance-wise segmentations are combined to get the final segmentaton.

**Some Details**

**Architecture of the network** is shown as the following figure. In the network, *unpooling* operation captures example-specific structures by tracing the original locations with strong activations back to image space. On the other hand, *deconvolution* operation learnes filters to capture class-specific shapes.

<img class="img-responsive center-block" src="https://raw.githubusercontent.com/joshua19881228/my_blogs/master/Computer_Vision/Reading_Note/figures/deconv.png" alt="" width="640"/>

**Training** contains two stages. At first stage, simpler data are used to train the network. The simpler data are generated using object annotations and contains constraint appearance of objects. At second stage, complex data are similarly generated but from object proposals.

**Inference** includes a CRF can further bootstrap the performance.

### ADVANTAGES ###

1. It handles objects in various scales effectively and identifies fine details of objects .
2. Deconvolution can generate finer segmentations.

### DISADVANTAGES ###

1. Large number of proposals are needed to get better result, which means higher computational complexity.