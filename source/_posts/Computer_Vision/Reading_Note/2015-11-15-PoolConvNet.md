---
date: 2015-11-15 00:00:00
title: "READING NOTE: Pooling the Convolutional Layers in Deep ConvNets for Action Recognition"
category: ["Computer Vision"]
tag: ["Reading Note", "Action Recognition", "CNN"]
---

**TITLE**: Pooling the Convolutional Layers in Deep ConvNets for Action Recognition

**AUTHOR**: Zhao, Shichao  and Liu, Yanbin and Han, Yahong and Hong, Richang

**FROM**: [arXiv:1511.02126](http://arxiv.org/abs/1511.02126)


### CONTRIBUTIONS ###

1. Propose an efficient video representation framework basing on VGGNet and Two-Stream ConcNets.
2. Trajectory pooling and line pooling are used together to extract features from convolutional layers.
3. A frame-diff layer is used to get local descriptors.


### METHOD ###

<img class="img-responsive center-block" src="https://raw.githubusercontent.com/joshua19881228/my_blogs/master/Computer_Vision/Reading_Note/figures/PoolConvNet.png" alt="" width="640"/>

1. Two succession frames are sent to a siamese VGGNet and a frame-diff layer is used to extract spatial features.
2. Compute temporal feature in one frame using optical-flow net of Two-Stream ConvNet.
3. Extract features in ConvNet feature maps along point trajectories or along lines in a dense sampling manner.
4. Use BoF method to generate video representation
5. Classify video using a SVM classifier.

### ADVANTAGES ###

1. Using deeper network to extract features, which are more discriminative.
2. Different from Two-Stream ConvNet, in this work spatial features are extracted on every frame, which would provide more information.

### DISADVANTAGES ###

1. The two branches are trained independently. Jointly training in a multi-task manner may benefit.

### OTHERS ###

1. The difficulty of human action recognition is caused by some inherent characteristics of action videos such as intra-class variation, occlusions, view point changes, background noises, motion speed and actor differences.
2. Despite the good performance, Dense Trajectory based action recognition algorithms suffer from huge computation costs and large disk affords.