---
date: 2018-01-10 00:00:00
title: "Reading Note: Single-Shot Refinement Neural Network for Object Detection"
category: ["Computer Vision"]
tag: ["Reading Note", "Object Detection"]
---

**TITLE**: Single-Shot Refinement Neural Network for Object Detection

**AUTHOR**: Shifeng Zhang, LongyinWen, Xiao Bian, Zhen Lei, Stan Z. Li

**ASSOCIATION**: CACIA, GE Global Research

**FROM**: [arXiv:1711.06897](https://arxiv.org/abs/1711.06897)

## CONTRIBUTION ##

1. A novel one-stage framework for object detection is introduced, composed of two inter-connected modules, i.e., the ARM (Anchor Refinement Module) and the ODM (Object Detection Module). This leads to performance better than the two-stage approach while maintaining high efficiency of the one-stage approach. 
2. To ensure the effectiveness, TCB (Transfer Connection Block) is designed to transfer the features in the ARM to handle more challenging tasks, i.e., predict accurate object locations, sizes and class labels, in the ODM.
3. RefineDet achieves the latest state-of-the-art results on generic object detection

## METHOD ##

The idea of this work can be seen as an improvement based on [DSSD](https://joshua19881228.github.io/2017-02-10-DSSD/) method. The DSSD method uses multi-scale feature maps to predict categories and regress bounding boxes. In DSSD, deconvolution is also used to increase the resolution of the last feature maps. In this work, a binary classifier and a coarse regressor is added to the downsampling stages. Their outputs are the inputs to the multi-category classifier and fine regressor. The framework this single-shot refinement neural network is illustrated in the following figure.

![Framework](https://raw.githubusercontent.com/joshua19881228/my_blogs/master/Computer_Vision/Reading_Note/figures/Reading_Note_20180110_SSRNN.png "Framework")

### Anchor Refinement Module ###

The ARM is designed to (1) identify and remove negative anchors to reduce search space for the classifier, and (2) coarsely adjust the locations and sizes of anchors to provide better initialization for the subsequent regressor.

In training phase, for a refined anchor box, if its negative confidence is larger than a preset threshold θ (i.e., set θ = 0.99 empirically), we will discard it in training the ODM.

### Object Detection Module ###

The ODM takes the refined anchors as the input from the former to further improve the regression and predict multi-class labels.

### Transfer Connection Block ###

TCB is introduced to convert features of different layers from the ARM, into the form required by the ODM, so that the ODM can share features from the ARM. Another function of the TCBs is to integrate large-scale context by adding the high-level features to the transferred features to improve detection accuracy. An illustration of TCB can be found in the following figure. 

![TCB](https://raw.githubusercontent.com/joshua19881228/my_blogs/master/Computer_Vision/Reading_Note/figures/Reading_Note_20180110_TCB.png "TCB")

### Training ###

The training method is much like SSD. The experiment result and comparison with other method can be found in the following table.

![TCB](https://raw.githubusercontent.com/joshua19881228/my_blogs/master/Computer_Vision/Reading_Note/figures/Reading_Note_20180110_experiment.png "TCB")